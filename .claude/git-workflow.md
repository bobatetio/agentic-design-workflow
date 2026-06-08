# CLAUDE.md — Git Workflow for GitHub

> Authored by the TokScript development team. Stored here so the design-to-code
> CLAUDE.md can defer to it for all Git/branch/PR/CI rules. This file is the source
> of truth for *where and how code ships*. It is not modified by the design workflow.

Rules the agent MUST follow for every code task in this repo. Read top to bottom before starting work.

## Branch Model

* `development` — integration branch. All feature/fix work merges here first.
* `staging` — pre-production. Receives changes by merging `development` in.
* `main` — production. Every repo has one. **OFF-LIMITS to the agent.** Never check out, merge into, push to, or tag `main`. Promotion `staging → main` is done by a human, outside this workflow.

`development`, `staging`, and `main` are PROTECTED. Never commit or push to them directly. Changes reach `development` and `staging` only via a merged PR or a controlled promotion merge. `main` is never touched by the agent at all.

## Mandatory Workflow (every task)

1. **Sync everything first.**
```bash
   git fetch --all --prune
   git checkout development
   git pull origin development
```

2. **Branch off `development`.** Never branch off `staging`/`main`.
```bash
   git checkout -b feature/<short-desc>
```

3. **Do the work in that branch.** Commit in small, logical chunks (see Commit Rules).

4. **Re-sync before opening the PR** so the branch is not stale:
```bash
   git fetch origin
   git rebase origin/development   # resolve conflicts here, not in the PR
```

5. **Push and open a PR into `development`.**
```bash
   git push -u origin feature/<short-desc>
```
   Open PR: base = `development`, compare = your branch. Fill the PR description (see PR Rules).

6. **Wait for CI to pass and review.** Do not merge a red build.

7. **Merge the PR into `development`** (squash merge), then delete the branch:
```bash
   git push origin --delete feature/<short-desc>
```

8. **Promote to staging.** Merge `development` into `staging` and push. This is the last branch the agent touches:
```bash
   git checkout staging
   git pull origin staging
   git merge --no-ff development
   git push origin staging
```

9. **Build.** Do not assume a build ran. Tell the user: the build can be triggered from the **GitHub → Actions** tab (or it runs automatically on push to `staging`, depending on the workflow). Report the workflow name and how to run it. **Stop here — do not promote to `main`;** a human handles that.

## Branch Naming

* `feature/<desc>` — new functionality
* `fix/<desc>` — bug fix
* `hotfix/<desc>` — urgent fix; branch off `development` and flow through the normal PR → staging path (the agent does not push hotfixes straight to `main`)
* `chore/<desc>` — tooling, config, deps
* Use kebab-case and, where there's a tracked issue, prefix the ID: `feature/DEV-803-bitbucket-migration`.

## Commit Rules

* Use Conventional Commits: `feat:`, `fix:`, `chore:`, `refactor:`, `docs:`, `test:`, `perf:`.
* One logical change per commit. Imperative mood ("add", not "added").
* **Verify author identity before the first commit** — the commit email must match the work GitHub account:
```bash
  git config user.name
  git config user.email
  # must be the work account email, not a personal one
```
  If it's wrong, fix it for this repo (`git config user.email "<work-email>"`) before committing. Do not push commits under the wrong author.
* Never commit secrets, `.env`, keys, or tokens. Check the diff before staging.

## Pull Request Rules

* One PR = one focused change. Keep them small and reviewable.
* PR description must include: **what** changed, **why**, how it was **tested**, and the linked issue (`Closes DEV-XXX`).
* CI must be green before merge. No merging past failing checks.
* Prefer **squash merge** into `development` to keep history clean.
* Delete the source branch after merge.
* Self-merge is allowed **only after at least one approval** and a green CI run. No merging an unapproved or red PR.

## Merge & Promotion Rules

* Agent flow is one-way and stops at staging: `feature → development → staging`. Never merge `staging` back down into a feature branch; rebase the feature branch onto `development` instead.
* Use `--no-ff` for the `development → staging` promotion merge so it's a traceable commit.
* Resolve conflicts on the feature branch by rebasing onto latest `development`, never by force-merging the protected branch into your branch.
* `staging → main` promotion and release tagging are **manual, human-only** — the agent never performs them.

## Build / CI

* Pushing to `staging` should trigger the build workflow in GitHub Actions.
* The agent never claims a build "succeeded" without checking the Actions run status.
* Manual builds: GitHub → Actions → select workflow → **Run workflow** (requires `workflow_dispatch` in the workflow file).

## Testing (by risk level — run before opening the PR)

* **Small fix:** lint + unit + regression
* **Feature:** + integration + API + edge cases
* **Auth / payment / data:** + idempotency + authz + concurrency
* **Public API:** + contract + backward-compat
* **Infrastructure:** + chaos + failover
* **UI:** + E2E + accessibility
* **Performance:** + load + stress

Project-specific: scraping → schema validation + retry/backoff; SSO → token expiry + refresh rotation; webhooks → signature verification + idempotency.

## Hard Rules (never do)

* **Never check out, merge into, push to, or tag `main`.** It is human-only.
* Never `git push --force` to `development`, `staging`, or `main`. (Use `--force-with-lease` only on your own feature branch, never shared branches.)
* Never commit directly to a protected branch.
* Never merge with a failing or skipped CI run.
* Never delete remote branches other than your own merged feature branch.
* Never rewrite history on a branch that others have pulled.
* If unsure about a destructive step (rebase of shared history, branch deletion, force push), stop and ask.

## Recovery

* If a wrong branch was pushed: revert via a new PR, do not force-push to fix a protected branch.
* If a branch was deleted by mistake: recover from `git reflog` or the remote before it's GC'd.
* Keep changes reversible: prefer revertable squash commits over history rewrites.
