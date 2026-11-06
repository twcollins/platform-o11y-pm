# Templates

Fill-in templates for each workflow. Keep them lean. Delete prompts you do not need rather
than padding. Every template should make the user problem and the evidence obvious.

The templates work for both stack options. Where a prompt differs by stack, it shows both as
"Grafana: ... / Elastic: ...". Keep only the line for the stack you chose, and take the
specifics from that stack's primer and GTM reference.

---

## Vision brief

```
# [Domain] product vision

## Who this is for
[Primary segment and persona. One sentence.]

## The problem we remove
[The core job-to-be-done and what it costs the user today. Two or three sentences,
grounded in a real pain: cost surprise, slow setup, or signal-to-insight gap.]

## Why this stack wins here
[The durable advantage. Name the one or two that actually apply. Avoid listing them all
by reflex.
Grafana: open and composable, OTel-native, cost and cardinality control, unified signals.
Elastic: search-grade analytics, one platform for o11y and security, OTel-native, deploy
anywhere, ML and AI on your own data.]

## The vision (12 to 18 months)
[An outcome, not an output. "Any team can do X in under Y without Z."]

## What we are betting on
- [Net-new bet 1, and the use case it opens]
- [Net-new bet 2]

## What we are deliberately not doing
- [Scope we are saying no to, and why]

## How this connects to the wider suite
[Integration points: application observability, knowledge graph, assistant or
natural-language layer (and, on Elastic, the Security solution sharing the same data).
Where does a user cross from this domain into the next?]

## How we will know it worked
[2 to 4 outcome measures: adoption, time-to-value, retention, cost predictability.]
```

## Roadmap (Now / Next / Later)

```
# [Domain] roadmap, [quarter/year]

## Now (in build)
- [Item], outcome: [user outcome], confidence: high, evidence: [link/quote]

## Next (committed direction)
- [Item], outcome: [user outcome], confidence: medium, evidence: [...]

## Later (exploring)
- [Theme], problem we are still validating: [...]

## Not now
- [Tempting item we are explicitly deferring, and the reason]
```
Rule: every Now/Next item ties to an outcome and a piece of evidence. Later holds themes, not
dated features.

---

## Discovery guide

```
# Discovery: [topic] ([segment])

## Goal
[The one decision this discovery should inform.]

## Segment
[Self-serve individual / growth platform team / enterprise SRE org. Be specific.
Elastic: also note deployment (Serverless / Hosted / self-managed or air-gapped) and who
else shares the cluster.]

## Interview questions (job-focused, not feature-focused)
1. Walk me through the last time you [tried to do the job]. What happened?
2. What did it cost you when it broke or took too long?
3. What did you try instead? What workaround are you living with now?
4. If this worked perfectly, what would change for you?
5. (Only at the end) Is there anything you wish the tool did?

## What to listen for
- The workaround (it is the unmet need)
- Cost surprises (Grafana: metric cardinality / Elastic: ingest volume, mapping explosion,
  shard sprawl, retention sizing, cluster operating effort)
- Time-to-value friction in setup
- The gap between "I have a dashboard" and "I know what is wrong"

## Findings note (output)
- Pattern: [the recurring theme in one line]
- Evidence: [2 to 3 verbatim quotes, tagged by segment]
- Segments it applies to: [...]
- Implication: [what this means for the roadmap]
```

---

## Feedback synthesis

```
# Feedback synthesis, [period]

## Opportunities (problems, not solutions)
| Opportunity (problem) | Reach | Impact | Strategic fit | Confidence | Evidence |
|---|---|---|---|---|---|
| [Underlying problem] | [how many / which segment] | [high/med/low] | [high/med/low] | [high/med/low] | [quotes/data] |

## Top calls and tradeoffs
For each prioritised opportunity:
- We will: [what we are doing]
- We gain: [outcome]
- We defer: [what slips]
- Who is unhappy: [stakeholder/segment], and why that is acceptable
- Evidence behind the call: [...]
```

## PRD (brief, not a spec)

```
# PRD: [feature/initiative]

## Problem
[The user problem, with evidence. If you cannot point to evidence, stop and go back to
discovery.]

## Who has it
[Segment and persona, and how widespread.]

## Success measure
[The single number or outcome that tells us this worked.]

## Constraints
[Cost-model impact, scale, security, what must not break.]

## Out of scope
[What this is not solving.]

## Open questions for engineering and design
[Leave the solution space open. State the problem and the guardrails, not the implementation.]
```

---

## Competitive matrix

```
# Competitive view: [domain]

| Competitor | Archetype | Where they beat us | Where they are weak | How they price |
|---|---|---|---|---|
| DataDog | All-in-one suite | [honest strength] | [honest weakness] | [model] |
| [Cloud built-in] | Cloud-native default | [...] | [...] | [...] |
| Grafana: Chronosphere / Elastic: Splunk | Grafana: cost/scale challenger / Elastic: log and SIEM incumbent | [...] | [...] | [...] |
| Grafana: Elastic or Splunk / Elastic: Grafana LGTM | Grafana: log players / Elastic: composable OSS stack | [...] | [...] | [...] |
| Elastic only: OpenSearch | The fork / AWS default | [...] | [...] | [...] |

## The substitution risk to watch
[Usually "the cloud built-in is good enough". State exactly where it stops being good
enough. That boundary is the wedge.]

## So what (decisions)
- Build: [...]
- Message: [...]
- Price/package: [...]
```

---

## Pricing and packaging brief

```
# Pricing and packaging: [domain/product]

## Value metric
[What the customer pays along. State why this metric matches value and how it affects
behaviour.
Grafana: active series, ingested bytes, DPM.
Elastic: ingest and retained GB (Serverless), provisioned resources (Hosted), subscription
tier (self-managed). Say which deployments this proposal covers.]

## Tiers
| Tier | Job it serves | What unlocks the upgrade | Rough willingness to pay |
|---|---|---|---|
| Free/self-serve | [...] | [the trigger to upgrade] | [...] |
| Pro/team | [...] | [...] | [...] |
| Enterprise | governance, scale, support, security | [...] | [...] |

## Cost-surprise guardrails
[How a customer avoids accidental bill shock. If there is no answer here, the packaging
is not done.
Grafana: aggregation, adaptive sampling, drop rules, tiered retention, alerts on spend.
Elastic: ingest pipeline drops, ILM and data tiers, searchable snapshots and frozen tier,
LogsDB storage efficiency, APM sampling, spend alerts on Serverless.]

## Risks
[Where this pricing punishes good behaviour or invites gaming, and the mitigation.]
```

## Battlecard (per competitor)

```
# Battlecard: us vs [competitor]

## Their pitch (steelmanned)
[How they would describe themselves honestly.]

## Three reasons we win
1. [Reason + proof]
2. [...]
3. [...]

## Where they genuinely win
[Be honest. The field needs to know this to stay credible.]

## Top objection and the real answer
[The hardest pushback and a straight response, not spin.]

## Landmines to avoid
[Claims not to make, comparisons that backfire.]
```

## Datasheet / web copy outline

```
# [Product] datasheet

## One-line value
[The outcome, for the technical buyer. No adjectives doing the work.]

## The problem
[Specific, recognisable to an SRE or platform engineer.]

## How it works
[The actual workflow, with the signals and steps. Show, do not assert.]

## Proof
[Numbers, a reference workflow, a before/after. Specifics build trust with this audience.]

## What you need to get started
[Honest setup story and time-to-value.]
```
