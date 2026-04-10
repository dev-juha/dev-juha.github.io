# Copilot Instructions

## Project Overview

This is **Mule Nook** (`blog.jptn.fi`), a Jekyll blog using the [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) theme (gem-based, v7.0+). Content focuses on MuleSoft integration topics.

## Build & Serve

```bash
bundle install              # Install dependencies
bundle exec jekyll serve    # Local dev server at http://127.0.0.1:4000
bundle exec jekyll build    # Production build to _site/
```

HTML validation (currently disabled in CI but available):

```bash
bundle exec htmlproofer _site --disable-external
```

## Architecture

- **Theme**: `jekyll-theme-chirpy` gem — layouts, sass, and most assets come from the gem. Run `bundle info --path jekyll-theme-chirpy` to inspect theme source files.
- **Overrides**: Files in `_includes/` override same-named theme files (e.g., custom `mermaid.html`, `comments/giscus.html`, `favicons.html`).
- **Plugin** (`_plugins/posts-lastmod-hook.rb`): Automatically sets `last_modified_at` on posts from git history. Requires `fetch-depth: 0` in CI checkout.
- **Deployment**: Push to `main` triggers `.github/workflows/pages-deploy.yml` → builds with Jekyll → deploys to GitHub Pages.
- **Comments**: Giscus (GitHub Discussions-based), configured in `_config.yml` under `comments.giscus`.
- **Analytics**: GoatCounter (id: `jptn`).

## Creating a New Blog Post

1. **Create a branch** from `main` named `post/<slug>` (e.g., `post/tick-tock-ai`).
2. **Create the post file** in `_posts/` with filename `YYYY-MM-DD-slug.md`.
3. **Add required front matter**:

    ```yaml
    ---
    layout: post
    title: "Post Title"
    date: YYYY-MM-DD HH:MM:SS +0300
    categories: CategoryName
    tags: tag-1, tag-2
    author: dev-juha
    description: Brief summary for SEO.
    ---
    ```

4. **Write content** in Markdown below the front matter following the writing style guidelines.
5. **Commit** with `feat(post): add <post title>`.
6. **Push the branch** and open a pull request to `main`.

### Writing style

- No emojis.
- No em dashes (`—`) or en dashes (`–`). Use commas, periods, or rewrite the sentence instead.
- Write at a 7th grade reading level. Use simple, clear language.
- Keep sentences short and complete.

### Post content features

- **Mermaid diagrams**: Use fenced code blocks with `mermaid` language identifier — the custom `_includes/mermaid.html` handles rendering and dark-mode switching.
- **Images**: Place in `assets/img/` and reference with markdown. CDN prefix from `_config.yml` is applied to paths starting with `/`.
- **Authors**: Defined in `_data/authors.yml`. Use the key (e.g., `dev-juha`) in post front matter.

## Tabs (Navigation Pages)

Pages in `_tabs/` (about, archives, categories, tags) use `layout: page` with an `order` field for sidebar ordering.

## Branching Strategy

No direct commits to `main`. Each post (or change) gets its own branch, pushed to GitHub, and merged via pull request.

## Commit Conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/) and [git-cliff](https://git-cliff.org/) (`cliff.toml`) for changelog generation. Prefix commits with: `feat:`, `fix:`, `doc:`, `refactor:`, `perf:`, `style:`, `test:`, `chore:`, `ci:`.
