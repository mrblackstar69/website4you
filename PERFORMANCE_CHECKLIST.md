# Performance & Optimization Checklist
## B2B Equipment Rental Website

---

## 📊 TARGET METRICS

| Metric | Target | Tool |
|--------|--------|------|
| **Lighthouse Score** | 90+ | PageSpeed Insights |
| **First Contentful Paint** | < 1.5s | Web Vitals |
| **Largest Contentful Paint** | < 2.5s | Web Vitals |
| **Cumulative Layout Shift** | < 0.1 | Web Vitals |
| **Time to Interactive** | < 3.5s | Web Vitals |
| **Total Blocking Time** | < 200ms | Web Vitals |

---

## 🖼️ IMAGE OPTIMIZATION CHECKLIST

### Format Selection
```
✅ WebP as primary format (30% smaller than JPEG)
✅ AVIF as next-gen (40% smaller than WebP)
✅ JPEG/PNG as fallback for old browsers
✅ SVG for icons and logos
✅ Video format (WebM/MP4) for background animations
```

### Size Optimization
```javascript
// Рекомендовані розміри для різних kontekстів:

// Hero section
- Desktop: 1920w (max 350KB WebP)
- Tablet: 1024w (max 200KB WebP)
- Mobile: 375w (max 120KB WebP)

// Product cards
- Desktop: 400w (max 80KB WebP)
- Mobile: 300w (max 50KB WebP)
- Thumbnail: 100w (max 15KB WebP)

// Background images
- 1200w (max 200KB WebP)
- 600w (max 100KB WebP)
```

### Compression Tools
```bash
# ImageOptim (macOS)
# Squoosh (Online): https://squoosh.app/

# Command-line options:
# For JPEG → WebP conversion
cwebp image.jpg -o image.webp -q 75

# For batch conversion
for file in *.jpg; do
  cwebp "$file" -o "${file%.jpg}.webp" -q 75
done

# Using ImageMagick
mogrify -format webp -quality 75 *.jpg

# Using AVIF
avifenc input.jpg output.avif --quality 70
```

### Picture Element Template
```html
<picture>
  <source srcset="image-avif.avif" type="image/avif">
  <source srcset="image-webp.webp" type="image/webp">
  <source srcset="image-jpg.jpg" type="image/jpeg">
  <img src="image-jpg.jpg" alt="Description" loading="lazy" width="400" height="300">
</picture>
```

---

## 🚀 PERFORMANCE OPTIMIZATION

### 1. Critical Path Optimization
```
1st Priority (Critical):
- HTML document
- Critical CSS (hero, header, above-the-fold)
- Core JS (framework, routing)

2nd Priority (High):
- Font files (WOFF2)
- Above-the-fold images
- Main product images

3rd Priority (Medium):
- Non-critical JS
- Additional CSS
- Hover states

Deferred:
- Analytics scripts
- Video backgrounds
- Social widgets
```

### 2. Rendering Optimization
```css
/* Reduce Cumulative Layout Shift */
.hero { 
  min-height: 600px; /* Reserve space */
  contain: layout style paint; /* CSS containment */
}

img {
  content-visibility: auto; /* Defer rendering */
}

/* Avoid layout shifts */
* {
  box-sizing: border-box; /* Consistent sizing */
}

body {
  overflow-y: scroll; /* Reserve scrollbar space */
}
```

### 3. CSS Optimization
```
✅ Critical CSS inlined (< 14KB)
✅ Non-critical CSS deferred with media queries
✅ Remove unused CSS with PurgeCSS/Tailwind
✅ Minimize and compress
✅ Use CSS variables instead of duplication
✅ Media queries split by breakpoint
```

### 4. JavaScript Optimization
```javascript
// Code Splitting
const ProductCard = () => import('./ProductCard.js');

// Lazy loading with dynamic imports
button.addEventListener('click', async () => {
  const module = await import('./modal.js');
  module.openModal();
});

// Debouncing scroll events
const debounce = (fn, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
};

window.addEventListener('scroll', debounce(() => {
  // Handle scroll
}, 100));

// Intersection Observer instead of scroll listener
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      // Load content
    }
  });
});
```

### 5. Network Optimization
```
HTTP/2:
✅ Multiplexing requests
✅ Server push for critical assets
✅ Header compression (HPACK)

Resource Hints:
<link rel="dns-prefetch" href="https://cdn.example.com">
<link rel="preconnect" href="https://api.example.com">
<link rel="prefetch" href="/next-page.html">
<link rel="preload" as="image" href="hero.webp">

Compression:
✅ Gzip/Brotli compression (text assets)
✅ Image compression (WebP/AVIF)
✅ Code minification (CSS, JS)
```

---

## 📝 FRONTEND OPTIMIZATIONS

### HTML
```html
<!-- Async/Defer scripts -->
<script src="analytics.js" async></script>
<script src="main.js" defer></script>

<!-- Fonts with display: swap -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

<!-- Preload critical resources -->
<link rel="preload" as="image" href="hero.webp" imagesrcset="
  hero-mobile.webp 375w,
  hero-tablet.webp 1024w,
  hero-desktop.webp 1920w">

<!-- Meta tags -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="...">
<link rel="canonical" href="https://example.com">
```

### CSS Best Practices
```css
/* Minimal critical CSS (~14KB) */
/* Hero section */
/* Header and navigation */
/* Button styles */
/* Above-the-fold layout */

/* Defer non-critical styles */
@media print {
  link[href*="print.css"] { ... }
}

/* Optimize selectors (less nesting) */
/* BAD: .header .nav ul li a { } */
/* GOOD: .nav-link { } */

/* Use CSS containment */
.product-card {
  contain: layout style paint;
}
```

### JavaScript Best Practices
```javascript
// Use vanilla JS for critical features
// Only load frameworks if necessary

// Remove global scope pollution
(function() {
  // Localized scope
})();

// Event delegation
document.addEventListener('click', (e) => {
  if (e.target.matches('.product-card')) {
    // Handle product card click
  }
});

// Memory leak prevention
element.addEventListener('click', handler);
// Later...
element.removeEventListener('click', handler);
```

---

## 🔄 CACHING STRATEGY

### Server-side (HTTP Headers)
```
# .htaccess example
<FilesMatch "\\.(jpg|jpeg|png|gif|webp|avif)$">
  Header set Cache-Control "public, max-age=31536000, immutable"
</FilesMatch>

<FilesMatch "\\.(css|js)$">
  Header set Cache-Control "public, max-age=31536000, immutable"
</FilesMatch>

<FilesMatch "\\.html$">
  Header set Cache-Control "public, max-age=3600, must-revalidate"
</FilesMatch>

# Or nginx.conf
location ~* \.(jpg|jpeg|png|gif|webp|avif)$ {
  expires 365d;
  add_header Cache-Control "public, immutable";
}

location ~* \.(css|js)$ {
  expires 365d;
  add_header Cache-Control "public, immutable";
}

location ~* \.html$ {
  expires 1h;
  add_header Cache-Control "public, must-revalidate";
}
```

### Service Worker Strategy
```javascript
// service-worker.js
const CACHE_VERSION = 'v1';
const CACHE_ASSETS = [
  '/',
  '/css/main.css',
  '/js/main.js',
  '/images/placeholder.jpg'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_VERSION).then(cache => {
      return cache.addAll(CACHE_ASSETS);
    })
  );
});

self.addEventListener('fetch', event => {
  // Cache-first strategy for static assets
  if (event.request.url.includes('/images/')) {
    event.respondWith(
      caches.open(CACHE_VERSION).then(cache => {
        return cache.match(event.request).then(response => {
          return response || fetch(event.request).then(response => {
            cache.put(event.request, response.clone());
            return response;
          });
        });
      })
    );
  }
  
  // Network-first for HTML
  if (event.request.headers.get('accept').includes('text/html')) {
    event.respondWith(
      fetch(event.request).then(response => {
        const cache = caches.open(CACHE_VERSION);
        cache.then(c => c.put(event.request, response.clone()));
        return response;
      }).catch(() => {
        return caches.match(event.request);
      })
    );
  }
});
```

---

## 📱 MOBILE OPTIMIZATION

### Responsive Design
```css
/* Mobile-first approach */
/* Base styles for mobile */

/* Tablet breakpoint */
@media (min-width: 768px) {
  /* Tablet adjustments */
}

/* Desktop breakpoint */
@media (min-width: 1280px) {
  /* Desktop adjustments */
}

/* Large desktop */
@media (min-width: 1920px) {
  /* Large screen optimizations */
}

/* Touch-friendly sizes */
button, a {
  min-height: 44px; /* Apple's recommendation */
  min-width: 44px;
}

/* Optimize for different inputs */
@media (pointer: coarse) {
  /* Touch-friendly adjustments */
  button {
    padding: 1.25rem; /* Larger for touch */
  }
}

@media (hover: hover) {
  /* Hover effects only on devices that support it */
  button:hover {
    background: #ff8533;
  }
}
```

### Mobile JavaScript
```javascript
// Detect touch device
const isTouchDevice = () => {
  return (('ontouchstart' in window) ||
          (navigator.maxTouchPoints > 0) ||
          (navigator.msMaxTouchPoints > 0));
};

// Handle both touch and mouse events
element.addEventListener('pointerdown', handleInteraction);
element.addEventListener('pointermove', handleInteraction);
element.addEventListener('pointerup', handleInteraction);

// Viewport meta optimization
// <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

// Apple specific
// <meta name="apple-mobile-web-app-capable" content="yes">
// <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```

---

## ✅ IMPLEMENTАTION CHECKLIST

### Phase 1: Critical (Week 1)
```
[ ] Optimize hero image (WebP + AVIF)
[ ] Inline critical CSS (~14KB)
[ ] Defer non-critical JavaScript
[ ] Set up HTTP/2
[ ] Enable Gzip compression
[ ] Implement lazy loading for images
[ ] Add resource hints (preload, prefetch)
[ ] Optimize font loading
[ ] Remove render-blocking resources
```

### Phase 2: High Priority (Week 2)
```
[ ] Implement Service Worker
[ ] Set up caching headers
[ ] Code splitting for JS
[ ] Minify CSS/JS
[ ] Remove unused CSS (PurgeCSS)
[ ] Optimize hero animations (scroll-driven)
[ ] Implement intersection observer
[ ] Add Web Vitals monitoring
[ ] Optimize product images
[ ] Set up CDN for assets
```

### Phase 3: Medium Priority (Week 3-4)
```
[ ] Implement responsive images (srcset)
[ ] Create WebP fallbacks
[ ] Optimize fonts for performance
[ ] Lazy load off-screen images
[ ] Implement image placeholder
[ ] Optimize third-party scripts
[ ] Set up error monitoring
[ ] Optimize time to interactive
[ ] Reduce layout shifts
[ ] Test on slow networks (3G)
```

### Phase 4: Nice-to-Have (Week 5+)
```
[ ] Implement AVIF format
[ ] Advanced caching strategies
[ ] Progressive Web App features
[ ] Advanced animations optimization
[ ] Database query optimization
[ ] API response optimization
[ ] Multi-region CDN
[ ] Advanced bundle analysis
```

---

## 🧪 TESTING TOOLS

### Performance Testing
```
1. Google PageSpeed Insights
   https://pagespeed.web.dev/

2. WebPageTest
   https://webpagetest.org/

3. GTmetrix
   https://gtmetrix.com/

4. Lighthouse (Chrome DevTools)
   F12 → Lighthouse tab

5. Web Vitals Extension
   https://chrome.google.com/webstore/
```

### Browser DevTools
```javascript
// Measure performance
performance.mark('hero-start');
// ... code ...
performance.mark('hero-end');
performance.measure('hero', 'hero-start', 'hero-end');

// Get measurements
const measures = performance.getEntriesByType('measure');
console.table(measures);

// Monitor Core Web Vitals
web-vitals library:
import {getCLS, getFID, getFCP, getLCP, getTTFB} from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getFCP(console.log);
getLCP(console.log);
getTTFB(console.log);
```

---

## 📊 MONITORING & ANALYTICS

### Set up Real User Monitoring
```javascript
// Using Web Vitals
import {getCLS, getFID, getFCP, getLCP, getTTFB} from 'web-vitals';

function sendMetric(metric) {
  // Send to analytics service
  fetch('/api/metrics', {
    method: 'POST',
    body: JSON.stringify(metric),
    headers: {
      'Content-Type': 'application/json'
    }
  });
}

getCLS(sendMetric);
getFID(sendMetric);
getFCP(sendMetric);
getLCP(sendMetric);
getTTFB(sendMetric);

// Custom events
performance.mark('products-loaded');
performance.measure('products-load-time', 'navigationStart', 'products-loaded');
```

### Analytics Integration
```javascript
// Google Analytics event tracking
gtag('event', 'page_view', {
  'page_path': window.location.pathname,
  'page_title': document.title,
  'web_vitals': {
    'FCP': fcpValue,
    'LCP': lcpValue,
    'CLS': clsValue,
    'FID': fidValue
  }
});
```

---

## 🔐 SECURITY CHECKLIST

```
[ ] HTTPS enabled (SSL/TLS)
[ ] Security headers configured
  - X-Frame-Options
  - X-Content-Type-Options
  - Content-Security-Policy
  - Referrer-Policy
[ ] CORS configured correctly
[ ] Input validation on client & server
[ ] XSS prevention (sanitize user input)
[ ] CSRF tokens for forms
[ ] Rate limiting on API endpoints
[ ] Environment variables secured
[ ] Dependencies scanned for vulnerabilities
[ ] Regular security audits
```

---

## 📈 SUCCESS METRICS

Track these metrics weekly:

```
Week 1:
- Lighthouse Score: baseline → 75+
- FCP: < 2s
- LCP: < 3s

Week 2:
- Lighthouse Score: 80+
- FCP: < 1.5s
- LCP: < 2.5s
- CLS: < 0.2

Week 3:
- Lighthouse Score: 85+
- FCP: < 1.2s
- LCP: < 2s
- CLS: < 0.1

Week 4:
- Lighthouse Score: 90+
- All Core Web Vitals: Green
- TTI: < 3s
- TBT: < 200ms
```

---

## 🎯 CONCLUSION

Optimization is an ongoing process:
1. **Measure** - Test consistently
2. **Identify** - Find bottlenecks
3. **Optimize** - Implement fixes
4. **Monitor** - Track improvements
5. **Iterate** - Repeat continuously

Target: **90+ Lighthouse score** with **all Core Web Vitals GREEN**
