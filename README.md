## v6.5.20.03 - 2026-05-20

### Bug Fixes
- Fixed Google Cast button hang: tapping Cast no longer freezes the app.
  Root cause: CastContext.getSharedInstance() was called synchronously (blocking
  the main thread). Now uses the async overload introduced in Cast SDK 21.x.
- Fixed SafeCastButton hang: CastButtonFactory.setUpMediaRouteButton() now runs
  after the button is attached to a window and only with an Activity context.
- Fixed app hang and self-close after granting media permissions on Vivo, Xiaomi
  and Samsung OEM ROMs. Root cause: permission result triggered a double-build of
  MediaController when the Activity was recreated by the system after the dialog.
- Fixed delayed-build Job not being cancelled when Activity stops, which caused
  orphaned MediaController futures on fast back-press.

### Android Auto Redesign
- Redesigned Android Auto main screen from GridTemplate to ListTemplate.
- Local Music row now shows subtitle "Local songs, albums and artists" with a
  blue-tinted MUSIC icon for clear glance recognition.
- Online Radio row now shows subtitle "Live Thailand and international radio" with
  a green-tinted RADIO icon.
- ListTemplate renders as modern pill-style rows on car displays instead of small
  flat icon squares, matching current Android Auto UX guidelines.
- Grid fallback retained in code for hosts that restrict ListTemplate.

## Files

- `latest.json` - update manifest checked by the Android app
- `releases/` - APK files for each release
- `CHANGELOG.md` - short release history

## Current latest version

Stable Version: 6.5.20.03
