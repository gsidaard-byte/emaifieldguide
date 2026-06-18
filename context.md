# EM x AI Field Guide Site Context

Last updated: 2026-06-18

## Latest Handoff

Latest pushed commit at wrap time:

`134c8de Add project code words`

GitHub Pages reported `built` for commit `134c8dea73412dcd41d6b6644db3269536e16191`.

Only known untracked file at wrap time:

`Screenshot_Video_NotAligned.png`

## Working Folder

Use this folder:

`/Users/sidg/Desktop/Service/EMxAI/Video Docx/files2`

This is a small static site. The main page is `em-ai-walkthrough.html`; `index.html` immediately redirects to it.

## Published Site

GitHub repo:

`https://github.com/gsidaard-byte/emaifieldguide.git`

GitHub Pages URL:

`https://gsidaard-byte.github.io/emaifieldguide/`

The old repo/site name was `em-ai-field-guide-videos`; it was renamed to `emaifieldguide`.

## Important Files

- `em-ai-walkthrough.html`: the full single-page interactive field guide. Most content, CSS, and JavaScript live inline here.
- `index.html`: redirect entry point for GitHub Pages.
- `codewords.md`: short commands such as `BOOT`, `WRAP`, `SHIP`, `CHECK`, `VIDEO`, `ALIGN`, and `NOTE` for future sessions.
- `notebooklm_videos/`: contains the 18 MP4 explainer videos generated from NotebookLM.
- `README.md`: short project summary.
- `Screenshot_Video_NotAligned.png`: a user-provided screenshot showing a previous video alignment issue. It is intentionally untracked unless the user asks to commit it.

## Session Code Words

The project has a codeword system in `codewords.md`.

Useful examples:

- `BOOT`: read the context files, inspect git status/log, and summarize the current state.
- `WRAP`: finish cleanly, commit/push if appropriate, check Pages, and update context.
- `SHIP`: commit and publish site changes.
- `CHECK`: verify the live site and layout.
- `VIDEO`: work on the 18 NotebookLM video integrations.
- `ALIGN`: focus on the video/text alignment issue.
- `NOTE`: update project memory files.

## Current Video Integration

The habit videos are mapped in the `VIDEOS` object in `em-ai-walkthrough.html`.

Each habit page renders a video with:

```html
<div class="habit-video" id="hVideo">
  <div class="video-slot" id="videoSlot">...</div>
</div>
```

The relevant CSS is:

```css
.habit-video{margin:30px 0 0;width:100%;}
.video-slot{
  position:relative;width:100%;aspect-ratio:16/9;
  background:var(--navy);border-radius:10px;overflow:hidden;
}
```

Do not reintroduce `max-width:640px` on `.habit-video`. That caused the video to appear too narrow and left-aligned compared with the step cards and page text.

## Recent Layout Issue

The user reported that the video was still left-aligned and too narrow. The actual issue was a previous rule:

```css
.habit-video{margin:30px auto 0;max-width:640px;}
```

That was changed to:

```css
.habit-video{margin:30px 0 0;width:100%;}
```

Live verification in Chromium showed the video and the first step card both measured `860px` wide on the GitHub Pages URL. If the user still sees the old narrow video, suspect browser cache first.

`index.html` currently redirects with a query string:

```html
em-ai-walkthrough.html?v=b412930
```

This was added to force a fresh load after the CSS alignment fix.

## Deployment Notes

Typical workflow:

```bash
git status --short
git add <changed-files>
git commit -m "<message>"
git push
gh api repos/gsidaard-byte/emaifieldguide/pages/builds/latest --jq '{status, error: .error.message, commit: .commit}'
```

Network access may require approval in Codex. If `git push`, `curl`, or `gh api` fails with DNS or network errors, retry with escalated network permission.

## Verification Notes

For layout verification, use Playwright/Chromium if possible. A useful check is to open:

`https://gsidaard-byte.github.io/emaifieldguide/em-ai-walkthrough.html?v=b412930`

Then run:

```js
openHabit(HABITS.find(h => h.name === 'Value Awareness'))
```

Expected rendered measurements at a `1720px` viewport:

- `.habit-video`: width `860px`
- `.video-slot`: width `860px`
- `.step-card.revealed`: width `860px`

The inner step paragraph intentionally remains `max-width:640px`; the step card itself is wider.

## Git State Caution

The file `Screenshot_Video_NotAligned.png` may appear as untracked. Do not delete or commit it unless the user explicitly asks.

Avoid reverting unrelated changes. The user may make edits between sessions.
