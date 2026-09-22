# Marginal Notes: starter blog

A minimal Jekyll blog (using the built-in `minima` theme, lightly
restyled) set up to deploy for free on GitHub Pages.

## 1. Try it locally (optional but recommended)

You'll need Ruby installed. Then, from this folder:

```bash
gem install bundler
bundle install
bundle exec jekyll serve
```

Visit `http://localhost:4000` to preview the site. Edit files and it'll
live-reload.

If you'd rather skip local setup entirely, you can push straight to
GitHub and let GitHub Pages build it for you (see below). You just
won't get a local preview before publishing.

## 2. Put it on GitHub

```bash
git init
git add .
git commit -m "Initial commit: Marginal Notes starter blog"
git branch -M main
git remote add origin https://github.com/<yourusername>/<your-repo-name>.git
git push -u origin main
```

Create the empty repo on GitHub first (github.com/new) if you haven't
already, using whatever name you'd like for the URL
(`<yourusername>.github.io` if you want it at the root of your GitHub
Pages domain, or any other name for a project site).

## 3. Turn on GitHub Pages

In your repo: **Settings → Pages → Build and deployment → Source**, set
it to "Deploy from a branch," branch `main`, folder `/ (root)`. Save.
GitHub will build and publish the site within a minute or two, at:

- `https://<yourusername>.github.io` if your repo is named
  `<yourusername>.github.io`, or
- `https://<yourusername>.github.io/<repo-name>` otherwise.

## 4. Authorship

Every post now shows a "By Landon" byline automatically (via
`_layouts/post.html`), and the footer (`_includes/footer.html`) has a
copyright line plus your LinkedIn link (GitHub is left out on purpose,
add `github: yourusername` back under `minima.social_links` in
`_config.yml` if you ever want it there). To change the name shown,
either edit those two files directly, or set `author: Landon` in
`_config.yml` (already set). The post layout falls back to that if a
post doesn't set its own `author:` in its front matter.

## 5. Customize

- `_config.yml`: title, description, social links (LinkedIn is set,
  GitHub is intentionally left out), and the `url` / `baseurl` (set
  these once you know your GitHub Pages URL, especially if it's a
  project site with a `/repo-name` path, otherwise links can break).
- `about.md`: your bio and links (LinkedIn and your resume link are
  already wired up).
- `assets/resume.pdf`: your resume, linked from the nav bar ("Resume")
  and the about page. Replace this file whenever you update your
  resume, no other changes needed, the links stay the same.
- `_posts/`: delete the two placeholder posts and start writing your
  own. Jekyll post filenames must follow `YYYY-MM-DD-title.md`.
- `assets/main.scss`: colors/fonts if you want to restyle further.

## 5. Writing a new post

Add a file to `_posts/` named `YYYY-MM-DD-your-title.md`:

```markdown
---
layout: post
title: "Your Title"
date: 2026-10-01 09:00:00 -0700
categories: labor-markets
---

Your post content in Markdown goes here.
```

Commit and push, GitHub Pages rebuilds automatically on every push to
`main`, usually live within a minute or two.

## 6. Custom domain (optional)

If you want `yourdomain.com` instead of the github.io URL: buy a domain,
add a `CNAME` file to this repo's root containing just the domain name,
and point your domain's DNS at GitHub Pages per
[GitHub's custom domain docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

## Later: adding the data layer

When you're ready to pull in live economic data (BLS/FRED via your AWS
pipeline), the simplest path that keeps this as a static site is to have
your pipeline drop a pre-rendered chart image or a small JSON/CSV file
into this repo (e.g. via a scheduled GitHub Action or a step in your AWS
pipeline that commits back to the repo), then reference it from a post
or a dedicated `/data` page. No need to turn this into a dynamic backend
just to show live-ish numbers.
