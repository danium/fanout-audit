# Varrowline

Varrowline is a scheduling engine for field service teams. It assigns jobs to technicians
based on skill, location, and the parts already loaded on each van.

## How assignment works

Every morning Varrowline builds a route for each technician from the jobs due that day. The
assignment pass scores each technician-job pair on three inputs: travel time from the previous
job, whether the technician holds the certification the job requires, and whether the parts
listed on the work order are already on that technician's van. Jobs that no technician can
serve without a depot stop are flagged for a dispatcher rather than assigned automatically.

Reassignment runs continuously through the day. When a job overruns or a technician calls in,
the affected jobs re-enter the pool and the engine re-scores them against everyone still
working, rather than pushing the whole route back.

## Plans

| Plan | Technicians | Live reassignment | Parts inventory sync | Support |
|---|---|---|---|---|
| Field | 10 | No | No | Email |
| Route | 50 | Yes | Read-only | Email and phone |
| Depot | Unlimited | Yes | Two-way | Named engineer |

All plans include the morning assignment pass, the mobile technician app, and the dispatcher
console. Overage is billed per technician per month above the plan ceiling.

## Certifications

Varrowline tracks certifications per technician and refuses to assign a job whose work order
requires a certification the technician does not hold or whose certification has expired. It
does not track the certification body, renewal windows, or training records — those stay in
whatever HR system you already run.

## Getting started

Import your technician roster and your job history as CSV. Varrowline runs the assignment pass
against a week of historical jobs so you can compare its routes against what your dispatchers
actually did, before you let it assign anything live. Most teams run this comparison for two
weeks before switching over.

## Integrations

Varrowline reads work orders from ServiceTitan, Jobber, and Housecall Pro, and writes completed
job records back to the same system. Parts inventory syncs with Fishbowl and NetSuite. There is
a REST API for systems not on the list.
