# zonixmed-doctor-chat-test

Internal-test-only static page used by the ZonixMed Expo app's
"Doctor-side chat tester" floating button. Hosts the doctor-side chat
emulator so a QA / developer can scan a QR code from inside the patient
chat and chat with themselves as the doctor.

**This repo is temporary.** Delete it when the doctor-side chat tester
feature is no longer needed.

## Live URL

`https://heidarimilad.github.io/zonixmed-doctor-chat-test/doctor-chat.html`

## What it contains

- `doctor-chat.html` — copy of the doctor-side chat emulator with a
  **hardcoded `accessToken`** (a doctor-side dev JWT, pre-filled into the
  `accessToken` textarea). URL query params `?doctorId=`,
  `?patientId=`, `?appointmentId=` are read on page load and used to
  pre-fill the three corresponding fields (overriding any localStorage
  value so the params always win).
- `.nojekyll` — disables Jekyll processing on GitHub Pages so the file
  is served as-is.

## Security note

The hardcoded JWT is a **dev / internal-test access token**, not a
production credential. It is committed to this public repo because the
test page must be reachable by any test device. Rotate or remove this
JWT (and the file) before extending this test beyond the internal
testing window.

## How to publish

1. Create the GitHub repo `zonixmed-doctor-chat-test` (public) under
   the `HeidariMilad` account.
2. Push the contents of this directory to the `main` branch.
3. In GitHub -> Settings -> Pages:
   - **Source**: Deploy from a branch
   - **Branch**: `main` / **Folder**: `/` (root)
4. Wait ~1 minute; the page becomes available at the Live URL above.

## How the Expo app uses it

The Expo app builds a URL of the form

```
https://heidarimilad.github.io/zonixmed-doctor-chat-test/doctor-chat.html
  ?doctorId=<doctor-profile-id>
  &patientId=<patient-user-id>
  &appointmentId=<appointment-id>
```

and shows it as a QR code inside a dev-only modal triggered by a
floating button on the patient chat screen. See
`docs/superpowers/specs/2026-06-20-internal-doctor-chat-tester.md` in
the main app repo for the full spec.

## Teardown

When testing is complete:

1. Delete this GitHub repo.
2. Remove `packages/app/features/chat/test/` from the app repo.
3. Remove the FAB wiring from `ChatChannelScreen.tsx` and
   `apps/expo/app/chat/[channelId].tsx`.
4. `npm uninstall react-native-qrcode-svg react-native-svg` in
   `apps/expo`.
