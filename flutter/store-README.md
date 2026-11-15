# Store listing of {{App name}}

Everything needed to fill the Play Console. Texts are in `listing/<language>/`, images in `graphics/` and `screenshots/`.

## Listing

| Field | Value |
|---|---|
| Name | {{App name}} (`listing/<language>/title.txt`) |
| Default language | {{language}} (also add {{others}}) |
| Short description | `listing/<language>/short_description.txt` (≤ 80 characters) |
| Full description | `listing/<language>/full_description.txt` (≤ 4000 characters) |
| What's new | `listing/<language>/whats_new.txt` (≤ 500 characters), updated at each release |
| Icon | `graphics/icon-512.png` (512 × 512) |
| Feature graphic | `graphics/feature-graphic-<language>.png` (1024 × 500) |
| Phone screenshots | `screenshots/<language>/` (1080 × 1920, 9:16), 2 to 8 images |
| Category | {{category}} |
| Contact e-mail / website / privacy policy | {{...}} |
| Package name | `{{com.example.app}}` (final after the first upload) |

## Declarations (Play Console → App content)

- **Ads**: {{none}}.
- **App access**: {{restricted: provide a demo account whose e-mail is already confirmed}}.
- **Target audience**: {{age groups; say explicitly which groups are NOT ticked and why}}.
- **Content rating (IARC)**: answer the questionnaire exactly.
- **Account deletion**: in the app ({{path}}) and at {{url}}.
- **Permissions**: {{list with the reason of each}}.

### Data safety

| Category | Data | Collected | Purpose | Optional |
|---|---|---|---|---|
| Personal info | E-mail address | yes | App functionality, account management | no |

Shared with third parties: {{no}}. Used for ads, analytics or profiling: {{no}}.

## Steps in the Play Console

1. Create the app (name, default language, free/paid, declarations).
2. Fill "App content", then the main listing (texts, icon, graphic, screenshots).
3. Testing → Closed testing: create the track, add testers, upload the `.aab`, check, roll out.
4. A recent personal developer account needs a closed test with the minimum number of testers for 14 days before asking for production access.
5. Copy the SHA-1 of the app signing key into Firebase.
