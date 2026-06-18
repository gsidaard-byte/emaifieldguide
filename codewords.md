# Project Code Words

Use these short code words at the start of a new session to quickly restore context for the EM x AI Field Guide site.

## BOOT

Meaning: Get fully up to speed before doing any work.

When the user says `BOOT`, do this:

1. Set the working folder to `/Users/sidg/Desktop/Service/EMxAI/Video Docx/files2`.
2. Read `context.md`, `.codex/context.md`, and this `codewords.md`.
3. Run `git status --short` and `git log --oneline -8`.
4. Note that `Screenshot_Video_NotAligned.png` may be untracked and should not be committed unless requested.
5. Summarize the current repo state, latest commits, published URL, and any uncommitted changes.

Use this when starting a fresh session.

## WRAP

Meaning: Finish the current work cleanly.

When the user says `WRAP`, do this:

1. Run `git status --short`.
2. Summarize what changed in this session.
3. If changes should be saved, commit with a clear message.
4. Push to `origin/main` if the user wants the public site updated.
5. Check GitHub Pages build status:

```bash
gh api repos/gsidaard-byte/emaifieldguide/pages/builds/latest --jq '{status, error: .error.message, commit: .commit}'
```

6. Update `context.md` if the session changed project state, URLs, important decisions, or known issues.

Use this before ending a session.

## SHIP

Meaning: Publish the site update.

When the user says `SHIP`, do this:

1. Review `git status --short`.
2. Stage only relevant files.
3. Commit with a concise message.
4. Push to GitHub.
5. Check the Pages build status.
6. Report the commit hash and public URL.

Public URL:

`https://gsidaard-byte.github.io/emaifieldguide/`

## CHECK

Meaning: Verify the live site and current layout.

When the user says `CHECK`, do this:

1. Open or fetch the public site:

`https://gsidaard-byte.github.io/emaifieldguide/em-ai-walkthrough.html?v=b412930`

2. If layout is in question, use Chromium/Playwright if available.
3. For the video alignment issue, inspect the `Value Awareness` habit:

```js
openHabit(HABITS.find(h => h.name === 'Value Awareness'))
```

Expected layout at a `1720px` viewport:

- `.habit-video`: width `860px`
- `.video-slot`: width `860px`
- `.step-card.revealed`: width `860px`

If the user still sees the old narrow video, suspect browser cache or a different URL first.

## VIDEO

Meaning: Work on the NotebookLM explainer video integration.

When the user says `VIDEO`, remember:

- The 18 MP4 files live in `notebooklm_videos/`.
- Videos are mapped in the `VIDEOS` object in `em-ai-walkthrough.html`.
- The video container should stay full content width:

```css
.habit-video{margin:30px 0 0;width:100%;}
```

- Do not restore the old `max-width:640px` video rule.

## ALIGN

Meaning: Work specifically on the video/text alignment issue.

When the user says `ALIGN`, remember:

- The previous screenshot showed the video too narrow and left-aligned.
- The fix was to remove the `640px` cap from `.habit-video`.
- The step card is full width, but inner step paragraphs intentionally have `max-width:640px`.
- Compare the video against `.step-card.revealed`, not only against `.step-card p`.

## NOTE

Meaning: Update project memory.

When the user says `NOTE`, update `context.md` and, if relevant, this `codewords.md` with the latest decisions, URLs, commands, or gotchas.

Keep notes concise and operational.
