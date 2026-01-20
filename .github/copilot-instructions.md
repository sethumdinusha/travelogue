# Travelogue - AI Agent Instructions

## Project Overview
**Travelogue** is a static travel blog website showcasing adventure stories from Sri Lanka. Built with Bootstrap 5 for responsive design, it features image galleries, carousels, and smooth scroll animations. Three main pages document different mountain peaks (landing, Namunukula, Narangala).

## Architecture & Key Components

### Multi-Page Structure
- **index.html** - Main landing page with dark theme (`data-bs-theme="dark"`), hero section, galleries
- **namunukula.html** - Destination detail page (light theme)
- **narangala.html** - Destination detail page (light theme)
- Theme control: Use `data-bs-theme="dark"` or `data-bss-forced-theme="light"` attributes

### Asset Organization
- `assets/bootstrap/` - Bootstrap 5 framework (CSS/JS minified)
- `assets/css/` - Custom styles: `bss-overrides.css` (spacing utilities), animation classes, responsive fixes
- `assets/fonts/` - Font Awesome icons (two versions: `font-awesome.min.css`, `fontawesome-all.min.css`)
- `assets/img/` - 40+ travel photography assets (hero backgrounds, gallery images)
- `assets/js/` - Gallery/carousel scripts + `grayscale.js` (navbar collapse on scroll)

## Critical Patterns & Conventions

### 1. Responsive Typography with Clamp()
Use CSS `clamp()` for fluid, responsive fonts that scale smoothly:
```css
.brand-heading { font-size: clamp(2.5em, 8vw, 4em); } /* Min: 2.5em, Preferred: 8vw, Max: 4em */
.text-focus p { font-size: clamp(0.95em, 2.5vw, 1.2em); }
```
This eliminates need for multiple media query font sizes. Works across all devices from 320px to 4K displays.

### 2. Custom CSS Utilities (bss-overrides.css)
Bootstrap overrides using `!important` for spacing/sizing. When adding styles:
- Use naming: `mt-6`, `mb-5`, `pt-10` (margin-top, margin-bottom, padding-top)
- Always use `!important` to override Bootstrap defaults
- Mobile breakpoints: `@media (max-width: 768px)` for tablets, `(max-width: 576px)` for phones

### 3. Animation Framework
Reusable scroll-triggered animations in `<style>` blocks:
```css
/* Fade-in on scroll */
.scroll-reveal { opacity: 0; transform: translateY(40px); }
.scroll-reveal.visible { opacity: 1; transform: translateY(0); }

/* Card hover effects */
.card:hover { transform: translateY(-10px); box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4); }
```
Apply via HTML classes, not separate files.

### 4. Responsive Spacing with Clamp()
Replace fixed padding/margins with clamp for smooth scaling:
```css
.text-focus { padding: clamp(1.5rem, 5vw, 2.5rem); }
.content-section { padding: clamp(3rem, 8vw, 6rem) 0; }
```
Avoids "jumps" between breakpoints - spacing adapts continuously.

### 5. Flexbox for Cards & Layouts
Cards use `display: flex` with `flex-direction: column` and `flex-grow: 1` on `.card-body`:
```html
<div class="card"><img class="card-img-top w-100">
  <div class="card-body p-3 p-md-4">
    <h4 class="card-title">Title</h4>
    <p>Description</p>
    <a class="btn mt-auto">Button</a> <!-- mt-auto pushes to bottom -->
  </div>
</div>
```
Buttons naturally stick to bottom regardless of content height.

### 6. Hero Section & Mastheads
Uses flexbox centering for all screen sizes:
```html
<header class="masthead" style="min-height: 100vh;display: flex;align-items: center;justify-content: center;">
  <div class="intro-body w-100">
    <div class="container px-3"> <!-- px-3 adds responsive padding -->
```
- Remove all fixed padding/margin - use container padding and clamp()
- Hero image stretches to viewport height on all devices

### 7. Mobile-First Container Padding
All containers use responsive padding via classes:
- `px-3` on desktop (1rem padding each side)
- Adjusted in media queries: `@media (max-width: 576px) { .container { padding-left: 0.75rem; } }`
- Avoid hardcoded margins between sections - use content-section padding only

### 8. Touch-Friendly Button Sizing
Buttons maintain minimum 44px height for mobile (WCAG standard):
```css
.btn-default {
  min-height: 44px;
  padding: clamp(0.5rem, 1vw, 0.75rem) clamp(1rem, 2vw, 1.5rem);
}
```
Applies to all interactive elements - links, buttons, nav items.

### 9. Navigation Shrinking Behavior
`grayscale.js` handles navbar scroll behavior:
- Adds `navbar-shrink` class when `scrollTop > 100px`
- Collapses responsive menu when nav links clicked
- Applied to `#mainNav` element
- Use `background-color: rgba(0, 0, 0, 0.7)` for semi-transparent nav

### 10. Media Gallery Implementations
- **Lightbox Gallery** - `data-bss-baguettebox` attribute triggers baguetteBox lightbox
- **Carousel/Slider** - Basic Bootstrap carousel with custom CSS
- **FancyBox Gallery** - jQuery-based lightbox for advanced galleries
- Gallery classes embedded in `<style>` blocks within each page

## File Organization Workflow

### Adding New Pages
1. Copy existing `.html` file as template
2. Preserve theme attribute: `data-bs-theme="dark|light"` and `data-bss-forced-theme` if needed
3. Link all external assets: Bootstrap CSS/JS, Google Fonts, CDN libraries (FancyBox, BaguetteBox)
4. Embed animations in `<style>` block at top (don't create separate CSS files)
5. Place hero background image in `assets/img/`, reference by name

### Adding Images
- Store in `assets/img/`
- Hero backgrounds: use descriptive names like `hero-background-[topic].jpg`
- Gallery images: numbered sequentially (`1.jpg`, `12.jpg`, `IMG_6379.jpg`, etc.)
- Compress before adding to repo

### CSS Updates
- Modify `bss-overrides.css` for global utilities only
- Page-specific styles go in `<style>` blocks within HTML (minimizes HTTP requests)
- Always use `!important` for Bootstrap overrides
- Test across breakpoints immediately

## External Dependencies
- **Bootstrap 5**: `assets/bootstrap/css/bootstrap.min.css` + JS
- **Google Fonts**: Lora, Cabin, Aclonica, Agbalumo, Aldrich, Almendra SC, Amita (loaded via CDN)
- **Font Awesome**: `assets/fonts/font-awesome.min.css` or `fontawesome-all.min.css`
- **FancyBox v2.1.5**: CDN link for image lightbox
- **BaguetteBox**: Minified JS in `assets/js/Lightbox-Gallery-baguetteBox.min.js`
- **jQuery**: Assumed loaded for FancyBox (verify in script order)

## Common Tasks

### Fix Mobile Layout Issues
1. **Test responsive breakpoints**: 576px (mobile), 768px (tablet), 1200px+ (desktop)
2. **Use clamp() for typography** instead of multiple font-size rules in media queries
3. **Check container padding**: Use `px-3` class or clamp padding, never hardcode to 0
4. **Verify button heights**: All buttons must be minimum 44px height for touch devices
5. **Test hero section**: Ensure image fills viewport and text is readable on all sizes
6. **Use DevTools**: Test with device emulation across all breakpoints

### Add Animation to Elements
1. Define keyframes + default/hover states in `<style>` block
2. Apply class to HTML element (e.g., `class="scroll-reveal"`)
3. Use JS to toggle `visible` class on scroll (see scroll-reveal pattern)
4. Ensure `transition` property matches animation timing
5. Test on mobile: animations may feel slow on low-end devices

### Update Gallery
1. Add images to `assets/img/`
2. Use `data-bss-baguettebox` wrapper for automatic lightbox
3. Test image load performance (large travel photos can be slow)
4. Optimize images: target <500KB each for web performance

### Add Responsive Padding/Spacing
- **Never use hardcoded px values** for padding/margin between sections
- Use clamp(): `padding: clamp(3rem, 8vw, 6rem) 0;`
- Add `px-3` to containers for responsive side padding
- Mobile media queries only adjust small breakpoint specifics (< 576px)

## Development Notes
- No build system required - static HTML/CSS/JS served directly
- Changes take effect immediately (no compilation step)
- Browser DevTools responsive mode excellent for testing breakpoints
- Consider image optimization (JPGs should be <500KB each for web)
