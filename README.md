# AI Team

Claude Code agents that work together for a founder's company. Each team member is its own
plugin in its own repository; they share **one private workspace** in your project, so the lawyer
knows what the accountant registered and the other way round.

| Member | Plugin | What it does | Version |
|---|---|---|---|
| 🧭 Lead | [ai-lead](https://github.com/HPAIF/ai-lead) | **single entry point**: routes requests to members, combines answers, one board, one agenda, one inbox | 0.1 |
| ⚖️ Lawyer | [ai-lawyer](https://github.com/HPAIF/ai-lawyer) | legal research with verified official sources, contract review, legal history, legal watch | 0.2 |
| 🧮 Accountant | [ai-accountant](https://github.com/HPAIF/ai-accountant) | tax & accounting research, document registers, month-end close, tax watch | 0.2 |
| 🏗️ CTO | [ai-cto](https://github.com/HPAIF/ai-cto) | ADRs, architecture and code reviews, tech debt, tech roadmap, build-vs-buy | 0.1 basic |
| ⚙️ DevOps | [ai-devops](https://github.com/HPAIF/ai-devops) | infrastructure inventory, CI/CD and releases, monitoring, incidents, backups, cloud cost | 0.1 basic |
| 🛡️ Security | [ai-security](https://github.com/HPAIF/ai-security) | threat models, security reviews, dependency and secret scans, policies, incident response | 0.1 basic |
| 🧩 Product | [ai-product](https://github.com/HPAIF/ai-product) | discovery, problem framing, specs, roadmap, product metrics, feedback log | 0.1 basic |
| 📣 Marketing | [ai-marketing](https://github.com/HPAIF/ai-marketing) | ICP and personas, positioning, content plans and drafts, launches, channel reviews | 0.1 basic |
| 🤝 Sales | [ai-sales](https://github.com/HPAIF/ai-sales) | target accounts, prospect research, outreach drafts, discovery calls, proposals, pipeline | 0.1 basic |

Also in HPAIF: [ai-member-template](https://github.com/HPAIF/ai-member-template) (start a new member) ·
[messenger](https://github.com/HPAIF/messenger) (macOS app to chat with the team, MVP).

> These agents research, prepare, track and warn. They are **not** licensed professionals, and they
> never sign, file, pay, send, publish, deploy or contact anyone for you.

## Install

In Claude Code, add this catalog once, then install the members you want:

```
/plugin marketplace add HPAIF/ai-team
/plugin install ai-lead@ai-team
/plugin install ai-lawyer@ai-team
/plugin install ai-accountant@ai-team
```

Then, in your company folder: **"set up the AI lead"**, **"set up the AI lawyer"** … Each `setup`
creates or joins the private `ai-team/` workspace. Work through the lead as your entry point:
`claude --agent ai-lead:lead`. Any member also installs on its own, straight from its repository
(`/plugin marketplace add HPAIF/ai-lawyer`, then `/plugin install ai-lawyer@ai-lawyer`).

Share the team with everyone working on a project by committing `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "ai-team": { "source": { "source": "github", "repo": "HPAIF/ai-team" } }
  },
  "enabledPlugins": {
    "ai-lead@ai-team": true,
    "ai-lawyer@ai-team": true,
    "ai-accountant@ai-team": true
  }
}
```

## How the team works

```
GitHub (public: rules, skills, templates)          Your project (private: your data)
───────────────────────────────────────           ─────────────────────────────────────
ai-team        ← this catalog                      ai-team/
ai-lead        ← entry point ── installs into ──►   ├── team.md        who is installed
ai-lawyer, ai-accountant, ai-cto, ai-devops,        ├── company/       shared facts, one file per product
ai-security, ai-product, ai-marketing, ai-sales     ├── lead/          board, company inbox
                                                    └── <member>/      each member's history, research, learnings
```

- **[Workspace contract](docs/workspace.md)** — the folder layout and rules every member follows.
- **[Design decisions](docs/design-decisions.md)** — why it is built this way.
- **[Adding a member](docs/adding-a-member.md)** — how to build the next agent (start from [ai-member-template](https://github.com/HPAIF/ai-member-template)).

## Upgrades and self-improvement
- Members are versioned plugins. `/plugin marketplace update ai-team` then
  `/plugin update <member>@ai-team` gets new rules and skills; your workspace is never overwritten.
- Each member records lessons in its private `learnings.md`. General lessons become anonymised
  pull requests to the member's repository through its `improve` skill — always with your
  approval of the exact text.

## License
MIT — see each repository.
