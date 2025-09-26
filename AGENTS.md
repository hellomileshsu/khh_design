# Repository Guidelines

## Project Structure & Module Organization
`index.html` drives the entire capture flow, including the landing state, camera overlay, and preview/export logic, so treat it as the single source of truth. Third-party playback support lives in `gifler.min.js`; update or replace it in-place to keep vendor diffs isolated. Visual assets (`*.png`, `*.gif`, `ezgif-*.webp`) sit in the repository root for quick iteration, while `.github/workflows/jekyll-docker.yml` provides the GitHub Pages build recipe. When you introduce new helpers, keep the flat structure: small utilities beside `index.html`, heavier tooling inside purpose-named subfolders referenced with relative paths.

## Build, Test, and Development Commands
- `python3 -m http.server 4000` (run from the repo root) to preview the static site locally.
- `npx serve .` for a quick HTTPS-like local run that mimics production headers.
- `docker run -v "$PWD":/srv/jekyll -v "$PWD"/_site:/srv/jekyll/_site jekyll/builder:latest jekyll build --future` to reproduce the GitHub Action build and confirm Pages output.

## Coding Style & Naming Conventions
Use two-space indentation across HTML, CSS, and inline scripts, matching the current layout. Prefer descriptive class names that signal state (`.camera-page`, `.record-btn`) and keep inline JavaScript concise—lift repeated logic into top-level functions near the script footer. Avoid sweeping reformatting; explain any layout or vendor-library changes in code review notes. If you add tooling, commit the configuration alongside usage instructions.

## Testing Guidelines
There is no automated suite yet, so validate changes manually on mobile Safari, Chrome, and Firefox with a full record → overlay → download flow. Capture a short clip, confirm overlay alignment, and verify that the MP4 download still works. If you introduce new scripts, consider adding a lightweight smoke test (Playwright or Cypress) and document the command that runs it.

## Commit & Pull Request Guidelines
Recent history favors short, lower-case summaries such as `ui change` and `debug`; keep messages concise but clarify scope (e.g., `ui record-button`) and lead with an imperative verb when possible. Reference any linked issue IDs, call out Safari permission adjustments, and attach before/after screenshots for UI or asset updates. PR descriptions should note test steps you executed so reviewers can replay camera interactions quickly.

## Asset Handling
Preserve existing filenames when replacing imagery to avoid breaking hard-coded references in `index.html`. Store experimental base64 payloads in `spray_base64.txt`, and compress new animations before committing. Document the source and license for any external assets in the PR description.
