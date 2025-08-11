# Ryan Shook's Blog

A modern, professional blog built with Jekyll, Chirpy theme, and Sveltia CMS. Hosted for free on GitHub Pages with a clean content management interface.

**🌐 Live Blog**: [ryanshook.github.io/blog](https://ryanshook.github.io/blog)  
**✍️ CMS Admin**: [ryanshook.github.io/blog/admin](https://ryanshook.github.io/blog/admin)

## Tech Stack

- **Static Site Generator**: Jekyll with Chirpy theme
- **Hosting**: GitHub Pages (free)
- **Content Management**: Sveltia CMS (modern successor to Netlify CMS)
- **Authentication**: Cloudflare Workers (free tier)
- **Build Process**: GitHub Actions
- **Cost**: $0/month

## Features

- 🎨 **Modern Design**: Clean, professional Chirpy theme with dark/light mode
- 📱 **Mobile Editing**: Write and edit posts from any device
- 🔍 **Built-in Search**: Full-text search across all posts
- 📊 **Analytics Ready**: SEO optimized with structured data
- ⚡ **Fast Performance**: Static site generation for lightning speed
- 🔒 **Secure**: Git-based workflow with OAuth authentication
- 🎯 **Developer Friendly**: Syntax highlighting, ToC generation, PWA support

## Quick Start

### For Content Creators
1. Visit [ryanshook.github.io/blog/admin](https://ryanshook.github.io/blog/admin)
2. Login with GitHub
3. Create and edit posts through the web interface
4. Posts automatically save and publish

### For Developers
```bash
# Clone and setup
git clone https://github.com/RyanShook/blog.git
cd blog
bundle install

# Local development
bundle exec jekyll serve
# Visit http://localhost:4000/blog
```

## Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Sveltia CMS   │───▶│ Cloudflare Worker │───▶│ GitHub OAuth    │
│   (Web Editor)  │    │   (Auth Handler)  │    │ (Authentication)│
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│ GitHub Repo     │───▶│ GitHub Actions   │───▶│ GitHub Pages    │
│ (Blog Content)  │    │   (Jekyll Build) │    │   (Live Blog)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

## Content Management

### Writing Workflow
1. **Access CMS**: Visit `/admin` on the blog
2. **Authenticate**: One-click GitHub OAuth login
3. **Create/Edit**: Rich text editor with Markdown support
4. **Publish**: Changes automatically commit to repository
5. **Deploy**: GitHub Actions rebuilds site (2-3 minutes)

### Post Format
Posts use Jekyll front matter with Chirpy theme fields:

```yaml
---
title: Your Post Title
date: 2025-08-11 12:00:00 -0600
categories: [technology, blogging]
tags: [jekyll, tutorial]
author: ryan
---

Your post content in Markdown...
```

## File Structure

```
├── _config.yml          # Jekyll + Chirpy configuration
├── _layouts/            # Custom layout overrides
├── _posts/              # Blog posts (Markdown files)
├── _tabs/               # Navigation pages (About, Categories, etc.)
├── admin/               # Sveltia CMS configuration
├── assets/              # Images, CSS, and other assets
├── .github/workflows/   # GitHub Actions for deployment
└── index.html           # Blog home page
```

## Customizations

- **Custom Footer**: Social media icons (X/Twitter, Facebook, LinkedIn, Instagram)
- **Clean Branding**: Removed theme attribution for professional appearance
- **Custom Avatar**: Personal photo integration
- **Responsive Design**: Mobile-optimized spacing and layout
- **Performance Optimized**: Custom CSS and build process

## Development

### Prerequisites
- Ruby 3.0+
- Jekyll 4.3+
- Bundler

### Setup
```bash
# Install dependencies
bundle install

# Serve locally
bundle exec jekyll serve --livereload

# Build for production
JEKYLL_ENV=production bundle exec jekyll build
```

### Deployment
Automatic deployment via GitHub Actions when pushing to `main` branch.

## Authentication Setup

The blog uses Cloudflare Workers for CMS authentication:

1. **GitHub OAuth App**: Handles user authentication
2. **Cloudflare Worker**: Processes OAuth flow securely
3. **Sveltia CMS**: Connects to GitHub via authenticated API

See the [setup blog post](https://ryanshook.github.io/blog/posts/how-i-built-a-free-blog-with-jekyll-github-pages-and-sveltia-cms/) for detailed implementation.

## Contributing

This is a personal blog, but feel free to:
- Report issues
- Suggest improvements
- Use as a template for your own blog

## License

Content: All rights reserved  
Code: MIT License

## Resources

- **Jekyll**: [jekyllrb.com](https://jekyllrb.com)
- **Chirpy Theme**: [github.com/cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)
- **Sveltia CMS**: [github.com/sveltia/sveltia-cms](https://github.com/sveltia/sveltia-cms)
- **GitHub Pages**: [pages.github.com](https://pages.github.com)