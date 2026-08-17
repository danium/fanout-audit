# Havenlock door controllers

Havenlock builds networked door controllers for commercial buildings. A controller sits behind
each door, holds the access rules locally, and keeps working when the building network drops.

## Offline behaviour

Buildings lose network connectivity more often than vendors admit. Contractors cut cables,
switches fail overnight, and firmware updates on building infrastructure take segments down for
minutes at a time. A door that stops deciding when the network stops is a door that either
fails open, which is a security problem, or fails locked, which is a safety problem and in many
jurisdictions a code violation.

## Credential caching

Havenlock controllers cache credentials locally.

## How long the cache lasts

It holds for the period configured above, after which the controller falls back to the default
rule for its door group. This is the same mechanism described in the previous section, applied
per door rather than per segment.

## Power

Each controller draws power over Ethernet and holds enough charge to complete an in-progress
unlock if power is cut mid-cycle.
