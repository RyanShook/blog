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
![](/blog/assets/img/posts/image.png)I recently set up this blog using a completely free tech stack that gives me a professional, modern blogging platform without any ongoing costs. Here's how I built a blog with Jekyll, GitHub Pages, and a clean CMS interface - all hosted for free.

## The Tech Stack I'm Using

**Website**: Jekyll with Chirpy theme  
**Hosting**: GitHub Pages (free)  
**CMS**: Sveltia CMS (successor to Netlify CMS)  
**Authentication**: Cloudflare Workers (free tier)  
**Domain**: Custom domain support  
**Cost**: $0/month

## Why This Stack?

After researching various blogging platforms, I chose this setup because:

- **Zero ongoing costs** - Everything runs on free tiers
- **Full control** - I own my content and can customize everything
- **Modern CMS** - Easy post editing from any device, including mobile
- **Fast performance** - Static site generation means lightning-fast loading
- **Professional appearance** - Chirpy theme provides a clean, technical blog look
- **Git-based** - All content is version controlled and backed up

## The Setup Process

### 1. Blog Foundation: Jekyll + Chirpy Theme

I started with Jekyll, a static site generator that GitHub Pages supports natively. Instead of a basic Jekyll setup, I chose the **Chirpy theme** which provides:

- Clean, professional design with dark/light mode toggle
- Built-in search functionality
- Table of contents generation
- Syntax highlighting for code
- Categories and tags organization
- SEO optimization
- PWA (Progressive Web App) support

The theme is specifically designed for technical writing and blogging, making it perfect for sharing thoughts on technology and development.

### 2. Free Hosting: GitHub Pages

GitHub Pages provides free static site hosting directly from your GitHub repository. The setup is straightforward:

- Create a repository for your blog
- Enable GitHub Pages in repository settings
- Choose "GitHub Actions" as the source (required for Chirpy theme)
- Your blog is automatically deployed whenever you push changes

The custom GitHub Actions workflow handles the Jekyll build process, including all the Chirpy theme's advanced features that aren't supported by GitHub's default Jekyll build.

### 3. Content Management: Sveltia CMS

The biggest challenge was finding a user-friendly way to create and edit blog posts. I wanted a CMS interface rather than editing Markdown files directly. After trying several options, I landed on **Sveltia CMS**.

Sveltia CMS is a modern successor to Netlify CMS, offering:

- **Clean, intuitive interface** - Better UX than its predecessors
- **Mobile editing** - I can write posts from my phone
- **Rich text editor** with Markdown support
- **Media management** for images and files
- **Git-based** - All changes are committed to your repository
- **No vendor lock-in** - Your content stays in your repository

### 4. Authentication: Cloudflare Workers

The trickiest part was authentication. Most CMS solutions require a backend service, but I wanted to stay in the "free tier" ecosystem. The solution was **Cloudflare Workers**.

I deployed a simple OAuth handler to Cloudflare Workers (free tier) that:

- Handles GitHub OAuth authentication
- Provides secure token exchange
- Enables the CMS to authenticate users and commit to GitHub
- Runs entirely on Cloudflare's free tier (100,000 requests/day)

### 5. Custom Styling and Features

To make the blog my own, I added:

- **Custom footer** with social media icons (Twitter/X, Facebook, LinkedIn, Instagram)
- **Removed theme attribution** for a cleaner look
- **Custom avatar** from my main website
- **Responsive design** adjustments

## The Workflow

My current blogging workflow is incredibly smooth:

1. **Writing**: Open `ryanshook.github.io/blog/admin` on any device
2. **Authentication**: One-click login with GitHub
3. **Editing**: Rich text editor with live preview
4. **Publishing**: Hit save - changes automatically commit to GitHub
5. **Deployment**: GitHub Actions rebuilds and deploys the site (2-3 minutes)

I can write posts from my laptop, phone, or any device with a web browser.

## What I Learned

### GitHub Pages + Custom Themes

GitHub Pages' default Jekyll support is limited. Using GitHub Actions for deployment unlocks full Jekyll functionality and theme support.

### CMS Authentication Complexity

Most headless CMS solutions assume you're using their hosting platform. Setting up authentication for a GitHub Pages site required creativity - the Cloudflare Workers approach turned out to be the cleanest solution.

### The Power of Static Sites

The performance and reliability of a static site is remarkable. No server maintenance, no security updates, no downtime concerns.

## Costs and Limitations

**Ongoing costs**: $0/month  
**Setup time**: \~2 hours for someone familiar with Git/GitHub  
**Limitations**: 

- No server-side functionality (but rarely needed for a blog)
- GitHub Pages has bandwidth limits (generous for most blogs)
- Requires some technical knowledge for initial setup

## Would I Recommend This Stack?

Absolutely, if you:

- Want full control over your blog
- Prefer writing in Markdown
- Don't mind a bit of initial technical setup
- Value performance and reliability
- Want to avoid ongoing subscription costs

For non-technical users, a hosted solution like WordPress.com or Ghost might be easier, but you'll pay monthly fees and have less control.

## The Result

I now have a professional blog that:

- Loads instantly
- Looks great on all devices
- Has a modern CMS interface
- Costs nothing to maintain
- Is fully under my control
- Can handle traffic spikes effortlessly

The combination of Jekyll's static site generation, GitHub's reliable hosting, and Sveltia's modern CMS interface creates a blogging platform that rivals paid solutions - all while keeping my content in my own Git repository.

## Resources

If you want to set up something similar:

- **Jekyll**: [jekyllrb.com](https://jekyllrb.com)
- **Chirpy Theme**: [github.com/cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)
- **Sveltia CMS**: [github.com/sveltia/sveltia-cms](https://github.com/sveltia/sveltia-cms)
- **GitHub Pages**: [pages.github.com](https://pages.github.com)
- **Cloudflare Workers**: [workers.cloudflare.com](https://workers.cloudflare.com)

The entire setup process took about 2 hours and resulted in a blog that's both powerful and completely free to operate.
