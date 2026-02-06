# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**VNery** is a static marketing website for a Brazilian dumpster/debris container rental service ("Locação de Caçambas") based in São Paulo. The site is a single-page application (SPA) with no backend—all content is in `index.html` with assets stored in the `assets/` directory.

**Technology Stack:**
- HTML5, CSS3, JavaScript (jQuery-based)
- Bootstrap 5.3.0 (responsive framework)
- jQuery plugins: Magnific Popup, jQuery Validate, jQuery Easing, ScrollReveal
- Google Maps API (embedded map)
- Docker + nginx (deployment)
- Google Analytics & Google Ads Conversion Tracking

## Common Development Commands

### Building & Deployment

**Build Docker image:**
```bash
docker build -t vnery:latest .
```

**Run Docker container locally:**
```bash
docker run -p 8080:80 vnery:latest
# Then visit http://localhost:8080
```

**Run nginx directly (for testing without Docker):**
```bash
# If nginx is installed locally, serve the current directory
nginx -c $(pwd)/nginx.conf  # or use system nginx with proper config
```

### Testing & Development

**No automated tests** - This is a static site. Testing is primarily manual:
- Open `index.html` in a browser to preview changes
- Test responsive design at breakpoints: 600px, 800px, 1000px, 1500px
- Verify all internal anchor links work: #page-top, #about, #services, #services3, #contact
- Test external links: WhatsApp, Instagram, Facebook, Google Maps
- Verify Google Analytics and conversion tracking fire correctly

**Live preview:**
- Open `index.html` directly in a browser for quick iterations
- Or use a local web server: `python -m http.server 8000` and visit http://localhost:8000

## Architecture & Structure

### Entry Point
- **index.html** (~30.5 KB) - Complete website as a single HTML file

### Main Sections (Navigation Anchors)
1. **Header/Navigation** - Fixed navbar with logo and menu links
2. **#about** - Company description ("Quem Somos")
3. **#services** - Service feature boxes, showcase images, and warnings
4. **#services3** - Rental request call-to-action section
5. **#contact** - Phone, WhatsApp link, embedded Google Maps
6. **Payment Conditions** - Image showing rental payment terms
7. **Footer** - Social media links

### Asset Organization

```
assets/
├── Images/
│   ├── banner_caçamba_de_entulho.png  # Hero banner
│   ├── vnery.jpg                      # Logo
│   ├── cacamba-0X.jpg (6 files)      # Service showcase
│   ├── condicao-pagamento.jpg        # Payment info graphic
│   └── header*.jpg                    # Header graphics
├── CSS/
│   ├── bootstrap.min.css             # Framework
│   ├── creative.min.css              # Custom theme
│   ├── font-awesome.min.css          # Icons
│   ├── magnific-popup.css            # Gallery styling
│   └── css, css(1-4)                 # Google Maps styles
└── JavaScript/
    ├── jquery.min.js                 # Library
    ├── bootstrap.bundle.min.js       # Framework JS
    ├── creative.min.js               # Custom interactions
    ├── Plugins: jquery.easing.min.js, jquery.magnific-popup.min.js, jquery.validate.min.js, scrollreveal.min.js
    └── Google Maps files: js, common.js, controls.js, map.js, marker.js, util.js
```

## Key Implementation Details

### Navigation & Scrolling
- Scroll-trigger links use `.js-scroll-trigger` class for automatic anchor scrolling
- Smooth scrolling with jQuery easing (1000ms duration)
- Fixed navigation bar (`#mainNav`) that shrinks on scroll

### Styling & Layout
- Bootstrap grid system with custom `creative.min.css` overrides
- Inline styles for padding/margins on sections
- Primary color: `#191970` (dark blue)
- Accent color: `#F05F40` (orange)
- Responsive breakpoints: 400px, 600px, 800px, 1000px, 1500px

### Service Boxes Pattern
- Repeated `.service-box` elements with icons and descriptions
- 8 feature boxes in the services section
- 6 additional service showcase images below

### Animations & Effects
- ScrollReveal for scroll-triggered animations (`.sr-*` classes)
- Magnific Popup for image gallery/lightbox functionality
- jQuery animations for smooth transitions

### External Services Integration
- **Google Analytics**: ID `G-NPB4JYBBVB`
- **Google Ads Conversion**: ID `AW-11157823009`
- **Facebook Pixel**: Domain verification tracking
- **Google Maps**: Embedded map centered on Ipiranga, São Paulo
- **WhatsApp Business**: Link `https://wa.me/5511952167504`

### Contact Information
- Phone: `(11) 95216-7504`
- WhatsApp: `https://wa.me/5511952167504`
- Instagram: `https://www.instagram.com/vnerycacambas`
- Facebook: `https://www.facebook.com/vnerycacambas`

## Deployment

### Docker Deployment
The `Dockerfile` sets up a production-ready nginx deployment:
- Copies all site files to `/usr/share/nginx/html`
- Exposes port 80
- Runs nginx in foreground mode

### Typical Deployment Flow
1. Make changes to `index.html` or assets
2. Commit changes to git
3. Build Docker image: `docker build -t vnery:latest .`
4. Push to registry or deploy to server
5. Container serves static files via nginx

## Code Conventions

- **Minified assets**: Use `.min.js` and `.min.css` for production libraries
- **Class naming**: Custom classes like `.btn-custom`, `.sr-icons`, `.sr-contact`
- **Inline styles**: Heavy use of inline CSS for section padding/margins
- **Comments**: HTML comments used sparingly; mostly self-documenting structure
- **Internationalization**: Content is in Portuguese (pt-BR)
- **SEO**: Meta tags target keywords like "Caçambas em São Paulo", "Aluguel de caçambas"

## Common Editing Tasks

### Adding a New Service Box
1. Find the services section in `index.html` (search for `.service-box`)
2. Copy an existing service box structure
3. Update the content, icon class, and description
4. Adjust scroll reveal delay in the class (e.g., `data-aos-delay="100"`)

### Updating Contact Information
- Phone number appears in multiple places—search for `(11) 95216-7504` and update all instances
- WhatsApp link: `https://wa.me/5511952167504` (update phone number in URL)

### Adding Images
1. Place image files in `assets/` directory
2. Reference in `index.html` with relative path: `assets/image-name.jpg`
3. Consider image optimization (compression) for faster load times

### Modifying Styling
- Global theme styles: `assets/creative.min.css` (or original source if available)
- Inline styles: Edit directly in `index.html` style blocks
- Bootstrap overrides: Add custom CSS in `<style>` tags or new stylesheet

### Tracking Updates
- Google Analytics: Already integrated; no changes needed unless switching accounts
- Google Ads: Update conversion ID `AW-11157823009` in the conversion tracking script if needed
- Facebook Pixel: Update pixel ID if needed in the Facebook domain verification script

## Performance Notes

- All assets are static; no CDN setup (can be added for image optimization)
- Page is relatively heavy (~30.5 KB HTML + multiple image assets)
- Consider lazy loading for images below the fold if performance becomes an issue
- Google Maps iframe embedded directly; could be optimized for slower connections
