---
name: video-downloader
description: Download videos from YouTube, Instagram, TikTok, Twitter/X, and other platforms. Use when a video URL is provided and needs to be downloaded locally.
---

# Video Downloader Skill

Download videos from YouTube, Instagram, TikTok, Twitter/X, and other platforms.

## Usage

When the user provides a video URL to download (for watching, analysis, or importing into Remotion), use this skill.

## How to Download

Run the download script:

```bash
python3 ~/.claude/skills/video-downloader/scripts/download_video.py "VIDEO_URL" -o "OUTPUT_DIR" -q QUALITY -f FORMAT
```

## Supported Platforms

- YouTube
- Instagram (reels, posts)
- TikTok
- Twitter/X
- Facebook
- Vimeo
- And 1000+ other sites supported by yt-dlp

## After Download

1. The script outputs the downloaded file path
2. Use that path for further processing (Remotion import, frame extraction, etc.)
3. For Remotion: copy to `public/raw/` folder or download directly there
