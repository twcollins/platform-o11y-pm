# GTM reference (Elastic Stack): competitors, pricing patterns, positioning

Detail to support competitive analysis (workflow 4) and GTM and packaging (workflow 5) when the
product is built on the Elastic Stack. It mirrors `gtm-reference.md` (the Grafana version).
Treat the specifics here as a starting frame. Verify current pricing, tier contents, and feature
claims before putting them in customer-facing collateral, since vendors, Elastic included,
change them often.

## Competitor detail by archetype

### All-in-one commercial suites
- Datadog: the category reference point and the most common competitor in infra and APM
  deals. Polished out-of-the-box infra monitoring and a huge integration catalogue. Weaknesses
  from Elastic's side: cost at log volume (customers often index only a fraction of their logs
  to control the bill), proprietary agent, and no self-managed option. The wedge is usually
  logs: "keep and search all of them" versus "sample and pay per indexed GB".
- Dynatrace: strong in large enterprise APM with automatic instrumentation and causal AI.
  Premium price. Elastic competes on flexibility, log analytics depth, and cost, less on
  automatic root cause in pure APM deals.
- New Relic: consolidated, usage-plus-users pricing. Elastic competes on deployment choice and
  log economics.

### Log and SIEM incumbents
- Splunk (Cisco): the classic displacement target. Powerful and entrenched, expensive at
  volume, and some customers are uncertain about direction after the acquisition. Elastic's
  pitch is comparable search power, lower cost at volume, and observability plus security on
  one platform. Migration tooling and query translation (SPL to ES|QL) reduce switching cost
  and are worth roadmap attention.

### Composable open-source stacks
- Grafana LGTM and Prometheus: often already in place for metrics and Kubernetes. Where they
  genuinely win: Prometheus-native metrics, PromQL familiarity, Loki's cheap log storage, and an
  open, composable story. Where Elastic wins: ad hoc search and analytics over log content,
  integrated ML and AIOps, one data store across signals and security, and an opinionated
  solution rather than an assembly job. Do not pitch Elastic as a better Prometheus. Pitch
  coexistence for metrics where needed, and win on logs and correlation.

### The fork
- OpenSearch and Amazon OpenSearch Service: closest like-for-like, often cheaper on paper and
  already approved inside AWS accounts. Elastic's case: the product gap (ES|QL, LogsDB storage
  efficiency, observability solution depth, AI Assistant), pace of innovation, and support from
  the company that builds the engine. Avoid landmines: do not overstate the gap on basic search
  and dashboards, where they are close.

### Cloud-native built-ins
- CloudWatch, Google Cloud Operations, Azure Monitor: the silent substitution risk. They stop
  being good enough at multi-cloud and hybrid coverage, at long log retention economics, at
  search and analytics depth, and at correlation with security data. Elastic frequently enters
  as the destination for cloud logs when built-in log search becomes expensive or limiting.

## Pricing and packaging patterns for Elastic

- Two packaging axes, not one: subscription tier (feature-gated) and deployment model
  (self-managed, Elastic Cloud Hosted, Elastic Cloud Serverless). Any packaging proposal must
  say how it works on each, or explicitly which it applies to.
- Value metric differs by deployment. Serverless Observability is priced along ingest and
  retention volume. Hosted is priced along provisioned resources. Self-managed is priced by
  subscription, commonly on resource units. Each shapes behaviour differently, so model it.
- Tier gating: historically features such as machine learning anomaly detection, advanced
  security, and searchable snapshots or the frozen tier sit in higher tiers. Gating a cost
  control behind a premium tier can be read as punishing customers for wanting lower bills.
  Decide deliberately.
- Retention is a packaging lever. Data tiers and searchable snapshots let customers keep long
  retention cheaply. Packaging can make "keep everything, query it later" a selling point
  against per-GB-indexed competitors.
- Storage efficiency is pricing power. LogsDB mode and synthetic `_source` reduce storage
  needs, which on resource-priced deployments directly lowers the bill. That is a strong
  competitive proof point, but it can lower revenue per GB, so align pricing and product early.
- Bill-shock control is part of the product: ingest pipelines that drop or reduce data,
  sampling for APM, ILM policies with sane defaults, spend visibility and alerting on
  Serverless. Packaging without guardrails is incomplete.
- Consolidation pricing: deals often bundle observability and security on shared resources or
  committed spend. The infra PM needs to know how o11y usage draws down a shared commitment.

## Durable positioning angles

These are positions to prove with the product, not taglines:
- Search-grade analytics on all your data. Fast, ad hoc investigation across high-volume logs
  and fields, with ES|QL, versus tools that make you decide in advance what to index or label.
- One platform for observability and security. Same data, same agent, same cluster, fewer
  vendors.
- OpenTelemetry-native. EDOT distributions, OTLP ingest, and ECS contributed to OTel semantic
  conventions. Customers own their instrumentation.
- Deploy anywhere. Serverless, hosted, self-managed, on-premises, and air-gapped, with the same
  product. A real differentiator in regulated and sovereign environments.
- AI and ML on your own data. Anomaly detection, log pattern analysis, and an AI Assistant
  grounded in the customer's telemetry and runbooks.

Honest weak points to plan around rather than hide:
- Prometheus-native metrics workflows and PromQL familiarity favour Grafana stacks.
- Operating Elasticsearch at scale is hard, which is why the cloud offerings exist; do not
  claim self-managed is effortless.
- Licence history still costs trust with some open-source buyers.

When writing a battlecard or datasheet, pick the angle that matches the competitor and the
segment, and back it with a concrete workflow or number. The technical buyer discounts
adjectives and rewards specifics.
