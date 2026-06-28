# grafana-infra-o11y-pm

A reusable [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that
turns Claude into a competent product manager for an infrastructure observability domain built
on the Grafana stack (Grafana, OpenTelemetry, Prometheus, Mimir, Loki, Tempo, Alloy, plus
Kubernetes Monitoring, Cloud Provider Observability, and Host Monitoring).

It packages the day-to-day PM work of the role (vision, discovery, prioritisation, competitive
analysis, and go-to-market) into a single skill, each task grounded in the reality of
observability products where the buyer is technical and the data has a cost model.

## What is in here

```
grafana-infra-o11y-pm/
├── SKILL.md                      # the skill: workflow router and method for each PM task
├── references/
│   ├── domain-primer.md          # the Grafana stack and infra o11y in PM terms
│   ├── templates.md              # fill-in templates for every workflow
│   └── gtm-reference.md          # competitor detail, pricing patterns, positioning
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

### As a reference for a person

Even without Claude, `references/domain-primer.md`, `templates.md`, and `gtm-reference.md` work
as a standalone PM playbook for the domain. The templates are copy-and-fill.

## Building the .skill package

To produce an installable `.skill` file from this repo, use the skill-creator packaging script
(part of Anthropic's skill tooling):

```bash
python -m scripts.package_skill path/to/grafana-infra-o11y-pm ./dist
```

This zips the skill (excluding build cruft) into `grafana-infra-o11y-pm.skill`.

## Scope and a caveat

This is a working first draft, not an evaluated and benchmarked skill. The competitive and
pricing detail in `gtm-reference.md` reflects a point-in-time view of the market. Vendors change
pricing and features often, so verify any specific claim before putting it in customer-facing
collateral.

## License

MIT. See [LICENSE](LICENSE). Swap or remove this if you would rather keep the repo private or
license it differently.
