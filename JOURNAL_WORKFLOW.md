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

## 2. Ask Codex to shape the narrative

Paste this prompt into a Codex task after writing one or several entries:

```text
Read the raw entries in journal/entries/YYYY-MM-DD.md [and any related dates].
Create a warm, specific family-journal story for this Astro site.

First, identify only the missing details that prevent a clear narrative. Ask me up to five focused questions, grouped in one message. Do not invent facts, quotes, feelings, dates, names, or locations.

After I answer, create a draft MDX post in src/content/blog/<slug>.mdx. Keep personal or safety-sensitive details out. Use the frontmatter below, connect the events into a natural story, and add the provided photos or videos with meaningful alt text/captions. Leave draft: true until I explicitly approve publication.
```

Codex can also begin with a draft if the notes already have a complete arc. In that case, it should mark assumptions or open questions rather than filling gaps with inventions.

## 3. Review and publish

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
