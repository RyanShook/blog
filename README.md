# Ryan Shook's Blog

This is the source code for the blog at ryanshook.org/blog, built with Jekyll and hosted on GitHub Pages as a separate repository.

## Blog Features

- **CMS Integration**: Uses Decap CMS (formerly Netlify CMS) for easy content editing
- **Static Site**: Fast, secure, and hosted on GitHub Pages
- **Responsive Design**: Works on all devices
- **SEO Optimized**: Includes meta tags and structured data

## Setup Instructions

### 1. Create & Setup Repository
1. Create a new GitHub repository (e.g., `blog` or `ryanshook-blog`)
2. Push this code to the repository
3. Go to repository Settings > Pages
4. Set source to "Deploy from a branch"
5. Select "main" branch and "/ (root)" folder
6. Set custom domain to `blog.ryanshook.org` OR configure as subdirectory

### 2. Enable Netlify Identity (for CMS)
1. Sign up for [Netlify](https://netlify.com)
2. Connect your GitHub repository
3. Go to Site Settings > Identity
4. Enable Identity
5. Set registration preferences to "Invite only"
6. Enable Git Gateway in Identity settings

### 3. Configure Domain
**Option A: Subdomain (blog.ryanshook.org)**
1. Add CNAME file with `blog.ryanshook.org`
2. Add DNS CNAME record: `blog` → `yourusername.github.io`
3. Enable HTTPS in GitHub Pages settings

**Option B: Subdirectory (ryanshook.org/blog)**
1. No CNAME file needed
2. The `baseurl: "/blog"` in _config.yml handles the path
3. Your blog will be available at the repository's GitHub Pages URL

## Using the CMS

1. Visit `/admin` on your website
2. Login with Netlify Identity
3. Create and edit blog posts through the web interface
4. Changes are automatically committed to your GitHub repository

## Local Development

```bash
bundle install
bundle exec jekyll serve
```

Visit http://localhost:4000/blog to preview your site (note the /blog path).

## File Structure

```
├── _config.yml          # Jekyll configuration
├── _layouts/            # Page layouts
│   ├── default.html     # Base layout
│   └── post.html        # Blog post layout
├── _posts/              # Blog posts (Markdown)
├── admin/               # CMS configuration
│   ├── config.yml       # CMS settings
│   └── index.html       # CMS interface
├── index.html           # Blog home page
└── assets/              # Images and other assets
```

## Writing Posts

Posts are stored in `_posts/` and follow the naming convention:
`YYYY-MM-DD-post-title.md`

Each post should have front matter:
```yaml
---
layout: post
title: "Your Post Title"
date: 2025-08-10
categories: ["blog"]
tags: ["tag1", "tag2"]
excerpt: "Brief description"
---
```