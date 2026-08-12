# Our Netherlands Chapter

A private-family-facing journal for sharing life in the Netherlands. It is a static Astro site designed to be deployed as Cloudflare Worker static assets.

## Local development

```sh
npm install
npm run dev
```

Run `npm run check` and `npm run build` before publishing. `npm run preview` serves the built site locally.

## Writing a story

Keep raw daily notes in `journal/entries/`. That directory is deliberately gitignored, so unfinished thoughts, names, and personal details cannot accidentally ship with the website.

When you are ready, ask Codex to turn one or more entries into a family-ready narrative. The exact intake template and a ready-to-paste prompt are in [JOURNAL_WORKFLOW.md](./JOURNAL_WORKFLOW.md). Published stories are Markdown or MDX files in `src/content/blog/`; see the frontmatter template in that guide.

Place public images in `public/images/<story-slug>/` and use paths such as `/images/<story-slug>/canal-walk.jpg` in a post. Put small, web-optimized MP4 files in `public/videos/<story-slug>/`, or host long videos with a video service and embed the URL in MDX.

## Cloudflare deployment

This project is intentionally static: `wrangler.jsonc` serves `dist/` directly and has no runtime Worker script or bindings. That is the simplest, lowest-maintenance setup for a journal. It also means raw journal entries never need to reach Cloudflare.

After you create the GitHub repository and Cloudflare Worker:

1. Set the Worker name in `wrangler.jsonc` if it differs from `netherlands-blog`.
2. Set `site` in `astro.config.mjs` to the final `https://...` URL for correct canonical URLs, RSS, and sitemap links.
3. Install dependencies, then run `npm run deploy` locally, or configure your Cloudflare build to run `npm run deploy`.

The deploy command builds first, then uploads the `dist/` static assets. Do not add a `main` field or the Cloudflare adapter unless you later need server-rendering, authentication, or bindings.

## Privacy note

Anything under `public/` and `src/content/blog/` is published to everyone who can access the site. Check names, addresses, school details, travel plans, and media before publishing. A public site is not a private family archive.
