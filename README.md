# 🚗 Bugzila Car Play v6.5.20.12 – Vivo Video List + Thai Top 100 Playback Fix

## Fixed

- Fixed Video page not showing video files on Vivo V40 Pro Android 16
- Added video permission request directly from the Video page
- Added Android 14+ partial visual access handling
- Video scanner now tries multiple MediaStore video collections safely
- Added safer video query with MIME and size checks
- Fixed Thai Top 100 station tap playback
- Tap on a Thai Top 100 station now starts normal app playback instead of auto-casting
- Added Media3 HLS dependency for `.m3u8` Online Radio streams
- Normalized stream URLs before playback

## Notes

For Vivo Android 16, open App Info > Permissions > Photos and videos and allow video access if the Video page is still empty.
For Thai Top 100, some endpoints are `.m3u8` HLS streams and now require `media3-exoplayer-hls`, which is included in this version.

# 🚗 Bugzila Car Play v6.5.20.11 – Compile Hotfix for Android Auto Voice Search

## Fixed

- Fixed Kotlin compile error in `LocalMediaLibraryService.kt`
- Fixed unsupported escape sequence in voice search regex
- Changed regex patterns to Kotlin raw strings:
  - `"""[\\p{Punct}]"""`
  - `"""\\s+"""`

## Still Active

- Android Auto Local Music microphone search
- Thai and English song-name search
- Thai Top 100 beta radio category
- Safe Modern Local Music scan
- Cast / HLS radio improvements

## Files

- `latest.json` - update manifest checked by the Android app
- `releases/` - APK files for each release
- `CHANGELOG.md` - short release history

## Current latest version

Stable Version: 6.5.20.03
