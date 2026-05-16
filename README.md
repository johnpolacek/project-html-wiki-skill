# Project HTML Wiki Skill

A skill for AI coding agents creating and maintaining an HTML-first project memory layer for agentic development.

Zero-context intake and bootstrapping are modes. The skill also supports feature planning, plan updates, codebase-to-wiki sync, decision capture, project-context logging, roadmap maintenance, and additive wiki audits/upgrades.

Inspired by [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), [Thariq’s The Unreasonable Effectiveness of HTML](https://x.com/trq212/status/2052809885763747935) and the growing practice of using standalone HTML artifacts for agent-generated specs, explainers, reports, and planning documents. This skill adapts persistent, LLM-maintained project memory into browser-readable HTML pages for source context, project plans, roadmap state, agent guidance, and codebase-to-wiki sync.

## Install

Install globally for your user account:

```bash
npx skills add https://github.com/johnpolacek/project-html-wiki-skill -g
```

Or install only in the current project:

```bash
npx skills add https://github.com/johnpolacek/project-html-wiki-skill
```

When run inside a repository, the Skills CLI installs project-locally by default. Use `-g` or `--global` when you want the skill available across projects.

This repository contains one root-level skill, so no `--skill` flag is needed. Repositories that publish multiple skills use `--skill <skill-name>` to select one skill from the collection, for example `npx skills add https://github.com/vercel-labs/skills --skill find-skills`.

## Requirements

- A software project with either an existing repository or enough source context to create project wiki files.
- Git is recommended. For brand-new projects, the skill initializes Git unless the user opts out.
- No generator or CLI runtime is required; this is an HTML artifact contract and template set for AI coding agents.
- The skill is agent-agnostic. It is intended to work in Codex, Claude Code, and other agents that support local skills.

## Workflow

```text
User Prompt / Repo Evidence
        |
        v
wiki/Sources.html
        |
        v
Source Briefs + Roadmap
        |
        v
wiki/plans/
        |
        v
Implementation
        |
        v
sync_changes -> wiki/log.html + updated plans/source docs
```

## What It Helps With

- Bootstraps a new project into a maintained `wiki/` structure.
- Interviews first when you want to start from no project idea, repo, PRD, or notes, then bootstraps only after enough direction is confirmed.
- Generates standalone, browser-readable HTML pages with embedded CSS and optional inline SVG or JavaScript for richer review.
- Applies frontend design guidance when generating HTML artifacts so wiki pages have deliberate typography, layout, hierarchy, color, and interaction choices instead of generic document styling.
- Selects artifact-specific HTML patterns for planning, code review, reports, design prototypes, and custom editors instead of using one generic page shape.
- Encourages reader goals, visual structures, copy/export controls, evidence confidence badges, annotated code snippets, browser preview, and visual QA.
- Imports an existing repository without overwriting user-authored files.
- Creates a required source index at `wiki/Sources.html`.
- Generates PRD, technical, marketing, or design briefs only when project evidence justifies them.
- Creates or updates feature and implementation plans under `wiki/plans/`.
- Syncs recent codebase changes back into the wiki when work happened before planning or made project knowledge stale.
- Records durable planning, validation, decision, and project-context history in `wiki/log.html` without duplicating Git history.
- Adds managed agent guidance with bounded update markers, following the repository's existing agent-file convention when present.
- Initializes Git for new projects unless the user opts out.
- Leaves starter app scaffolding outside the core skill; explicit scaffold requests should be handled as separate implementation work.

## When To Use

Use this skill when the user explicitly wants to create, import, maintain, audit, or sync an HTML-first project wiki for a software project. Also use it when the project already has this wiki structure and the user asks to update project memory, plans, source briefs, or durable agent context.

Do not use it for ordinary coding tasks unless the project already uses this wiki structure or the user asks to update project wiki context.

Example prompts:

```text
Use $project-html-wiki to bootstrap a wiki for this existing app.
Use $project-html-wiki to plan this feature before implementation.
Use $project-html-wiki to sync recent code changes back into the wiki.
Use $project-html-wiki to audit this project's wiki and agent guidance files.
Use $project-html-wiki to help me figure out a new project from scratch.
```

## What The Agent Does

The agent inspects the repo and source material first, identifies whether the target is the repo root or a specific package/app root, preserves user-authored docs, writes bounded managed sections where appropriate, and creates or updates the minimal wiki artifacts needed for the requested mode. It avoids placeholder leakage, skips unsupported source briefs, records only durable project context in `wiki/log.html`, and reports skipped or blocked work explicitly.

## Core Outputs

A completed HTML-first bootstrap creates or preserves:

- `AGENTS.md`
- `CLAUDE.md` when the repository already uses Claude Code guidance or the user explicitly requests it
- `wiki/AGENTS.md`
- `wiki/index.html`
- `wiki/log.html`
- `wiki/Sources.html`
- `wiki/plans/index.html`
- `wiki/roadmap.html`
- `.agents/skills/project-wiki-maintainer/SKILL.md` when repo-local skills are already in use or explicitly requested

Source briefs under `wiki/sources/` are conditional. The skill creates them only when the available source material supports a separate durable artifact.

## Use In A Project

Skill installation and project wiki initialization are separate steps. Installing this skill makes the workflow available to the agent; it does not automatically modify every repository.

### Initialize A Project

From a new or existing project, run:

```text
Use $project-html-wiki to initialize this project.
```

For an existing repo, you can be more explicit:

```text
Use $project-html-wiki to bootstrap this existing app into an HTML-first project wiki.
```

For an existing project that has source files but no HTML-first wiki yet, give the agent the import posture explicitly:

```text
Use $project-html-wiki to onboard this existing project into the HTML-first project wiki flow.

Please inspect the repo first, preserve existing files and Git history, identify whether this is a live product, internal tool, library, archive, or unknown lifecycle, then create only the safe baseline wiki and agent-guidance artifacts.

Important constraints:
- Do not rewrite existing docs or agent guidance outside managed blocks.
- Do not create `wiki/plans/mvp/` unless repo evidence clearly says this is greenfield or pre-launch MVP work.
- Do not install dependencies, generate scaffold code, or choose a new stack.
- Catalog repository evidence and unknowns in `wiki/Sources.html`.
- Report created, preserved, skipped, blocked, and `present_but_not_upgraded` artifacts.
```

The agent should inspect the project first, identify the target root, preserve existing files and Git history, then create or update the wiki baseline, source index, roadmap, planning area, and bounded agent guidance. It should create source briefs only when project evidence justifies them, skip repo-local skill creation unless `.agents/skills/` is already in use or explicitly requested, and report created, preserved, skipped, blocked, and `present_but_not_upgraded` artifacts.

During initialization, the agent should record a repo automation policy in `wiki/AGENTS.md`. The default is conservative: ask before commits, pushes, dependency installs, or long commands. A common low-friction setting is to auto-commit docs-only wiki changes while continuing to ask before code commits and pushes.

For an existing live project, the agent should treat the project as post-MVP unless source evidence says otherwise. It should create `wiki/plans/index.html` and `wiki/roadmap.html`, but skip `wiki/plans/mvp/` by default. Focused `features/`, `maintenance/`, or `releases/` plans should be created only when a concrete workstream exists.

For a brand-new project, invoke the skill with source context such as a prompt, PRD, notes, or initial repo files. The agent creates the wiki baseline, source index, roadmap, planning area, bounded agent guidance, and Git initialization unless opted out.

For an empty project directory with a rough idea, provide enough thin context to bootstrap without intake:

```text
Use $project-html-wiki to initialize this empty project directory.

Project idea: I want to build a small web app for tracking recurring home maintenance tasks.

Known context:
- Audience: me and my household
- Purpose: remember what needs to be done, when it was last done, and what is overdue
- Primary outcome: a simple task list with recurring schedules and completion history
- Interface type: web app
- Constraints: keep the first version local-first and simple; no accounts or payments
- Unknowns to preserve: exact tech stack, data storage choice, notification approach

Please bootstrap the HTML-first project wiki, name the unknowns clearly, initialize Git if needed, and do not generate app scaffold code yet.
```

### Start With No Source Context

If you want to start a project but do not yet have a repo, PRD, notes, or concrete idea, ask:

```text
Use $project-html-wiki to help me figure out a new project from scratch.
```

The agent should run `intake_discovery` first. It should ask focused questions about project type, audience, problem, desired outcome, constraints, interface shape, and the first useful milestone. It should not create `wiki/`, `AGENTS.md`, source briefs, plans, roadmap files, repo-local skills, dependency manifests, or scaffold files during intake.

After the intake summary includes a working title, target audience, project purpose, primary outcome, rough product/interface type, and known constraints or explicit unknowns, the agent should ask for confirmation. Once confirmed, it can proceed to `bootstrap_new` and preserve the confirmed unknowns in `wiki/Sources.html`.

Thin context is different. If you already have a rough but concrete idea or explicit unknowns, the skill may bootstrap a minimal wiki that names gaps instead of inventing certainty.

### Enable Auto-Commits

By default, the skill asks before committing. To make project wiki documentation changes managed by the Project HTML Wiki Skill commit automatically in one repo, ask during initialization or audit:

```text
Use $project-html-wiki to set this repo's automation policy to auto-commit docs-only wiki changes.
```

The agent should record this in `wiki/AGENTS.md`:

```markdown
## Automation Policy

- Commit docs-only wiki changes: auto
- Commit code changes: ask
- Push changes: ask
- Install dependencies: ask
- Run long commands: ask
- Create plans before code: meaningful-only
```

With that policy, the agent may commit completed docs-only project wiki changes managed by the Project HTML Wiki Skill automatically. It should stage only intended wiki/docs/agent-guidance files, leave unrelated dirty files untouched, and avoid pushing unless explicitly requested or the repo policy allows auto-push.

### Start A Plan

Before meaningful product, architecture, schema, API, dependency, build, auth, integration, deployment, or durable UI direction changes in a project that uses this wiki structure, ask:

```text
Use $project-html-wiki to plan this feature before implementation.
```

You can name the feature directly:

```text
Use $project-html-wiki to create a plan for billing export.
```

The agent should inspect the repo, wiki, source briefs, roadmap, and existing plans before writing. It should aim for maximum useful clarification and usually ask a focused batch of 3-7 high-leverage questions about scope, non-goals, architecture, schema, UX, dependencies, validation, rollout, risks, sequencing, or the next execution unit. It should proceed directly to a plan only when the user asks to skip questions, the change is a small fast-path fix, or repo/wiki/source evidence already answers the important planning questions.

Small local reversible fixes do not need plans. Typo fixes, narrow tests for existing behavior, private helper refactors, and local TypeScript fixes should use the fast path unless they reveal durable project knowledge.

### Work With Plans In The CLI

To inspect the current plan state without opening every file, ask:

```text
Use $project-html-wiki to show me the current plan state for this project.
```

To continue the next unit:

```text
Use $project-html-wiki to continue the current plan.
```

Continue/resume prompts are orientation prompts by default. The agent should inspect the current branch or worktree, Git state, `wiki/plans/index.html`, and relevant plan files, then summarize the current plan, current unit, blockers, and next choices. It should not create a plan or edit code unless you explicitly ask it to plan or implement after orientation.

If no active plan exists, the agent should say so, ask clarification questions, and wait for your direction before creating a plan or changing code.

For CLI-friendly planning:

```text
Use $project-html-wiki to plan this feature for CLI-friendly execution.
```

The agent should treat `wiki/plans/index.html` as the plan dashboard. It should keep the first screen of the dashboard and every active plan useful in a terminal: status, planning shape, current unit, blockers, next action, validation, and exact files to read next. Completed plans should move under `wiki/plans/zzz_completed/` and be removed from active current-plan slots while leaving compact archive links in the dashboard. For handoffs, the agent should summarize the current plan state and provide paths or commands such as:

```bash
sed -n '1,80p' wiki/plans/index.html
sed -n '1,120p' wiki/plans/features/blog-authoring.html
```

It should avoid dumping long HTML into the chat unless you ask for the full file.

### Complete Or Update A Plan

After implementing planned work, ask:

```text
Use $project-html-wiki to update the plan for the work we just completed.
```

The agent should inspect current changes, validation results, and the relevant plan. It should update the plan only when the implementation completed, invalidated, narrowed, expanded, or redirected planned work. It should update `wiki/log.html` only for durable project-context changes, not routine commits, lint fixes, test runs, or obvious Git history.

Every execution unit should include a `Verification` section with automated, manual, or explicitly deferred checks. A unit should not be marked complete unless verification is recorded or deferred with a reason.

When the top-level plan, all stages, all units, completion gates, and required verification records support completion, the agent should move the whole completed plan tree into `wiki/plans/zzz_completed/`. For example, a finished feature plan moves from `wiki/plans/features/billing-export.html` to `wiki/plans/zzz_completed/features/billing-export.html`, while a finished MVP moves as the whole `wiki/plans/mvp/` tree to `wiki/plans/zzz_completed/mvp/`.

If implementation changed source truth, roadmap direction, architecture, UI direction, or known constraints, the agent should also update `wiki/Sources.html`, source briefs, `wiki/roadmap.html`, or `wiki/index.html` as needed.

### Sync Unplanned Changes

If code changed before a plan existed, ask:

```text
Use $project-html-wiki to sync recent code changes back into the wiki.
```

The agent should inspect Git status, diffs, recent commits when relevant, current plans, roadmap, source briefs, and `wiki/log.html`. It should avoid inventing retrospective intent. If the repo shows what changed but not why, it should record the observed change and the unknown decision rather than turning it into a confident plan narrative.

### Audit Or Upgrade

To check an existing project wiki, ask:

```text
Use $project-html-wiki to audit this project's wiki and agent guidance files.
```

The agent should validate required wiki files, managed blocks, source indexes, planning structure, source brief status metadata, log policy, monorepo target-root safety, and repo-local skill behavior. It should add missing artifacts only when safe, preserve user-authored docs, and report warnings or blockers with next actions.

## Sample Workflows

Bootstrap a new project from a rough idea:

```text
Use $project-html-wiki to initialize this project from this idea: a small internal dashboard for reviewing invoice exceptions.
Use $project-html-wiki to create a plan for the first review queue workflow.
Use $project-html-wiki to update the plan after implementation.
```

Import an existing app, then plan a feature:

```text
Use $project-html-wiki to import this existing Next.js app into an HTML-first project wiki.
Use $project-html-wiki to create a plan for billing export.
Use $project-html-wiki to sync recent code changes back into the wiki.
```

Recover after unplanned work:

```text
Use $project-html-wiki to sync recent code changes back into the wiki.
Use $project-html-wiki to update the roadmap and closest feature plan based on what changed.
```

In all workflows, the agent should keep routine implementation history in Git and reserve `wiki/log.html` for durable project context that would help a future agent make better decisions.

## Works Best With

- Clear source material such as a PRD, README, technical notes, existing code, or user-provided product direction.
- Projects where agents will return across multiple sessions and need durable memory beyond Git history.
- Teams that want planning docs and codebase knowledge to stay synchronized during agentic development.
- HTML artifact workflows where `$frontend-design` is available and can improve readability, polish, and purposeful interaction.

## Repository Layout

- `LICENSE` - MIT license
- `SKILL.md` - installable skill entrypoint
- `scripts/bootstrap-smoke-test.mjs` - generates and validates a disposable moderate-import wiki under `.tmp/bootstrap-smoke/`
- `references/canonical-bootstrap-contract.md` - full bootstrap contract
- `references/generated-baseline-artifacts.md` - required artifact templates and managed-block markers
- `references/html-artifact-patterns.md` - HTML artifact pattern selector, reader-goal, visual structure, interaction, code snippet, evidence UI, style system, examples, and visual QA guidance
- `references/example-minimal-bootstrap.md` - filled minimal bootstrap example
- `references/example-moderate-import.md` - moderate existing-repo import example
- `references/planning-contract.md` - feature planning, plan updates, codebase sync, and project-context log guidance
- `references/upgrade-contract.md` - additive upgrade behavior
- `references/validation-checklist.md` - validation scenarios

## License

MIT

## Validation

Run the bootstrap smoke test:

```bash
node scripts/bootstrap-smoke-test.mjs
```

The script generates a disposable `Invoice Review` wiki under `.tmp/bootstrap-smoke/`, validates required artifacts, checks key relative links, and fails on common placeholder leakage in generated HTML.
