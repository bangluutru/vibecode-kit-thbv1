---
trigger: glob
glob: "**/*.{js,jsx,ts,tsx,css,scss,html,vue,svelte,dart,swift,kt,xml}"
---

# FRONTEND.MD - Client-Side Mastery

> **Mục tiêu**: Quản lý thống nhất Giao diện Web & Mobile. Một nguồn chân lý cho trải nghiệm người dùng.

---

## 🎨 1. PREMIUM UX/UI (Ref: .shared/ui-ux-pro-max/AESTHETICS.md)

1. **Aesthetics**: Màu sắc HSL tinh chỉnh, Typography chuẩn (Inter/Roboto). Tuân thủ Glassmorphism 2.0.
2. **Spacing**: Hệ thống lưới 4px/8px. Whitespace là sang trọng.
3. **Feedback**: Mọi tương tác (Click, Tap) đều phải có phản hồi thị giác ngay lập tức (Framer Motion).

---

## 📱 2. MOBILE & RESPONSIVE

1. **Touch Targets**: Button tối thiểu 44x44px (Chuẩn ngón tay cái).
2. **Safe Areas**: Tôn trọng tai thỏ (Notch) và Home Indicator trên iOS/Android.
3. **Mobile-First**: Code CSS cho mobile trước, override cho PC sau.
4. **Viewport**: Luôn có `<meta name="viewport" content="width=device-width, initial-scale=1">`.
5. **iOS Zoom Prevention**: Input/select/textarea font-size ≥ 16px (bắt buộc). Xem chi tiết bên dưới.

---

## 🍎 2.1 iOS SAFARI WEB — CRITICAL RULES

> **Bug phổ biến nhất**: iPhone tự zoom khi focus vào input có font-size < 16px, gây tràn layout.

### MUST DO:
```css
/* ✅ BẮT BUỘC: Tất cả form elements phải ≥ 16px */
input, select, textarea {
  font-size: 16px; /* hoặc max(16px, 1rem) */
}

/* ✅ Safe area cho notch & home indicator */
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* ✅ Fix 100vh trên iOS (address bar ẩn/hiện) */
.full-height {
  height: 100dvh; /* dynamic viewport height */
}

/* ✅ Momentum scrolling cho container scroll */
.scroll-container {
  -webkit-overflow-scrolling: touch;
  overscroll-behavior: contain;
}

/* ✅ Prevent pull-to-refresh trên web app */
html {
  overscroll-behavior: none;
}
```

### NEVER DO:
```css
/* ❌ KHÔNG BAO GIỜ disable zoom bằng viewport */
/* <meta name="viewport" content="maximum-scale=1"> ← VI PHẠM A11Y */

/* ❌ KHÔNG dùng font-size < 16px cho input */
input { font-size: 14px; } /* → iPhone sẽ auto-zoom */

/* ❌ KHÔNG dùng 100vh cho full-screen trên iOS */
.container { height: 100vh; } /* → bị che bởi address bar */
```

### Checklist iOS Safari Web:
- [ ] Tất cả `<input>`, `<select>`, `<textarea>` có `font-size ≥ 16px`
- [ ] Dùng `100dvh` thay `100vh` cho full-screen layout
- [ ] Safe area insets được áp dụng (`env(safe-area-inset-*)`)
- [ ] Không disable user zoom (vi phạm WCAG)
- [ ] `position: fixed` hoạt động đúng khi keyboard mở
- [ ] Scroll không bị stuck/bounce không mong muốn
- [ ] Test thực tế trên iPhone Safari (không chỉ Chrome DevTools)

> **Ref**: `.shared/mobile-web-gotchas/` — Danh sách đầy đủ iOS/Android web bugs & fixes.

---

## ⚡ 3. PERFORMANCE DOMAIN

1. **Core Web Vitals**: LCP < 2.5s, CLS < 0.1, FID < 100ms.
2. **Optimistic UI**: Cập nhật giao diện TRƯỚC khi API trả về (Zalo/Facebook style).
3. **Asset Optimization**: Ảnh WebP, Video lazy-load.

---

## 🛡️ 4. STATE & COMPONENT

1. **Atomic Design**: Component nhỏ, tái sử dụng cao (`<Button />`, `<INPUT />`).
2. **State**: Server State (TanStack Query) !== Client State (Zustand/Context). Tách biệt rõ ràng.
