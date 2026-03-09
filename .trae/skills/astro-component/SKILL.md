---
name: "astro-component"
description: "Generates Astro components with best practices, TypeScript support, and Tailwind styling. Invoke when user needs to create new Astro components or pages."
---

# Astro Component Generator

## When to Use
- Creating new Astro components
- Building page layouts
- Adding interactive elements
- Implementing pSEO patterns

## Best Practices
1. Use TypeScript interfaces for Props
2. Implement semantic HTML5 structure
3. Add Tailwind CSS classes for styling
4. Include proper accessibility attributes
5. Support dark mode if applicable
6. Add responsive design breakpoints

## Component Template
```astro
---
export interface Props {
  title?: string;
  className?: string;
}

const { title = 'Default Title', className = '' } = Astro.props;
---

<section class={`w-full ${className}`}>
  <h2 class="text-2xl font-bold">{title}</h2>
  <slot />
</section>
```

## pSEO Support
- Always use interface Props for content
- Support bulk page generation
- Include proper meta tags
- Add structured data when relevant

## File Naming Conventions
- Components: PascalCase (e.g., `Hero.astro`, `Header.astro`)
- Pages: lowercase with hyphens (e.g., `about.astro`, `contact.astro`)
- Layouts: PascalCase with Layout suffix (e.g., `Layout.astro`)

## Props Interface Pattern
```typescript
export interface Props {
  // Required props (no default)
  id: string;
  
  // Optional props with defaults
  title?: string;
  description?: string;
  image?: string;
  className?: string;
}

const {
  id,
  title = 'Default Title',
  description = '',
  image = '/images/default.jpg',
  className = ''
} = Astro.props;
```

## Responsive Design Pattern
```html
<!-- Mobile-first approach -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6 lg:gap-8">
  <!-- Content -->
</div>

<!-- Typography scaling -->
<h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl">
  Responsive Heading
</h1>
```

## Slot Usage
```astro
<!-- Component with slots -->
<div class="card">
  <div class="card-header">
    <slot name="header" />
  </div>
  <div class="card-body">
    <slot />
  </div>
  <div class="card-footer">
    <slot name="footer" />
  </div>
</div>
```

## Accessibility Requirements
- Use semantic HTML elements (nav, main, section, article, aside)
- Add aria-labels for interactive elements
- Include alt text for images
- Ensure keyboard navigation support
- Maintain color contrast ratios

## R2 Image Component Pattern

### ResponsiveImage Component
```astro
---
// components/ResponsiveImage.astro
export interface Props {
  src: string;
  alt: string;
  className?: string;
  baseUrl?: string;
}

const { 
  src, 
  alt, 
  className = '',
  baseUrl = import.meta.env.R2_PUBLIC_URL || ''
} = Astro.props;

// Remove file extension for base name
const baseName = src.replace(/\.[^/.]+$/, '');
const fileExt = src.match(/\.[^/.]+$/)?.[0] || '.jpg';
---

<picture>
  <!-- Desktop -->
  <source 
    media="(min-width: 1024px)" 
    srcset={`${baseUrl}${baseName}-desktop${fileExt}`}
    type={`image/${fileExt.replace('.', '')}`}
  />
  <!-- Tablet -->
  <source 
    media="(min-width: 768px)" 
    srcset={`${baseUrl}${baseName}-tablet${fileExt}`}
    type={`image/${fileExt.replace('.', '')}`}
  />
  <!-- Mobile (fallback) -->
  <img 
    src={`${baseUrl}${baseName}-mobile${fileExt}`}
    alt={alt}
    class={className}
    loading="lazy"
    decoding="async"
    width="640"
    height="360"
  />
</picture>
```

### Usage Example
```astro
---
import ResponsiveImage from './ResponsiveImage.astro';
---

<ResponsiveImage 
  src="/hero-solar.jpg" 
  alt="Solar panel installation"
  className="w-full h-auto rounded-2xl"
/>
```

### Environment Variables (.env)
```
R2_PUBLIC_URL=https://pub-xxx.r2.dev
```

### Image Requirements
- Always provide three sizes: `-mobile`, `-tablet`, `-desktop`
- Use lazy loading for below-fold images
- Include width/height to prevent layout shift
- Use descriptive alt text for accessibility
