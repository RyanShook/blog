# Quick Setup Guide - Chirpy Theme

## What You Have

A complete Jekyll blog using the **Chirpy theme** with CMS that can be deployed to `ryanshook.org/blog` exactly like your tanks-game!

## Chirpy Theme Features:
- **Modern Design**: Clean, responsive layout with dark/light mode toggle
- **Technical Writing Focused**: Syntax highlighting, math expressions, diagrams
- **Built-in Features**: Search, TOC, PWA support, SEO optimized
- **Navigation**: Categories, tags, archives, about pages
- **Professional**: Perfect for tech blogging and storytelling

## Files Created:
- **Jekyll + Chirpy**: All blog functionality with professional theme
- **CMS Interface**: Web editor at `/admin` configured for Chirpy post format
- **Navigation**: Categories, Tags, Archives, About pages
- **Sample Content**: Welcome post in Chirpy format
- **Configuration**: Set up for `/blog` path with Chirpy theme

## Next Steps:

### 1. Create Repository
```bash
cd blog-repo
git init
git add .
git commit -m "Initial Chirpy blog setup with Decap CMS"
git remote add origin https://github.com/RyanShook/blog.git  # or your preferred repo name
git push -u origin main
```

### 2. Enable GitHub Pages
1. Go to your new repository settings
2. Pages section → Deploy from branch → main branch
3. Your blog will be live at `https://yourusername.github.io/blog`

### 3. Set Up CMS (Netlify)
1. Connect your new repo to Netlify
2. Enable Identity service
3. Enable Git Gateway
4. Visit `yourblog.com/admin` to start editing

### 4. Customize Your Blog
- **Avatar**: Add your photo to `assets/img/avatar.jpg`
- **Favicons**: Add favicons to `assets/img/favicons/`
- **Social Links**: Update in `_config.yml`
- **About Page**: Edit `_tabs/about.md`
- **Google Analytics**: Add your ID in `_config.yml`

## Chirpy Post Format:
Posts use this front matter format:
```yaml
---
title: Your Post Title
date: 2025-08-10 10:00:00 -0600
categories: [technology, programming]
tags: [jekyll, blogging]
author: ryan
pin: false  # Pin to top of blog
image:
  path: /assets/img/posts/your-image.jpg
  alt: Image description
---
```

## How It Works:
- **Write**: Use CMS at `/admin` or edit Markdown files directly
- **Chirpy Features**: Automatic TOC, syntax highlighting, search, dark mode
- **Publish**: Commits automatically trigger GitHub Pages rebuild
- **Access**: Blog available at your configured domain + `/blog`

This gives you a professional, feature-rich blog that mirrors your tanks-game setup pattern!