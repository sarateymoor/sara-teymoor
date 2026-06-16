# Post Content

Transcribe, generate captions, and publish a video across all platforms via Dropbox + Late API.

## Instructions

### Step 1: Get the video

**Default: Scan Dropbox `/Content/ready/` folder**

```bash
DROPBOX_TOKEN=$(grep DROPBOX_ACCESS_TOKEN $HOME/.env | cut -d'=' -f2)

curl -s -X POST https://api.dropboxapi.com/2/files/list_folder \
  -H "Authorization: Bearer $DROPBOX_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"path": "/Content/ready", "include_media_info": true}'
```

Ask: **"Which video do you want to post? **

Only proceed to Step 2 after confirmation.

**Alternative sources (if user provides directly):**
- A Google Drive link: download with `yt-dlp`
- A video URL: download with `python3 ~/.claude/skills/video-downloader/scripts/download_video.py "URL" -o /tmp/post-content`
- A local file path: use directly

### Step 2: Download from Dropbox to local temp

```bash
mkdir -p /tmp/post-content
DROPBOX_TOKEN=$(grep DROPBOX_ACCESS_TOKEN $HOME/.env | cut -d'=' -f2)

curl -s -X POST https://content.dropboxapi.com/2/files/download \
  -H "Authorization: Bearer $DROPBOX_TOKEN" \
  -H "Dropbox-API-Arg: {\"path\": \"/Content/ready/FILENAME\"}" \
  -o /tmp/post-content/video.mov
```

### Step 3: Extract audio and transcribe

```bash
ffmpeg -i "/tmp/post-content/video.mov" -ar 16000 -ac 1 -y /tmp/post-content/audio.wav

python3 -c "
import whisper
model = whisper.load_model('medium')
result = model.transcribe('/tmp/post-content/audio.wav')
print(result['text'])
"
```

### Step 4: Read voice guides and generate captions

Before writing ANY captions, read:
1. `reference/caption-voice.md` - Your caption style and rules
2. `reference/scripting-voice.md` - Your voice and language

### Step 5: Generate captions for all platforms

Using the transcript + caption voice guide, generate captions. Captions are ONE LINE. The video does the talking.

### Step 6: Revision loop

After presenting captions, ask: **"Want me to adjust any of these? Or ready to post?"**

### Step 7: Publish via Late API

When the user says "post" or "ready", publish to all platforms using Dropbox direct link + Late API.

**IMPORTANT:** Always confirm with the user before posting. Never auto-post without explicit approval.

### Step 8: Move file to /posted/

After successful posting, move the video from `ready/` to `posted/`.

$ARGUMENTS
