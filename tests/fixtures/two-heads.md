# Cabletrace

## Distributed tracing that does not require you to instrument everything

Cabletrace collects traces from your services using eBPF, so you get request-level visibility without adding a single line of instrumentation code to your application.

Traditional tracing asks you to wrap every outbound call, propagate a context object through every function signature, and remember to do it again in every new service. Teams start strong and drift. Six months in, half the services are instrumented and the traces have holes in exactly the places you need them.

Cabletrace attaches to the kernel instead. It observes socket-level activity, reconstructs request boundaries from protocol parsing, and correlates spans using timing and connection identity. You install an agent per node and traces appear.

### What you get

- Full request waterfalls across HTTP, gRPC, and Postgres
- Automatic service dependency maps built from observed traffic
- p50, p95, and p99 latency per endpoint, per service, per version
- Retention of 30 days at full fidelity, longer with sampling

### Supported environments

Kubernetes 1.24 and above on kernel 5.8 or newer. Bare metal Linux with the same kernel requirement. Cabletrace does not support Windows containers or kernels older than 5.8, because the eBPF features it depends on are not present.

---

## Migrating to Cabletrace from OpenTelemetry

If you already run OpenTelemetry, you do not have to rip it out. Most teams run both during transition and cut over per service.

### Step 1: Install the Cabletrace agent alongside your existing collector

The agent runs as a DaemonSet and does not conflict with an OTel collector on the same node. Both will report the same requests during this period, which is expected and which you should account for in any volume-based billing on the OTel side.

### Step 2: Compare traces for one service

Pick a service with good existing instrumentation. Run both for a week. Cabletrace will show spans your manual instrumentation missed, and your manual instrumentation will show application-internal spans that Cabletrace cannot see, because it observes syscalls and not function calls. This asymmetry is the main thing to understand before you commit.

### Step 3: Decide your span strategy per service

For services where the manual spans are mostly network boundaries, Cabletrace replaces them entirely. For services with meaningful in-process spans, such as a worker that does a long CPU-bound transform between two network calls, keep the manual instrumentation for those spans and let Cabletrace handle the rest. The agent accepts OTLP on a local port so your manual spans join the same trace.

### Step 4: Remove the OTel collector

Once every service has been compared and assigned a strategy, drop the collector. Point any remaining OTLP exporters at the Cabletrace agent's local endpoint.

### What breaks during migration

Trace IDs are not preserved across the cutover. Traces that begin under OTel and end under Cabletrace will appear as two separate traces for the duration of the transition. If you have alerting keyed on trace continuity, expect noise during the overlap window and silence it in advance.
