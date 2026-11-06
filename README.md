# platform-o11y-pm

A reusable [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that
turns Claude into a competent product manager for an infrastructure observability domain. It
supports two stack options:

- **Grafana stack**: Grafana, OpenTelemetry, Prometheus, Mimir, Loki, Tempo, Alloy, plus
  Kubernetes Monitoring, Cloud Provider Observability, and Host Monitoring.
- **Elastic Stack**: Elasticsearch, Kibana, Elastic Agent and Fleet, EDOT (Elastic
  Distributions of OpenTelemetry), ES|QL, and the Elastic Observability solution (infrastructure,
  Kubernetes, cloud, logs, APM, profiling).

The workflows and templates are shared; each stack has its own domain primer and GTM reference.
Claude picks the stack from context, or asks if the request does not make it clear.

It packages the day-to-day PM work of the role (vision, discovery, prioritisation, competitive
analysis, and go-to-market) into a single skill, each task grounded in the reality of
observability products where the buyer is technical and the data has a cost model.

## What is in here

```
platform-o11y-pm/
├── SKILL.md                      # the skill: workflow router and method for each PM task
├── references/
│   ├── templates.md              # fill-in templates for every workflow (both stacks)
│   ├── domain-primer.md          # Grafana: the stack and infra o11y in PM terms
│   ├── gtm-reference.md          # Grafana: competitor detail, pricing patterns, positioning
│   ├── elastic-domain-primer.md  # Elastic: the stack and infra o11y in PM terms
│   └── elastic-gtm-reference.md  # Elastic: competitor detail, pricing patterns, positioning
├── README.md
└── LICENSE
```

`SKILL.md` is the machine-facing entry point that Claude loads and follows. The README is for
humans browsing the repo.

## The five workflows

The skill is organised around the core PM tasks for the domain. Each has a method in `SKILL.md`
and a matching template in `references/templates.md`.

| Workflow | What it covers |
|---|---|
| Vision and roadmap | Turn ideas and feedback into an outcome-led vision and a Now/Next/Later roadmap, separating mature products from net-new bets. |
| Customer discovery | Job-to-be-done interviewing, the recurring infra-o11y pains (cost surprises, time-to-value, the dashboard-to-insight gap), and tight findings notes. |
| Synthesis and prioritisation | Cluster feedback into problems, score on reach, impact, fit, and confidence, and write defensible tradeoffs and PRDs for engineering. |
| Competitive and market analysis | Map competitors by archetype, find the real wedge against cloud-native built-ins, and turn findings into decisions. |
| GTM and packaging | Value-metric pricing, tiers by job rather than feature count, bill-shock guardrails, battlecards, and datasheets for the technical buyer. |

## How to use it

### With Claude (as a skill)

Install the packaged `.skill` file in a Claude product that supports Agent Skills, or point
Claude at this folder. Once installed, it triggers automatically when you do observability
product work. You can also invoke it directly, for example:

- "Draft a product vision for Host Monitoring on the Grafana stack."
- "Build a battlecard for us versus CloudWatch."
- "Synthesise this feedback into a prioritised opportunity list for engineering."
- "Propose a pricing and packaging model for Kubernetes Monitoring."
- "We build on Elastic. Write a battlecard for us versus Datadog for infra and logs."
- "Draft a roadmap for host monitoring on Elastic Observability, covering Serverless and
  self-managed customers."

### As a reference for a person

Even without Claude, the reference files work as a standalone PM playbook for either stack:
`templates.md` plus the Grafana pair (`domain-primer.md`, `gtm-reference.md`) or the Elastic
pair (`elastic-domain-primer.md`, `elastic-gtm-reference.md`). The templates are copy-and-fill.

## Building the .skill package

To produce an installable `.skill` file from this repo, use the skill-creator packaging script
(part of Anthropic's skill tooling):

```bash
python -m scripts.package_skill path/to/platform-o11y-pm ./dist
```

This zips the skill (excluding build cruft) into `platform-o11y-pm.skill`.

## Scope and a caveat

This is a working first draft, not an evaluated and benchmarked skill. The competitive and
pricing detail in `gtm-reference.md` and `elastic-gtm-reference.md` reflects a point-in-time view of the market. Vendors change
pricing and features often, so verify any specific claim before putting it in customer-facing
collateral.

## License

MIT. See [LICENSE](LICENSE). Swap or remove this if you would rather keep the repo private or
license it differently.
