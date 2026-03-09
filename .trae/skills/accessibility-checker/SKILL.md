---
name: "accessibility-checker"
description: "Checks and improves website accessibility for WCAG compliance. Invoke when building components or auditing accessibility."
---

# Accessibility Checker

## WCAG 2.1 Levels
- **Level A**: Basic accessibility
- **Level AA**: Standard compliance (target)
- **Level AAA**: Enhanced accessibility

## Color Contrast

### Requirements
- Normal text: 4.5:1 (AA), 7:1 (AAA)
- Large text: 3:1 (AA), 4.5:1 (AAA)
- UI components: 3:1

### Brand Colors Check
```css
/* Primary Red #EE0000 on White #FFFFFF */
/* Ratio: 5.2:1 ✓ Passes AA */

/* White #FFFFFF on Dark #131313 */
/* Ratio: 15.8:1 ✓ Passes AAA */

/* Gray #666666 on White #FFFFFF */
/* Ratio: 5.7:1 ✓ Passes AA */
```

## Semantic HTML

### Required Elements
```html
<!-- Document structure -->
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Page Title | Site Name</title>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  <header><!-- Site header --></header>
  <nav><!-- Navigation --></nav>
  <main><!-- Main content --></main>
  <aside><!-- Sidebar --></aside>
  <footer><!-- Footer --></footer>
</body>
</html>
```

### Heading Hierarchy
```html
<h1>Page Title (only one)</h1>
  <h2>Section Title</h2>
    <h3>Subsection Title</h3>
  <h2>Another Section</h2>
```

## Images

### Alt Text Guidelines
```html
<!-- Informative images -->
<img src="solar-panel.jpg" alt="Solar panel installation on residential roof">

<!-- Decorative images -->
<img src="decoration.jpg" alt="">

<!-- Complex images -->
<figure>
  <img src="chart.jpg" alt="Bar chart showing energy savings over 5 years">
  <figcaption>Energy savings increased by 40% annually</figcaption>
</figure>
```

## Forms

### Accessible Form Pattern
```html
<form>
  <div class="form-group">
    <label for="email">Email Address *</label>
    <input 
      type="email" 
      id="email" 
      name="email"
      required
      aria-required="true"
      aria-describedby="email-error"
    >
    <span id="email-error" class="error" role="alert"></span>
  </div>
  
  <button type="submit" aria-label="Submit form">
    Submit
  </button>
</form>
```

## Interactive Elements

### Buttons vs Links
```html
<!-- Use button for actions -->
<button onclick="submitForm()">Submit</button>

<!-- Use link for navigation -->
<a href="/about">About Us</a>
```

### Focus Management
```css
/* Visible focus indicator */
:focus-visible {
  outline: 2px solid #EE0000;
  outline-offset: 2px;
}

/* Skip link */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #EE0000;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

## ARIA Labels

### When to Use
```html
<!-- Icon buttons -->
<button aria-label="Close menu">
  <svg><!-- X icon --></svg>
</button>

<!-- Complex components -->
<nav aria-label="Main navigation">
  <!-- Navigation items -->
</nav>

<!-- Live regions -->
<div aria-live="polite" aria-atomic="true">
  <!-- Dynamic content updates -->
</div>
```

## Keyboard Navigation

### Focusable Elements
All interactive elements must be keyboard accessible:
- Links (`<a>`)
- Buttons (`<button>`)
- Form inputs
- Custom components (add tabindex)

### Tab Order
```html
<!-- Logical tab order -->
<a href="#">First</a>
<button>Second</button>
<input type="text"> <!-- Third -->
```

## Testing Tools

### Automated
- Lighthouse accessibility audit
- axe DevTools
- WAVE browser extension

### Manual
- Keyboard navigation test
- Screen reader test (NVDA, JAWS, VoiceOver)
- Color contrast analyzer
- Zoom to 200%

## Checklist

### Perceivable
- [ ] Text alternatives for images
- [ ] Captions for videos
- [ ] Color not sole means of conveying info
- [ ] Resizable text up to 200%

### Operable
- [ ] Keyboard accessible
- [ ] No keyboard traps
- [ ] Skip links provided
- [ ] Focus indicators visible

### Understandable
- [ ] Language specified
- [ ] Consistent navigation
- [ ] Error messages clear
- [ ] Input labels associated

### Robust
- [ ] Valid HTML
- [ ] ARIA used correctly
- [ ] Compatible with assistive tech
