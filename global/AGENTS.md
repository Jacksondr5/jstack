# Jackson's preferences for every agent

These hold on every machine and in every project. A machine's own rules follow
after them.

## Working with me

- **I'm usually not watching your thread.** When you need something from me,
  send it through the Outer Loop system (until that exists, my J5 Inbox). Don't
  leave it in the thread. Sometimes I'll come and talk to you, when I need to.
  We'll go through the topic, then I'll leave, so anything still open after that
  goes back through the Outer Loop system.
- **Don't hand me testing an agent can do.** If an agent can't test it, tell me
  why.
- **Once we agree something in a repo I own should be an issue, file it right
  away** and give me the number. Posting to someone else's repo still needs my
  go-ahead.

## Repositories and data

- **Keep my hosts, paths, ports and devices out of shared repos.** They go in my
  private rules and my machine-local config. Scripts read config and use
  neutral defaults.
- **Nothing from real data goes into commits,** test strings and examples
  included. Use invented or seed text.

## Skills

- **A skill holds only what I decided or told you,** never general advice a
  capable model already follows or procedures I never used.

## Screenshots for pull requests

**Every PR that changes UI must have screenshots on the PR.** This overrides any repository's own guidance, including rules that make screenshots optional, "upon request", or that ask before starting a browser.

A reviewer should be able to judge a UI change from the PR page alone, without running anything.

**Permission.** Capturing the screenshots for a PR you were asked to open is part of that request. You don't need separate permission to start a browser for it.

**What to capture**

- Before and after, for every UI change: before from the base branch, after from the PR's head. Use the same data, the same viewport, and the same theme in both, so the difference is the change.
- Capture every state the PR touches: empty, loading, error, the populated case, and any state the change adds. If a state is hard to reach, say so rather than skip it silently.
- Show the full screen in every pair, plus a crop of the area that changed when the change is small. Reviewers judge each "after" screen as a whole.
- Motion, timing, or an interaction sequence needs a short video. Record it as an MP4 with the Browser panel (`preview_recording_start` / `preview_recording_stop`), not a GIF.

**How to capture**

- Use an isolated dev server in your own worktree. In the J5 repository, that is the test home the `j5-live-test` skill sets up, never a copy of real data. Elsewhere, seed it with realistic test data. Never capture from, or point a browser at, a live install.
- Check each file is what it claims to be before uploading: `file shot.png` should say "PNG image data", and `file clip.mp4` should say "ISO Media". Tools sometimes write JPEG or WebP bytes under a `.png` name, which GitHub won't render.

**Where to put them**

- In the J5 repository (`Jacksondr5/j5code`), push images to the `j5/evidence` branch under `pr-<number>/`, from a separate worktree or clone checked out on that branch. Other agents push there too, so fetch and rebase before pushing. Elsewhere, use that repository's evidence convention, or attach the images to the PR.
- Embed raw links, which render inline: `https://raw.githubusercontent.com/<owner>/<repo>/<branch>/<path>.png`. A `github.com/.../blob/...` link shows a file page, not an image.
- Videos go on GitHub itself, because GitHub only plays its own attachments. Open the PR (or its issue) in the Browser panel under the "GitHub Video Upload" profile, give the MP4 to the comment box's file input with `preview_upload`, take the `github.com/user-attachments/assets/…` URL it writes, and empty the box without submitting. Put the URL on its own line in the PR description. Open nothing else under that profile; if it's signed out, ask me to sign in.
- Check each image link returns the image before posting: `curl -sI <url>` should show `200` and an `image/` content type. For a video URL, `gh api markdown` on it should produce `<video`; once the description is saved, a signed-out GET (`curl -sL -o /dev/null -w '%{http_code} %{content_type}' <url>`) should show `200 video/mp4`. A HEAD request gets 403.
- Never commit screenshots to the PR's own branch.

**How to present them**

- Put them in the PR description as before/after pairs, with a one-line caption saying which state each shows.
- Give each image alt text with the caption and the side: `![Viewer search with no matches, after](…)`, not `![After](…)`. Viewers that show the images on their own, like J5's, show only the alt text.
- When a later push changes the UI, replace the images in the description (upload new files with new names). Don't post a new "evidence" comment on every push.
- Keep captions to what the image shows. Commit SHAs, temp paths, fixture receipts, and capture logs bury the images.
- If you couldn't capture something, say so in the description. Never imply a UI was verified when it wasn't.
