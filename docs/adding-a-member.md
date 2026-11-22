# Adding a team member

## Local development layout

Clone the catalog repository and put every member repository **inside** it; the catalog's
`.gitignore` ignores `/ai-*/` and `/.github/`, so each folder still commits and pushes to its own
repository:

```
HPAIF/                 ← HPAIF/ai-team (catalog, docs)
├── ai-lawyer/         ← HPAIF/ai-lawyer
├── ai-accountant/     ← HPAIF/ai-accountant
└── .github/           ← HPAIF/.github (organization profile)
```

```bash
git clone git@github.com:HPAIF/ai-team.git HPAIF && cd HPAIF
git clone git@github.com:HPAIF/ai-lawyer.git
git clone git@github.com:HPAIF/ai-accountant.git
git clone git@github.com:HPAIF/.github.git
```

## A new member

Use `ai-lawyer` as the reference implementation. A new member (e.g. `ai-cto`, `ai-devops`,
`ai-security`) is a new repository with this shape:

```
ai-<member>/
├── .claude-plugin/plugin.json     name, version 0.1.0, description, repository, license, keywords incl. "ai-team"
├── RULES.md                       role, hard rules, team work, learn on the job, answer format, skills, workspace map
├── agents/<member>.md             reads ${CLAUDE_PLUGIN_ROOT}/RULES.md, checks ai-team/<member>/, loads memory
├── skills/
│   ├── setup/                     creates shared files if missing + ai-team/<member>/, registers in team.md
│   │   └── workspace/             copy of the shared templates + the member's own folder template
│   ├── log-job/                   journal, decisions, obligations, company facts, learnings
│   ├── improve/                   learnings → anonymised, approved PR to this repo
│   └── <member skills>/
├── hooks/hooks.json + session-context.sh   silent unless ai-team/<member>/ exists
├── templates/
├── README.md · CHANGELOG.md · CONTRIBUTING.md · LICENSE · .gitignore (includes ai-team/)
```

Checklist:
1. Follow the [workspace contract](workspace.md): own folder, shared `company/`, append-only
   history, `learnings.md`, obligations table in the standard format.
2. Copy the shared templates (`README.md`, `team.md`, `company/`) **verbatim** from an existing
   member's `skills/setup/workspace/`.
3. Add hand-off lines to `RULES.md` ("questions about X belong to member Y").
4. `claude plugin validate .` passes; grep the repo for company data before the first push.
5. Add the member to `.claude-plugin/marketplace.json` and the table in this repo's README;
   add the `ai-team` topic and put it in the "AI Team" star list.

Technical members will mostly work on the project's code rather than on documents; their
workspace folder holds decisions (ADRs), learnings and obligations, while the code itself stays
where it is.
