# 🤖 Android Chrome Web Bugs & Fixes

> Các bugs phổ biến trên Android Chrome khi phát triển web app.

---

## 1. ⌨️ Virtual Keyboard Resize

### Bug
Trên Android, khi virtual keyboard mở, `window.innerHeight` thay đổi (giảm). Điều này khác iOS (iOS không thay đổi innerHeight).

### Fix
```javascript
// ✅ Dùng visualViewport API (hoạt động cả iOS & Android)
if (window.visualViewport) {
  window.visualViewport.addEventListener('resize', () => {
    const height = window.visualViewport.height;
    document.documentElement.style.setProperty('--viewport-height', `${height}px`);
  });
}
```

```css
.full-height {
  height: var(--viewport-height, 100dvh);
}
```

---

## 2. 🎨 Address Bar Color

### Bug
Android Chrome address bar màu trắng mặc định, không match với app design.

### Fix
```html
<!-- ✅ Theme color cho address bar -->
<meta name="theme-color" content="#1a1a2e">

<!-- ✅ Dark mode aware -->
<meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#1a1a2e" media="(prefers-color-scheme: dark)">
```

---

## 3. 🔤 Font Rendering

### Bug
Android render font khác iOS. System font là Roboto, không phải SF Pro.

### Fix
```css
/* ✅ Cross-platform font stack */
body {
  font-family: 
    -apple-system,           /* iOS Safari */
    BlinkMacSystemFont,      /* Chrome Mac */
    'Segoe UI',              /* Windows */
    Roboto,                  /* Android */
    'Helvetica Neue',        /* Older iOS */
    Arial,                   /* Fallback */
    sans-serif;
}

/* Hoặc dùng Google Fonts để đồng nhất */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
body {
  font-family: 'Inter', sans-serif;
}
```

---

## 4. 📱 Back Button & Navigation

### Bug
Android có nút Back cứng/gesture. Nếu web app dùng SPA routing, nút Back có thể thoát app thay vì navigate back.

### Fix
```javascript
// ✅ Handle Android back button trong SPA
window.addEventListener('popstate', (event) => {
  // Custom back navigation logic
  if (isModalOpen) {
    closeModal();
    event.preventDefault();
  }
});

// Push state khi mở modal để back button đóng modal
function openModal() {
  history.pushState({ modal: true }, '');
  // Show modal
}
```

---

## 5. 🎯 Touch Event Differences

### Bug
Android touch events có thể khác iOS:
- `touchstart` → `touchmove` threshold khác
- Scroll chaining khác

### Fix
```css
/* ✅ Consistent touch behavior */
.draggable {
  touch-action: none;           /* Disable all browser touch actions */
}

.horizontal-scroll {
  touch-action: pan-x;          /* Only horizontal scroll */
}

.vertical-scroll {
  touch-action: pan-y;          /* Only vertical scroll */
}
```

---

## 6. 📋 Android Chrome Checklist

- [ ] `theme-color` meta tag set
- [ ] Font stack includes Roboto
- [ ] Virtual keyboard resize handled
- [ ] Back button/gesture handled in SPA
- [ ] `touch-action` set on interactive elements
- [ ] No layout shift khi keyboard mở/đóng
- [ ] PWA manifest.json configured (nếu applicable)
- [ ] File upload works (`<input type="file">`)
- [ ] Camera/GPS permissions handled gracefully
