# Esipher build template

A GitHub Actions workflow that turns raw AI-site-builder source into a
built, Esipher-ready ZIP — for users who don't have Node.js installed
and don't want to use the command line.

It detects npm/yarn/pnpm/bun automatically, installs dependencies, runs
the project's build script, and hands back the output (`dist/`,
`build/`, `out/`, or `.output/public/` — whichever exists) as a
downloadable artifact. No server of yours is involved: GitHub's own
runners do the work, for free.

## One-time setup (you do this once)

1. Create a new, empty repository on GitHub (e.g. `esipher-build-template`).
2. Copy this folder's contents — just the `.github/workflows/build.yml`
   file — into that repo's root, so the path is
   `.github/workflows/build.yml` in the new repo.
3. Commit and push.
4. In that repo's **Settings**, check **"Template repository"**. This
   is what lets end users click "Use this template" and get their own
   copy, workflow included, with zero git knowledge.
5. Put the repo's URL in Esipher's upload-page instructions (already
   drafted there as a placeholder — swap in your real link once this
   exists).

## What an end user does with it

No account, install, or command line beyond a browser:

1. Open your template repo's page and click **"Use this template" →
   "Create a new repository"**. Any name is fine. This gives them
   their own copy, workflow file included.
2. In their new repo, click **Add file → Upload files**, then drag in
   their project's files (the contents of the ZIP their AI builder
   exported — `package.json`, `src/`, etc. — not a build, the raw
   source).
   - GitHub's browser uploader caps out around 100 files / 25 MB per
     file per drop. Fine for typical site-export source; can be a snag
     on very asset-heavy projects (many uploads in a row, or a git
     client, handles those — out of scope for a non-technical flow).
3. Commit — this push triggers the workflow automatically.
4. Wait about a minute, then open the **Actions** tab, click the
   run that just completed, and scroll to **Artifacts** at the bottom.
5. Download `esipher-build.zip` and upload *that* to the Esipher
   plugin.

## Known limitations

- Requires a (free) GitHub account.
- If the project's `package.json` has no `build` script (or uses an
  unusual name for it), the "Build" step fails — the run's log shows
  the real error from npm/yarn/pnpm/bun.
- Private repos on a free GitHub plan get 2,000 Actions minutes/month;
  public repos get unlimited minutes. One build here takes roughly
  1–3 minutes, so this isn't a practical constraint for occasional use.
- This workflow doesn't validate the output's *structure* the way the
  planned "Dist Packager" tool would (root-absolute asset paths, stray
  `node_modules/` left in, etc.) — it just runs the build and hands
  back whatever came out. Esipher's own upload step still does its
  usual validation.
