## Fan-out coverage: how to migrate from OpenTelemetry to Cabletrace

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (2)
  what is Cabletrace
    → "Cabletrace collects traces from your services using eBPF, so you get request-level visibility without adding a single line of instrumentation code to your application."

  how to decide which services should keep manual OpenTelemetry instrumentation after migrating to Cabletrace
    → "For services where the manual spans are mostly network boundaries, Cabletrace replaces them entirely."

NOT EXTRACTABLE (5)
  how does Cabletrace capture traces without instrumentation
    → found in: paragraph beginning "Cabletrace attaches to the kernel instead" (under "Distributed tracing that does not require you to instrument everything")
    → fails: criterion 2 (unresolved reference — "instead" points to the manual-instrumentation problem described two paragraphs earlier, which is not present in this passage)
    → fix: open with "Cabletrace attaches to the Linux kernel rather than instrumenting application code," resolving the comparison inside the passage itself

  what's the difference between what Cabletrace captures and what OpenTelemetry manual instrumentation captures
    → found in: "Step 2: Compare traces for one service"
    → fails: criterion 5 (assumes the reader already knows a comparison is underway — "your manual instrumentation" and "before you commit" both depend on context stated earlier in the section)
    → fix: rewrite as a self-contained statement naming both systems directly, e.g. "Cabletrace and OpenTelemetry manual instrumentation capture different span types: Cabletrace sees syscalls but not in-process function calls, while manual instrumentation sees function-level spans only where a developer added them."

  what are the requirements to run Cabletrace (kernel version, Kubernetes version, Windows support)
    → found in: "Supported environments"
    → fails: criterion 3 (the first two sentences list version numbers with no named subject — "Cabletrace" appears only in the third sentence)
    → fix: open with "Cabletrace requires Kubernetes 1.24 or newer running kernel 5.8 or newer" so the entity is named immediately

  does running Cabletrace alongside OpenTelemetry affect OTel billing during migration
    → found in: "Step 1: Install the Cabletrace agent alongside your existing collector"
    → fails: criterion 2 (unresolved "Both," "this period," and "the OTel side" — none is defined within the passage itself)
    → fix: rewrite as one self-contained sentence, e.g. "Running the Cabletrace agent and an OpenTelemetry collector on the same node causes both to report the same requests, so account for this overlap in OpenTelemetry's volume-based billing."

  how to remove the OpenTelemetry collector after migrating to Cabletrace
    → found in: "Step 4: Remove the OTel collector"
    → fails: criterion 2 (unresolved "the collector" — the OpenTelemetry collector is named only in the heading, not in the passage body)
    → fix: name it in the first sentence, e.g. "Once every service has a strategy, remove the OpenTelemetry collector and point remaining OTLP exporters at the Cabletrace agent's local endpoint."

ABSENT (4)
  why don't trace IDs carry over during the OpenTelemetry-to-Cabletrace cutover
    → no passage answers this
    → suggested: the page states that trace IDs aren't preserved and what breaks as a result, but never states the cause — a sentence explaining that Cabletrace derives trace identity from kernel-observed correlation rather than from OpenTelemetry's propagated trace context

  when should you migrate from OpenTelemetry to Cabletrace
    → no passage answers this
    → suggested: a passage stating what conditions (team size, instrumentation drift, infrastructure readiness) make migrating worthwhile, and when staying on OpenTelemetry alone is the better call

  does Cabletrace actually capture spans that manual OpenTelemetry instrumentation misses
    → no passage answers this
    → suggested: a passage citing a specific benchmark, example trace comparison, or methodology behind the claim, rather than the claim stated once with no support

  how to install the Cabletrace agent alongside an existing OpenTelemetry collector
    → no passage answers this
    → suggested: the actual installation steps or command (e.g., the manifest or CLI invocation used to deploy the DaemonSet), not just a description of the resulting behavior

WHAT GOOD LOOKS LIKE
  criterion 2 — no unresolved references
    before: The thing that actually worked was splitting every schema change into two deploys that are individually reversible. In the first deploy, you make the additive change only. In the second deploy, which happens after the first has been stable for at least one full business day, you switch the application to read from the new shape and only then remove the old one.
    after:  Expand and contract splits a database schema change into two separately deployed steps, each of which can be reversed on its own. The expand deploy makes only additive changes — a nullable column, a new table, an index created concurrently — while the application still reads the old shape. The contract deploy, run once the expand deploy has been stable for at least one full business day, switches reads to the new shape and removes the old one.
