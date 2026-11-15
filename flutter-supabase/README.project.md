# {{Project name}}

<!-- One paragraph: what the product does, for whom, on which platforms, in which languages. -->
{{One-paragraph product summary.}}

- Production: {{url}} · Development: {{url}}
- Deployment guide: **[DEPLOY.md](DEPLOY.md)** (what to run, when, what it does) · Store listing: [store/README.md](store/README.md)

## Environments

| | `dev` | `prd` |
|---|---|---|
| State | {{in service / not created yet}} | {{in service / not created yet}} |
| Backend | {{Supabase project or self-hosted instance}} | {{second instance}} |
| GitHub Environment | `dev` | `prd` |
| Android application id | `{{com.example.app.dev}}` | `{{com.example.app}}` |

<!-- Say WHY the environments are separated the way they are (separate instances, not schemas, ...). -->

## Features

<!-- One bullet per user-visible feature. Keep it in sync with the in-app help and the store listing. -->
- **{{Feature}}**: {{one sentence, with the rule that is not obvious}}.

## Architecture

| Layer | Technology |
|---|---|
| App | {{Flutter version}}, Riverpod, `gen-l10n` translations (`app/{{app}}/lib/l10n`) |
| Backend | Supabase: Postgres, Auth, Row Level Security |
| Business logic | SQL functions (`SECURITY DEFINER`): sensitive writes never go through direct table writes |
| Scheduled jobs | {{Edge Functions + pg_cron, or none}} |
| Delivery | GitHub Actions (environments `dev` and `prd`) |

## Repository layout

| Folder | Content |
|---|---|
| `app/{{app}}/` | Flutter application (`lib/`, `test/`, `android/`, `web/`) |
| `supabase/migrations/` | Versioned schema, one new migration per change |
| `supabase/tests/` | SQL tests (`run.sh`), one per schema change |
| `.github/workflows/` | Automations (see DEPLOY.md); most are thin callers of `dadamagouil/devops` |
| `DEPLOY.md` | Deployment and release procedure |

## Data model (summary)

<!-- The tables, who writes them, which function is the only way to change them. -->

## Working on the project

1. One branch per topic, one pull request to `develop`. The `build` check must be green before merging.
2. Merging deploys to **dev** automatically; test there.
3. A version is created and released by hand from a `vX.Y.Z` tag (see DEPLOY.md).

Checks to run before every push (free, no AI, identical to CI), from `app/{{app}}`:

```bash
dart format --output=none --set-exit-if-changed $(git ls-files '*.dart' | grep -v 'l10n/app_localizations')
flutter analyze
flutter test
```

SQL: `STUB=1 supabase/tests/run.sh "postgresql://postgres@localhost:5432/test"`.
