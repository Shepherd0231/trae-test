---
name: "deployment-guide"
description: "Guides deployment of Astro websites to various platforms including Vercel, Netlify, and Cloudflare. Invoke when preparing to deploy or configure CI/CD."
---

# Deployment Guide

## Platform Options

### 1. Vercel (Recommended)

#### Setup
```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel
```

#### Configuration (vercel.json)
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "framework": "astro"
}
```

#### Environment Variables
```bash
vercel env add PUBLIC_API_URL
```

### 2. Netlify

#### Configuration (netlify.toml)
```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

#### Deploy
```bash
# Install Netlify CLI
npm i -g netlify-cli

# Deploy
netlify deploy --prod
```

### 3. Cloudflare Pages

#### Configuration
```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import cloudflare from '@astrojs/cloudflare';

export default defineConfig({
  output: 'server',
  adapter: cloudflare(),
});
```

#### Deploy
- Connect GitHub repo to Cloudflare Pages
- Build command: `npm run build`
- Output directory: `dist`

### 4. GitHub Pages

#### Configuration
```javascript
// astro.config.mjs
export default defineConfig({
  site: 'https://username.github.io',
  base: '/repo-name',
});
```

#### GitHub Actions (.github/workflows/deploy.yml)
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: withastro/action@v2
      - uses: actions/deploy-pages@v3
```

## Pre-Deployment Checklist

### Build Verification
```bash
# Clean build
npm run build

# Preview locally
npm run preview
```

### Performance Checks
- [ ] Lighthouse score > 90
- [ ] All images optimized
- [ ] No console errors
- [ ] All links working

### SEO Verification
- [ ] Meta tags present
- [ ] Sitemap generated
- [ ] Robots.txt configured
- [ ] Canonical URLs correct

### Security
- [ ] HTTPS enabled
- [ ] Security headers set
- [ ] No sensitive data exposed
- [ ] Dependencies updated

## Domain Configuration

### Custom Domain

#### Vercel
1. Add domain in Vercel dashboard
2. Configure DNS records
3. Wait for SSL certificate

#### Netlify
1. Add custom domain in settings
2. Update DNS to point to Netlify
3. Enable HTTPS

### DNS Records
```
Type: A
Name: @
Value: 76.76.21.21 (Vercel)

Type: CNAME
Name: www
Value: cname.vercel-dns.com
```

## Environment Variables

### Local (.env)
```
PUBLIC_API_URL=https://api.example.com
PRIVATE_API_KEY=secret_key
```

### Production
Set in hosting platform dashboard:
- Vercel: Project Settings > Environment Variables
- Netlify: Site Settings > Build & Deploy > Environment
- Cloudflare: Pages > Settings > Environment Variables

## CI/CD Pipeline

### GitHub Actions Example
```yaml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm run build

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - uses: withastro/action@v2
        with:
          node-version: 20
```

## Post-Deployment

### Verification
- [ ] Site loads correctly
- [ ] All pages accessible
- [ ] Forms working
- [ ] Analytics tracking

### Monitoring
- Set up uptime monitoring (UptimeRobot)
- Configure error tracking (Sentry)
- Monitor Core Web Vitals

### Backup
- Regular database backups
- Version control for content
- Asset backup strategy

## Rollback Strategy

### Vercel
```bash
# List deployments
vercel ls

# Rollback to previous
vercel --version <deployment-id>
```

### Netlify
Use "Publish deploy" in deploy list to rollback.

## Troubleshooting

### Build Failures
- Check Node.js version
- Verify environment variables
- Review build logs

### 404 Errors
- Check routing configuration
- Verify base path settings
- Check trailing slashes

### Performance Issues
- Enable CDN caching
- Optimize images
- Check third-party scripts
