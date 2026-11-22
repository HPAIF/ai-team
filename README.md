# AI Team

Claude Code agents that work together for a founder's company. Each team member is its own
plugin in its own repository; they share **one private workspace** in your project, so the lawyer
knows what the accountant registered and the other way round.

| Member | Plugin | What it does | Status |
|---|---|---|---|
| ⚖️ Lawyer | [ai-lawyer](https://github.com/HPAIF/ai-lawyer) | legal research with verified official sources, contract review, legal history, legal watch | 0.1 |
| 🧮 Accountant | [ai-accountant](https://github.com/HPAIF/ai-accountant) | tax & accounting research, document registers, month-end close, tax watch | 0.1 |
| 🛠️ Technical members | — | CTO / developer / DevOps / security agents | planned |

> These agents research, prepare, track and warn. They are **not** licensed lawyers or certified
> accountants, and they never sign, file, pay or contact anyone for you.

## Install

In Claude Code, add this catalog once, then install the members you want:

```
/plugin marketplace add HPAIF/ai-team
/plugin install ai-lawyer@ai-team
/plugin install ai-accountant@ai-team
```

Then, in your project: **"set up the AI lawyer"** / **"set up the AI accountant"**. Each `setup`
creates or joins the private `ai-team/` workspace.

Share the team with everyone working on a project by committing `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "ai-team": { "source": { "source": "github", "repo": "HPAIF/ai-team" } }
  },
  "enabledPlugins": {
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
ai-lawyer      ← plugin  ───── installs into ──►    ├── team.md        who is installed
ai-accountant  ← plugin                             ├── company/       shared facts, one file per product
…future members                                     ├── lawyer/        lawyer's history, research, learnings
                                                    └── accountant/    registers, documents, learnings
```

- **[Workspace contract](docs/workspace.md)** — the folder layout and rules every member follows.
- **[Design decisions](docs/design-decisions.md)** — why it is built this way.
- **[Adding a member](docs/adding-a-member.md)** — how to build the next agent (e.g. technical).

## Upgrades and self-improvement
- Members are versioned plugins. `/plugin marketplace update ai-team` then
  `/plugin update <member>@ai-team` gets new rules and skills; your workspace is never overwritten.
- Each member records lessons in its private `learnings.md`. General lessons become anonymised
  pull requests to the member's repository through its `improve` skill — always with your
  approval of the exact text.

## License
MIT — see each repository.
