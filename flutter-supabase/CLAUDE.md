# Instructions for working in this repository

These rules apply to all code written here, by a person or by Claude.
The owner **reads the code**: it must be easy for a human to review.

## Readable first

- **Small and clear**: short functions that do one thing, explicit names, nothing clever when something simple works.
- **Comments explain the why** (business rule, trap, non-obvious choice), not the what. Each file or public class
  says in one sentence what it is for.
- **No duplication**: when a block exists twice, make a widget or a function (or move it to the shared
  `dadamagouil/libs` packages) instead of copying.
- **Reasonable file size**: above ~400 lines, split (a screen, its dialogs and small widgets can live in
  neighbouring files).
- **Logic apart from display**: calculations (statistics, conversion, validation) live in `lib/core/` as pure,
  testable functions, not in widgets.
- **Reviewable diff**: one commit = one intention; no unrelated reformatting or renaming.
- **Readable tests**: a test says what it checks in its title, with a single scenario.

## Checks before every push and pull request

Automatic, no AI, identical to CI. From `app/{{app}}`:

```bash
dart format --output=none --set-exit-if-changed $(git ls-files '*.dart' | grep -v 'l10n/app_localizations')
flutter analyze    # strict rules of analysis_options.yaml: must print "No issues found"
flutter test
```

- SQL: `STUB=1 supabase/tests/run.sh <postgres-url>`.
- CI also runs **actionlint** and **Gitleaks**; **Dependabot** proposes dependency updates weekly.
- An `// ignore:` or a disabled rule is only accepted with a comment saying why.
- A deliberately un-awaited `Future` is written `unawaited(...)`.
- Formatting and lint fixes go in their own commits, with no behaviour change.

## Project conventions

- **SQL migrations**: one new migration per change, **never** edit an applied one. Each schema change has its
  test in `supabase/tests`.
- **Security**: RLS everywhere; sensitive writes through `security definer` functions; never a `service_role` key in the app.
- **Flutter**: generated files (`lib/l10n/app_localizations*.dart`) are committed after `flutter gen-l10n`; all
  languages are updated together.
- **Documentation**: always maintain `README.md`, `DEPLOY.md`, the in-app help and the store listing **in the
  same commit or pull request** as the change, never "later".
- **Tickets**: every request gets a GitHub issue (one checkbox per point), kept up to date while working and
  closed when delivered. A "for later" idea has its own issue with the proposals written in it.
- **Runner**: workflows pick their runner with the repository variable `RUNNER_LABELS` (fallback
  `["ubuntu-latest"]`, never a personal machine).
- **Branches**: work on the branch given by the session; `develop` is the integration branch.
