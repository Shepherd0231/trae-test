---
name: "performance-optimizer"
description: "Optimizes Astro website performance including image optimization, bundle size, and Core Web Vitals. Invoke when improving site speed or Lighthouse scores."
---

# Performance Optimizer

## Core Web Vitals Targets
- **LCP** (Largest Contentful Paint): < 2.5s
- **FID** (First Input Delay): < 100ms
- **CLS** (Cumulative Layout Shift): < 0.1
- **FCP** (First Contentful Paint): < 1.8s
- **TTFB** (Time to First Byte): < 600ms

## Image Optimization

### Astro Image Component
```astro
---
import { Image } from 'astro:assets';
import heroImage from '../assets/hero.jpg';
---

<Image 
  src={heroImage} 
  alt="Hero image"
  width={1920}
  height={1080}
  quality={80}
  format="webp"
/>
```

### Manual Optimization
- Use WebP format when possible
- Implement responsive images with srcset
- Lazy load below-the-fold images
- Use appropriate image sizes

## Bundle Optimization

### Code Splitting
```astro
<!-- Only load on client -->
<script>
  const module = await import('./heavy-module.js');
</script>
```

### Dynamic Imports
```astro
---
const HeavyComponent = await import('../components/Heavy.svelte');
---
```

## Font Optimization

### Font Display
```css
@font-face {
  font-family: 'Poppins';
  src: url('/fonts/poppins.woff2') format('woff2');
  font-display: swap; /* Prevent FOIT */
}
```

### Preload Critical Fonts
```html
<link rel="preload" href="/fonts/poppins-bold.woff2" as="font" type="font/woff2" crossorigin>
```

## Caching Strategy

### Static Assets
```javascript
// astro.config.mjs
export default defineConfig({
  vite: {
    build: {
      rollupOptions: {
        output: {
          assetFileNames: 'assets/[name]-[hash][extname]'
        }
      }
    }
  }
});
```

## Performance Testing

### Lighthouse CI
```bash
npm install -g @lhci/cli
lhci autorun
```

### Web Vitals Extension
Install Chrome extension for real-time monitoring.

## Common Optimizations

### 1. Reduce JavaScript
- Use Astro's zero-JS by default
- Only hydrate interactive components
- Use `client:visible` for below-fold components

### 2. Optimize CSS
- Purge unused Tailwind classes
- Inline critical CSS
- Use CSS containment

### 3. Preload Critical Resources
```html
<link rel="preload" href="/styles/critical.css" as="style">
<link rel="preload" href="/fonts/main.woff2" as="font">
```

### 4. Use CDN
- Host images on CDN
- Use edge functions for dynamic content
