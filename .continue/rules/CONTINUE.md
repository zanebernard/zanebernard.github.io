# Zane Bernard – Videography Portfolio: Project Guide

> This file is automatically loaded by [Continue](https://continue.dev) to give AI assistants full context about this project. Keep it up to date as the site evolves.

---

## 1. Project Overview

**Purpose:** A personal portfolio website for Zane Bernard, a professional filmmaker and videographer. The site showcases his work across three categories — Weddings, Short Films, and Motion Graphics — and serves as his primary client-facing web presence.

**Live URL:** Hosted on GitHub Pages (custom domain configured via `CNAME`)

**Key Technologies:**
| Technology | Role |
|---|---|
| HTML5 | Page structure and content |
| CSS3 | All styling, layout, and responsive design |
| Vanilla JavaScript | Video overlay / lightbox interactivity |
| Vimeo Player API (iframe) | Embedded video streaming |
| Google Fonts (Inter) | Typography |
| GitHub Pages | Static site hosting |

**High-Level Architecture:**
This is a **single-page static website** — no build tools, no frameworks, no package managers. Everything is plain HTML, CSS, and JavaScript. The entire site is one `index.html` file styled by `styles.css`. There is no server-side code.

---

## 2. Getting Started

### Prerequisites
- A modern web browser (Chrome, Firefox, Safari, Edge)
- A code editor (VS Code recommended)
- Git (for version control and deploying to GitHub Pages)
- An internet connection (Vimeo videos are streamed externally)

### Installation / Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/zanebernard/zanebernard.github.io.git
   cd zanebernard.github.io
   ```

2. **Open the site locally:**
   Simply open `index.html` in your browser directly, or use a live-reload server for a better experience:
   ```bash
   # Using VS Code's Live Server extension (recommended)
   # Right-click index.html → "Open with Live Server"

   # Or using Python's built-in HTTP server
   python -m http.server 8080
   # Then visit http://localhost:8080
   ```

3. **No build step required.** There are no dependencies to install — this project has no `package.json`, `node_modules`, or build pipeline.

### Deployment

Changes pushed to the `main` branch on GitHub are automatically published via **GitHub Pages**.

```bash
git add .
git commit -m "your message"
git push origin main
```

The site typically goes live within 30–60 seconds of the push.

---

## 3. Project Structure

```
zanebernard.github.io/
│
├── index.html              # The entire website (single page)
├── styles.css              # All CSS — layout, theming, animations, responsive design
├── CNAME                   # Custom domain configuration for GitHub Pages
├── README.md               # Brief project description
│
├── short form.mp4          # Locally hosted short-form vertical video (autoplays in hero)
│
├── Untitled-1.png          # Logo displayed in the site header
├── portrait.jpg            # [Available asset — verify usage]
│
│   ── Thumbnail Images (used as video poster frames) ──
├── davis and taylor.png
├── michael and jenniger.png
├── ryan and amber.png
├── bria and anthony.png
├── brandi and branden.png
├── cancelled.png
├── dogma 95.png
├── luthier doc.png
├── lease finance.png
├── hospital.png
│
└── .continue/
    └── rules/
        └── CONTINUE.md     # This file — AI project context guide
```

### Key File Roles

| File | Purpose |
|---|---|
| `index.html` | All site content: header, showreel, gallery sections, about, contact, overlay player, footer, and all JS |
| `styles.css` | Global styles, dark theme, video grid, hover effects, lightbox overlay, responsive breakpoints |
| `CNAME` | Tells GitHub Pages to serve the site at the custom domain |
| `*.png` thumbnails | Static preview images that appear before a video is clicked |
| `short form.mp4` | The only locally-hosted video; everything else streams from Vimeo |

---

## 4. Development Workflow

### Coding Standards & Conventions

- **No framework, no transpilation** — write standard HTML5 and CSS3 directly.
- **CSS is global** — all styles live in `styles.css`. Avoid inline styles except where dynamically required (e.g., `background-image` for thumbnails, which are data-driven and must be inline).
- **JavaScript is inline** — the two `<script>` blocks at the bottom of `index.html` handle all interactivity. Keep JS minimal and vanilla.
- **Dark theme** — The site uses a near-black dark palette (`#121212`, `#1a1a1a`, `#000`). All new UI elements must respect this aesthetic.
- **File naming** — Current assets use spaces in filenames (e.g., `davis and taylor.png`). When referencing these in HTML, encode spaces as `%20` or wrap in quotes within CSS `url()`.

### Adding a New Video

1. **Upload the video to Vimeo** and get its numeric video ID from the URL (e.g., `vimeo.com/video/123456789` → ID is `123456789`).
2. **Add a thumbnail image** — export a still frame from the video, save as a `.png`, and place it in the root directory.
3. **Add a new card** in the appropriate `<section>` in `index.html`:
   ```html
   <div class="video" data-video-id="YOUR_VIMEO_ID">
     <div class="video-wrapper">
       <div class="video-thumbnail" style="background-image: url('your-thumbnail.png');">
         <div class="overlay">
           <p class="title">Video Title</p>
           <span class="button">▶ Watch</span>
         </div>
       </div>
     </div>
   </div>
   ```
4. The JavaScript at the bottom of `index.html` automatically picks up any element with `data-video-id` and wires up the click-to-play lightbox. No JS changes needed.

### Adding a New Section

1. Add a new `<section>` block in `index.html` following the existing pattern.
2. Use the `.video-gallery > .scroll-container > .videos` structure to get automatic horizontal scrolling on mobile.
3. Style tweaks go in `styles.css`.

### Editing the About / Contact Sections

These are plain HTML text blocks near the bottom of `index.html` — just edit the text directly.

### Testing

There is no automated test suite. Testing is manual:

- **Cross-browser:** Check in Chrome, Firefox, and Safari (especially for the `video` autoplay behavior on Safari).
- **Responsive:** Use browser DevTools to simulate mobile widths. The key breakpoint is `max-width: 768px` in `styles.css`.
- **Video playback:** Verify Vimeo iframes load and that the lightbox opens/closes correctly.
- **Autoplay:** The Vimeo showreel and local `short form.mp4` both autoplay muted — confirm this works on mobile (browsers require `muted` and `playsinline` attributes for autoplay, which are already set).

---

## 5. Key Concepts

### Video Lightbox / Overlay
Clicking any thumbnail card triggers a fullscreen overlay (`#video-overlay`). The JS injects the Vimeo embed URL (with `?autoplay=1`) into an `<iframe id="video-player">`. Closing the overlay clears the `src` to stop playback. The overlay is always present in the DOM but hidden via `display: none`.

### `data-video-id` Attribute
This is the mechanism that links a thumbnail card to its Vimeo video. The JS loops over all `.video-wrapper` elements, reads this attribute (checking the element itself and its parent), and attaches a click event listener. Note: in `Short Films`, the `data-video-id` is on the `.video-wrapper` element itself rather than the parent `.video` — the JS handles both cases.

### Scroll Galleries (Mobile)
On mobile, video grids switch from a CSS Flexbox wrap layout to a horizontal scroll container (`.scroll-container`). CSS scroll-snap (`scroll-snap-type: x mandatory`) is used so cards snap cleanly when swiping. A small JS snippet resets `scrollLeft` to `0` on load to fix a Safari rendering quirk.

### Vimeo Background Embed (Showreel)
The hero showreel uses Vimeo's `background=1` embed parameter, which hides all player controls and creates an ambient, autoplaying video banner effect.

### Static Hosting via GitHub Pages
The `CNAME` file contains the custom domain. GitHub Pages serves the root `index.html` automatically. There is no routing — this is a true single-page site.

---

## 6. Common Tasks

### Update the Showreel Video
1. Upload new video to Vimeo.
2. In `index.html`, find the `<iframe>` inside `<section class="featured">`.
3. Replace the Vimeo video ID in the `src` URL:
   ```
   https://player.vimeo.com/video/NEW_ID_HERE?autoplay=1&muted=1&loop=1&background=1&playsinline=1
   ```

### Replace the Logo
1. Export your new logo as a PNG.
2. Replace `Untitled-1.png` in the root directory (keep the same filename), **or** update the `src` attribute in the `<header>` of `index.html`:
   ```html
   <img src="your-new-logo.png" alt="Zane Videography Logo" class="logo">
   ```
3. Adjust `.logo` max-height in `styles.css` if needed.

### Update Contact Email
Find this line in `index.html` and update both the `href` and the display text:
```html
<p>Email me at <a href="mailto:zanebernardworks@gmail.com">zanebernardworks@gmail.com</a></p>
```

### Change the Custom Domain
1. Update the content of the `CNAME` file to the new domain.
2. Update the DNS settings with your domain registrar to point to GitHub Pages.
3. Verify in the repository's **Settings → Pages** tab.

### Remove a Video from the Gallery
Delete the entire `<div class="video" data-video-id="...">` block for that card in `index.html`. The layout will automatically reflow.

---

## 7. Troubleshooting

### Videos not playing / Vimeo iframe not loading
- **Check Vimeo privacy settings** — the video must be set to "Anyone can view" or embedded on the specific domain.
- **Check the video ID** — confirm the numeric ID in `data-video-id` matches the actual Vimeo URL.
- **CORS / browser console** — open DevTools → Console for specific error messages.

### `short form.mp4` not autoplaying
- All major mobile browsers block autoplay unless the video has both `muted` and `playsinline` attributes. These are already set, but double-check if you replace the element.
- Safari on iOS may still require a user gesture for some formats. Ensure the mp4 is encoded in a web-compatible format (H.264 is safest).

### Horizontal scroll gallery not snapping correctly on mobile
- The scroll-snap is defined in `styles.css` under `@media (max-width: 768px)`. Confirm the `.scroll-container` has `scroll-snap-type: x mandatory` and each `.video` child has `scroll-snap-align: start`.
- The `scrollLeft = 0` reset in the load listener is a Safari fix — do not remove it.

### Site not updating after push
- GitHub Pages can take up to a few minutes. Check the **Actions** tab in your GitHub repo for deployment status.
- Hard refresh the browser (`Ctrl+Shift+R` / `Cmd+Shift+R`) to bypass cache.

### Layout broken on a specific screen size
- Use browser DevTools to identify which CSS rule is conflicting.
- All responsive overrides are in the `@media (max-width: 768px)` block at the bottom of `styles.css`.
- The `overflow-x: hidden` on `body` and `section` prevents horizontal scroll on mobile — be careful when adding wide elements.

### Lightbox overlay doesn't close properly
- The close button (`×`) sets `player.src = ''` to stop the video and hides the overlay. If it's broken, check that the `#video-overlay`, `.close-button`, and `#video-player` IDs/classes haven't been renamed in `index.html`.

---

## 8. References

- **GitHub Pages Docs:** https://docs.github.com/en/pages
- **Vimeo Player Parameters:** https://developer.vimeo.com/player/sdk/embed
- **CSS Scroll Snap:** https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll_snap
- **HTML `<video>` element:** https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video
- **Google Fonts – Inter:** https://fonts.google.com/specimen/Inter
- **CSS Flexbox Guide:** https://css-tricks.com/snippets/css/a-guide-to-flexbox/

---

*Last updated: 2025 · Maintained by Zane Bernard*
