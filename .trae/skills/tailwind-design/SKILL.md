---
name: "tailwind-design"
description: "Applies consistent Tailwind CSS design patterns, color schemes, and responsive layouts. Invoke when styling components or creating design systems."
---

# Tailwind Design System

## Brand Colors
- Primary Red: `#EE0000`
- Dark Background: `#131313`
- Light Background: `#F8F8F8`
- Text Primary: `#131313`
- Text Secondary: `#666666`
- White: `#FFFFFF`

## Typography
- Headings: Poppins, bold
- Body: Roboto, regular
- Small: 12-14px
- Base: 16px
- Large: 18-20px
- XL: 24-32px
- 2XL: 36-48px
- 3XL: 60-72px

## Spacing Scale
- xs: 4px (1)
- sm: 8px (2)
- md: 16px (4)
- lg: 24px (6)
- xl: 32px (8)
- 2xl: 48px (12)
- 3xl: 64px (16)

## Responsive Breakpoints
- sm: 640px
- md: 768px
- lg: 1024px
- xl: 1280px
- 2xl: 1536px

## Common Patterns

### Card Pattern
```html
<div class="bg-white rounded-2xl p-6 shadow-lg hover:shadow-xl transition-shadow duration-300">
  <!-- Card content -->
</div>
```

### Button Primary
```html
<button class="px-6 py-3 bg-[#EE0000] text-white rounded-full hover:bg-[#CC0000] transition-colors duration-300">
  Get Started
</button>
```

### Button Secondary
```html
<button class="px-6 py-3 border-2 border-[#EE0000] text-[#EE0000] rounded-full hover:bg-[#EE0000] hover:text-white transition-all duration-300">
  Learn More
</button>
```

### Container
```html
<div class="max-w-[90rem] mx-auto px-6 lg:px-9">
  <!-- Content -->
</div>
```

### Section Spacing
```html
<section class="py-20 lg:py-28">
  <!-- Section content -->
</section>
```

## Animation Classes
- `transition-all duration-300` - Smooth transitions
- `hover:scale-105` - Scale on hover
- `animate-pulse` - Pulsing animation
- `animate-bounce` - Bouncing animation
- `animate-slow-zoom` - Slow zoom effect

## Grid Patterns

### 2-Column Grid
```html
<div class="grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-12">
  <!-- Columns -->
</div>
```

### 3-Column Grid
```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 lg:gap-8">
  <!-- Columns -->
</div>
```

### 12-Column Grid
```html
<div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
  <div class="lg:col-span-7"><!-- Main content --></div>
  <div class="lg:col-span-5"><!-- Sidebar --></div>
</div>
```

## Dark Theme Patterns
```html
<!-- Dark section -->
<section class="bg-[#131313] text-white">
  <div class="text-white/60">Secondary text</div>
  <div class="text-white/80">Primary text</div>
</section>
```

## Glass Morphism
```html
<div class="bg-white/10 backdrop-blur-sm border border-white/10 rounded-xl">
  <!-- Glass content -->
</div>
```

## Gradient Overlays
```html
<!-- Left to right gradient -->
<div class="bg-gradient-to-r from-black via-black/80 to-transparent"></div>

<!-- Top to bottom gradient -->
<div class="bg-gradient-to-t from-black via-transparent to-transparent"></div>
```
