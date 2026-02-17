# Технічний аналіз: Сучасні тренди веб-розробки для B2B сайтів прокату техніки
**Дата:** 17 лютого 2026  
**Фокус:** Premium-дизайн в чорно-помаранчевій гамі з оптимальною performance  

---

## 1. HERO СЕКЦІЇ З ВЕЛИКИМИ ЗОБРАЖЕННЯМИ

### Найкращі практики

#### 1.1 Структура та верстка
- **Responsive design**: Hero повинна адаптуватися від мобільних (100vh) до десктопа (120vh)
- **Fixed background vs Full-width image**: Для техніки краще full-width з `object-fit: cover`
- **Градієнтні overlay**: Запобігає читаємістю тексту над зображеннями
- **Lazy loading**: Primary hero видима - `loading="eager"`, backup images - `loading="lazy"`

#### 1.2 Оптимізація зображень для hero
```html
<!-- Рекомендована структура -->
<section class="hero">
  <picture>
    <!-- Mobile: 375px width, optimized -->
    <source 
      media="(max-width: 768px)" 
      srcset="hero-mobile-375w.webp 1x, hero-mobile-750w.webp 2x"
      type="image/webp">
    
    <!-- Tablet: 1024px width -->
    <source 
      media="(max-width: 1280px)" 
      srcset="hero-tablet-1024w.webp 1x, hero-tablet-2048w.webp 2x"
      type="image/webp">
    
    <!-- Desktop: full-width -->
    <source 
      srcset="hero-desktop-1920w.webp 1x, hero-desktop-3840w.webp 2x"
      type="image/webp">
    
    <!-- Fallback -->
    <img 
      src="hero-fallback.jpg" 
      alt="Equipment rental showcase"
      loading="eager"
      decoding="async"
      class="hero__image">
  </picture>

  <!-- Dark overlay for text contrast -->
  <div class="hero__overlay"></div>

  <!-- Content -->
  <div class="hero__content">
    <h1 class="hero__title">Оренда профессійної техніки</h1>
    <p class="hero__subtitle">Найбільший парк обладнання в регіоні</p>
    <button class="hero__cta">Отримати пропозицію</button>
  </div>
</section>
```

#### 1.3 CSS для hero-секції
```css
.hero {
  position: relative;
  height: 100vh;
  min-height: 600px;
  max-height: 120vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero__image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  z-index: 1;
}

/*優先: content-visibility для performance */
.hero__image {
  content-visibility: auto;
  contain: layout style paint;
}

.hero__overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    135deg,
    rgba(0, 0, 0, 0.6) 0%,
    rgba(255, 102, 0, 0.3) 100%
  );
  z-index: 2;
}

.hero__content {
  position: relative;
  z-index: 3;
  text-align: center;
  color: white;
  padding: 2rem;
  max-width: 600px;
}

.hero__title {
  font-size: clamp(2rem, 8vw, 4rem);
  font-weight: 700;
  margin-bottom: 1rem;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.hero__subtitle {
  font-size: clamp(1rem, 3vw, 1.5rem);
  font-weight: 300;
  margin-bottom: 2rem;
  opacity: 0.95;
}

.hero__cta {
  background: #ff6600;
  color: #000;
  border: none;
  padding: 1rem 2.5rem;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  box-shadow: 0 10px 30px rgba(255, 102, 0, 0.3);
}

.hero__cta:hover {
  background: #ff8533;
  transform: translateY(-3px);
  box-shadow: 0 15px 40px rgba(255, 102, 0, 0.4);
}

/* Mobile responsiveness */
@media (max-width: 768px) {
  .hero {
    height: 100vh;
    max-height: none;
  }
  
  .hero__overlay {
    background: linear-gradient(
      135deg,
      rgba(0, 0, 0, 0.7) 0%,
      rgba(255, 102, 0, 0.4) 100%
    );
  }
}
```

### Інноваційні підходи

#### 1.4 Parallax ефект з scroll-driven animations
```css
@supports (animation-timeline: view()) {
  .hero__image {
    animation: heroParallax linear;
    animation-timeline: view();
    animation-range: entry 0% cover 50%;
  }
  
  @keyframes heroParallax {
    from {
      transform: scale(1.1) translateY(-10%);
    }
    to {
      transform: scale(1) translateY(0);
    }
  }
}

/* Fallback для браузерів без підтримки */
@supports not (animation-timeline: view()) {
  .hero__image {
    animation: none;
  }
}
```

---

## 2. ОПТИМАЛЬНІ HOVER ЕФЕКТИ ДЛЯ КАРТОК ПРОДУКТІВ

### 2.1 Сучасна архітектура картки
```html
<article class="product-card">
  <div class="product-card__image-container">
    <img 
      src="equipment-1.jpg" 
      alt="Екскаватор JCB 3CX"
      class="product-card__image"
      loading="lazy"
      width="400"
      height="300">
    <div class="product-card__badge">Популярна модель</div>
    <button class="product-card__quick-view" aria-label="Швидкий перегляд">
      <svg><!-- eye icon --></svg>
    </button>
  </div>
  
  <div class="product-card__content">
    <h3 class="product-card__title">Екскаватор JCB 3CX</h3>
    <p class="product-card__category">Екскавуючі машини</p>
    
    <div class="product-card__specs">
      <span class="spec">Потужність: 75 кВ</span>
      <span class="spec">Глибина: 6м</span>
    </div>
    
    <div class="product-card__price">
      <span class="price">₴5,500</span>
      <span class="period">/день</span>
    </div>
    
    <button class="product-card__cta">Замовити</button>
  </div>
  
  <div class="product-card__hover-panel">
    <p class="product-card__description">
      Надійна гідравлічна система, оптимальна для земляних робіт та облаштування території.
    </p>
    <div class="product-card__actions">
      <button class="btn-secondary">Порівняти</button>
      <button class="btn-secondary">До закладок</button>
    </div>
  </div>
</article>
```

### 2.2 CSS для картки з мультишаровими ефектами
```css
.product-card {
  position: relative;
  background: #fff;
  border-radius: 0.75rem;
  overflow: hidden;
  border: 1px solid #e5e5e5;
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  cursor: pointer;
  display: flex;
  flex-direction: column;
  height: 100%;
}

/* Стиль для контейнера зображення */
.product-card__image-container {
  position: relative;
  width: 100%;
  aspect-ratio: 4/3;
  overflow: hidden;
  background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
}

.product-card__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* Zoom ефект на hover */
.product-card:hover .product-card__image {
  transform: scale(1.12) rotate(0.5deg);
}

/* Badge позиціонування */
.product-card__badge {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  background: #ff6600;
  color: #fff;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  font-size: 0.85rem;
  font-weight: 600;
  z-index: 10;
  animation: badgePulse 2s ease-in-out infinite;
}

@keyframes badgePulse {
  0%, 100% {
    box-shadow: 0 0 0 0 rgba(255, 102, 0, 0.7);
  }
  50% {
    box-shadow: 0 0 0 6px rgba(255, 102, 0, 0);
  }
}

/* Quick view button */
.product-card__quick-view {
  position: absolute;
  bottom: 1rem;
  right: 1rem;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  background: #ff6600;
  color: #fff;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transform: translate(20px, 20px) scale(0.7);
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  z-index: 8;
}

.product-card:hover .product-card__quick-view {
  opacity: 1;
  transform: translate(0, 0) scale(1);
}

/* Content блок */
.product-card__content {
  padding: 1.5rem;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.product-card__title {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0 0 0.5rem 0;
  color: #1a1a1a;
  line-height: 1.3;
}

.product-card__category {
  font-size: 0.9rem;
  color: #666;
  margin: 0 0 1rem 0;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

/* Специфікації */
.product-card__specs {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
  padding: 1rem 0;
  border-top: 1px solid #f0f0f0;
  border-bottom: 1px solid #f0f0f0;
  font-size: 0.9rem;
  color: #555;
  flex-wrap: wrap;
}

.spec {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* Ціна */
.product-card__price {
  display: flex;
  align-items: baseline;
  gap: 0.5rem;
  margin: 1rem 0;
  font-size: 1.5rem;
}

.price {
  font-weight: 700;
  color: #ff6600;
}

.period {
  font-size: 0.9rem;
  color: #666;
}

/* Основна кнопка */
.product-card__cta {
  background: linear-gradient(135deg, #ff6600 0%, #ff8533 100%);
  color: #fff;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  margin-top: auto;
}

.product-card__cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(255, 102, 0, 0.35);
}

/* Hover panel (додаткова інформація) */
.product-card__hover-panel {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(180deg, transparent, rgba(0, 0, 0, 0.95));
  color: #fff;
  padding: 2rem 1.5rem 1.5rem;
  transform: translateY(100%);
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  z-index: 5;
}

.product-card:hover .product-card__hover-panel {
  transform: translateY(0);
}

.product-card__description {
  font-size: 0.95rem;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.product-card__actions {
  display: flex;
  gap: 0.75rem;
}

.btn-secondary {
  flex: 1;
  background: transparent;
  border: 1px solid #ff6600;
  color: #ff6600;
  padding: 0.5rem;
  border-radius: 0.25rem;
  cursor: pointer;
  font-size: 0.85rem;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background: #ff6600;
  color: #fff;
}

/* Стан для мобільних */
@media (max-width: 768px) {
  .product-card__quick-view {
    opacity: 1;
    transform: translate(0, 0) scale(1);
  }
  
  .product-card__hover-panel {
    position: static;
    transform: none;
    background: #f9f9f9;
    color: #1a1a1a;
    display: none;
  }
  
  .product-card:active .product-card__hover-panel {
    display: block;
  }
}

/* Container query для адаптивної ширини картки */
@supports (container-type: inline-size) {
  .product-card {
    container-type: inline-size;
  }
  
  .product-card__title {
    font-size: clamp(1rem, 8cqw, 1.25rem);
  }
  
  @container (max-width: 250px) {
    .product-card__specs {
      flex-direction: column;
      gap: 0.5rem;
    }
  }
}
```

### 2.3 JavaScript для мікроінтеракцій
```javascript
class ProductCard {
  constructor(element) {
    this.element = element;
    this.image = element.querySelector('.product-card__image');
    this.cta = element.querySelector('.product-card__cta');
    this.quickView = element.querySelector('.product-card__quick-view');
    
    this.init();
  }
  
  init() {
    // Паралаксний ефект при русі миші
    this.element.addEventListener('mousemove', (e) => this.handleMouseMove(e));
    this.element.addEventListener('mouseleave', (e) => this.handleMouseLeave(e));
    
    // Ripple ефект на кліку
    this.cta.addEventListener('click', (e) => this.createRipple(e));
    
    // Quick view modal
    this.quickView.addEventListener('click', () => this.openQuickView());
  }
  
  handleMouseMove(e) {
    const rect = this.element.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    
    const xPercent = (x / rect.width) * 2 - 1;
    const yPercent = (y / rect.height) * 2 - 1;
    
    // Subtle rotation based on mouse position
    const rotationX = yPercent * 2;
    const rotationY = -xPercent * 2;
    
    this.element.style.transform = 
      `perspective(1000px) rotateX(${rotationX}deg) rotateY(${rotationY}deg) scale(1.02)`;
    
    // Light gradient follow
    this.element.style.background = 
      `radial-gradient(circle at ${x}px ${y}px, rgba(255,102,0,0.1), transparent)`;
  }
  
  handleMouseLeave() {
    this.element.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale(1)';
    this.element.style.background = '#fff';
  }
  
  createRipple(e) {
    const ripple = document.createElement('span');
    const rect = this.cta.getBoundingClientRect();
    const size = Math.max(rect.width, rect.height);
    const x = e.clientX - rect.left - size / 2;
    const y = e.clientY - rect.top - size / 2;
    
    ripple.style.width = ripple.style.height = size + 'px';
    ripple.style.left = x + 'px';
    ripple.style.top = y + 'px';
    ripple.classList.add('ripple');
    
    this.cta.appendChild(ripple);
    
    setTimeout(() => ripple.remove(), 600);
  }
  
  openQuickView() {
    // Модальне вікно для швидкого перегляду
    console.log('Opening quick view modal');
  }
}

// Ініціалізація всіх карток
document.querySelectorAll('.product-card').forEach(card => {
  new ProductCard(card);
});
```

---

## 3. СУЧАСНІ CSS ТЕХНІКИ

### 3.1 Container Queries
```css
/* Контейнер для карток */
.products-grid {
  container-type: inline-size;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

/* Адаптивні стилі на основі ширини контейнера */
@container (min-width: 500px) {
  .product-card__title {
    font-size: 1.5rem;
  }
  
  .product-card__content {
    padding: 2rem;
  }
}

@container (max-width: 400px) {
  .product-card__specs {
    display: none;
  }
  
  .product-card__price {
    font-size: 1.25rem;
  }
}
```

### 3.2 :has() селектор для складних взаємодій
```css
/* Змініть стиль картки, якщо у неї є badge */
.product-card:has(.product-card__badge) {
  border: 2px solid #ff6600;
  box-shadow: 0 0 20px rgba(255, 102, 0, 0.15);
}

/* Підсвітлення сусідніх карток при hover */
.product-card:has(:hover) {
  z-index: 10;
}

.products-grid > .product-card:has(~ :hover) {
  opacity: 0.7;
}

/* Активація стилів залежно від стану input */
.filter-form:has(input:checked) .apply-button {
  background: #ff6600;
  box-shadow: 0 4px 15px rgba(255, 102, 0, 0.3);
}

/* Стиль у контексту батьківського hover */
.products-section:has(.product-card:hover) .section-title {
  color: #ff6600;
}
```

### 3.3 Scroll-Driven Animations
```css
/* Анімація при скролюванні в view */
@supports (animation-timeline: view()) {
  .feature-item {
    animation: slideInUp linear;
    animation-timeline: view();
    animation-range: entry 0% cover 30%;
  }
  
  @keyframes slideInUp {
    from {
      opacity: 0;
      transform: translateY(40px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  /* Додатково складна анімація */
  .feature-item:nth-child(2) {
    animation-range: entry 20% cover 50%;
  }
  
  .feature-item:nth-child(3) {
    animation-range: entry 40% cover 70%;
  }
}

/* Parallax з scroll-timeline */
@supports (animation-timeline: view()) {
  .parallax-section {
    animation: parallaxMove linear;
    animation-timeline: view();
  }
  
  @keyframes parallaxMove {
    from {
      transform: translateY(50px);
    }
    to {
      transform: translateY(-50px);
    }
  }
}

/* Стовпчик текстури, що рухається разом зі скролом */
@supports (animation-timeline: scroll()) {
  .background-pattern {
    animation: patternScroll linear;
    animation-timeline: scroll();
  }
  
  @keyframes patternScroll {
    from {
      background-position: 0 0;
    }
    to {
      background-position: 100px 100px;
    }
  }
}
```

### 3.4 Advanced CSS Grid & Subgrid
```css
.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.product-card {
  display: grid;
  grid-template-rows: auto 1fr auto;
}

/* Subgrid для вирівнювання по колонах */
@supports (grid-template-columns: subgrid) {
  .products-grid {
    grid-template-columns: subgrid;
  }
  
  .product-card {
    grid-column: span 1;
    display: grid;
    grid-template-columns: subgrid;
  }
}
```

---

## 4. МІКРОІНТЕРАКЦІЇ ДЛЯ CONVERSION OPTIMIZATION

### 4.1 Loading States
```html
<div class="loading-skeleton">
  <div class="skeleton skeleton--image"></div>
  <div class="skeleton skeleton--title"></div>
  <div class="skeleton skeleton--text"></div>
  <div class="skeleton skeleton--button"></div>
</div>
```

```css
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

@keyframes loading {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}

.skeleton--image {
  width: 100%;
  aspect-ratio: 4/3;
  border-radius: 0.75rem;
}

.skeleton--title {
  height: 1.5rem;
  width: 80%;
  margin: 1rem 0;
  border-radius: 0.25rem;
}

.skeleton--text {
  height: 1rem;
  width: 100%;
  margin: 0.5rem 0;
  border-radius: 0.25rem;
}

.skeleton--button {
  height: 2.5rem;
  width: 100%;
  margin-top: 1rem;
  border-radius: 0.5rem;
}
```

### 4.2 Button Feedback Animations
```css
.btn {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

/* Ripple effect */
.btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.btn:active::before {
  width: 300px;
  height: 300px;
}

/* Success state */
.btn.success {
  background: #4caf50;
  animation: successPulse 0.6s ease-out;
}

@keyframes successPulse {
  0% {
    box-shadow: 0 0 0 0 rgba(76, 175, 80, 0.7);
  }
  50% {
    box-shadow: 0 0 0 10px rgba(76, 175, 80, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(76, 175, 80, 0);
  }
}

/* Error state */
.btn.error {
  background: #f44336;
  animation: errorShake 0.4s ease-in-out;
}

@keyframes errorShake {
  0%, 100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-5px);
  }
  75% {
    transform: translateX(5px);
  }
}
```

### 4.3 Scroll Reveal Components
```javascript
class ScrollReveal {
  constructor() {
    this.elements = document.querySelectorAll('[data-reveal]');
    this.setupIntersectionObserver();
  }
  
  setupIntersectionObserver() {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.revealElement(entry.target);
          observer.unobserve(entry.target);
        }
      });
    }, {
      threshold: 0.1,
      rootMargin: '0px 0px -50px 0px'
    });
    
    this.elements.forEach(el => observer.observe(el));
  }
  
  revealElement(element) {
    element.classList.add('revealed');
    element.style.animationDelay = element.dataset.delay || '0s';
  }
}

// Ініціалізація
new ScrollReveal();
```

```css
[data-reveal] {
  opacity: 0;
  animation: revealUp 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

@keyframes revealUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

[data-reveal].revealed {
  animation-play-state: running;
}
```

### 4.4 Form Interactions
```html
<div class="form-group">
  <input 
    type="text" 
    id="name"
    class="form-input"
    placeholder="Ваше імʼя"
    required>
  <label for="name" class="form-label">Ваше імʼя</label>
  <span class="form-error" data-error="required">Поле обов'язкове</span>
</div>
```

```css
.form-group {
  position: relative;
  margin-bottom: 1.5rem;
}

.form-input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 2px solid #e5e5e5;
  border-radius: 0.5rem;
  font-size: 1rem;
  transition: all 0.3s ease;
  background: #fff;
}

.form-input:focus {
  outline: none;
  border-color: #ff6600;
  box-shadow: 0 0 0 3px rgba(255, 102, 0, 0.1);
  background: #fafafa;
}

.form-label {
  position: absolute;
  left: 1rem;
  top: 0.75rem;
  background: #fff;
  padding: 0 0.25rem;
  color: #666;
  font-size: 1rem;
  font-weight: 500;
  transition: all 0.3s ease;
  pointer-events: none;
}

/* Анімація label при фокусі та заповненні */
.form-input:focus ~ .form-label,
.form-input:not(:placeholder-shown) ~ .form-label {
  top: -0.75rem;
  font-size: 0.85rem;
  color: #ff6600;
}

.form-error {
  position: absolute;
  bottom: -1.5rem;
  left: 0;
  font-size: 0.85rem;
  color: #f44336;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.form-input:invalid:not(:focus) ~ .form-error[data-error="required"] {
  opacity: 1;
}

/* Success state */
.form-input.success {
  border-color: #4caf50;
}

.form-input.success ~ .form-label {
  color: #4caf50;
}
```

---

## 5. PERFORMANCE ОПТИМІЗАЦІЇ ДЛЯ САЙТІВ З БАГАТЬМА ЗОБРАЖЕННЯМИ

### 5.1 Image Optimization Strategy
```html
<!-- Responsive images з modern форматами -->
<picture>
  <!-- WebP для сучасних браузерів -->
  <source 
    srcset="
      image-400w.webp 400w,
      image-800w.webp 800w,
      image-1200w.webp 1200w"
    sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 33vw"
    type="image/webp">
  
  <!-- AVIF для найкращої компресії -->
  <source 
    srcset="
      image-400w.avif 400w,
      image-800w.avif 800w,
      image-1200w.avif 1200w"
    sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 33vw"
    type="image/avif">
  
  <!-- Fallback JPEG -->
  <img 
    src="image-800w.jpg"
    alt="Equipment rental showcase"
    loading="lazy"
    decoding="async"
    width="800"
    height="600"
    srcset="image-400w.jpg 400w, image-800w.jpg 800w">
</picture>
```

### 5.2 Lazy Loading + Intersection Observer
```javascript
class ImageOptimizer {
  constructor() {
    this.images = document.querySelectorAll('img[data-src]');
    this.initIntersectionObserver();
    this.initErrorHandling();
  }
  
  initIntersectionObserver() {
    const imageObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.loadImage(entry.target);
          imageObserver.unobserve(entry.target);
        }
      });
    }, {
      rootMargin: '50px 0px',
      threshold: 0.01
    });
    
    this.images.forEach(img => imageObserver.observe(img));
  }
  
  loadImage(img) {
    const src = img.dataset.src;
    const srcset = img.dataset.srcset;
    
    // Preload image
    const tempImg = new Image();
    tempImg.onload = () => {
      img.src = src;
      if (srcset) img.srcset = srcset;
      img.classList.add('loaded');
      img.removeAttribute('data-src');
      img.removeAttribute('data-srcset');
    };
    tempImg.onerror = () => {
      img.classList.add('error');
    };
    tempImg.src = src;
  }
  
  initErrorHandling() {
    this.images.forEach(img => {
      img.addEventListener('error', () => {
        img.classList.add('error');
        img.src = 'fallback.jpg';
      });
    });
  }
}

// Ініціалізація
new ImageOptimizer();
```

### 5.3 CSS для lazy loaded images
```css
img[data-src] {
  background: linear-gradient(
    90deg,
    #e5e5e5 0%,
    #f0f0f0 50%,
    #e5e5e5 100%
  );
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
  opacity: 0.5;
}

img.loaded {
  animation: fadeIn 0.4s ease-out forwards;
  background: none;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

img.error {
  opacity: 0.3;
}
```

### 5.4 CDN + Service Worker для Image Caching
```javascript
// service-worker.js
const CACHE_NAME = 'images-v1';
const IMAGE_URLS = [
  '/images/hero-desktop-1920w.webp',
  '/images/hero-tablet-1024w.webp',
  '/images/hero-mobile-375w.webp'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => {
      return cache.addAll(IMAGE_URLS);
    })
  );
});

self.addEventListener('fetch', event => {
  // Тільки для image запитів
  if (!event.request.url.includes('/images/')) {
    return;
  }
  
  event.respondWith(
    caches.match(event.request).then(response => {
      return response || fetch(event.request).then(response => {
        // Cache new images on the fly
        return caches.open(CACHE_NAME).then(cache => {
          cache.put(event.request, response.clone());
          return response;
        });
      }).catch(() => {
        return caches.match('/images/fallback.jpg');
      });
    })
  );
});
```

### 5.5 Стратегія кеширования HTTP headers
```
# Рекомендації для .htaccess або nginx.conf

# Кеш для зображень (30 днів)
<FilesMatch "\\.(jpg|jpeg|png|gif|webp|avif)$">
  Header set Cache-Control "public, max-age=2592000"
  Header set Expires "Wed, 20 Mar 2027 04:00:00 GMT"
</FilesMatch>

# Кеш для CSS/JS (14 днів)
<FilesMatch "\\.(css|js)$">
  Header set Cache-Control "public, max-age=1209600"
</FilesMatch>

# HTML без кеша (проверка при кожному запиті)
<FilesMatch "\\.html$">
  Header set Cache-Control "no-cache, must-revalidate"
</FilesMatch>
```

### 5.6 Progressive Image Loading
```javascript
class ProgressiveImageLoader {
  constructor() {
    this.setupMutationObserver();
  }
  
  setupMutationObserver() {
    const observer = new MutationObserver((mutations) => {
      mutations.forEach(mutation => {
        if (mutation.type === 'childList') {
          mutation.addedNodes.forEach(node => {
            if (node.nodeType === 1 && node.querySelector) {
              const newImages = node.querySelectorAll('img[data-src]');
              newImages.forEach(img => this.prepareImage(img));
            }
          });
        }
      });
    });
    
    observer.observe(document.body, {
      childList: true,
      subtree: true
    });
  }
  
  prepareImage(img) {
    // Завантажити низькорозрізняючу версію спочатку
    const blurSrc = img.dataset.blur;
    if (blurSrc) {
      const blurImg = new Image();
      blurImg.src = blurSrc;
      blurImg.onload = () => {
        img.style.backgroundImage = `url(${blurSrc})`;
        img.classList.add('blur-loaded');
      };
    }
    
    // Потім завантажити повну версію
    const fullSrc = img.dataset.src;
    const fullImg = new Image();
    fullImg.src = fullSrc;
    fullImg.onload = () => {
      img.src = fullSrc;
      img.classList.add('full-loaded');
    };
  }
}

new ProgressiveImageLoader();
```

---

## 6. ТЕХНІЧНІ РЕКОМЕНДАЦІЇ ARCHITETURE

### 6.1 BEM Methodology для чорно-помаранчевої гами
```css
/* Root variables */
:root {
  /* Основні кольори */
  --color-primary-orange: #ff6600;
  --color-orange-light: #ff8533;
  --color-orange-dark: #e55a00;
  
  --color-primary-black: #1a1a1a;
  --color-black-dark: #0d0d0d;
  --color-black-light: #2d2d2d;
  
  /* Neutral */
  --color-white: #ffffff;
  --color-gray-light: #f5f5f5;
  --color-gray-medium: #e5e5e5;
  --color-gray-dark: #666666;
  
  /* Shadows */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 12px 24px rgba(0, 0, 0, 0.2);
  --shadow-orange: 0 4px 15px rgba(255, 102, 0, 0.3);
  
  /* Typography */
  --font-family-sans: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-family-mono: 'Courier New', monospace;
  
  /* Spacing */
  --spacing-xs: 0.5rem;
  --spacing-sm: 1rem;
  --spacing-md: 1.5rem;
  --spacing-lg: 2rem;
  --spacing-xl: 3rem;
  
  /* Transitions */
  --transition-fast: 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);
  --transition-normal: 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  --transition-slow: 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* BEM структура */
.section {
  background: var(--color-white);
}

.section__container {
  max-width: 1280px;
  margin: 0 auto;
  padding: var(--spacing-lg);
}

.section__title {
  font-size: 2.5rem;
  color: var(--color-primary-black);
  margin-bottom: var(--spacing-lg);
}

.section__subtitle {
  font-size: 1.25rem;
  color: var(--color-primary-orange);
  margin-bottom: var(--spacing-md);
}

.section--dark {
  background: var(--color-primary-black);
  color: var(--color-white);
}

.section--dark .section__title {
  color: var(--color-white);
}

.section--dark .section__subtitle {
  color: var(--color-orange-light);
}
```

### 6.2 Performance Checklist
```
✅ HTML
- [ ] Семантичний HTML5
- [ ] Правильні meta tags (viewport, OG tags, Twitter Card)
- [ ] Структурована розмітка (Schema.org)
- [ ] Доступність (ARIA labels)

✅ CSS
- [ ] CSS Grid & Flexbox замість float
- [ ] CSS Variables для кольорів
- [ ] Мінімізація !important
- [ ] Mobile-first approach
- [ ] Crítica CSS inline

✅ JavaScript
- [ ] Code splitting (lazy loading)
- [ ] Debounce для resize/scroll евентів
- [ ] Memory leaks prevention
- [ ] Async/await замість callbacks

✅ Зображення
- [ ] WebP & AVIF формати
- [ ] Responsive images (srcset)
- [ ] Lazy loading (loading="lazy")
- [ ] CDN для distribution
- [ ] Compression (ImageOptim, Squoosh)

✅ Сервер
- [ ] Gzip/Brotli compression
- [ ] Cache-Control headers
- [ ] HTTP/2 Push для critical assets
- [ ] CORS налаштування
```

---

## 7. ГОТОВІ CODE SNIPPETS ДЛЯ ВПРОВАДЖЕННЯ

### 7.1 Пакет依賴для проекту
```json
{
  "devDependencies": {
    "autoprefixer": "^10.4.17",
    "postcss": "^8.4.32",
    "postcss-preset-env": "^9.1.4",
    "cssnano": "^6.0.2",
    "imagemin": "^8.0.1",
    "imagemin-webp": "^8.0.0",
    "webpack": "^5.89.0",
    "webpack-cli": "^5.1.4"
  }
}
```

### 7.2 PostCSS конфіг
```javascript
module.exports = {
  plugins: {
    'postcss-preset-env': {
      stage: 3,
      features: {
        'nesting-rules': true,
        'custom-properties': true
      }
    },
    autoprefixer: {},
    cssnano: {
      preset: ['default', {
        discardComments: {
          removeAll: true,
        },
      }]
    }
  }
};
```

---

## ВИХІДНІ ВИСНОВКИ

1. **Hero секції** - Використовуйте WebP/AVIF, scroll-driven animations, градієнтні overlay
2. **Картки продуктів** - Комбінуйте transform, opacity, scroll-reveal для мікроінтеракцій
3. **CSS техніки** - Container queries для адаптивності, :has() для контексту, scroll-timeline
4. **Мікроінтеракції** - Skeleton loaders, ripple buttons, form label animations
5. **Performance** - Lazy loading, responsive images, service workers, CDN
6. **Архітектура** - BEM, CSS variables, mobile-first, 80+ Lighthouse score

**Target Metrics:**
- Lighthouse Performance: 90+
- First Contentful Paint (FCP): < 1.5s
- Largest Contentful Paint (LCP): < 2.5s
- Cumulative Layout Shift (CLS): < 0.1
- Time to Interactive (TTI): < 3.5s
