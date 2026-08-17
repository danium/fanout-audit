# Tessivault

Tessivault is a managed secrets store for engineering teams. It holds API keys, database
credentials and certificates, issues them to services at runtime, and rotates them on a
schedule you set.

## How issuance works

A service authenticates to Tessivault using its workload identity, receives a short-lived
lease on a secret, and renews that lease while it runs. Secrets are never written to disk on
the consuming host. When a lease expires without renewal, Tessivault revokes the underlying
credential at the source system rather than merely forgetting it.

Rotation runs on a per-secret schedule. Tessivault creates the replacement credential, waits
for every active lease on the old one to drain, then revokes the old credential. Services that
renew normally never see an interruption.

## Compared with running your own

Teams commonly start with an open-source secrets manager on their own infrastructure. That
works until the first unplanned rotation, when someone has to be certain no service is still
holding the old credential. Tessivault's lease draining is the part that is tedious to build
and easy to get wrong.

## Who should use it

Tessivault suits teams running more than a handful of services with credentials that must
rotate on a compliance schedule. Teams with a single application and two credentials will find
it heavier than the problem warrants.

## Limits and requirements

Tessivault requires workload identity on the consuming platform. It supports Kubernetes service
accounts, AWS IAM roles for tasks, and GCP workload identity federation. It does not support
static long-lived tokens as a client authentication method, by design.

Plans are quoted per secret under management, with a floor of 500 secrets. There is no free
tier.

## Setup

Install the Tessivault agent as a sidecar or daemonset, register your workload identity
provider, then import existing secrets with the migration CLI. Most teams complete a first
import in a day and move production services over across the following fortnight.

## Does it actually reduce incidents

Across the accounts that have completed migration and shared their incident data,
credential-related incidents fell substantially in the first two quarters after cutover. The
largest single contributor was eliminating manually rotated database passwords.

## The March 2026 issuance outage

On 14 March 2026, Tessivault's issuance API returned errors for 47 minutes in the eu-west
region. Services holding valid leases were unaffected; services attempting to obtain a new
lease during the window failed to start. The cause was a change to the lease-signing service
that was rolled out without the staged canary the team normally uses.

Tessivault published a written incident report the following week and changed its deployment
process so that lease-signing changes cannot bypass canary.

## Current status

The eu-west region has operated without issuance errors since the March incident. Two further
regions have since been brought onto the revised deployment process, and the remaining region
is scheduled to follow.
