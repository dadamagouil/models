# Deployment: everything goes through GitHub Actions

Nothing to install on your computer. This document says **what to run, when, and what it does**.

To run a workflow: **Actions** tab → pick the workflow on the left → **Run workflow**.
For any workflow that touches production (`prd`), choose the **version tag** (for example `v1.5.0`) as
"Use workflow from", **never `develop`**: the `prd` environment refuses anything that does not come from a tag.

## 1. Overview

| | dev (test) | prd (production) |
|---|---|---|
| When | automatic on every merge to `develop` | by hand, from a `vX.Y.Z` version |
| Supabase | {{dev project}} | {{prd project}} |
| Web | {{dev url}} | {{prd url}} |
| Android | pre-release `dev-latest`, id `{{id}}.dev` | `.aab` / `.apk` attached to the release, id `{{id}}` |
| Secrets | GitHub environment `dev` | GitHub environment `prd` |

### Secrets per environment

| Secret | Use |
|---|---|
| `SUPABASE_URL` | Public URL of the environment's Supabase API |
| `SUPABASE_PUBLISHABLE_KEY` | Public API key (`sb_publishable_...`) |
| `DATABASE_URL` or `SUPABASE_ACCESS_TOKEN` + `SUPABASE_DB_PASSWORD` + `SUPABASE_PROJECT_REF` | Apply migrations (self-hosted / cloud) |
| `RELEASE_TOKEN` | Personal token (Contents + Workflows write) when a tag must move or secrets must be written |

Never commit a secret: even deleted later, it stays in the history (Gitleaks scans all of it).

### Workflows, one by one

**Automatic**

| Workflow | Triggered by | What it does |
|---|---|---|
| {{Android APK (dev)}} | merge to `develop` or pull request | checks, builds the dev APK, updates `dev-latest` |
| {{Deploy Supabase backend}} | merge touching `supabase/` | applies migrations (and functions) to **dev** |
| {{Quality checks}} | every pull request | actionlint (workflow files) and Gitleaks (secrets) |

**By hand, for a version** (in this order): {{Create a release}} → {{Deploy}} → {{Play Store}}.

**On demand / once**: {{list}}.

### Where automations run (GitHub runners or a self-hosted runner)

By default workflows run on GitHub runners (`ubuntu-latest`). To use a self-hosted runner, define the
repository variable `RUNNER_LABELS` (Settings → Secrets and variables → Actions → Variables) as a JSON list,
for example `["self-hosted","Linux","X64"]` (it must be valid JSON: a bare word breaks every workflow at start-up). Absent or `["ubuntu-latest"]`: GitHub runners, which depend on no
machine. Each workflow starts with the `prepare-runner` action, which checks the tools and installs Java, the
Android SDK and Node.js when needed. A runner with little memory: the dev APK is built for **arm64 only** (`--target-platform android-arm64`: one native compilation, less memory and time, lighter file; it runs on every recent 64-bit phone, not on a 32-bit phone nor an x86_64 emulator; the production bundle stays multi-architecture), and Gradle's memory can be lowered **on the runner** in `~/.gradle/gradle.properties` (it wins over the repository's), for example `org.gradle.jvmargs=-Xmx3G -XX:MaxMetaspaceSize=1G`.
Running pull-request code on your own machine is only acceptable on a
**private** repository (never use a self-hosted runner on a public one: a pull request from a fork would run on your machine).

## 2. One-time setup

1. Create the GitHub environments `dev` and `prd` (prd may require approval).
2. Fill their secrets (table above).
3. {{Other one-time steps: Firebase, Play Console, Cloudflare...}}

## 3. Life of a version, step by step

1. Run **Create a release** (bump: patch / minor / major).
2. Run **Deploy** for `prd` with that version.
3. Run the store workflow; publish only after checking the draft.

## 4. Backend (Supabase)

Migrations are applied by the deploy workflow. To test one without risk, use the dry-run workflow: it runs the
migration and its test in one request that ends with an error on purpose, so nothing stays in the base.

## 5. Common problems

| Symptom | Cause / fix |
|---|---|
| "Tools missing on the runner" | install them on the machine, or go back to GitHub runners |
| `detected dubious ownership` | the `prepare-runner` action fixes it; make sure it runs first |
