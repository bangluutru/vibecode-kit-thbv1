# 📱 Mobile Web Gotchas

> Tổng hợp các lỗi phổ biến khi phát triển web app trên iOS Safari & Android Chrome, kèm fix.

## Contents

| File | Purpose |
|------|---------|
| `ios-safari-bugs.md` | Tất cả iOS Safari web bugs & fixes |
| `android-chrome-bugs.md` | Android Chrome specific issues |
| `responsive-checklist.md` | Checklist test responsive trên thiết bị thật |

## Tại sao cần file này?

Chrome DevTools responsive mode **KHÔNG phát hiện được** nhiều bugs chỉ xảy ra trên thiết bị thật:
- iOS auto-zoom trên form inputs
- iOS 100vh bị address bar che
- iOS `position: fixed` khi virtual keyboard mở
- Android soft keyboard đẩy layout
- Touch event timing khác mouse event

## Usage

Referenced by:
- `rules/frontend.md` (Section 2.1)
- `frontend-specialist` agent
- `accessibility-auditor` agent
- `test-engineer` agent (responsive testing)
