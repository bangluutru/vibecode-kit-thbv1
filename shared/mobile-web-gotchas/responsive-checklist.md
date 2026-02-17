# 📱 Responsive Testing Checklist — Thiết bị thật

> Checklist kiểm tra responsive TRÊN THIẾT BỊ THẬT. Chrome DevTools responsive mode **KHÔNG đủ** để phát hiện tất cả bugs.

---

## Tại sao phải test trên thiết bị thật?

Chrome DevTools **KHÔNG** mô phỏng:
- ❌ iOS Safari auto-zoom trên input (bug #1)
- ❌ iOS address bar ẩn/hiện (100vh bug)
- ❌ Virtual keyboard behavior
- ❌ Touch event timing thực tế
- ❌ Performance thực tế (CPU/RAM hạn chế)
- ❌ Network conditions thực tế (4G/5G)
- ❌ Safe area insets thực tế (notch, Dynamic Island)

---

## Thiết bị test tối thiểu

| Thiết bị | Viewport | Đại diện cho |
|----------|----------|-------------|
| iPhone SE | 375×667 | Màn hình nhỏ nhất phổ biến |
| iPhone 14/15 | 390×844 | iPhone mainstream |
| iPhone 14/15 Pro Max | 430×932 | iPhone lớn nhất |
| Samsung Galaxy S23 | 360×780 | Android mainstream |
| iPad | 810×1080 | Tablet |

### Không có thiết bị thật?

| Tool | Platform | Mức độ tin cậy |
|------|----------|---------------|
| **BrowserStack** | iOS + Android | ⭐⭐⭐⭐⭐ Tốt nhất |
| **LambdaTest** | iOS + Android | ⭐⭐⭐⭐ |
| **Xcode Simulator** | iOS only | ⭐⭐⭐ (tốt cho layout, kém cho touch) |
| **Android Studio Emulator** | Android only | ⭐⭐⭐ |
| **Chrome DevTools** | Chỉ viewport | ⭐⭐ (chỉ để dev, KHÔNG dùng để QA) |

---

## Checklist Test Responsive

### Phase 1: Layout (5 phút)

Mở app trên mỗi thiết bị test:

- [ ] **Không scroll ngang** (horizontal scroll = broken layout)
- [ ] **Text không bị cắt** hoặc tràn ra ngoài
- [ ] **Ảnh không bị overflow** (max-width: 100%)
- [ ] **Spacing hợp lý** (không quá chật trên mobile)
- [ ] **Navigation hoạt động** (hamburger menu, tab bar)
- [ ] **Footer không che content**
- [ ] **Modals/popups không bị tràn** viewport

### Phase 2: Forms — iOS Focus (5 phút)

Trên **iPhone Safari**:

- [ ] Tap vào input → **KHÔNG bị auto-zoom**
- [ ] Nhập text → keyboard không che input
- [ ] Chuyển giữa các input → layout không nhảy
- [ ] Submit form → hoạt động đúng
- [ ] Select dropdown → hiển thị đúng
- [ ] Date picker → sử dụng native iOS picker
- [ ] Autofill → tên/email/password fill đúng

### Phase 3: Forms — Android Focus (5 phút)

Trên **Android Chrome**:

- [ ] Tap vào input → keyboard mở smooth
- [ ] Layout **không bị push up** quá mức
- [ ] Quay landscape → form vẫn đúng
- [ ] Back button → đóng keyboard (không back page)
- [ ] Autofill hoạt động

### Phase 4: Scroll & Touch (3 phút)

- [ ] Scroll smooth (không lag, không stuck)
- [ ] Scroll trong modal → không scroll page bên dưới
- [ ] Pull-to-refresh → disabled nếu web app
- [ ] Swipe gestures → không conflict với browser gestures
- [ ] Long-press → không select text không mong muốn

### Phase 5: Visual (3 phút)

- [ ] Font render đúng (không fallback font xấu)
- [ ] Dark mode (nếu support) → không có text vô hình
- [ ] Safe areas → content không bị notch che
- [ ] Bottom → Home Indicator không che button
- [ ] Landscape mode → layout không vỡ

### Phase 6: Performance (3 phút)

- [ ] First load dưới 3s (test trên 4G thật)
- [ ] Scroll 60fps (không judder)
- [ ] Tap response < 100ms
- [ ] Không bị crash sau 5 phút sử dụng

---

## Quick Test Script

Dùng terminal để test responsive nhanh:

```bash
# ✅ Lighthouse mobile audit
npx lighthouse https://your-app.com \
  --preset=perf \
  --emulated-form-factor=mobile \
  --output=html \
  --output-path=./lighthouse-mobile.html

# ✅ Check font-size input (should be ≥ 16px)
# Paste vào Console trên Safari DevTools:
# document.querySelectorAll('input, select, textarea').forEach(el => {
#   const size = parseFloat(getComputedStyle(el).fontSize);
#   if (size < 16) console.warn(`⚠️ ${el.tagName}#${el.id}: font-size=${size}px (< 16px)`);
# });
```

---

## Template báo cáo test

```markdown
## Responsive Test Report — [Date]

### Tester: [Name]
### URL: [URL]

| Test | iPhone SE | iPhone 15 | Galaxy S23 | iPad |
|------|-----------|-----------|------------|------|
| Layout OK | ✅/❌ | | | |
| Form zoom | ✅/❌ | | | |
| Keyboard | ✅/❌ | | | |
| Scroll | ✅/❌ | | | |
| Performance | ✅/❌ | | | |

### Issues Found:
1. [Issue description + screenshot]
2. [Issue description + screenshot]
```
