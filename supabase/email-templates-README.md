# {{App}} e-mail templates (Supabase)

Supabase accepts one template per e-mail type, not one per language: each template contains **all languages
in the same mail** ({{list}}), the main language first with the big button.

| Supabase type | File | Subject |
|---|---|---|
| Confirm signup | `confirm-signup.html` | `confirm-signup.subject.txt` |
| Reset Password | `reset-password.html` | `reset-password.subject.txt` |
| Change Email Address | `change-email.html` | `change-email.subject.txt` |

Supabase variables used: `{{ .ConfirmationURL }}` (the link, **except** for "Reset Password"), `{{ .SiteURL }}`
and `{{ .TokenHash }}` (password reset), `{{ .Email }}` and `{{ .NewEmail }}` (address change). Do not change them.

## Password reset: a browser-independent link

The classic link goes through a code exchange (PKCE) that **only works in the browser where the request was
made**: from a mail app that opens links in its embedded browser you land on the sign-in page. The "Reset
Password" template therefore uses `{{ .SiteURL }}?token_hash={{ .TokenHash }}&type=recovery`: the app
validates the token itself (`verifyOTP`), whatever the browser. Supabase's *Site URL* must be the app address.
(The matching Dart helpers are `recoveryTokenHash` / `isAuthLink` in `dadamagouil/libs`, package `app_core`.)

## Setting up (dev, then prd)

1. Supabase → **Authentication** → **Emails** → **Templates**.
2. Pick the type. **Subject**: paste the `.subject.txt` file. **Message body** (source / HTML): paste the `.html` file.
3. **Save**. Repeat for the two other types, then for the other environment.
4. "Magic Link", "Invite user" and "Reauthentication" are not used: leave them as they are.

## Testing

Password reset, sign-up with confirmation, address change: check the text, the button, the link (it must open
the app) and the display on phone and desktop.
