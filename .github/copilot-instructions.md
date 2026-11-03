# Copilot instructions for MemoirsCLJS

## Project type and architecture

- This repository is a Jekyll-based static site for a personal technical notes blog, not an application with runtime code.
- Site metadata lives in `_config.yml` and is intentionally minimal: title and description only.
- Most content is Markdown under `_posts/`, organized by topic or technology, for example `_posts/Dotnet/WPF/` and `_posts/Other/Frontend/Basic/`.
- The root-level `index.md` is the home page, and `categories.md` builds a category index from Jekyll's `site.categories` data.
- Static assets such as images live under `Images/`.
- Deployment is GitHub Pages based; the production workflow is `.github/workflows/jekyll-gh-pages.yml` and it runs on pushes to `main`.

## Build, test, and lint commands

- There is no repo-local test suite, lint configuration, or package manifest in this project.
- The authoritative validation path is the GitHub Pages workflow in `.github/workflows/jekyll-gh-pages.yml`, which builds the site with `actions/jekyll-build-pages@v1`.
- If local Jekyll validation is needed in a Ruby environment, use:
  - `bundle exec jekyll build`
  - `bundle exec jekyll serve`
- There is no single-test command to run because the repository does not contain automated tests.

## Content and file conventions

- Blog posts are Markdown files under `_posts/` and generally follow the filename pattern `YYYY-MM-DD-Title.md`.
- Files are usually grouped by topic folder names (for example `Dotnet`, `Other`, `Database`, `Frontend`) and then by subtopic.
- Existing posts commonly use minimal front matter. Topic/category metadata is usually represented with `categories: ...` in the YAML front matter.
- The site intentionally mixes Chinese and English content; preserve the language and style of each article rather than enforcing a single language across the repo.
- Keep article titles, URLs, and category names consistent with the surrounding content and folder structure.

## Editing guidance

- Prefer small, focused documentation edits. This repo is content-first and does not have application code to refactor or debug.
- When adding a new post, mirror the existing folder taxonomy and naming conventions instead of inventing a new structure.
- Preserve the Jekyll conventions already used by the project: Markdown headings, code fences, and simple front matter over custom scripting.
- Avoid introducing frameworks, package managers, or runtime tooling unless a change clearly requires them.
