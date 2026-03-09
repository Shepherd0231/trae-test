---
name: "code-reviewer"
description: "Reviews Astro and TypeScript code for best practices, performance, and accessibility. Invoke when user asks for code review or before merging changes."
---

# Code Reviewer

## When to Use
- Before committing code
- When refactoring components
- After adding new features
- When optimizing performance

## Review Checklist

### Astro Best Practices
- [ ] Props interface is properly defined
- [ ] TypeScript types are used correctly
- [ ] Slots are used appropriately
- [ ] Client directives are used when needed
- [ ] No unnecessary JavaScript in static components

### Performance
- [ ] Images are optimized and lazy-loaded
- [ ] No unnecessary re-renders
- [ ] CSS is properly scoped
- [ ] No large bundles imported

### Accessibility
- [ ] Semantic HTML elements used
- [ ] Alt text for images
- [ ] ARIA labels for interactive elements
- [ ] Keyboard navigation support
- [ ] Color contrast ratios met

### Code Quality
- [ ] Consistent naming conventions
- [ ] No console.log statements
- [ ] Proper error handling
- [ ] Comments for complex logic

## Common Issues

### Issue: Missing TypeScript Types
```typescript
// Bad
const props = Astro.props;

// Good
export interface Props {
  title: string;
}
const { title } = Astro.props as Props;
```

### Issue: Unoptimized Images
```astro
<!-- Bad -->
<img src="large-image.jpg" />

<!-- Good -->
<img 
  src="optimized-image.jpg" 
  alt="Descriptive text"
  width="800"
  height="600"
  loading="lazy"
/>
```

### Issue: Missing Accessibility
```astro
<!-- Bad -->
<div onclick={handleClick}>Click me</div>

<!-- Good -->
<button onclick={handleClick} aria-label="Perform action">
  Click me
</button>
```
