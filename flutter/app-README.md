# {{App name}}: Flutter app

Complete Flutter project (`android/`, `web/`... included). See `../../DEPLOY.md` to get the APK without installing anything.

```bash
cd app/{{app}}
flutter pub get
flutter run \
  --dart-define=SUPABASE_URL=https://XXXX.supabase.co \
  --dart-define=SUPABASE_PUBLISHABLE_KEY=sb_publishable_...
flutter analyze && flutter test
```

Build-time values (`--dart-define`) are read by `AppConfig` (package `app_supabase` in `dadamagouil/libs`).
No URL or key is ever written in the code.

Server push: see `PUSH.md`. Web build: see `DEPLOY.md`.
