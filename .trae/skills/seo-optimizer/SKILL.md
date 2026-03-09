---
name: "seo-optimizer"
description: "Optimizes Astro websites for search engines with meta tags, structured data, and pSEO strategies. Invoke when building pages or improving SEO."
---

# SEO Optimizer

## Meta Tags Template
```astro
---
const { 
  title, 
  description, 
  image = '/images/og-default.jpg',
  keywords = ''
} = Astro.props;

const siteName = 'Solar&Co';
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
---

<head>
  <!-- Basic Meta -->
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{title} | {siteName}</title>
  <meta name="description" content={description} />
  <meta name="keywords" content={keywords} />
  <link rel="canonical" href={canonicalURL} />
  
  <!-- Open Graph -->
  <meta property="og:site_name" content={siteName} />
  <meta property="og:title" content={title} />
  <meta property="og:description" content={description} />
  <meta property="og:image" content={image} />
  <meta property="og:type" content="website" />
  <meta property="og:url" content={canonicalURL} />
  
  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content={title} />
  <meta name="twitter:description" content={description} />
  <meta name="twitter:image" content={image} />
  
  <!-- Favicon -->
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
</head>
```

## pSEO Strategy
1. Use dynamic routes: `[city]/[service].astro`
2. Generate 25,000+ pages from data
3. Create sitemap.xml automatically
4. Implement hreflang for i18n

## Dynamic Route Example
```astro
---
// src/pages/[city]/[service].astro
export function getStaticPaths() {
  const cities = ['washington-dc', 'baltimore', 'virginia-beach'];
  const services = ['solar', 'cable', 'internet'];
  
  return cities.flatMap(city => 
    services.map(service => ({
      params: { city, service },
      props: { city, service }
    }))
  );
}

const { city, service } = Astro.props;
---

<Layout 
  title={`${service} services in ${city}`}
  description={`Best ${service} services in ${city}. Contact us today!`}
>
  <!-- Page content -->
</Layout>
```

## Structured Data - LocalBusiness
```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Solar&Co",
  "image": "https://example.com/logo.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "Washington",
    "addressRegion": "DC",
    "postalCode": "20001",
    "addressCountry": "US"
  },
  "telephone": "+1-202-555-0123",
  "url": "https://example.com",
  "openingHours": "Mo-Fr 09:00-18:00"
}
```

## Structured Data - Service
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "Solar Panel Installation",
  "provider": {
    "@type": "LocalBusiness",
    "name": "Solar&Co"
  },
  "areaServed": {
    "@type": "City",
    "name": "Washington DC"
  },
  "description": "Professional solar panel installation services"
}
```

## Hreflang for i18n
```html
<link rel="alternate" hreflang="en" href="https://example.com/en/" />
<link rel="alternate" hreflang="zh" href="https://example.com/zh/" />
<link rel="alternate" hreflang="es" href="https://example.com/es/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/" />
```

## Performance Optimization
- Lazy load images with `loading="lazy"`
- Use `decoding="async"` for images
- Optimize images to < 500KB
- Minimize JavaScript bundles
- Use Astro's partial hydration

## Image SEO
```html
<img 
  src="/images/solar-panel.jpg" 
  alt="Professional solar panel installation on residential rooftop"
  width="800"
  height="600"
  loading="lazy"
  decoding="async"
/>
```

## Internal Linking
- Use descriptive anchor text
- Link to relevant pages
- Maintain logical site structure
- Create topic clusters

## Content Guidelines
- Use H1 for main title (only one per page)
- Use H2-H6 for subheadings
- Include keywords naturally
- Write for humans first, SEO second
- Keep content fresh and updated
