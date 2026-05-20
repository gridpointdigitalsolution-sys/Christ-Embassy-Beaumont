# Christ Embassy Beaumont - Website Handover Documentation

**Last Updated:** 2026-05-20  
**Project Path:** `F:\Bro Ernest CANADA\christ-embassy-beaumont\`  
**GitHub Repo:** https://github.com/gridpointdigitalsolution-sys/Christ-Embassy-Beaumont  
**Branch Pastor:** Ernest Osaghae  
**Church:** Christ Embassy Beaumont (Believers' LoveWorld Inc.), Beaumont, Alberta, Canada

---

## Architecture Overview

Single-file HTML website (`index.html`) with ALL CSS and JavaScript embedded inline. No build tools, no frameworks, no package manager.

### Tech Stack
- **HTML5** single-file with embedded `<style>` and `<script>`
- **Google Fonts:** Playfair Display (headings) + Lato (body)
- **No external JS libraries** - pure vanilla JavaScript
- **Python HTTP server** for local preview (port 8080 or 8090)

### Page Structure (SPA-like)
The site uses 3 `.page` divs that show/hide via `switchPage()` function:

| Page ID | Purpose | Nav Link |
|---------|---------|----------|
| `#home-page` | Main landing page | Home |
| `#about-page` | About the church, pastor, beliefs | About |
| `#rhapsody-page` | Rhapsody of Realities daily devotional | Rhapsody |

**Page switching JS:**
```javascript
function switchPage(pageId) {
  // Hides all .page divs, shows target, scrolls to top
  // Updates nav active state
}
```

### Brand Colors (CSS Variables)
```css
--ce-royal: #1A2E7A;   /* Primary dark blue */
--ce-blue: #3253BC;    /* Secondary blue */
--ce-gold: #C9A84C;    /* Gold accent */
--ce-gold-lt: #F5C842; /* Light gold */
```

---

## File Structure

```
christ-embassy-beaumont/
  index.html              # THE website (all HTML/CSS/JS)
  logo.png                # LoveWorld gold heart logo
  pastor-ernest.jpg       # Branch pastor photo
  HANDOVER.md             # This file
  .claude/launch.json     # Local dev server config
  media/
    video-1.mp4 ... video-7.mp4    # Church videos (~160MB total)
    optimized/
      *.jpg (28 photos)            # Compressed church photos
```

### Media Files (28 optimized images)
All in `media/optimized/`:
- group-photo-reccenter.jpg, church-entrance.jpg
- sunday-worship-1.jpg through sunday-worship-3.jpg
- bible-study-1.jpg through bible-study-3.jpg
- choir-1.jpg, choir-2.jpg
- communion-service.jpg
- youth-fellowship-1.jpg, youth-fellowship-2.jpg
- community-outreach-1.jpg through community-outreach-3.jpg
- prayer-meeting-1.jpg, prayer-meeting-2.jpg
- children-ministry-1.jpg, children-ministry-2.jpg
- men-fellowship.jpg, women-fellowship.jpg
- baptism-service.jpg
- new-year-service.jpg
- thanksgiving-service.jpg
- pastor-teaching.jpg
- church-picnic.jpg
- christmas-service.jpg

### Videos (7 files)
- `media/video-1.mp4` through `media/video-7.mp4`
- WARNING: video-6.mp4 (55.94MB) and video-7.mp4 (50.53MB) exceed GitHub's recommended 50MB limit

---

## Key Features

### 1. Navigation
- Fixed top nav with glass-morphism effect
- Logo links to home page via `switchPage('home')`
- Mobile hamburger menu
- Announcement ribbon above nav

### 2. Hero Slideshow
- Auto-rotating image slideshow with 5s interval
- Manual prev/next buttons
- Progress dots
- CSS: `aspect-ratio: 16/10`, `object-position: center 30%`

### 3. Image Gallery (Moments of Glory)
- CSS Grid with `auto-fill, minmax(280px, 1fr)`
- Mixed aspect ratios: `4/3` default, `16/9` for span-2 items
- ALL images clickable with lightbox popup

### 4. Lightbox System
**CRITICAL:** The lightbox `<div id="lightbox">` is placed OUTSIDE all `.page` divs (between about-page and footer). This is intentional - `position: fixed` breaks inside transformed elements.

```javascript
function openLightbox(el) {
  // Handles cached images (onload won't fire)
  // Uses setTimeout + img.complete check as fallback
}
```

**Known fix applied:** `.page` CSS does NOT use `transform` property. Using `transform` on parent breaks `position: fixed` on children (CSS spec behavior).

### 5. Rhapsody of Realities (3rd Page)
- **Tabbed interface:** Adults / Teens / Kids
- **Adults tab:** Embedded iframe from `https://read.rhapsodyofrealities.org`
- **Teens tab:** Embedded iframe from `https://teevotogo.org`
- **Kids tab:** Embedded iframe from `https://lovetoons.org`
- **LIVE FEED:** These iframes auto-update daily from their source. No manual update needed.
- **KingsChat section:** Embedded iframe from `https://kingschat.online/user/ror`

### 6. Contact Form
- **Formspree** endpoint: `https://formspree.io/f/xpwzgqjk`
- Fields: Name, Email, Message
- JS validation before submit

### 7. Scroll-to-Top Button
- Fixed position, bottom-left corner
- Appears after scrolling 300px
- Smooth scroll animation

### 8. Scroll Reveal Animations
- `IntersectionObserver` on `.reveal` elements
- Counter animation on stats section
- Threshold: 0.15

### 9. Iframe Lazy Loading (Performance)
- All 10 iframes use `data-src` instead of `src`
- `IntersectionObserver` loads them when 200px from viewport
- Massively improves initial page load speed
- Iframes load seamlessly before user scrolls to them

---

## External Dependencies (10 Iframes)

| Source | Where Used | Purpose |
|--------|-----------|---------|
| `youtube.com/embed/SFqJ_0jkxoA` | Home + About | Church intro video |
| `youtube.com/embed/` (2nd) | Home | Additional video |
| `read.rhapsodyofrealities.org` | Home + Rhapsody | Daily devotional (Adults) |
| `teevotogo.org` | Rhapsody | Teen devotional |
| `lovetoons.org` | Home + Rhapsody | Kids content |
| `kingschat.online/user/ror` | Rhapsody | KingsChat feed |
| `google.com/maps/embed` | Home + About | Church location map |

**Note:** Some iframes are duplicated across pages intentionally (home preview + full rhapsody page).

---

## SEO & Meta

### Open Graph / Twitter Cards
- OG image: absolute URL to `group-photo-reccenter.jpg`
- Twitter card: `summary_large_image`

### Schema.org
- Type: `Church` (JSON-LD in `<head>`)
- Includes address, geo coordinates, opening hours, parent organization

### ACTION REQUIRED When Domain is Set:
Update these lines in `index.html` when Hostinger domain is active:
1. `<meta property="og:image">` - change base URL
2. `<meta property="og:url">` - change to domain
3. `<meta name="twitter:image">` - change base URL
4. `<link rel="canonical">` - change to domain
5. Schema.org `"url"` field - change to domain

Search for `gridpointdigitalsolution-sys.github.io` to find all instances.

---

## Deployment

### GitHub Repository
- **Repo:** https://github.com/gridpointdigitalsolution-sys/Christ-Embassy-Beaumont
- **Branch:** `main`
- Push from local: `git push origin main`

### Hostinger Deployment
The user plans to deploy via Hostinger connected to this GitHub repo.

**Steps for Hostinger:**
1. Go to Hostinger Dashboard > Websites > Manage
2. Go to Advanced > Git or use Auto Deploy feature
3. Connect to the GitHub repo
4. Set branch: `main`
5. Set document root to repo root (where index.html is)
6. After connecting, update all meta URLs to Hostinger domain

### Local Development
```bash
cd "F:\Bro Ernest CANADA\christ-embassy-beaumont"
python -m http.server 8080
# Open http://localhost:8080
```

---

## Known Issues & Considerations

1. **Large video files:** video-6.mp4 (56MB) and video-7.mp4 (51MB) exceed GitHub's 50MB recommendation. Consider Git LFS if repo grows.

2. **Pastor Chris image:** Now stored locally as `pastor-chris.png` (previously loaded from christembassy.org).

3. **Single-file architecture:** At ~1600+ lines, index.html is large. If major features are added, consider splitting CSS/JS into separate files.

4. **Iframe loading:** 10 iframes from 6 external domains. Heavy on initial load. Consider lazy-loading iframes not in viewport.

5. **No HTTPS enforcement:** Add to .htaccess on Hostinger if needed.

---

## Quick Reference - Key JS Functions

| Function | Purpose |
|----------|---------|
| `switchPage(pageId)` | Navigate between home/about/rhapsody |
| `openLightbox(el)` | Open image popup (handles cached images) |
| `closeLightbox()` | Close image popup |
| `switchRhapTab(tab, btn)` | Switch Rhapsody devotional tabs |
| `initSlideshow()` | Start hero image rotation |
| `handleMobileMenu()` | Toggle mobile nav |

---

## Contact Information

- **Church Phone:** +1 (780) 807-4622
- **Service Time:** Every Sunday at 10:00 AM
- **Location:** 5001 Rue Eaglemont, Beaumont, AB T4X 0H9, Canada
- **Formspree Contact Form ID:** xpwzgqjk

---

*This handover file was created to ensure continuity if another developer or AI continues work on this project. All critical architecture decisions, known bugs, and deployment details are documented above.*
