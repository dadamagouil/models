# Models

File models (templates) for new projects, organised by technology: README, `DEPLOY.md`, `CLAUDE.md`, store
listing, push guide, data imports, e-mail templates, lint rules, Dependabot, `.gitignore`, pull request and issue
templates.

> Related repositories: [`dadamagouil/libs`](https://github.com/dadamagouil/libs) (shared code) and
> [`dadamagouil/devops`](https://github.com/dadamagouil/devops) (CI/CD workflows; thin caller workflows are in its
> `templates/<stack>/workflows` because they only make sense next to the workflows they call).

## Structure: one folder per technology or stack

| Folder | For | Content |
|---|---|---|
| [`github/`](github) | any repository on GitHub | pull request and issue templates |
| [`flutter/`](flutter) | a Flutter / Android app | app README, store listing, lint rules, Dependabot |
| [`supabase/`](supabase) | a Supabase backend | folder conventions, data imports, auth e-mail templates |
| [`flutter-supabase/`](flutter-supabase) | a project that combines both | project README, `DEPLOY.md`, `CLAUDE.md`, push guide, `.gitignore` |

A new technology or stack (Python, Node, Terraform...) gets its **own folder** with its models and a row in the
tables below. A model that fits every technology goes in `github/` (or a new `common/` folder).

## How to use a model

1. Copy the file into the new project (target path in the table below).
2. Replace every `{{placeholder}}` (search for `{{`). A placeholder says what is expected.
3. Delete the sections that do not apply, and keep the HTML comments only while you still need them.
4. Keep the file true afterwards: a change to a feature, command, variable or workflow updates the matching
   document in the same pull request.

| Model | Copy to | Notes |
|---|---|---|
| `github/pull_request_template.md` | `.github/pull_request_template.md` | |
| `github/ISSUE_TEMPLATE/request.md` | `.github/ISSUE_TEMPLATE/request.md` | One checkbox per point |
| `flutter/app-README.md` | `app/<app>/README.md` | How to run the Flutter app |
| `flutter/store-README.md` | `store/README.md` | Play Console listing and declarations |
| `flutter/analysis_options.yaml` | `app/<app>/analysis_options.yaml` | Strict, readable-code lint rules |
| `flutter/dependabot.yml` | `.github/dependabot.yml` | Weekly dependency updates (replace `{{app}}`) |
| `supabase/README.md` | `supabase/README.md` | Folder layout and SQL test convention |
| `supabase/imports-README.md` | `supabase/imports/README.md` | One-off data imports |
| `supabase/email-templates-README.md` | `supabase/email-templates/README.md` | Auth e-mails |
| `flutter-supabase/README.project.md` | `README.md` | Overview: features, environments, architecture, layout |
| `flutter-supabase/DEPLOY.md` | `DEPLOY.md` | Secrets, workflows one by one, runners, release steps |
| `flutter-supabase/CLAUDE.md` | `CLAUDE.md` | Rules for Claude and for people: readable code, checks, conventions |
| `flutter-supabase/PUSH.md` | `app/<app>/PUSH.md` | Server push with Firebase Cloud Messaging |
| `flutter-supabase/gitignore` | `.gitignore` | |

## Adding or changing a model

One model per file, placeholders written `{{like this}}`, HTML comments to guide the person filling it. A model
that mentions a workflow, a package or a command is updated in the same pull request as that change in
`devops` / `libs`.

Models are written in English. Translate the copy if the project's documents are in another language.

## Runners

This repository's own check (`.github/workflows/quality.yml`) follows the repository variable `RUNNER_LABELS`,
a **JSON list** such as `["self-hosted","Linux","X64"]`; without it, GitHub runners are used. The same variable
is described in `flutter-supabase/DEPLOY.md` for the projects built from these models.

## Versions

Releases are tags `vX.Y.Z`, created by the `Release` workflow (Actions → Release → Run workflow, or by the owner's
automated task) only when something changed since the last one. Use `patch` for fixes and documentation,
`minor` for something new, `major` for a change that breaks users.

## License

MIT, see [LICENSE](LICENSE).
