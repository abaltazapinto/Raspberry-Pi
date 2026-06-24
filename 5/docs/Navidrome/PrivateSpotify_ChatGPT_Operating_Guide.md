# PrivateSpotify — ChatGPT Operating Guide

Use this guide whenever André uploads/sends a screenshot showing a newly downloaded music file the downloaded music is always in the HOME directory, MP4 file, audio file, terminal output, or Navidrome state.

The goal is to guide André through a clean, repeatable pipeline:

```text
Downloaded file on desktop
        ↓
incoming/
        ↓
identify format
        ↓
extract/convert audio if needed
        ↓
write metadata
        ↓
validate metadata
        ↓
ready/Artist/Album/
        ↓
copy to Raspberry Pi /srv/music
        ↓
Refresh/Scan Navidrome
        ↓
test on web/mobile
```

## Current known architecture

### Desktop / Samorinha

```text
~/Music/PrivateSpotify/library/
├── incoming/
├── ready/
└── problem/
```

Meaning:

```text
incoming/ = raw downloaded files
ready/    = clean audio files with metadata
problem/  = invalid/problematic files
```

### Raspberry Pi / Porto

```text
/srv/music
```

This is the real Navidrome music library.

### Navidrome access

```text
http://100.70.175.106:4533
```

The IP is accessed through the private/VPN network.

---

# Global rules for the assistant

1. Give only one next action at a time.
2. Always identify which machine the command runs on: Desktop or Raspberry Pi.
3. Prefer one-line commands because André's terminal executes on Enter.
4. Avoid multi-line shell commands unless explicitly requested.
5. Do not use `/srv/navidrome/data` for music.
6. Do not use `~/Music` on the Pi as the Navidrome library.
7. The Navidrome library is `/srv/music`.
8. Prefer `.m4a` when the source audio is AAC.
9. Do not force MP3 unless compatibility demands it.
10. Always validate metadata before copying to the Pi.
11. Always ask one decision/confirmation question at the end.

---

# Legal/source note

The assistant should not help bypass copyrights or platform terms.

For the technical pipeline, assume André already has a file he is allowed to store/convert. Focus on:

```text
file handling
format identification
audio extraction
metadata
library organization
copying to Raspberry Pi
Navidrome scan/debug
```

---

# Standard workflow after a file is downloaded

## Step 1 — move raw downloaded file to incoming

Run on **Desktop**.

Use this when the file is still in the home folder:

```bash
mv ~/"NAME"* ~/Music/PrivateSpotify/library/incoming/
```

Example:

```bash
mv ~/"Buena Vista Social Club - El Cu"* ~/Music/PrivateSpotify/library/incoming/
```

Then:

```bash
cd ~/Music/PrivateSpotify/library/incoming && ls -lh
```

Expected result: the raw file is visible in `incoming`.

---

## Step 2 — identify file type

Run on **Desktop**, inside `incoming`.

```bash
file *
```

Interpretation:

### Good audio already

```text
MP3 Audio
Apple iTunes ALAC/AAC-LC (.M4A) Audio
FLAC audio
Ogg data
```

Go to metadata step.

### MP4/DASH/video but possibly usable

```text
ISO Media, MPEG v4 system
Dynamic Adaptive Streaming over HTTP
MP4 Base Media
```

Go to stream inspection.

### Bad/problem file

```text
HTML document
ASCII text
data
```

Move to problem:

```bash
mv "FILE_NAME" ~/Music/PrivateSpotify/library/problem/
```

---

## Step 3 — inspect streams

Run on **Desktop**, inside `incoming`.

```bash
for f in *; do echo "=== $f ==="; ffprobe -v error -show_entries stream=index,codec_type,codec_name -of compact "$f"; done
```

Interpretation:

### Good

```text
codec_type=audio|codec_name=aac
```

Use `.m4a`.

### Video plus audio

```text
codec_type=video|codec_name=h264
codec_type=audio|codec_name=aac
```

Extract audio to `.m4a`.

### No audio

Move to `problem`.

---

## Step 4 — extract audio from MP4/DASH to M4A

Run on **Desktop**, inside `incoming`.

Use one-line command:

```bash
ffmpeg -i "INPUT_FILE" -vn -c:a copy "Artist - Title.m4a"
```

Example:

```bash
ffmpeg -i "Mi Cuba Querida.mp4" -vn -c:a copy "Mi Cuba Querida.m4a"
```

Example if the input has no extension:

```bash
ffmpeg -i "Buena Vista Social Club - El Cuarto De Tula" -vn -c:a copy "Buena Vista Social Club - El Cuarto De Tula.m4a"
```

Important:

```text
-vn must come before the output file.
```

Wrong:

```bash
ffmpeg -i "INPUT" "OUTPUT.m4a" -vn
```

Correct:

```bash
ffmpeg -i "INPUT" -vn -c:a copy "OUTPUT.m4a"
```

---

## Step 5 — write metadata

Run on **Desktop**, inside `incoming`.

Template:

```bash
ffmpeg -i "Artist - Title.m4a" -c copy -metadata title="Title" -metadata artist="Artist" -metadata album="Album" -metadata track="1" "Artist - Title.tagged.m4a" && mv "Artist - Title.tagged.m4a" "Artist - Title.m4a"
```

### Example — Buena Vista Social Club / El Cuarto De Tula

```bash
ffmpeg -i "Buena Vista Social Club - El Cuarto De Tula.m4a" -c copy -metadata title="El Cuarto De Tula" -metadata artist="Buena Vista Social Club" -metadata album="Singles" -metadata track="1" "Buena Vista Social Club - El Cuarto De Tula.tagged.m4a" && mv "Buena Vista Social Club - El Cuarto De Tula.tagged.m4a" "Buena Vista Social Club - El Cuarto De Tula.m4a"
```

### Example — Mi Cuba Querida

```bash
ffmpeg -i "Mi Cuba Querida.m4a" -c copy -metadata title="Mi Cuba Querida" -metadata artist="Buena Vista Social Club" -metadata album="Singles" -metadata track="1" "Mi Cuba Querida.tagged.m4a" && mv "Mi Cuba Querida.tagged.m4a" "Mi Cuba Querida.m4a"
```

### Example — Ana Carla Maza / Caribe

```bash
ffmpeg -i "Ana Carla Maza - Caribe.m4a" -c copy -metadata title="Caribe" -metadata artist="Ana Carla Maza" -metadata album="Caribe" -metadata track="1" "Ana Carla Maza - Caribe.tagged.m4a" && mv "Ana Carla Maza - Caribe.tagged.m4a" "Ana Carla Maza - Caribe.m4a"
```

---

## Step 6 — validate metadata

Run on **Desktop**, inside `incoming`.

```bash
ffprobe -v quiet -show_entries format_tags -of default=noprint_wrappers=1 "Artist - Title.m4a"
```

Expected:

```text
TAG:title=...
TAG:artist=...
TAG:album=...
TAG:track=...
```

If these tags are missing, do not copy to Pi yet.

---

## Step 7 — move clean file to ready

Run on **Desktop**.

General structure:

```text
ready/Artist/Album/Artist - Title.m4a
```

For singles:

```bash
mkdir -p ~/Music/PrivateSpotify/library/ready/"Artist"/"Singles" && mv "Artist - Title.m4a" ~/Music/PrivateSpotify/library/ready/"Artist"/"Singles"/
```

For real album:

```bash
mkdir -p ~/Music/PrivateSpotify/library/ready/"Artist"/"Album" && mv "Artist - Title.m4a" ~/Music/PrivateSpotify/library/ready/"Artist"/"Album"/
```

### Example — Buena Vista

```bash
mkdir -p ~/Music/PrivateSpotify/library/ready/"Buena Vista Social Club"/"Singles" && mv "Mi Cuba Querida.m4a" ~/Music/PrivateSpotify/library/ready/"Buena Vista Social Club"/"Singles"/
```

### Example — Ana Carla Maza

```bash
mkdir -p ~/Music/PrivateSpotify/library/ready/"Ana Carla Maza"/"Caribe" && mv "Ana Carla Maza - Caribe.m4a" ~/Music/PrivateSpotify/library/ready/"Ana Carla Maza"/"Caribe"/
```

---

## Step 8 — copy to Raspberry Pi

Run on **Desktop**.

```bash
scp -r ~/Music/PrivateSpotify/library/ready/"Artist" abaltaza@100.70.175.106:/srv/music/
```

Example:

```bash
scp -r ~/Music/PrivateSpotify/library/ready/"Buena Vista Social Club" abaltaza@100.70.175.106:/srv/music/
```

---

## Step 9 — verify on Raspberry Pi

Run on **Raspberry Pi**.

```bash
tree /srv/music
```

Expected structure:

```text
/srv/music/
├── Ana Carla Maza/
│   └── Caribe/
│       └── Ana Carla Maza - Caribe.m4a
└── Buena Vista Social Club/
    └── Singles/
        ├── Buena Vista Social Club - El Cuarto De Tula.m4a
        └── Mi Cuba Querida.m4a
```

Do not check `~/Music` on the Raspberry Pi. That is not the Navidrome library.

---

## Step 10 — refresh Navidrome

Open:

```text
http://100.70.175.106:4533
```

Click:

```text
Refresh / Scan
```

Then check:

```text
Albums → All
Artists
Songs
Recently Added
```

---

# Fixing existing files already on Raspberry Pi

Sometimes André already copied a file to `/srv/music`. In that case, fix directly on the Pi.

## Fix metadata directly on Pi

Example Ana Carla:

```bash
ffmpeg -i "/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.m4a" -c copy -metadata title="Caribe" -metadata artist="Ana Carla Maza" -metadata album="Caribe" -metadata track="1" "/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.tagged.m4a" && mv "/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.tagged.m4a" "/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.m4a"
```

Validate:

```bash
ffprobe -v quiet -show_entries format_tags -of default=noprint_wrappers=1 "/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.m4a"
```

---

# Reorganizing files already on Pi

## Move loose Ana Carla file into proper structure

Run on **Raspberry Pi**:

```bash
mkdir -p "/srv/music/Ana Carla Maza/Caribe" && mv "/srv/music/Ana Carla Maza - Caribe.m4a" "/srv/music/Ana Carla Maza/Caribe/"
```

Expected:

```text
/srv/music/Ana Carla Maza/Caribe/Ana Carla Maza - Caribe.m4a
```

---

# Troubleshooting

## Problem: command says “is a directory”

Cause: André typed a folder path as if it were a command.

Wrong:

```bash
~/Music/PrivateSpotify/library/incoming
```

Correct:

```bash
cd ~/Music/PrivateSpotify/library/incoming
```

## Problem: `Unable to choose an output format for '-vn'`

Cause: wrong `ffmpeg` argument order.

Wrong:

```bash
ffmpeg -i "INPUT" "OUTPUT.m4a" -vn
```

Correct:

```bash
ffmpeg -i "INPUT" -vn -c:a copy "OUTPUT.m4a"
```

## Problem: copied file but not in `~/Music` on Pi

Correct: Navidrome reads:

```text
/srv/music
```

Not:

```text
~/Music
```

Run:

```bash
tree /srv/music
```

## Problem: Unknown Artist

Check tags on the file that Navidrome actually reads:

```bash
ffprobe -v quiet -show_entries format_tags -of default=noprint_wrappers=1 "/srv/music/path/to/file.m4a"
```

If `TAG:artist` is present but Navidrome still shows unknown:

1. Restart Navidrome:
   ```bash
   cd /srv/navidrome && sudo docker compose restart navidrome
   ```
2. Click Refresh/Scan.
3. Hard refresh browser with Ctrl+F5.

## Problem: mobile app works but browser view looks outdated

Likely browser cache or still viewing old album page. Go to:

```text
Albums → All
```

or search for the artist.

---

# Assistant response style for this project

When André sends a screenshot:

1. State the current detected step.
2. Give exactly one command/action.
3. State expected result.
4. Ask one confirmation question.

Example:

```text
Ação — 1 passo:
Run this on Desktop:
<command>

Objetivo:
<what this proves>

Pergunta:
Did it create the .m4a file?
```
