# The AI Team workspace contract (v1)

Every AI Team agent is a separate Claude Code plugin, but they all work in **one private folder
inside your project**: `ai-team/`. This file is the contract they share. An agent may only read
and write the paths described here, so agents built separately still fit together.

> **The plugins are public. The workspace is private.** Nothing in `ai-team/` is ever part of a
> plugin, and agents never copy workspace content into a plugin, a web search or an issue.

## Layout

```
<your project>/
└── ai-team/                      ← private; created by the first agent's `setup` skill
    ├── README.md                 what this folder is + privacy warning
    ├── team.md                   installed members, jurisdiction, versions, settings
    ├── company/                  SHARED by all agents — the single source of company facts
    │   ├── profile.md            legal entity, people, tax & accounting, banking, risk settings
    │   ├── documents.md          register of the company's own corporate documents
    │   └── products/
    │       └── <product-slug>.md one file per product (what it does, data, money, suppliers)
    ├── lawyer/                   owned by ai-lawyer
    ├── accountant/               owned by ai-accountant
    └── <member>/                 every future member gets its own folder
```

Each member folder always contains:

| Path | Purpose |
|---|---|
| `inbox/` | drop zone for documents meant for this member |
| `history/journal.md` | append-only log of every job (`J-0001`…) |
| `history/decisions.md` | decisions taken in this member's area (`D-0001`…) |
| `history/obligations.md` | deadlines and actions (`O-0001`…), fixed table format read by the hooks |
| `research/` | memos, one per question |
| `digests/` | outputs of scheduled runs |
| `learnings.md` | what the member learned on the job (`L-0001`…) — drives self-improvement |

Members add their own folders (e.g. `lawyer/laws/`, `accountant/registers/`).

## Ownership rules

1. **Write only to your own folder** and to `company/`. Read anything in `ai-team/`.
2. **`company/` is shared.** Change a fact there only when the founder states it or a document
   proves it, and record the change in your own journal. If another member's files contradict
   `company/`, flag it to the founder — don't silently fix the other member's files.
3. **IDs are per member.** Refer to another member's record with its prefix:
   `lawyer:C-0003`, `accountant:INV-O-2027-0001`.
4. **History is append-only** everywhere: correct by adding an entry that references the old one.

## Hand-offs between members

`team.md` lists who is installed. When a question belongs to another member's area, say so,
write the exact question to ask, and (if the founder agrees) the main session delegates it to
that member's agent (e.g. `ai-lawyer:lawyer`). If the member isn't installed, say which plugin
would cover it.

## Privacy

- The founder decides how `ai-team/` is stored. Recommended: add `ai-team/` to the project's
  `.gitignore`, and run `git init` **inside** `ai-team/` for a private, tamper-evident history.
  Never push it to a public repository.
- Agents search the web for the *rule*, never for the company's facts.
- `learnings.md` may contain company details. Only generalised, anonymised lessons may leave the
  workspace, through a member's `improve` skill, after the founder approves the exact text.

## Versioning

This contract is versioned. `team.md` records `workspace_contract: 1`. A member that needs a
breaking change bumps the contract and ships a migration step in its `setup` skill.
