# bootstrap-pilot-p5-guide Constitution

## Core Principles

### I. Scope Boundaries

The project is a todo app: it lets a person capture, organize, complete and review tasks.

- In scope: creating, editing, completing, deleting and listing todo items, and the minimum
  supporting features each approved spec names (for example filtering, due dates, persistence).
- Out of scope unless a spec explicitly adds it: multi-user collaboration, payments, third-party
  integrations and anything not needed to manage a person's own todo items.
- Agents and `tdd-implementer` MUST check every change against the approved spec and this section.
  A change outside the scope of its spec MUST be split out or sent back through the specification
  step; it MUST NOT be slipped into an unrelated pull request.

Rationale: a small, clearly bounded product stays reviewable by one person.

### II. Spec-Driven Development

- Every feature MUST have an approved `spec.md` and `plan.md` before implementation starts. They
  are merged in their own pull request.
- The initial `tasks.md` MUST be generated and analyzed (`speckit-analyze`) on the Phase 1
  implementation branch before any code is written. There is no separate tasks-only issue or
  pull request.
- `tasks.md` MAY evolve in reviewed implementation pull requests. `speckit-analyze` MUST be
  rerun after structural edits (Phases, tasks, dependencies).
- Spec Kit steps run in order: specify, clarify, plan, tasks, analyze, implement.

Rationale: agreeing on the what and the how first keeps agent-written code reviewable.

### III. Traceable Work Tracking

- Each roadmap entry has an Epic and a spec-and-plan Planning issue.
- Each Phase in `tasks.md` is one GitHub issue. `T###` tasks MUST NOT become issues.
- Every branch is linked to an issue, and every pull request links its issue (`Closes #N`, or
  `Part of #N` for a Phase pull request).
- GitHub is the worklog: decisions and their reasons live in the issue and the pull request body.

Rationale: anyone can trace a line of code back to the decision that caused it.

### IV. Security Principle

- A change that touches authentication, authorization, user data access, secrets or
  infrastructure permissions MUST include a documented security review (`security-auditor`) before
  merge, and Critical and High findings MUST be fixed.
- Authorization MUST be enforced server-side wherever a server exists; client-side checks are
  convenience only.
- Secrets MUST NOT be committed to the repository. Untrusted input MUST be validated.
- Dependencies MUST be reviewed before they are added.

Rationale: even a small app handles personal data, and security gaps are cheapest to prevent.

### V. Quality Gate

- Every behavior change MUST have an automated test written before its implementation (red, then
  green). A bug fix MUST start with a test that reproduces the bug.
- The first approved plan MUST define the project's test, lint and format checks and add them as
  required CI checks. Until then, the gate is the existing required CI checks, which MUST pass
  before merge.
- Coverage is judged per change: new or changed behavior MUST be covered by tests. A numeric
  coverage minimum MAY be added by amendment once the technology baseline exists.
- The `code-reviewer` pass MUST be run on the exact final diff before a pull request is created.

Rationale: tests are the contract that lets agents change code safely.

## Technology Baseline and Dev Environment

### Technology Baseline

The languages, frameworks and tools for the todo app are chosen in the first approved plan
(`plan.md`) and MUST be recorded there with a one-line reason each. Once a plan is merged, its
choices are the baseline: agents MUST check changes against it and MUST NOT introduce another
language, framework or runtime dependency without a plan update or a recorded decision under
Team-Decision Rules. Tool versions MUST be pinned where the ecosystem supports it (for example
`.nvmrc` for Node). Choose the simplest option that meets the spec.

### Dev Environment

- Changes are developed and tried locally first. Local setup MUST be reproducible from the
  repository's documented commands.
- A deployment target, if the project has one, MUST be named in a plan. Changes MUST be
  verified in a non-production environment (or locally when none exists) before release.
- The first deployment MUST be preceded by a `security-auditor` review.

## Git Workflow, Merge Gates and Team-Decision Rules

### Git Workflow

- Work happens on issue-backed branches created from a freshly pulled `main`. Nothing is
  pushed to `main` directly.
- Required independent, non-author approvals before merge: **0**, because this is a solo
  project and GitHub does not let an author approve their own pull request. The same number is set in
  `[review] required_approvals` in `project-context.toml` and in the branch ruleset. If a second
  contributor joins, this number MUST be raised to at least 1 by amendment.
- Every commit needs the owner's explicit approval before it is made.

### Merge Gates

A pull request MAY merge only when all of these hold:

- Required CI checks pass.
- The `code-reviewer` review is complete on the final diff, with Critical and Required findings
  fixed and all other findings shown to the owner.
- `security-auditor` has run for security-sensitive changes, with Critical and High findings fixed.
- The pull request links its issue and records decisions and verification results in its body.
- The owner has reviewed the final diff and merges it.

### Team-Decision Rules

Even as a solo project, these decisions MUST be written down (as an issue, a `plan.md` section or
a pull request description with the options considered and the reason for the choice) before they
are acted on:

- Choosing or changing the language, framework, data store or hosting.
- Adding a runtime dependency.
- Changing this constitution, the roadmap's scope or the review rules.

If other contributors join, these decisions additionally need approval by another team member.

## Governance

This constitution supersedes other practices in this repository. Amendments are made through the
`speckit-constitution` skill in a pull request that states the reason for the change and the
new version. Versioning follows semantic versioning: MAJOR for incompatible removals or
redefinitions of governance, MINOR for a new principle or materially expanded guidance, PATCH
for clarifications. Every pull request review MUST verify compliance with this constitution, and
any deviation MUST be justified in the pull request body.

**Version**: 1.0.0 | **Ratified**: 2026-11-20 | **Last Amended**: 2026-11-20
