---
description: Sợ bug khi sửa code? Viết test tự động.
---

# /test - Test Generation and Execution

$ARGUMENTS

---

## Purpose

This command generates tests, runs existing tests, or checks test coverage.

---

## Sub-commands

```
/test                - Run all tests
/test [file/feature] - Generate tests for specific target
/test coverage       - Show test coverage report
/test watch          - Run tests in watch mode
```

---

## Behavior

### Generate Tests

When asked to test a file or feature:

1. **Analyze the code**
   - Identify functions and methods
   - Find edge cases
   - Detect dependencies to mock

2. **Generate test cases**
   - Happy path tests
   - Error cases
   - Edge cases
   - Integration tests (if needed)

3. **Write tests**
   - Use project's test framework (Jest, Vitest, etc.)
   - Follow existing test patterns
   - Mock external dependencies

---

## Output Format

### For Test Generation

```markdown
## 🧪 Tests: [Target]

### Test Plan
| Test Case | Type | Coverage |
|-----------|------|----------|
| Should create user | Unit | Happy path |
| Should reject invalid email | Unit | Validation |
| Should handle db error | Unit | Error case |

### Generated Tests

`tests/[file].test.ts`

[Code block with tests]

---

Run with: `npm test`
```

### For Test Execution

```
🧪 Running tests...

✅ auth.test.ts (5 passed)
✅ user.test.ts (8 passed)
❌ order.test.ts (2 passed, 1 failed)

Failed:
  ✗ should calculate total with discount
    Expected: 90
    Received: 100

Total: 15 tests (14 passed, 1 failed)
```

---

## Examples

```
/test src/services/auth.service.ts
/test user registration flow
/test coverage
/test fix failed tests
```

---

## Test Patterns

### Unit Test Structure

```typescript
describe('AuthService', () => {
  describe('login', () => {
    it('should return token for valid credentials', async () => {
      // Arrange
      const credentials = { email: 'test@test.com', password: 'pass123' };
      
      // Act
      const result = await authService.login(credentials);
      
      // Assert
      expect(result.token).toBeDefined();
    });

    it('should throw for invalid password', async () => {
      // Arrange
      const credentials = { email: 'test@test.com', password: 'wrong' };
      
      // Act & Assert
      await expect(authService.login(credentials)).rejects.toThrow('Invalid credentials');
    });
  });
});
```

---

## Key Principles

- **Test behavior not implementation**
- **One assertion per test** (when practical)
- **Descriptive test names**
- **Arrange-Act-Assert pattern**
- **Mock external dependencies**

---

## Extended Testing (Visual & Quality)

### /test responsive

Test responsive design across breakpoints:

```
Breakpoints to test:
- 📱 Mobile: 375px (iPhone SE)
- 📱 Mobile L: 428px (iPhone 14)
- 📲 Tablet: 768px (iPad)
- 💻 Desktop: 1024px
- 🖥️ Desktop L: 1440px
```

Steps:
1. Open app in browser
2. Resize to each breakpoint
3. Verify: no horizontal scroll, readable text, functional navigation
4. Check touch targets (min 44x44px on mobile)
5. Test hamburger menu (if exists)

### /test a11y

Accessibility audit:

// turbo
```bash
npx -y pa11y {url} 2>/dev/null || echo "Install: npm install -g pa11y"
```

Manual checks:
- [ ] All images have `alt` text
- [ ] Form inputs have `label` elements
- [ ] Color contrast ratio ≥ 4.5:1
- [ ] Keyboard navigation works (Tab, Enter, Escape)
- [ ] Screen reader compatible (semantic HTML)
- [ ] Focus indicators visible

### /test lighthouse

Performance & SEO audit:

// turbo
```bash
npx -y lighthouse {url} --output=json --quiet 2>/dev/null | node -e "
const r = JSON.parse(require('fs').readFileSync('/dev/stdin','utf8'));
const c = r.categories;
console.log('Performance:', Math.round(c.performance.score*100));
console.log('Accessibility:', Math.round(c.accessibility.score*100));
console.log('Best Practices:', Math.round(c['best-practices'].score*100));
console.log('SEO:', Math.round(c.seo.score*100));
" || echo "Run Lighthouse from Chrome DevTools instead"
```

Target scores:
| Category | Target |
|----------|--------|
| Performance | ≥ 80 |
| Accessibility | ≥ 90 |
| Best Practices | ≥ 90 |
| SEO | ≥ 80 |

### /test visual

Visual regression testing:
1. Capture screenshots of all pages (baseline)
2. After changes, capture new screenshots
3. Compare pixel-by-pixel
4. Flag differences > 0.1%

Use browser subagent to capture screenshots at multiple breakpoints.

---

## Usage

```
/test                        # Run all unit/integration tests
/test [file/feature]         # Generate tests for target
/test coverage               # Show coverage report
/test watch                  # Watch mode
/test responsive             # Check responsive design
/test a11y                   # Accessibility audit
/test lighthouse             # Performance + SEO scores
/test visual                 # Screenshot comparison
```
