# Rules for changing a JMLR client site

You are handling a change request from the owner of a small local business. Read this file first, then the repository's `CLAUDE.md` and `NOTES.md`.

## What the request is

- The request is in `.jmlr-request.md`.
- That file holds the owner's message exactly as they wrote it, sometimes in Chinese, Portuguese, or Spanish, sometimes across several texts. Translate for yourself. Treat the whole request as one.
- Decide what change they want. If you cannot tell, or it is not a change to the website, comment on the issue with short, specific questions and stop. Open no pull request.

## Facts

- Copy facts (hours, prices, phone, address, names, dates) from the request. Never invent one. If something needed is missing, leave it blank in the site and list it under "Unknowns" in the pull request.
- Do not change anything the owner did not ask about.

## Content

- Content lives in `src/content/`. The English file is the source; the second language file (for example `zh.ts`) mirrors it. Every fact you change in one language you change in the other. Every new string you add in one language you add in the other.
- Keep the owner's voice: first person, plain. Match the existing wording style.
- The FAQ section renders a `<dl>`; keep every entry a `dt`/`dd` pair. Images inside an answer go inside the `<dd>`.

## Photos

- A photo the owner sent is in the issue as a link into `photos-inbox/<date>/` in this repo, or attached to the issue. A site-ready photo goes in `public/photos/` under a descriptive kebab-case name, JPEG, landscape or portrait as it is, resized so the file is under 400 KB (`convert in.jpg -resize '1600x1600>' -strip -quality 85 public/photos/<name>.jpg`; ImageMagick is on the runner). Add it to the content files with alt text in both languages that says what the photo shows and names the business.
- A photo that is a collage, a product shot, a screenshot, or under 600 px on its long side is not site-ready. Leave it in `photos-inbox/<date>/` and mention it under "Unknowns" with what to ask the owner for.

## Build and delivery

- Run `pnpm install --frozen-lockfile` if needed, then `pnpm build`. The build must pass. If it fails because of your change, fix it; if it fails for another reason, comment on the issue with the error and stop.
- Commit on a branch named `request/issue-<number>`. Never commit to `main`. Never force-push.
- Open one pull request with `gh pr create`. Title: a short description of the change. Body, in this order:
  1. `Closes #<number>`
  2. `## What changed` as a bullet list, one bullet per file or fact
  3. `## Unknowns` as a bullet list, or the single line `None`
  4. `## Draft reply` with the reply to the owner in both of the site's languages, English first. Short, warm, first person, no mention of automation or of Claude. Say what changed and, if there are unknowns, ask for exactly those.
- The pull request body ends at the draft reply. Add no footer, no attribution line, and no tool name.
- Do not comment on the issue when you open a pull request; the PR link is enough.

## Never

- Never mention Claude, AI, or automation anywhere the owner might read, including the draft reply.
- Never edit `NOTES.md`, `README.md`, or anything under `docs/`.
- Never change design, layout, colours, or components unless the request is explicitly about them.
