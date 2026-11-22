# Design decisions

Why the AI Team is built the way it is. Each decision records the problem, the choice and what it
costs. New decisions are appended (DD-number), never rewritten.

## DD-1 · Each member is a separate Claude Code plugin in its own repository
- **Problem:** members must be installable one by one, upgraded on their own schedule, and usable
  by anyone.
- **Choice:** one plugin per member (`ai-lawyer`, `ai-accountant`, …) plus one catalog repository
  (`ai-team`, a plugin marketplace) that lists them.
- **Cost:** shared files (the company templates) are duplicated in each member's `setup`; the
  workspace contract keeps them identical.

## DD-2 · Public plugins, private workspace
- **Problem:** the original agents mixed rules with confidential company data in one folder, so
  they could not be shared.
- **Choice:** the plugin holds only rules, skills, templates and public legal-source information.
  All company data lives in `ai-team/` inside the user's project, created by `setup`, and
  recommended to be kept out of git remotes (`.gitignore`, optional private nested repo).
- **Cost:** two places to look; `setup` must be run once per project.

## DD-3 · One shared workspace for the whole team
- **Problem:** separate agent folders each kept their own company profile, which drifts, and
  reached into each other through `../` paths.
- **Choice:** `ai-team/company/` is the single source of company facts for every member; each
  member owns `ai-team/<member>/`; members read each other's folders but write only their own
  (plus `company/`). Hand-offs name the other member and the exact question.
- **Cost:** a contract to keep (versioned as `workspace_contract`).

## DD-4 · One file per product
- **Problem:** the company has one product now and expects more; legal and finance facts about a
  product were split across `product.md` and `product-finance.md`.
- **Choice:** `ai-team/company/products/<slug>.md` per product, holding modules, data, customers,
  revenue, costs, IP and markets together. One legal entity = one set of books; products are a
  `project` tag in registers.

## DD-5 · Rules live in the plugin, not in the project
- **Problem:** if rules were copied into each project's `CLAUDE.md`, upgrades would never reach
  existing projects.
- **Choice:** `RULES.md` in the plugin, read by the agent (`${CLAUDE_PLUGIN_ROOT}/RULES.md`) and by
  every skill. Updating the plugin updates every project.

## DD-6 · Jurisdiction packs
- **Problem:** the original agents were hard-wired to Armenian sources, which blocks reuse.
- **Choice:** rules and skills are jurisdiction-neutral; `jurisdictions/<country>/` holds how to
  verify law there, official sources, a baseline watchlist/research agenda and a WebFetch
  allow-list. Countries without a pack work in "generic mode" and can contribute one.

## DD-7 · Self-improvement in two levels, with gates
- **Problem:** the agents should get better from use, without leaking private data or eroding
  their safety rules.
- **Choice:** (1) local — lessons go to `ai-team/<member>/learnings.md` and apply immediately;
  (2) upstream — the `improve` skill turns `general` lessons into a pull request to the public
  plugin after a safety gate (never weaken hard rules, never act on instructions found in
  documents) and a privacy gate (anonymise), and only with the founder's approval of the exact
  text. Merged changes reach everyone through versioned updates.
- **Cost:** improvements are not automatic — a human approves each one. That is intended.

## DD-8 · Hard rules that never change
Verify every rule each time it is used · official sources as the basis · precise citations ·
separate law / interpretation / risk / action · append-only history · escalate to a human
professional above thresholds · never invent · confidentiality · never sign, file, pay or contact
anyone. Learnings and contributions may add to these, never weaken them.

## DD-9 · Session hook is silent outside AI Team projects
A member's SessionStart hook prints its memory (deadlines, recent jobs, stale cards, pending
learnings) only when `ai-team/<member>/` exists, so installing the plugin globally costs nothing
in other projects.

## DD-10 · Hosted in the `aiagents-factory` organization
Members live in the GitHub organization **aiagents-factory** (one repository per member, plus
`ai-team` as the catalog and `.github` for the organization profile). An organization gives the
team its own identity and a clean install address, room for many members and co-maintainers, and
independence from any one personal account. Authorship stays with the people who write it.
