---
name: platform-o11y-pm
description: >
  Product management toolkit for infrastructure observability products, with two stack
  options: the Grafana stack (Grafana, OpenTelemetry, Prometheus, Mimir, Loki, Tempo, Alloy)
  or the Elastic Stack (Elasticsearch, Kibana, Elastic Agent, EDOT, Elastic Observability).
  Use this whenever the user does product work for an observability or monitoring product:
  a product vision or roadmap, customer discovery or feedback synthesis, a PRD or one-pager,
  competitive analysis against DataDog, New Relic, Dynatrace, Chronosphere, Splunk,
  OpenSearch, Grafana, Elastic or the cloud-native tools, pricing, packaging, licensing or
  tiering decisions, GTM or sales-enablement collateral, or prioritising tradeoffs with
  engineering. Trigger it even when the user only says "roadmap", "discovery", "competitive
  analysis", "pricing", "PRD", "feedback synthesis" or "datasheet" in an observability,
  monitoring or infra context, and even if they do not name Grafana or Elastic.
---

# Infra observability product management (Grafana or Elastic)

This skill is a working toolkit for a product manager who owns an infrastructure
observability domain on either the Grafana stack or the Elastic Stack. It maps the day-to-day
PM tasks (vision, discovery, prioritisation, competitive work, and go-to-market) onto the
specific reality of observability products, where the buyer is technical, the data has a
cost model, and the competition is well funded.

## Step 0: choose the stack

The workflows and templates are shared. The stack decides which domain knowledge, competitors,
cost model, and positioning you use. Pick one before starting:

| Stack option | Pick it when | Domain primer | GTM reference |
|---|---|---|---|
| A. Grafana stack | The user mentions Grafana, Prometheus, Mimir, Loki, Tempo, Alloy, LGTM, or Grafana Cloud | `references/domain-primer.md` | `references/gtm-reference.md` |
| B. Elastic Stack | The user mentions Elastic, Elasticsearch, Kibana, ELK, Elastic Agent, Fleet, Beats, Logstash, EDOT, ES\|QL, or Elastic Cloud | `references/elastic-domain-primer.md` | `references/elastic-gtm-reference.md` |

If the request does not say which stack, and earlier conversation does not settle it, ask the
user once. If they do not mind, default to the Grafana stack and say so. If the user wants a
comparison of the two (for example, a battlecard for Elastic versus Grafana), read both primers
and write from the side the user's product is on.

Once chosen, stay consistent. Do not mix one stack's components, query languages, or cost levers
into the other's artifacts. "Reduce cardinality with Alloy drop rules" is a Grafana answer; the
Elastic equivalent is something like "control mapping growth and drop fields in an ingest
pipeline". The stack-specific notes in each workflow below flag where the two differ most.

Before doing any task below, skim the chosen stack's domain primer if you are not already
fluent in it. Good observability PM work falls apart when the PM cannot speak credibly about
cardinality or mapping explosions, OTel, pull versus push, data tiers, or why a customer's bill
doubled. The primer keeps that grounded.

## How to use this skill

Pick the workflow that matches the request. Each one has a short method below and a matching
template in `references/templates.md`. For competitive, pricing, and market specifics, also
read the chosen stack's GTM reference. Keep outputs concrete and evidence-led. Avoid filler,
avoid hand-waving about "delight", and tie every claim back to a user problem or a number.

| If the user wants to... | Use workflow | Template |
|---|---|---|
| Turn ideas and feedback into a vision or roadmap | 1. Vision and roadmap | Vision brief, Roadmap |
| Understand customer problems, needs, pain points | 2. Customer discovery | Discovery guide |
| Represent user feedback to engineering, justify tradeoffs | 3. Synthesis and prioritisation | Feedback synthesis, PRD |
| Size the market or analyse competitors | 4. Competitive and market analysis | Competitive matrix |
| Decide pricing, packaging, tiering, or build sales collateral | 5. GTM and packaging | Pricing brief, Battlecard, Datasheet |

A real request often spans two or three of these. Sequence them: discovery feeds synthesis,
synthesis feeds the roadmap, the roadmap and competitive picture feed GTM.

---

## 1. Vision and roadmap

Turn scattered ideas, feedback, and strategy input into a defensible direction for the
domain (for example Kubernetes monitoring, cloud provider observability, or net-new host
monitoring), then a roadmap that engineering and leadership can commit to.

Method:
- State the domain's reason to exist in one paragraph: who it serves, the problem it removes,
  and why this stack wins at it. Anchor on a real user job, not a feature list.
- Frame a 12 to 18 month vision as an outcome, not an output. "Any team can monitor their
  Kubernetes fleet in under ten minutes without writing PromQL" beats "ship 14 features".
- Separate the mature surface (defend retention and satisfaction) from net-new bets (open a
  new use case). Name the few bets that matter and say what you are deliberately not doing.
- Build the roadmap as Now / Next / Later, each item tied to an outcome and a rough
  confidence level. Resist date-stuffing the Later column.
- Show the integration story explicitly. Infra observability does not live alone; it connects
  to application observability, the knowledge graph, and any natural-language assistant layer.
  Call out where this domain plugs into the wider suite.

Stack notes:
- Grafana: example outcome "any team can monitor their Kubernetes fleet in under ten minutes
  without writing PromQL". Watch the tension between OSS projects and Grafana Cloud.
- Elastic: the domain is one of three solutions (Search, Observability, Security) on a shared
  platform, so frame platform asks so the other solutions also benefit. Plan for both
  deployment worlds (Serverless or Hosted, and self-managed or air-gapped) and for the
  installed base migrating from Beats and Logstash to Elastic Agent or EDOT. Example outcome:
  "a team that already ships logs to Elastic gets host and Kubernetes metrics, with correlated
  logs, in under ten minutes".

Pitfalls: a vision that is really a backlog; a roadmap that ignores the cost model; treating
self-serve and enterprise as one audience when their roadmaps diverge.

See the Vision brief and Roadmap templates in `references/templates.md`.

## 2. Customer discovery

Get to the actual problem behind a request. Observability users are precise and busy, so
discovery has to be sharp and respect their time.

Method:
- Define the segment before you talk to anyone: self-serve individual, growth-stage platform
  team, or large enterprise SRE org. Their pain, budget, and buying motion differ.
- Interview for the job-to-be-done, not the feature. Ask what they were trying to do, what it
  cost them when it broke, and what they tried instead. Watch for the workaround; the
  workaround is the unmet need.
- Probe the three recurring infra-o11y pains: cost surprises, time-to-value on setup, and
  the gap between "I have a dashboard" and "I know what is wrong". Most opportunities sit in
  one of these. On Grafana, cost surprises usually mean metric cardinality. On Elastic, they
  usually mean ingest and indexing volume, mapping explosions, shard sprawl, retention sizing,
  or the operational cost of running the cluster.
- On Elastic, also find out which deployment the customer runs (Serverless, Hosted,
  self-managed) and who else shares the cluster (often the security team). Both change the
  pain and the buyer.
- Distinguish what users say they want from the outcome they need. They will ask for a
  setting; the job is the outcome that setting was a guess at.
- Capture evidence verbatim and tag it by segment and pain. You will need it later to defend
  priorities to engineering.

Output: a short findings note, not a transcript dump. Lead with the pattern, support it with
two or three quotes, and name the segments it applies to.

See the Discovery guide in `references/templates.md`.

## 3. Synthesis and prioritisation

Turn raw feedback into a ranked, evidence-backed view that engineering and design can act on,
and that you can defend when a senior stakeholder pushes a pet feature.

Method:
- Cluster feedback into opportunities (problems), not solutions. Many requests collapse into
  one underlying problem once you remove the proposed fix.
- Size each opportunity on reach, impact, and strategic fit, and be honest about confidence.
  A loud enterprise account is not the same as a broad pattern.
- For the top opportunities, write the tradeoff plainly: what we gain, what we defer, who is
  unhappy, and what evidence supports the call. Engineering trusts a PM who shows the working.
- When you hand work to engineering, give them the problem, the evidence, the success measure,
  and the constraints, then leave the solution space open. A PRD is a brief, not a spec
  dictated at them.
- Re-represent the customer faithfully in every prioritisation discussion. You are the
  advocate; if you do not carry the user's voice into the room, no one will.

Output: a prioritised opportunity list plus a PRD for whatever is going into build.

See the Feedback synthesis and PRD templates in `references/templates.md`.

## 4. Competitive and market analysis

Build a clear-eyed view of where this domain wins and loses, so the roadmap and the GTM story
stand up to scrutiny. Read `references/gtm-reference.md` for the landscape detail.

Method:
- Map competitors by archetype, not just logo. The set depends on the stack:
  - Grafana: all-in-one commercial suites (DataDog, New Relic, Dynatrace), cost-and-scale
    challengers (Chronosphere, and Grafana itself on the Prometheus side), cloud-native
    built-ins (CloudWatch, Google Cloud Operations, Azure Monitor), log players (Elastic,
    Splunk), and the open-source baseline customers can assemble themselves.
  - Elastic: all-in-one suites (DataDog, Dynatrace, New Relic), the log and SIEM incumbent
    (Splunk), composable open-source stacks (Grafana LGTM and Prometheus), the fork
    (OpenSearch and Amazon OpenSearch Service), and cloud-native built-ins.
- For each, capture where they genuinely beat this stack, where they are weak, and how they
  price. Do not strawman them. A battlecard that oversells is worse than none.
- Anchor differentiation on the chosen stack's durable angles. Grafana: open and composable
  rather than locked in, OpenTelemetry-native, control over cost and cardinality, and a single
  experience across metrics, logs, and traces. Elastic: search-grade analytics over all your
  data, one platform for observability and security, OpenTelemetry-native, deploy anywhere
  including self-managed and air-gapped, and ML and AI on your own data. These are positions,
  not slogans, so back each with proof, and state the chosen stack's honest weak points.
- Watch the substitution risk that matters most in infra o11y: the customer deciding the
  cloud provider's built-in monitoring is "good enough". Know exactly where it stops being
  good enough and make that the wedge.
- Translate findings into roadmap and GTM input, not a static deck. The point of competitive
  work is a decision: build this, message that, price the other way.

See the Competitive matrix template in `references/templates.md`.

## 5. GTM and packaging

Decide how the product is sold and packaged, and equip marketing and sales to sell it. This is
where infra-o11y PM gets commercial.

Method:
- Pricing and packaging start from the value metric: what does the customer pay along? In this
  space it is usually data volume, and that choice shapes behaviour, so model it. A metric that
  punishes good instrumentation will be hated.
  - Grafana: active series, ingested log and trace bytes, data points per minute.
  - Elastic: depends on deployment. Serverless prices along ingest and retention volume, Hosted
    along provisioned resources, self-managed by subscription tier. Packaging has two axes
    (subscription tier and deployment model), so say how a proposal works on each.
- Set tiers around jobs and willingness to pay, not feature counts. Be explicit about what
  pulls a self-serve user to a paid tier and what pulls a team to enterprise.
- Pressure-test packaging against the cost surprise problem. If a customer can accidentally
  run up a large bill, that is a packaging and product failure, not just a billing event. The
  levers differ by stack: aggregation, adaptive telemetry, and drop rules on Grafana; ingest
  pipeline drops, ILM and data tiers, searchable snapshots, LogsDB storage efficiency, and APM
  sampling on Elastic.
- For sales enablement, give the field a battlecard (per competitor) and a one-page narrative
  they can actually use: the problem, the proof, the three reasons we win, and the honest
  answer to the top objection.
- For PMM collateral (datasheets, web copy, whitepapers), write to the technical buyer.
  Specific beats grand. Show the workflow and the numbers, not adjectives.

Pitfalls: pricing that ignores how the data model drives cost; packaging that hides the path
to value; collateral written for a CIO when the evaluator is an SRE.

See the Pricing brief, Battlecard, and Datasheet templates in `references/templates.md`.

---

## Working principles

- Speak the domain credibly. If you cannot explain why a query is expensive or what an OTel
  collector does, you will lose technical buyers. Use the chosen stack's primer, and keep
  stack terms consistent within an artifact.
- Lead with the user problem and the evidence. Every artifact this skill produces should make
  the underlying user job obvious.
- Respect the cost model in every decision. In observability, the data model is the business
  model. Roadmap, pricing, and packaging all bend around it.
- Write for the technical reader. Plain, exact language. No filler, no clichés, no
  manufactured excitement.

## Reference files

Shared:
- `references/templates.md`: ready-to-fill templates for every workflow above, for either
  stack.

Option A, Grafana stack:
- `references/domain-primer.md`: the Grafana stack, infra observability concepts, personas,
  buying motion, and competitive landscape, all in PM terms.
- `references/gtm-reference.md`: competitor detail, pricing and packaging patterns, and
  positioning angles for the go-to-market work.

Option B, Elastic Stack:
- `references/elastic-domain-primer.md`: the Elastic Stack, its cost model by deployment,
  personas, buying motion and licensing history, and competitive landscape, in PM terms.
- `references/elastic-gtm-reference.md`: competitor detail from Elastic's side, pricing and
  packaging patterns, positioning angles, and honest weak points.
