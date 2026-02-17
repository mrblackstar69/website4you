# Швидкий довідник | CSS/JS сніпети
## Для копіювання та вставлення

---

## 🎨 CSS VARIABLES (Копіюйте в корінь)

```css
:root {
  /* Colors */
  --color-primary: #ff6600;
  --color-primary-light: #ff8533;
  --color-primary-dark: #e55a00;
  --color-black: #1a1a1a;
  --color-white: #ffffff;
  --color-gray-light: #f5f5f5;
  --color-gray-medium: #e5e5e5;
  --color-gray-dark: #666666;

  /* Shadows */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 12px 24px rgba(0, 0, 0, 0.2);
  --shadow-orange: 0 4px 15px rgba(255, 102, 0, 0.3);

  /* Animations */
  --ease-out: cubic-bezier(0.34, 1.56, 0.64, 1);
  --transition-fast: 0.2s var(--ease-out);
  --transition-normal: 0.3s var(--ease-out);
  --transition-slow: 0.5s var(--ease-out);

  /* Spacing */
  --spacing-xs: 0.5rem;
  --spacing-sm: 1rem;
  --spacing-md: 1.5rem;
  --spacing-lg: 2rem;
  --spacing-xl: 3rem;
}
```

---

## 🎬 АНІМАЦІЇ (Копіюйте в CSS)

### Slide In
```css
@keyframes slideInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideInDown {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.element {
  animation: slideInUp 0.8s var(--ease-out) both;
}
```

### Pulse & Glow
```css
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}

@keyframes glow {
  0% { box-shadow: 0 0 0 0 rgba(255, 102, 0, 0.7); }
  70% { box-shadow: 0 0 0 10px rgba(255, 102, 0, 0); }
  100% { box-shadow: 0 0 0 0 rgba(255, 102, 0, 0); }
}

.badge {
  animation: pulse 2s ease-in-out infinite;
}

.button:hover {
  animation: glow 0.6s ease-out;
}
```

### Skeleton Loading
```css
@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    #e5e5e5 0%,
    #f0f0f0 50%,
    #e5e5e5 100%
  );
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
}
```

---

## 🖼️ RESPONSIVE IMAGES

```html
<!-- Basic Picture Element -->
<picture>
  <source 
    media="(max-width: 768px)"
    srcset="image-mobile.webp 1x, image-mobile@2x.webp 2x"
    type="image/webp">
  <source 
    srcset="image-desktop.webp 1x, image-desktop@2x.webp 2x"
    type="image/webp">
  <img src="image-fallback.jpg" alt="..." loading="lazy" width="400" height="300">
</picture>

<!-- With AVIF Support -->
<picture>
  <source type="image/avif" srcset="image.avif">
  <source type="image/webp" srcset="image.webp">
  <img src="image.jpg" alt="..." loading="lazy">
</picture>
```

---

## 🔘 BUTTON STYLES

### Primary Button
```css
.btn-primary {
  background: linear-gradient(135deg, #ff6600 0%, #ff8533 100%);
  color: #fff;
  padding: 1rem 2.5rem;
  border: none;
  border-radius: 0.5rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s var(--ease-out);
  box-shadow: var(--shadow-orange);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 15px 40px rgba(255, 102, 0, 0.45);
}

.btn-primary:active {
  transform: translateY(-1px);
}
```

### Secondary Button
```css
.btn-secondary {
  background: transparent;
  color: #fff;
  border: 2px solid #fff;
  padding: 0.75rem 2rem;
  border-radius: 0.5rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: #ff6600;
  color: #ff6600;
}
```

---

## 📱 MOBILE FIRST LAYOUT

```css
/* Mobile (default) */
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

/* Tablet */
@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
  }
}

/* Desktop */
@media (min-width: 1280px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
}

/* Flex alternative */
.flex-wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.flex-item {
  flex: 1;
  min-width: 300px;
}
```

---

## 🎯 MODERN CSS FEATURES

### Container Queries
```css
@supports (container-type: inline-size) {
  .container {
    container-type: inline-size;
  }

  @container (max-width: 350px) {
    .card__title { font-size: 1rem; }
  }

  @container (min-width: 400px) {
    .card__title { font-size: 1.25rem; }
  }
}
```

### :has() Selector
```css
/* Style parent when child matches */
.product-card:has(.badge) {
  border: 2px solid #ff6600;
}

/* Style siblings */
.form:has(input:invalid) .submit-button {
  opacity: 0.5;
  pointer-events: none;
}

/* Context styling */
.section:has(.product-card:hover) .section-title {
  color: #ff6600;
}
```

### Scroll-Driven Animations
```css
@supports (animation-timeline: view()) {
  .fade-in {
    animation: fadeIn linear;
    animation-timeline: view();
    animation-range: entry 0% cover 30%;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
}
```

---

## 📝 FORM STYLING

### Floating Label Input
```css
.form-group {
  position: relative;
  margin-bottom: 2rem;
}

.form-input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 2px solid #e5e5e5;
  border-radius: 0.5rem;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.form-input:focus {
  outline: none;
  border-color: #ff6600;
  box-shadow: 0 0 0 3px rgba(255, 102, 0, 0.1);
}

.form-label {
  position: absolute;
  left: 1rem;
  top: 0.75rem;
  background: #fff;
  padding: 0 0.25rem;
  color: #999;
  font-size: 1rem;
  transition: all 0.3s ease;
  pointer-events: none;
}

.form-input:focus ~ .form-label,
.form-input:not(:placeholder-shown) ~ .form-label {
  top: -0.75rem;
  font-size: 0.85rem;
  color: #ff6600;
}
```

---

## 📊 GRID & FLEXBOX

### CSS Grid (3 columns)
```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

/* 2 columns layout */
.grid-2 {
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
}

/* Single column fallback */
@media (max-width: 768px) {
  .grid { grid-template-columns: 1fr; }
}
```

### Flexbox (Navigation)
```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background: var(--color-black);
  color: var(--color-white);
}

.navbar__menu {
  display: flex;
  gap: 2rem;
  list-style: none;
}

.navbar__link {
  color: inherit;
  text-decoration: none;
  transition: color 0.3s ease;
}

.navbar__link:hover {
  color: var(--color-primary);
}

@media (max-width: 768px) {
  .navbar__menu {
    flex-direction: column;
    gap: 1rem;
  }
}
```

---

## 🎭 HOVER EFFECTS

### Scale Transform
```css
.scale-hover {
  transition: transform 0.3s var(--ease-out);
}

.scale-hover:hover {
  transform: scale(1.05);
}

/* With rotation */
.scale-rotate:hover {
  transform: scale(1.1) rotate(0.5deg);
}
```

### Shadow Lift
```css
.shadow-lift {
  transition: all 0.3s var(--ease-out);
  box-shadow: var(--shadow-sm);
}

.shadow-lift:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}
```

### Color Change
```css
.color-shift {
  transition: all 0.3s ease;
  color: var(--color-gray-dark);
}

.color-shift:hover {
  color: var(--color-primary);
}
```

---

## 💾 JAVASCRIPT UTILS

### Debounce Function
```javascript
const debounce = (fn, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
};

// Usage
window.addEventListener('scroll', debounce(() => {
  console.log('Scrolled');
}, 100));
```

### Throttle Function
```javascript
const throttle = (fn, delay) => {
  let lastCall = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastCall >= delay) {
      fn(...args);
      lastCall = now;
    }
  };
};
```

### Intersection Observer
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, {
  threshold: 0.1,
  rootMargin: '0px 0px -50px 0px'
});

document.querySelectorAll('[data-observe]').forEach(el => {
  observer.observe(el);
});
```

### Lazy Image Loading
```javascript
const loadImage = (img) => {
  const src = img.dataset.src;
  if (src) {
    img.src = src;
    img.removeAttribute('data-src');
    img.classList.add('loaded');
  }
};

// With intersection observer
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      loadImage(entry.target);
      imageObserver.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  imageObserver.observe(img);
});
```

---

## 📤 FORM VALIDATION

```javascript
class FormValidator {
  constructor(formSelector) {
    this.form = document.querySelector(formSelector);
    this.init();
  }

  init() {
    this.form.addEventListener('submit', (e) => this.handleSubmit(e));
  }

  handleSubmit(e) {
    e.preventDefault();
    
    if (this.validate()) {
      this.submit();
    }
  }

  validate() {
    const inputs = this.form.querySelectorAll('input, textarea, select');
    let isValid = true;

    inputs.forEach(input => {
      if (!input.checkValidity()) {
        input.classList.add('invalid');
        isValid = false;
      } else {
        input.classList.remove('invalid');
      }
    });

    return isValid;
  }

  submit() {
    const formData = new FormData(this.form);
    
    fetch('/api/submit', {
      method: 'POST',
      body: JSON.stringify(Object.fromEntries(formData))
    })
    .then(res => res.json())
    .then(data => {
      this.form.classList.add('success');
      setTimeout(() => this.form.reset(), 2000);
    })
    .catch(err => {
      console.error('Form submission error:', err);
      this.form.classList.add('error');
    });
  }
}

// Usage
new FormValidator('#contact-form');
```

---

## 🎥 RIPPLE EFFECT

```javascript
function createRipple(e) {
  const button = e.currentTarget;
  const ripple = document.createElement('span');
  const rect = button.getBoundingClientRect();
  
  const size = Math.max(rect.width, rect.height);
  const x = e.clientX - rect.left - size / 2;
  const y = e.clientY - rect.top - size / 2;

  ripple.style.width = ripple.style.height = size + 'px';
  ripple.style.left = x + 'px';
  ripple.style.top = y + 'px';
  ripple.classList.add('ripple');

  button.appendChild(ripple);
  
  setTimeout(() => ripple.remove(), 600);
}

// Add to button
button.addEventListener('click', createRipple);
```

### CSS для ripple
```css
.ripple {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.6);
  transform: scale(0);
  animation: ripple-effect 0.6s ease-out;
  pointer-events: none;
}

@keyframes ripple-effect {
  to {
    transform: scale(4);
    opacity: 0;
  }
}
```

---

## 🔍 PERFORMANCE

### Measure Performance
```javascript
// Mark time
performance.mark('operation-start');

// ... do something ...

performance.mark('operation-end');
performance.measure('operation', 'operation-start', 'operation-end');

// Get results
const measures = performance.getEntriesByName('operation');
console.log(measures[0].duration); // milliseconds
```

### Monitor Web Vitals
```javascript
import {getCLS, getFID, getFCP, getLCP, getTTFB} from 'web-vitals';

function sendMetric(metric) {
  console.log(metric.name, metric.value);
  
  // Send to analytics
  fetch('/api/metrics', {
    method: 'POST',
    body: JSON.stringify(metric)
  });
}

getCLS(sendMetric);
getFID(sendMetric);
getFCP(sendMetric);
getLCP(sendMetric);
getTTFB(sendMetric);
```

---

## 🛠️ USEFUL SNIPPETS

### Smooth Scroll
```css
html {
  scroll-behavior: smooth;
}

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }
}
```

### Truncate Text
```css
.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.truncate-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

### Aspect Ratio
```css
.aspect-16-9 {
  aspect-ratio: 16/9;
}

.aspect-4-3 {
  aspect-ratio: 4/3;
}

.aspect-square {
  aspect-ratio: 1;
}
```

### Center Content
```css
/* Flex center */
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Grid center */
.grid-center {
  display: grid;
  place-items: center;
}

/* Absolute center */
.absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

---

## ✨ QUICK REFERENCE TABLE

| Ефект | Тривалість | Easing |
|--------|-----------|--------|
| Button hover | 0.3s | ease-out |
| Card hover | 0.4s | ease-out |
| Fade in | 0.6s | ease-out |
| Scroll reveal | 0.8s | ease-out |
| Modal | 0.5s | ease-out |
| Loading | 1.5s | infinite |
| Pulse badge | 2s | infinite |

---

## 📦 COPY-PASTE READY BLOCKS

### Hero CTA Button
```html
<button class="btn-primary">
  <span>Отримати пропозицію</span>
  <span>→</span>
</button>

<style>
.btn-primary {
  background: linear-gradient(135deg, #ff6600, #ff8533);
  color: white;
  padding: 1rem 2.5rem;
  border: none;
  border-radius: 0.5rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  box-shadow: 0 4px 15px rgba(255, 102, 0, 0.3);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 15px 40px rgba(255, 102, 0, 0.45);
}
</style>
```

### Product Card
```html
<div class="product-card">
  <img src="..." alt="..." class="card-image">
  <h3 class="card-title">Product Name</h3>
  <p class="card-price">₴5,500 <span>/день</span></p>
  <button class="card-cta">Замовити</button>
</div>

<style>
.product-card {
  border-radius: 0.75rem;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.product-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.15);
}

.product-card:hover .card-image {
  transform: scale(1.12);
}
</style>
```

---

## 💡 TIPS

✅ Завжди використовуйте `cubic-bezier(0.34, 1.56, 0.64, 1)` для smooth animations  
✅ Мобільні: min 44px для touch targets  
✅ WebP розміри: 30-40% менше JPEG  
✅ LCP target: < 2.5s  
✅ CLS target: < 0.1  

---

**Updated:** 17 Feb 2026  
**Production Ready** ✅
