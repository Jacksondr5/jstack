---
name: pr-evidence
description: Load it when a PR needs screenshots or a video. How to capture before/after evidence, upload it to GitHub, check the links, and present it in the PR description.
---

# PR evidence

The rule that every UI PR carries screenshots is in my general rules. This skill is how to do it.

## Capture

- **Before and after.** Before from the PR's base branch, after from its head. Use the same data, the same viewport and the same theme in both, so the difference is the change.
- **Every state the PR touches:** empty, loading, error, the populated case, and any state the change adds. If a state is hard to reach, say so rather than skip it silently.
- **The full screen in every pair,** plus a crop of the area that changed when the change is small. Reviewers judge each "after" screen as a whole.
- **Motion, timing or an interaction sequence needs a short video.** Record an MP4 with the Browser panel (`preview_recording_start`, then `preview_recording_stop`), not a GIF.
- **Use an isolated dev server in your own worktree.** In the J5 repository, that is the test home the `j5-live-test` skill sets up, never a copy of real data. Elsewhere, seed it with realistic test data. Never capture from, or point a browser at, a live install.
- **Check each file is what it claims to be** before uploading: `file shot.png` should say "PNG image data", and `file clip.mp4` should say "ISO Media". Tools sometimes write JPEG or WebP bytes under a `.png` name, which GitHub won't render.

## Upload images

- **In the J5 repository (`Jacksondr5/j5code`),** run `node scripts/j5/pr-evidence.ts <pr> --pair "<caption>" <before.png> <after.png>` (repeat `--pair`; use `--new "<caption>" <after.png>` for a state the change adds). It pushes to `pr-<number>/` on the `j5/evidence` branch, retries when another agent pushed first, and prints the description's table. The `j5-live-test` skill covers the base and head servers.
- **Elsewhere,** use the repository's own evidence convention, or attach the images to the PR.
- Embed raw links, which render inline: `https://raw.githubusercontent.com/<owner>/<repo>/<branch>/<path>.png`. A `github.com/.../blob/...` link shows a file page, not an image.
- Never commit screenshots to the PR's own branch.

## Upload videos

GitHub plays only its own attachments, so videos go through a PR comment box without posting the comment.

1. Open the PR (or its issue) in the Browser panel under the **"GitHub Video Upload"** profile. Open nothing else under that profile. If it's signed out, ask me to sign in.
2. Give the MP4 to the comment box's file input: `preview_act` with `action: "upload"`, `locator: "#fc-new_comment_field"` and the file's absolute path in `paths`.
3. Read the `github.com/user-attachments/assets/…` URL it writes into `textarea#new_comment_field` with `preview_evaluate`.
4. Empty the box. Never submit it.
5. Put the URL on its own line in the PR description.

## Check the links

- **Images:** `curl -sI <url>` shows `200` and an `image/` content type.
- **Videos:** `gh api markdown` on the URL produces `<video`. Once the description is saved, a signed-out `curl -sL -o /dev/null -w '%{http_code} %{content_type}' <url>` shows `200 video/mp4`. A HEAD request gets 403.

## Present

- Before/after pairs in the PR description, each with a one-line caption saying which state it shows.
- Alt text names the caption and the side: `![Viewer search with no matches, after](…)`, not `![After](…)`. Viewers that show images on their own, like J5's, show only the alt text.
- When a later push changes the UI, replace the images in the description, uploading new files under new names. Don't post a new "evidence" comment on every push.
- Keep captions to what the image shows. Commit SHAs, temp paths, fixture receipts and capture logs bury the images.
- If you couldn't capture something, say so in the description. Never imply a UI was verified when it wasn't.
