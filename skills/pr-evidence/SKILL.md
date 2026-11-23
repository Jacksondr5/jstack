---
name: pr-evidence
description: Load it when a PR needs screenshots or a video. How to capture before/after evidence, upload it to GitHub, check the links, and present it in the PR description.
---

# PR evidence

The rule that every UI PR carries screenshots is in my general rules. This skill is how to do it.

If you run into difficulty getting the evidence, report that to the user or your Director. That's an issue that needs to be fixed.

## Capture

- **Before and after.** Before from the PR's base branch, after from its head. Use the same data, the same viewport and the same theme in both, so the difference is the change.
- **Every state the PR touches:** empty, loading, error, the populated case, and any state the change adds. If a state is hard to reach, say so rather than skip it silently.
- **The full screen in every pair,** plus a crop of the area that changed when the change is small. Reviewers judge each "after" screen as a whole.
- **Motion, timing or an interaction sequence needs a short video.** Record an MP4 with the Browser panel (`preview_recording_start`, then `preview_recording_stop`), not a GIF.
- **Use an isolated dev server in your own worktree.** In the J5 repository, that is the test home the `j5-live-test` skill sets up, never a copy of real data. Elsewhere, seed it with realistic test data. Never capture from, or point a browser at, a live install.
- **Check each file is what it claims to be** before uploading: `file shot.png` should say "PNG image data", and `file clip.mp4` should say "ISO Media". Tools sometimes write JPEG or WebP bytes under a `.png` name, which GitHub won't render.

## Upload

Images and videos go up the same way in every repository: through the PR's comment box in the Browser panel, without posting the comment. GitHub hosts them as attachments, and plays video only from its own attachments.

1. Open the PR (or its issue) in the Browser panel under the **"GitHub Video Upload"** profile. Open nothing else under that profile. If it's signed out, ask me to sign in.
2. Click the comment box ("Leave a comment"), then its **"Paste, drop, or click to add files"** button. A file picker opens.
3. Answer it with `preview_act`, `action: "upload"`, and every file's absolute path in `paths`. Several files go in one call.
4. Wait until the box holds one `github.com/user-attachments/assets/…` URL per file, and read them with `preview_evaluate` on `textarea[placeholder="Leave a comment"]`. GitHub writes an `<img>` tag for each image and a bare URL for each video. If the count doesn't match the files, post nothing: retry once, then report it.
5. Empty the box (select all, delete) and check it's empty. Never submit it.
6. Put the URLs in the PR description.

An upload stays private (a signed-out GET returns 404) until a description embeds it, and we can't delete it ourselves afterwards, only GitHub Support can. Upload only what goes in the PR.

Never commit screenshots to the PR's own branch.

## Check the links

Once the description is saved, a signed-out `curl -sL -o /dev/null -w '%{http_code} %{content_type}' <url>` shows `200 image/png` (or another `image/` type) for an image and `200 video/mp4` for a video. It can take a few seconds to turn from 404 to 200. Use GET: a HEAD request gets 403.

## Present

- Before/after pairs in the PR description, each with a one-line caption saying which state it shows.
- Alt text names the caption and the side: `alt="Viewer search with no matches, after"`, not `alt="After"` or the file name GitHub filled in. Viewers that show images on their own, like J5's, show only the alt text.
- When a later push changes the UI, replace the images in the description, uploading new files under new names. Don't post a new "evidence" comment on every push.
- Keep captions to what the image shows. Commit SHAs, temp paths, fixture receipts and capture logs bury the images.
- If you couldn't capture something, say so in the description. Never imply a UI was verified when it wasn't.
