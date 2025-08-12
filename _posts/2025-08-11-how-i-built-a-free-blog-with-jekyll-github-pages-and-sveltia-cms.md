---
title: How I Built a Free Blog with Jekyll, GitHub Pages, and Sveltia CMS
date: 2025-08-11 07:10:00 -05:00
categories:
  - technology
  - blogging
tags:
  - jekyll
  - github-pages
  - sveltia-cms
  - cloudflare
  - tutorial
author: Ryan Shook
---
![Blog Ingredients](/blog/assets/img/posts/blog.png)

I recently created this blog that costs me nothing to run and allows me to edit and publish without editing code. It’s fast, customizable, and has a clean editing interface — all without monthly hosting fees. Here’s the exact setup and process I used.

**The Stack**

- **Website:** Jekyll + Chirpy theme
- **Hosting:** GitHub Pages (free)
- **CMS:** Sveltia CMS (successor to Netlify CMS)
- **Authentication:** Cloudflare Workers (free tier)
- **Domain:** Namecheap
- **Cost:** $0/month

## **Why This Setup Works for Me**

- **No ongoing costs** – Uses free service tiers.
- **Full control** – All content and code are mine.
- **User-friendly editing** – CMS works on desktop and mobile.
- **Fast loading** – Static site generation ensures performance.
- **Clean design** – Chirpy theme provides a professional look.
- **Version control** – All content is tracked in Git.

## **The Setup Process**

### 1. Jekyll + Chirpy Theme

Jekyll powers the blog and the Chirpy theme adds:

- Light/dark mode toggle
- Search functionality
- Table of contents
- Syntax highlighting
- SEO optimization
- Progressive Web App support

### 2. GitHub Pages Hosting

Steps:

1. Create a GitHub repository
2. Enable **GitHub Pages**
3. Use **GitHub Actions** for Jekyll builds

### 3. Sveltia CMS for Content Editing

Sveltia CMS provides:

- Clean interface
- Mobile-friendly editing
- Rich text + Markdown support
- Image management
- Git-based commits directly to the repo

### 4. Authentication with Cloudflare Workers

A simple Cloudflare Worker handles GitHub OAuth authentication and secure token exchange. It’s free and lightweight.

### 5. Custom Adjustments

I added:

- Footer with social links
- Small design tweaks
- Branded avatar

## **My Blogging Workflow**

1. Visit `/admin` on my blog
2. Log in with GitHub
3. Write or edit in the CMS
4. Save — commits to GitHub
5. GitHub Actions rebuilds and deploys the site

## **What I Learned**

- GitHub Actions make Jekyll much more flexible.
- Static sites are fast and low-maintenance.
- Free-tier services can work together for a professional result.

## **Final Thoughts**

If you have some technical comfort and want a blog that’s fast, reliable, and free to run, this setup is worth considering. For those less technical, managed platforms like WordPress.com or Ghost may be simpler but come with costs.

**Resources:**

- [Jekyll](https://jekyllrb.com/)
- [Chirpy Theme](https://github.com/cotes2020/jekyll-theme-chirpy)
- [Sveltia CMS](https://github.com/sveltia/sveltia-cms)
- [GitHub Pages](https://pages.github.com/)
- [Cloudflare Workers](https://workers.cloudflare.com/)
