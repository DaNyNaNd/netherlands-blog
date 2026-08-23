# From daily notes to a family story

## 1. Capture a raw entry

Create `journal/entries/YYYY-MM-DD.md` from this template. The `journal/entries/` directory is ignored by Git.

```md
# YYYY-MM-DD — short label

## What happened
- Facts, in rough order:
- Who was there:
- Where we were:

## Details worth keeping
- A sound, smell, small surprise, or bit of dialogue:
- What felt different from home:

## Photos and videos
- `public/images/story-slug/photo.jpg` — what it shows
- `public/videos/story-slug/clip.mp4` — what happens in it

## Privacy and accuracy
- Details that must stay private:
- Things I am unsure about:
- People or names to omit:
```

Write freely. Fragments and bullet points are useful; the notes do not need to read well.

## 2. Prepare photos and videos for the web

Keep the full-resolution originals in Google Photos. Download copies into a temporary working folder and prepare those copies before adding anything to `public/`. Files under `public/` are published and served exactly as provided; Astro does not resize or compress them during the build.

### Photos: Preview and ImageOptim

1. Open the downloaded copy in Preview and choose **Tools → Adjust Size**.
2. Preserve the aspect ratio and set the longest edge to no more than `1600 px`.
   - Cover photos should preferably be landscape `4:3`, approximately `1600 × 1200`.
   - Portrait photos can be approximately `1200 × 1600`.
3. Export as JPEG with the quality slider around 80–85 percent.
4. Work on the exported copy: ImageOptim normally optimizes the file in place.
5. Drag the copy into ImageOptim to reduce its size and remove unnecessary metadata such as GPS coordinates.
6. Aim for `200–600 KB` per photo. Try to keep every photo below `800 KB`.

Use clear lowercase filenames such as `canal-walk.jpg` or `first-stroopwafel.jpg`. Put only the optimized files in `public/images/<story-slug>/`.

### Videos: HandBrake

Start with the **General → Fast 1080p30** preset, then use these settings:

- Format: `MP4`
- Web Optimized: enabled
- Video encoder: `H.264 (x264)`
- Resolution: no more than `1920 × 1080` for landscape or `1080 × 1920` for portrait; do not upscale smaller footage
- Frame rate: `30 fps`
- Audio: `AAC`, stereo, approximately `128 kbps`

Do not use **Constant Quality** when the output must remain under a specific file size. Its `RF` setting controls visual quality, so the resulting size is unpredictable. An iPhone source may also use efficient HEVC compression, which means an H.264 conversion can be larger than the original even after re-encoding.

Instead, select **Average Bitrate (kbps)** in the Video tab, enable **2-Pass Encoding**, and enable **Turbo First Pass**. Use the video's duration to choose a bitrate:

| Video duration | Average video bitrate for an approximately 20 MiB result |
| --- | ---: |
| 30 seconds | `5000 kbps` |
| 45 seconds | `3300 kbps` |
| 60 seconds | `2400 kbps` |
| 90 seconds | `1550 kbps` |
| 2 minutes | `1100 kbps` |
| 3 minutes | `700 kbps` |

These values leave room for the `128 kbps` audio track and MP4 overhead. If a longer video looks poor at the listed bitrate, reduce it to `720p`, trim it, or host it with a video service rather than forcing a low-quality 1080p encode.

As a simpler alternative, use HandBrake's **Web → Social 25 MB** preset that matches the clip's duration. HandBrake provides separate presets for 30-second 1080p, one-minute 720p, two-minute 540p, and longer clips.

Keep each locally hosted video below `20 MiB`. Cloudflare Workers static assets have a `25 MiB` per-file deployment limit, so the lower target leaves useful headroom.

Put only the finished MP4 in `public/videos/<story-slug>/`. Before publishing, play the complete exported file and check its orientation, color, audio, beginning, and ending.

### Final privacy and quality check

- Confirm that the optimized file—not the iPhone original—is in the repository.
- Check for visible addresses, school details, license plates, travel plans, or bystanders who should not be published.
- Confirm that location metadata has been removed from photos.
- Write down what each file shows so the post can use meaningful alt text and captions.
- Keep the combined media for a typical story reasonably small; a few megabytes is better than dozens.

## 3. Ask Codex to shape the narrative

Paste this prompt into a Codex task after writing one or several entries:

```text
Read the raw entries in journal/entries/YYYY-MM-DD.md [and any related dates].
Create a warm, specific family-journal story for this Astro site.

First, identify only the missing details that prevent a clear narrative. Ask me up to five focused questions, grouped in one message. Do not invent facts, quotes, feelings, dates, names, or locations.

After I answer, create a draft MDX post in src/content/blog/<slug>.mdx. Keep personal or safety-sensitive details out. Use the frontmatter below, connect the events into a natural story, and add the provided photos or videos with meaningful alt text/captions. Place media only after the complete paragraph or narrative beat it illustrates; never split prose or wrap text around media. Group related media using the site's media-group conventions, keep it chronologically relevant, and omit weak, duplicate, unrelated, or sensitive assets. Leave draft: true until I explicitly approve publication.
```

Codex can also begin with a draft if the notes already have a complete arc. In that case, it should mark assumptions or open questions rather than filling gaps with inventions.

### Story media layout rules

- Finish the complete paragraph or narrative beat before placing any image or video. Never insert media inside a sentence or between sentences that belong in the same paragraph.
- Do not float media or wrap body text around it. Text and media should each occupy their own stable block in the reading order.
- Place media immediately after the paragraph or short section it illustrates. Keep city, beach, home, wildlife, and other distinct moments grouped with their matching text and in chronological order.
- Group two to four closely related assets inside `<div class="media-group">...</div>`. Use `<div class="media-group media-group--three">...</div>` only when three items genuinely belong together.
- Use a standalone `<figure class="media-figure media-figure--compact">` for one supporting image, or `media-figure--feature` inside a group for a key visual beat.
- Do not rotate or skew story media. Preserve consistent alignment and let media groups collapse to a single column on small screens.
- Prefer omitting a duplicate, weak, unrelated, or sensitive asset over forcing every available file into the story.
- Make each caption explain why the media belongs at that exact point in the narrative.

## 4. Review and publish

Published stories use this frontmatter:

```md
---
title: A short, human title
description: One sentence that gives family the point of the story.
pubDate: 2026-08-12
coverImage: /images/story-slug/cover.jpg
coverAlt: A useful description of the photo.
locations:
  - Utrecht
draft: true
---

Your story goes here.
```

Set `draft: false` or remove the field only after review. Draft posts are excluded from the journal, individual routes, RSS feed, sitemap, and home page.

For a local check:

```sh
npm run check
npm run build
npm run preview
```

Then ask Codex to commit and deploy only after you approve the final story and its media.
