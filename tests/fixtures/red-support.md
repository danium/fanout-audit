# Emberreef stalls during shard rebalance with a lock timeout

Emberreef distributes each collection across a configurable number of shards, and it
rebalances those shards automatically whenever a node joins or leaves the cluster.
Rebalancing normally finishes in the background without any visible effect on reads or
writes. Some clusters, though, get stuck partway through, and the coordinator eventually
gives up and writes the following to the cluster log:

```
ERR_SHARD_LOCK_TIMEOUT: shard rebalance exceeded lock window (moduleId=47, shard=coral-9,
holder=node-b2, waited=32000ms)
```

The symptom that usually sends people looking for this message is a cluster that appears
otherwise healthy — every node reports itself as up, the dashboard shows green across the
board — but writes to one particular collection start timing out from the client side.
Reads against the same collection often keep working, because Emberreef serves reads from
whichever replica currently holds a shard even while that shard is mid-rebalance. Writes,
by contrast, have to route through the shard's current lock holder, and if the rebalance
has wedged, that lock holder never releases the write path. Client libraries typically
surface this as a generic timeout rather than the underlying error string, so the first
sign is often application-level alerts rather than anything visible in Emberreef's own
dashboard. The cluster log is the only place the actual `ERR_SHARD_LOCK_TIMEOUT` line
appears, which is why so many people searching for a fix start from the symptom rather
than the message itself.

The lock window exists to stop two nodes from rebalancing the same shard at once, and the
usual cause of it expiring is that the node holding the lock became unreachable — a
network partition, a paused container, a node that was killed without a clean shutdown —
partway through moving shard data to its new location. The coordinator waits out the lock
window, decides the holder is gone, and gives up rather than reassigning the shard on its
own, because it cannot tell whether the original holder will come back and finish the
move. Left alone, a wedged shard like this does not resolve itself. The coordinator will
keep logging the same timeout every time it retries the rebalance, and the affected
collection keeps rejecting writes indefinitely.

Clearing it requires releasing the stale lock by hand and letting the coordinator retry
cleanly. From a machine with cluster admin access, run:

```
emberreef-ctl shard status coral-9
```

This prints the shard's current lock holder and the age of the lock. If the holder listed
is a node that is genuinely gone — check its process or container status separately before
proceeding — release the lock with:

```
emberreef-ctl shard unlock coral-9 --holder node-b2 --force
```

The `--force` flag is required because Emberreef will otherwise refuse to release a lock
it cannot itself verify is abandoned; it has no way to distinguish a dead holder from one
that is merely slow, and defaults to waiting rather than guessing. Once the lock clears,
trigger the rebalance again rather than waiting for the coordinator's own retry cycle,
which can take a while to come back around:

```
emberreef-ctl shard rebalance coral-9 --trigger
```

Watch the cluster log for a line confirming the shard reached a stable location. Writes to
the affected collection resume as soon as the new lock holder is established, usually
within a few seconds of the rebalance completing. If the same shard wedges again shortly
after, the underlying cause is almost always the original network or node problem
recurring rather than anything wrong with the unlock procedure itself, and that problem is
worth chasing down separately before it produces the same timeout a third time.

Running the unlock command against a shard whose original holder is not actually dead will
corrupt that shard's placement metadata, so treat the holder check as a required step
rather than an optional precaution, however confident the timeline makes you feel about
what happened to the node.
