# 🍎 iOS Safari Web Bugs & Fixes

> Danh sách đầy đủ các bugs trên iOS Safari khi phát triển web app, kèm CSS/JS fix.

---

## 1. 🔍 Auto-Zoom Form Input (CRITICAL)

### Bug
Khi `font-size` của `<input>`, `<select>`, `<textarea>` nhỏ hơn **16px**, iOS Safari tự động zoom vào field đó khi user tap. Sau khi nhập xong, page **không tự zoom out**, gây tràn layout.

### Ảnh hưởng
- Form tràn ra ngoài viewport
- User phải pinch-to-zoom out thủ công
- Layout bị vỡ hoàn toàn trên iPhone

### Fix (CSS)
```css
/* ✅ Cách đúng: Set font-size ≥ 16px cho ALL form elements */
input,
select,
textarea,
input[type="text"],
input[type="email"],
input[type="password"],
input[type="number"],
input[type="search"],
input[type="tel"],
input[type="url"],
input[type="date"],
input[type="time"],
input[type="datetime-local"] {
  font-size: 16px;   /* MINIMUM 16px — không được nhỏ hơn */
  font-size: max(16px, 1rem); /* Responsive alternative */
}

/* Nếu design cần text nhỏ hơn, dùng transform scale */
.small-input {
  font-size: 16px;                    /* iOS vẫn thấy 16px */
  transform: scale(0.875);            /* Hiển thị như 14px */
  transform-origin: left top;
  width: calc(100% / 0.875);          /* Bù lại width */
}
```

### ❌ Cách sai (KHÔNG BAO GIỜ LÀM)
```html
<!-- ❌ Disable zoom = vi phạm WCAG 2.2 accessibility -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">

<!-- ✅ Cách đúng -->
<meta name="viewport" content="width=device-width, initial-scale=1">
```

---

## 2. 📐 100vh Bug

### Bug
`100vh` trên iOS Safari bao gồm cả address bar và toolbar. Khi address bar hiển thị, nội dung bị che khuất phía dưới. Khi scroll, address bar thu nhỏ, layout nhảy.

### Fix
```css
/* ✅ Dùng dvh (dynamic viewport height) */
.fullscreen {
  height: 100dvh;  /* Tự điều chỉnh theo address bar */
}

/* Fallback cho browser cũ */
.fullscreen {
  height: 100vh;        /* Fallback */
  height: 100dvh;       /* Override nếu support */
}

/* Alternative: Dùng CSS variable từ JS */
.fullscreen {
  height: calc(var(--vh, 1vh) * 100);
}
```

```javascript
// JS fallback: Tính viewport height thực tế
function setVH() {
  const vh = window.innerHeight * 0.01;
  document.documentElement.style.setProperty('--vh', `${vh}px`);
}
window.addEventListener('resize', setVH);
window.addEventListener('orientationchange', setVH);
setVH();
```

---

## 3. ⌨️ Virtual Keyboard + Fixed Position

### Bug
Khi virtual keyboard mở trên iOS, `position: fixed` elements (bottom nav, floating button) bị đẩy lên trên keyboard hoặc bị ẩn. Layout bị vỡ.

### Fix
```css
/* ✅ Dùng position: sticky thay fixed cho bottom elements */
.bottom-bar {
  position: sticky;
  bottom: 0;
}

/* Hoặc detect keyboard với visualViewport API */
```

```javascript
// ✅ Detect virtual keyboard
if (window.visualViewport) {
  window.visualViewport.addEventListener('resize', () => {
    const keyboardHeight = window.innerHeight - window.visualViewport.height;
    const isKeyboardOpen = keyboardHeight > 100;

    document.body.classList.toggle('keyboard-open', isKeyboardOpen);

    // Ẩn bottom bar khi keyboard mở
    const bottomBar = document.querySelector('.bottom-bar');
    if (bottomBar) {
      bottomBar.style.display = isKeyboardOpen ? 'none' : 'flex';
    }
  });
}
```

```css
/* CSS cho keyboard state */
body.keyboard-open .bottom-bar {
  display: none !important;
}

body.keyboard-open .main-content {
  padding-bottom: 0;
}
```

---

## 4. 🔄 Overscroll / Bounce Effect

### Bug
iOS Safari có bounce effect khi scroll quá đầu hoặc cuối trang. Trong web app, điều này có thể trigger pull-to-refresh hoặc gây UX tệ.

### Fix
```css
/* ✅ Disable overscroll cho toàn trang */
html {
  overscroll-behavior: none;
}

/* ✅ Cho phép scroll bên trong container nhưng không bounce ra ngoài */
.scroll-container {
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;  /* Smooth scroll */
  overscroll-behavior: contain;        /* Không ảnh hưởng parent */
}
```

---

## 5. 📎 Position Sticky trong Scroll Container

### Bug
`position: sticky` không hoạt động nếu parent có `overflow: hidden` hoặc `overflow: auto`.

### Fix
```css
/* ❌ Sticky không work */
.parent {
  overflow: hidden; /* ← Nguyên nhân */
}
.parent .sticky-header {
  position: sticky;
  top: 0;
}

/* ✅ Fix: Dùng overflow: clip thay overflow: hidden */
.parent {
  overflow: clip;   /* Hỗ trợ Safari 16+ */
}

/* ✅ Hoặc: Đưa sticky element ra ngoài overflow container */
```

---

## 6. 🎨 Safe Area Insets (Notch / Dynamic Island)

### Bug
Nội dung bị che bởi notch, Dynamic Island, hoặc Home Indicator nếu không khai báo safe area.

### Fix
```html
<!-- ✅ Trong <head> -->
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

```css
/* ✅ Áp dụng safe area insets */
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* ✅ Cho bottom nav bar */
.bottom-nav {
  padding-bottom: env(safe-area-inset-bottom);
}

/* ✅ Dùng max() để đảm bảo padding tối thiểu */
.bottom-nav {
  padding-bottom: max(16px, env(safe-area-inset-bottom));
}
```

---

## 7. 🔗 Tap Highlight & Touch Delay

### Bug
iOS thêm gray overlay khi tap, và có 300ms delay cho double-tap-to-zoom.

### Fix
```css
/* ✅ Remove tap highlight */
* {
  -webkit-tap-highlight-color: transparent;
}

/* ✅ Remove 300ms tap delay */
/* Đã tự động fix nếu có viewport meta đúng */
html {
  touch-action: manipulation; /* Disable double-tap zoom, giữ pinch zoom */
}
```

---

## 8. 📸 Image Rendering

### Bug
iOS Safari render ảnh lớn khác browser khác. HEIC không được hỗ trợ trên web.

### Fix
```html
<!-- ✅ Responsive images với format fallback -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" loading="lazy" decoding="async">
</picture>
```

```css
/* ✅ Prevent image overflow */
img {
  max-width: 100%;
  height: auto;
}
```

---

## 9. 🎭 @supports & Feature Detection

Dùng feature detection thay vì user-agent sniffing:

```css
/* ✅ Check for dvh support */
@supports (height: 100dvh) {
  .full-height {
    height: 100dvh;
  }
}

/* ✅ Check for safe-area support */
@supports (padding-bottom: env(safe-area-inset-bottom)) {
  .bottom-bar {
    padding-bottom: env(safe-area-inset-bottom);
  }
}

/* ✅ iOS-specific: Check for -webkit-touch-callout */
@supports (-webkit-touch-callout: none) {
  /* iOS Safari only styles */
}
```

---

## 10. 📋 iOS Safari Master Checklist

Trước khi release web app, test tất cả trên **iPhone thật** (không chỉ simulator):

### Form & Input
- [ ] Tất cả input `font-size ≥ 16px` (no auto-zoom)
- [ ] Keyboard type đúng (`type="email"`, `type="tel"`, `inputmode="numeric"`)
- [ ] Autocomplete/autofill hoạt động (`autocomplete="email"`)
- [ ] Form submit hoạt động khi nhấn "Done" trên keyboard

### Layout
- [ ] Dùng `100dvh` thay `100vh`
- [ ] Safe area insets áp dụng đúng
- [ ] `position: fixed` không bị keyboard đẩy
- [ ] Scroll smooth, không stuck
- [ ] Landscape mode không bị vỡ layout

### Visual
- [ ] Ảnh không bị overflow
- [ ] Font render đúng (system font hoặc loaded)
- [ ] Dark mode hoạt động (nếu support)
- [ ] Notch/Dynamic Island không che content

### Performance
- [ ] First load < 3s trên 4G
- [ ] Scroll smooth (60fps)
- [ ] No memory leaks (test 5 phút sử dụng liên tục)
- [ ] Images lazy-loaded

### Interaction
- [ ] Touch targets ≥ 44x44px
- [ ] No 300ms tap delay
- [ ] Swipe back (edge swipe) không conflict với app gestures
- [ ] Pull-to-refresh disabled (nếu web app)
