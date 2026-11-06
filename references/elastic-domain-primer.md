# Domain primer: Elastic Stack and infrastructure observability for PMs

This is the working knowledge a product manager needs to be credible in this domain when the
product is built on the Elastic Stack. It is written for product decisions, not for operating
the tools. Read the section you need. It mirrors `domain-primer.md` (the Grafana version) so
the two can be compared side by side.

## Contents
1. The signals: metrics, logs, traces, profiles
2. The Elastic Stack components
3. The product domains in scope
4. The cost model (why this is the business model)
5. Personas and who buys
6. The buying motion: free and self-managed to cloud
7. Competitive landscape at a glance

---

## 1. The signals

Elastic's centre of gravity is search and analytics over large volumes of semi-structured
data. That heritage shapes how each signal behaves as a product.

- Logs: the anchor signal and the usual land motion. Elasticsearch indexes log content
  (inverted index), so arbitrary search and ad hoc analysis across huge log volumes is fast and
  flexible. The trade-off is indexing cost and storage overhead compared with label-only
  systems like Loki. The LogsDB index mode and synthetic `_source` exist to cut that storage
  cost substantially, and they are central to the cost story.
- Metrics: stored in Elasticsearch, increasingly in time series data streams (TSDS) with
  downsampling. Historically the weaker signal against Prometheus-native stacks, both in
  ecosystem fit (PromQL, exporters) and in efficiency. Expect technical buyers to probe this.
- Traces: Elastic APM, with native OpenTelemetry ingest. Value is the usual one: slow request
  to the span to the log line, with service maps and dependency views.
- Profiles: Universal Profiling, eBPF-based, whole-system, no code changes. Elastic donated its
  profiling agent to OpenTelemetry, which supports the OTel-native positioning.

The PM lesson: Elastic correlates signals through shared fields (Elastic Common Schema, now
merged into OpenTelemetry semantic conventions) and one query surface, rather than through
separate per-signal backends. The product problem is the same as everywhere: move the user
from symptom to cause without friction. The Elastic angle is that every signal is searchable
the same way.

## 2. The Elastic Stack components

- Elasticsearch: the distributed search and analytics engine and the storage layer for every
  signal. Its operational characteristics (shards, mappings, heap, data tiers) show up directly
  in customer cost and pain.
- Kibana: the UI. Discover for exploration, dashboards and Lens for visualisation, plus the
  Observability solution views (Infrastructure inventory and Hosts, APM, Logs, Synthetics,
  SLOs, Alerts).
- Elastic Agent and Fleet: the unified agent and its central management. Integrations are the
  packaged, out-of-the-box collection and dashboards for a given source (Kubernetes, AWS,
  System, Nginx, and hundreds more). The integrations catalogue matters for time-to-value.
- Beats (Metricbeat, Filebeat and others) and Logstash: the older shippers and pipeline tool.
  Agent is the strategic path, but a large installed base still runs Beats and Logstash, so
  migration is a real roadmap concern.
- EDOT (Elastic Distributions of OpenTelemetry): Elastic's OTel collector and SDK
  distributions. The OTel-native ingestion path and an increasingly central part of the story.
- Query languages: ES|QL (piped query language, the strategic direction), KQL and Lucene
  syntax for search bars, and Query DSL for the API. ES|QL is to Elastic what PromQL and LogQL
  are to Grafana: it decides how easy analysis feels.
- Machine learning and AIOps: anomaly detection, log rate analysis, log pattern
  categorisation, and the AI Assistant for Observability (LLM-backed, grounded in the
  customer's own data). Elastic leans hard on "AI on your own data" as a differentiator.

Elastic Common Schema (ECS) is Elastic's field naming standard. Elastic contributed it to
OpenTelemetry, and it is converging with OTel semantic conventions. That matters for lock-in
arguments: the data model is closer to the open standard than it used to be.

## 3. The product domains in scope

For an infrastructure observability PM on Elastic, the typical surface is:

- Kubernetes observability: cluster, node, pod, and workload monitoring through the Kubernetes
  integration (Elastic Agent and kube-state-metrics) or the OTel-based onboarding. Competes
  with the Prometheus-plus-Grafana default most platform teams already know, so time-to-value
  and parity with Prometheus-shaped workflows matter.
- Cloud provider observability: AWS, Azure, and GCP integrations, with log shipping paths such
  as the Elastic Serverless Forwarder and Amazon Data Firehose. Competes with the cloud-native
  built-ins and, on AWS specifically, with Amazon OpenSearch Service.
- Host monitoring: the System integration and the Hosts and Inventory views. The classic
  "watch my machines" job, and often the first thing a logs customer adds when they expand.
- Logs as the domain anchor: log onboarding, parsing, and exploration (Discover, and newer
  AI-assisted log structuring and partitioning work). Elastic wins or loses many infra deals
  on how good the logs experience is.
- Neighbours: APM and application observability above, and Elastic Security (SIEM) beside.
  The same cluster often serves both observability and security, which is a cross-sell lever
  and a source of shared budget and shared ownership.

The recurring product tension: observability is one of three solutions (Search, Observability,
Security) on a shared platform. Roadmap investment in core Elasticsearch and Kibana is shared,
so the infra PM has to argue for platform work in terms the other solutions also benefit from,
while still shipping an opinionated observability experience on top.

## 4. The cost model

The data model is still the business model, but the cost shape differs from a Prometheus-style
stack and depends on how the customer deploys.

- Self-managed: the customer pays for infrastructure (nodes, storage, memory) plus a
  subscription tier, and pays heavily in operational effort. The real cost is often the team
  that keeps the cluster healthy. Capacity planning anxiety is a core pain.
- Elastic Cloud Hosted: priced on deployment resources (memory and storage across data tiers)
  plus the subscription level. Cost tracks how the cluster is sized, which tracks ingest and
  retention indirectly.
- Elastic Cloud Serverless: priced along ingest volume and retained volume (per GB) for
  Observability projects. Closer to the per-GB models customers know from other vendors, and
  so exposed to the same bill-shock dynamics.

Cost drivers specific to Elastic:
- Indexing overhead: full-text indexing everything is expensive. LogsDB mode, synthetic
  `_source`, and selective field mapping are the levers.
- Retention tiering: hot, warm, cold, and frozen data tiers with index lifecycle management
  (ILM) and searchable snapshots on object storage. Frozen tier is how long retention becomes
  affordable. This is Elastic's main answer to "logs are too expensive to keep".
- Mapping explosion: too many distinct fields (often from unstructured JSON) degrades the
  cluster and raises cost. This is the Elastic analogue of metric cardinality.
- Shard sprawl and oversharding: many small indices waste resources. Data streams and good
  defaults reduce this, but it remains a common self-managed failure.
- Metrics cardinality still matters for TSDS, and APM volume is controlled with head and tail
  based sampling.

Bill shock looks different by deployment: on Serverless it is an ingest spike; on Hosted it is a
forced resize; on self-managed it is a cluster outage and an emergency hardware request. Product
features that predict, attribute, and cap cost are first-class in all three.

## 5. Personas and who buys

- SRE and platform engineer: same jobs as anywhere, and they will compare the experience with
  Prometheus and Grafana, which they likely already know.
- The Elasticsearch owner: the person or small team who runs the cluster. Deeply technical,
  cares about stability, upgrades, shard and mapping hygiene, and the operational cost.
  Frequently the internal champion, and the most affected by any self-managed versus cloud
  decision.
- Ops and log-centric teams: long-standing ELK users who think in searches and dashboards.
  The expansion target for metrics, APM, and SLOs.
- Security team: often co-owner of the same cluster and budget through SIEM. Can accelerate or
  block observability expansion.
- Engineering manager or director, and increasingly a platform or tools consolidation owner:
  the budget holder, often motivated by vendor consolidation (one platform for logs, o11y, and
  security).

Self-serve and enterprise differ as usual, but Elastic has an extra split: customers who want a
managed service and customers who must run it themselves (on-premises, air-gapped, regulated,
or sovereign cloud). That second group is a real differentiator for Elastic and drives
packaging and roadmap choices that a cloud-only vendor never faces.

## 6. The buying motion: free and self-managed to cloud

- The funnel starts with free self-managed Elasticsearch and Kibana and an enormous installed
  base of ELK users. The free tier is the credibility engine and the top of funnel.
- Licensing history matters to technical buyers. Elastic moved from Apache 2.0 to a dual
  SSPL and Elastic License v2 model in 2021, which led AWS to fork OpenSearch. In 2024 Elastic
  added AGPLv3 as an option, so it can again describe the core as open source. Some open-source
  minded buyers still remember the change. Address it directly; do not dodge it.
- Paid conversion happens on two axes: subscription tier (features such as machine learning,
  searchable snapshots and the frozen tier, advanced security; check current tier contents) and
  deployment (self-managed to Elastic Cloud Hosted or Serverless).
- The land is often logs or security; the expansion is metrics, APM, profiling, and SLOs on the
  same platform. Infra observability PMs live in that expansion motion.
- Cloud marketplace procurement (AWS, Azure, GCP) and committed-spend consumption contracts are
  a significant part of enterprise buying, so packaging has to work within those mechanics.

## 7. Competitive landscape at a glance

Archetypes, seen from Elastic's side:

- All-in-one commercial suites: Datadog, Dynatrace, New Relic. They win on polish and
  out-of-the-box breadth for infra and APM. Elastic competes on search-grade log analytics,
  deployment flexibility, cost at log volume, and one platform for o11y and security.
- Splunk (now part of Cisco): the log-and-SIEM incumbent. A frequent displacement target on
  cost and on product direction uncertainty after the acquisition.
- Grafana and the Prometheus ecosystem: the default for metrics and Kubernetes for many
  platform teams, with Loki undercutting Elastic on log storage cost by not indexing content.
  Elastic's answer is query power over logs, built-in ML, and an integrated solution rather than
  a composed one. Elastic is weaker on the Prometheus-native metrics workflow and has to be
  honest about it.
- OpenSearch and Amazon OpenSearch Service: the fork, and the AWS-default option. Competes on
  price and on being already inside the AWS account. Elastic's response is the feature and
  innovation gap (ES|QL, LogsDB, AI Assistant, observability solution depth) and the vendor
  behind the product.
- Cloud-native built-ins: CloudWatch, Google Cloud Operations, Azure Monitor. Same
  "good enough until it is not" substitution risk as for any vendor. Elastic often enters as
  the place cloud logs go when built-in log search gets too expensive or too limited.

Durable differentiation angles for the Elastic Stack: search and analytics over high-volume,
messy data; one platform and one data store for observability and security; OpenTelemetry-native
ingestion with ECS contributed to the standard; deployment anywhere, including self-managed and
air-gapped; and ML and AI grounded in the customer's own data. Each is a position to prove with
the product, not a tagline.
