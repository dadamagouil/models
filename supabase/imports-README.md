# Data imports (one-off, outside migrations)

SQL scripts to **run once by hand** (Supabase Studio → SQL editor) to load existing data. They are not
migrations: they do not change the schema, and neither `supabase db push` nor the SQL tests ever run them.

If the repository is private, these files may hold personal data: never make them public.

| File | Content |
|---|---|
| `{{file}}.sql` | {{what it loads}} |

## Using a script

1. Apply all migrations first (the script needs the default rows they create).
2. Open the script and replace the placeholder at the top with the login e-mail of the account.
3. Paste it in Studio and run it: a final "Import done" message confirms the counts and totals.

Each script must be **replayable** (fixed identifiers, no duplicate) and **all or nothing** (a final check
cancels everything when figures differ).
