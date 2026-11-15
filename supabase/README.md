# Supabase conventions

The SQL test runner and the Supabase stub live in `.github/actions/supabase-sql-tests` (see its `run.sh`).
Reusable SQL building blocks (`touch_updated_at`, `forbid_column_change`, `make_invite_code`) live in
`dadamagouil/libs`, folder `sql/`.

Layout expected by the runner and the workflows:

```
supabase/
  config.toml
  migrations/<yyyymmddhhmmss>_<name>.sql   one new migration per change, never edited once applied
  tests/<name>.test.sql                    prints "RESULT OK" and leaves nothing in the base (rollback or final error)
  functions/<name>/index.ts                Edge Functions
```

A SQL test creates its own users and rows, checks each rule and prints `RESULT OK` when everything passed.
Two styles work: `begin; ... raise notice 'RESULT OK'; ... rollback;`, or a single `do $$ ... $$` block that
builds a text report and finishes with `raise exception 'RESULT OK : ...'` so nothing is kept (this one can
also be sent to a remote base in one request by `sql-check-remote.yml`).
