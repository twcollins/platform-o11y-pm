# Domain primer: Grafana stack and infrastructure observability for PMs

This is the working knowledge a product manager needs to be credible in this domain. It is
written for product decisions, not for operating the tools. Read the section you need.

## Contents
1. The signals: metrics, logs, traces, profiles
2. The Grafana stack components
3. The product domains in scope
4. The cost model (why this is the business model)
5. Personas and who buys
6. The buying motion: open source to cloud
7. Competitive landscape at a glance

---

## 1. The signals

Observability rests on a few data types, and the differences drive cost, product, and pricing.

- Metrics: numeric time series, cheap per point, queried with PromQL in this stack. Cost is
  driven by cardinality (the number of unique label combinations), not raw volume. A single
  badly chosen label can multiply series and cost.
- Logs: text events, high volume, queried in Loki with LogQL. Loki's angle is indexing only
  labels rather than full text, which lowers cost versus full-text systems.
- Traces: the path of a request across services, stored in Tempo, sampled to control volume.
  The product value is connecting a slow request to the exact span that caused it.
- Profiles: code-level CPU and memory data (Pyroscope). The newest signal, used to go from
  "this service is slow" to "this function is the cost".

The PM lesson: these signals have very different volume and cost shapes, and customers want
them correlated. The hard product problem is moving a user from one signal to the next (metric
spike to the trace to the log line to the line of code) without friction.

## 2. The Grafana stack components

- Grafana: the visualisation and dashboarding layer, the part most people recognise. The front
  door to everything else.
- Prometheus: the de facto open-source metrics standard. Pull-based scraping, PromQL. Grafana
  Labs builds with and around it.
- Mimir: scalable long-term storage for Prometheus metrics. Where "Prometheus does not scale
  for us" gets solved.
- Loki: log aggregation, label-indexed, cost-efficient. "Prometheus, but for logs."
- Tempo: distributed tracing backend, cheap object-storage based.
- Pyroscope: continuous profiling.
- Alloy: the OpenTelemetry-native collector and agent that gathers and ships telemetry. The
  ingestion layer; important because it is where instrumentation meets the platform.
- k6: load testing, which ties performance testing to observability.

The metrics, logs, and traces trio is often called the LGTM stack (Loki, Grafana, Tempo,
Mimir). OpenTelemetry (OTel) is the vendor-neutral standard for generating and shipping
telemetry, and "OTel-native" is a core positioning line because it means no lock-in to a
proprietary agent.

## 3. The product domains in scope

For an infrastructure observability PM, the typical surface is:

- Kubernetes Monitoring: out-of-the-box monitoring for Kubernetes clusters, workloads, and
  costs. High-value because almost every modern platform team runs Kubernetes and setup pain
  is real. A mature, popular product where the job is retention, satisfaction, and depth.
- Cloud Provider Observability: monitoring AWS, GCP, and Azure resources by pulling provider
  metrics and logs. Competes directly with the cloud-native built-ins. Mature.
- Host Monitoring: classic server and VM monitoring (CPU, memory, disk, network). Often the
  net-new or simplifying bet: make the basic "watch my machines" case effortless and a natural
  on-ramp to the rest.
- Application Observability: the application-layer neighbour. Infra o11y must integrate with
  it so users move from host to container to service to request seamlessly.

The recurring product tension: defend and deepen the mature products while opening net-new use
cases, without fragmenting the experience.

## 4. The cost model

In observability, the data model is the business model. Customers pay roughly along data
volume: active metric series, ingested log and trace bytes, or data points per minute. This
has direct product consequences.

- Cardinality is the cost driver for metrics. Product features that let users find, attribute,
  and control high-cardinality series are genuinely valuable because they map straight to the
  customer's bill.
- The "bill shock" problem is the defining pain of the category. A customer whose cost
  silently triples will churn or distrust the vendor. Cost visibility, prediction, and control
  are first-class product concerns, not billing afterthoughts.
- Pricing along volume can punish good behaviour (more instrumentation, more cost). Strong
  packaging gives customers ways to keep coverage high and cost predictable: aggregation,
  adaptive sampling, drop rules, tiered retention.

Any roadmap, pricing, or packaging decision that ignores this model will fail in the market.

## 5. Personas and who buys

- SRE: reliability owner, lives in incidents, cares about MTTD, MTTR, SLOs, and noise. Deep
  user, allergic to fluff.
- Platform engineer: builds the internal platform other developers use. Often the actual buyer
  and the person who standardises the org on a stack. Cares about self-service, multi-tenancy,
  cost governance.
- Application developer: wants their service observable without becoming an observability
  expert. Time-to-value and sane defaults matter most.
- Engineering manager or director: cares about cost, reliability outcomes, and team
  efficiency. Often the budget holder for the enterprise deal.

Self-serve users and enterprise buyers want different things. The self-serve individual wants
to be productive in minutes alone. The enterprise org wants governance, security, scale, and
support. Roadmaps and packaging usually have to serve both.

## 6. The buying motion: open source to cloud

Grafana-style companies run an open-source funnel. Users adopt the OSS tools for free, hit
scale or operational pain, and convert to the managed cloud or enterprise offering. This
shapes product strategy:

- The OSS project is the top of funnel and the credibility engine. It cannot be starved to
  push the paid product, or the funnel dries up.
- The paid conversion has to solve a real "I do not want to run this myself at scale" problem:
  managed scaling, retention, support, security, and governance.
- Self-serve cloud sign-up is the growth motion; enterprise sales is the expansion motion.
  Pricing and packaging have to make the path from one to the other obvious.

## 7. Competitive landscape at a glance

Archetypes matter more than logos:

- All-in-one commercial suites: DataDog (the market reference point, broad, polished,
  expensive, proprietary agent), New Relic, Dynatrace. They win on breadth and integration,
  and are vulnerable on cost and lock-in.
- Cost-and-scale challengers: Chronosphere (Prometheus-compatible, cost-control narrative),
  and the open Prometheus ecosystem itself. They compete on exactly the cost and openness
  ground this stack also claims, so differentiation has to be sharp.
- Cloud-native built-ins: CloudWatch, Google Cloud Operations, Azure Monitor. Free-ish,
  default, "good enough" for many until they are not. The biggest silent substitution risk in
  infra o11y. Know precisely where they fall short (cross-cloud, depth, correlation, Kubernetes
  detail) and make that the wedge.
- Elastic and Splunk: log-heavy heritage, strong in their niche, costly at scale.
- Honeycomb: strong on high-cardinality and developer-led tracing, a thought-leadership
  competitor more than a volume one.

Durable differentiation angles for the Grafana stack: open and composable rather than locked
in, OpenTelemetry-native, cost and cardinality control, and a unified experience across
signals. Each is a position to prove with the product, not a tagline.
