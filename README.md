# Bugzila Car Play v6.5.24.15

## Changes
- Removed Contact Developer from Burger menu > Help & Support.
- Improved Mobile Online Radio Player layout so the Play button is fully visible and matches Music Player size.
- Kept Favorite Radio Station control on the Online Radio Player.
- Checked Beta Test functionality: no active Beta Test menu or beta-test feature remains in the app. Only legal disclaimer text may mention beta/preview OTA builds.

## Notes
- Update `ota/latest.json` SHA-256 after building the final APK.

# Bugzila Car Play v6.5.24.14

Maintenance update for mobile player artwork and Online Radio controls.

## Changes
- Replaced `ic_bugzila_auto_artwork.png` with the updated Bugzila artwork.
- Removed unused PNG assets from `app/src/main/res/drawable`.
- Online Radio Player: enlarged the primary Play/Pause button to match the Music Player control size.
- Online Radio Player: removed Previous Station and Next Station side buttons from the player control row.
- Kept Favorite Radio Station icon on the Online Radio Player.
- Preserved Local Music controls, Cast metadata display, Android Auto behavior, and Global Cast Mode.

## Notes
- If a station has no artwork or the artwork fails to load, the updated Bugzila artwork is used as fallback.
- Build was not verified in this environment because Gradle dependencies may require internet access.

# Bugzila Car Play v6.5.24.13

## What's Changed
- Removed duplicated Release Notes section from About.
- Updated Online Radio Player controls so the Play button matches the Music Player size.
- Enabled Previous Station / Next Station by loading radio stations as a queue.
- Added a Favorite Radio Station icon directly on the Online Radio Player.
- Improved Player display for the currently Cast media for Local Music and Online Radio.

## Test Focus
- Open Online Radio > play any station > use Next / Previous in Player.
- Favorite/unfavorite a station from the Player.
- Connect Cast speaker > play Local Music and Online Radio > verify Now Playing metadata updates.

# Bugzila Car Play v6.5.24.12

## Update Focus
Legal acceptance and safer update notification workflow.

## Changes
- Removed Export Debug Log from Burger menu > Help & Support.
- Added Auto Check for Update toggle above Check for Updates.
- Auto Check is disabled by default.
- When enabled, the app checks for a new version after the main startup functions finish.
- When a newer version is found, the app sends a notification: “Bugzila Car Play เวอร์ชันใหม่พร้อมดาวน์โหลดแล้ว”.
- Tapping the notification opens Help & Support so the user can manually press update/download.
- Added first-launch Terms of Use, Software License & Legal Disclaimer acceptance.
- Added About > Legal / Disclaimer with short summary, full legal text, and an Accept checkbox.

## Notes
- Notification permission is requested on Android 13+.
- OTA install remains user-confirmed only.
- Cast and Android Auto behavior from v6.5.24.11 is preserved.

# Bugzila Car Play v6.5.24.11

## Cast control and UI reliability update

This release improves Global Cast behavior after real-device testing with Google Nest Mini / Cast devices.

### Fixed
- Casted Local Music / Online Radio metadata now updates the mini player and Now Playing screen.
- Main screen no longer depends on the purple Cast status bar by default.
- Top Cast icon now shows connection state:
  - white = not connected
  - green = connected
- Tapping the green Cast icon stops/disconnects the current Cast session.
- Cast Settings > Stop Casting now ends the Cast session, not only the remote media stream.
- Show Cast Status Bar setting now works live from Cast Settings.
- Enable Cast Button setting now works live from Cast Settings.
- Auto Stop Local Cast Server setting is honored when Cast session ends.
- Prefer Local Audio Cast setting is honored for local music casting.

### Notes
- Android Auto behavior remains unchanged.
- Per-song/per-station Cast icons remain removed.
- Global Cast Mode remains active.

# 🚗 Bugzila Car Play v6.5.22.03 – Player Main-Thread Restore Crash Fix

This release fixes the crash found from live logcat testing on Vivo V40 Pro.

## Crash Found in Logcat

`java.lang.IllegalStateException: Player is accessed on the wrong thread`

The crash happened inside:

`LocalMediaLibraryService.restoreLastState()`

The service was resolving the previous queue on `Dispatchers.IO`, then calling ExoPlayer methods on the IO worker thread.

## Fixed

- Kept MediaStore scan and queue resolving on `Dispatchers.IO`
- Moved all ExoPlayer calls back to `Dispatchers.Main`
- Fixed:
  - `player.setMediaItems(...)`
  - `player.setMediaItem(...)`
  - `player.prepare()`
  - `player.seekTo(...)`

## Still Active

- Stable v6.0 permission flow
- Stable v6.0 MediaStore scan model
- Music filter: `IS_MUSIC != 0`, `SIZE > 500 KB`, `DURATION >= 60 sec`, MP3/M4A/AAC/FLAC
- Video filter: `SIZE > 500 KB`, `DURATION >= 60 sec`, MP4/M4V/MOV/WEBM
# 🚗 Bugzila Car Play v6.5.22.02 – Revert Permission and Media Scan to Stable v6.0 Model

This release reverts the permission request and Local Music / Video MediaStore scan behavior back to the stable v6.0 model because v6.5.22.02 still caused app crashes on Vivo V40 Pro and Huawei P30 Pro.

## Reverted to v6.0 Behavior

- Startup permission request:
  - Android 13+ requests `READ_MEDIA_AUDIO` + `READ_MEDIA_VIDEO` together
  - Android 12 and below requests `READ_EXTERNAL_STORAGE`
- Removed startup `READ_MEDIA_VISUAL_USER_SELECTED`
- Local Music scan uses the stable v6.0 MediaStore query:
  - `MediaStore.Audio.Media.EXTERNAL_CONTENT_URI`
  - `IS_MUSIC != 0`
  - no custom folder/path filtering
  - no Vivo/Honor strict scan workaround
- Video scan uses the stable v6.0 MediaStore query:
  - `MediaStore.Video.Media.EXTERNAL_CONTENT_URI`
  - simple `_ID`, `TITLE`, `DURATION` projection
- Restored normal MediaController startup
- Restored normal CastContext startup
- Restored normal Equalizer startup

# 🚗 Bugzila Car Play v6.5.22.02 – Stable v6.0 Media Scan with Music/Video Filters

This release keeps the stable v6.0-style MediaStore scan model and adds stricter file filters for Local Music and Video.

## Local Music Filter

- `IS_MUSIC != 0`
- `SIZE > 500 KB`
- `DURATION >= 60 sec`
- `MIME_TYPE IN`:
  - `audio/mpeg` for MP3
  - `audio/mp4` for M4A / MP4 audio
  - `audio/aac` for AAC
  - `audio/flac` for FLAC

## Video Filter

- `SIZE > 500 KB`
- `DURATION >= 60 sec`
- `MIME_TYPE IN`:
  - `video/mp4` for MP4
  - `video/x-m4v` for M4V
  - `video/quicktime` for MOV
  - `video/webm` for WEBM

## Note

`IS_MUSIC` is an audio-only MediaStore column, so it is not used in the video query.

## Goal

Prioritize app stability on Vivo V40 Pro and Huawei P30 Pro by removing the newer experimental permission and scan workarounds.

Download Our Application
https://github.com/bugzilacarplay/BugzilaCarPlay-OTA/releases
