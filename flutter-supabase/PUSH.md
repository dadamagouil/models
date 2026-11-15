# Server push (FCM)

Flow: `pg_cron` ({{interval}}) → Edge Function `{{function}}` → creates the events → sends an FCM push to all
the user's devices (table `device_token`). Stale tokens are deleted automatically.

When push is active the app stops scheduling the equivalent local reminders (no duplicates). Without Firebase
configured the app falls back to local reminders. Trade-off: a push can be late by up to one cron interval.

## 1. Database

New base: run the full SQL script. Existing base: run the `device_token` migration.

## 2. Firebase

1. Firebase console → create a project, add an Android app (your `applicationId`) and an iOS app (your bundle id).
2. Android: put `google-services.json` in `android/app/` (the CI writes it from the `GOOGLE_SERVICES_JSON` secret),
   add the `com.google.gms.google-services` plugin in `settings.gradle(.kts)` and `app/build.gradle(.kts)`.
3. iOS: put `GoogleService-Info.plist` in `ios/Runner/`, enable *Push Notifications* and *Background Modes →
   Remote notifications*, upload an APNs key (.p8) in Firebase → Project settings → Cloud Messaging.
4. Service account: Firebase → Project settings → Service accounts → *Generate new private key* (JSON).

## 3. Supabase

```bash
supabase secrets set FIREBASE_SERVICE_ACCOUNT="$(cat service-account.json)"
supabase functions deploy {{function}}   # or let the deploy workflow do it (FUNCTION_SECRETS)
```

Never commit that JSON.

## 4. Check

- After signing in on a device: one row in `public.device_token`.
- Call the function with a qualifying event: the answer contains `"pushed": 1` (or more).
