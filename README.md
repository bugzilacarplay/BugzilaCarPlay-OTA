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
