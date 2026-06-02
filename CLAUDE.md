# Project Design Guidelines

## Overview

This project contains a pair of auth pages — login and register — built with a consistent dark theme. Both pages share the same visual language, validation patterns, and component style so future pages can follow the same conventions.

---

## Pages

### login.html
**Purpose:** User authentication.

**Components:**
- Email input (required, validated for format)
- Password input (required, with show/hide toggle)
- Remember me checkbox
- Forgot password link
- Submit button
- Social login: Google, GitHub, Facebook
- Cross-link to register page

**Validation:**
- `Email bắt buộc` — empty email
- `Email không hợp lệ` — invalid format
- `Mật khẩu bắt buộc` — empty password

**Language:** Vietnamese.

---

### register.html
**Purpose:** New user account creation.

**Components:**
- Full Name (required)
- Email (required, validated for format)
- Phone (optional, must be exactly 10 digits if provided)
- Gender (optional, radio: Male / Female / Other)
- Address (optional)
- Password (required, min 8 characters, with show/hide toggle)
- Confirm Password (required, must match)
- Terms checkbox (required)
- Submit button
- Cross-link to login page

**Validation:**
- `Full name is required` — empty name
- `Email is required` / `Invalid email address` — email validation
- `Phone must be exactly 10 digits` — only if phone is filled
- `Password is required` / `Password must be at least 8 characters`
- `Please confirm your password` / `Passwords do not match`
- `You must agree to the terms and conditions` — checkbox unchecked

**Language:** English.

---

## Design System

### Color Palette
| Token | Value | Usage |
|---|---|---|
| `dark.bg` | `#090A14` | Page background |
| `dark.card` | `#0F1222` | Card / form background |
| `dark.border` | `#1E2236` | Input / card borders |
| `dark.muted` | `#2A2F4A` | Hover states |
| `primary` | `#DF6B33` | CTA buttons, links, focus rings |
| `primaryHover` | `#C85B28` | Button hover |

### Typography
- **Headings / CTA labels:** Poppins, font-weight 700–800
- **Body / inputs / labels:** Inter, font-weight 400–600
- Loaded via Google Fonts CDN

### Layout
- Centered single-column card on `min-h-screen`
- `max-w-md` card width, `px-4` horizontal padding
- Responsive padding: `p-6` mobile, `sm:p-8` tablet+
- Social buttons collapse to icon-only on mobile (`hidden sm:inline`)

### Icons
- **Lucide Icons** loaded from `unpkg.com/lucide@latest`
- Use `data-lucide="icon-name"` attributes in HTML
- Call `lucide.createIcons()` after DOM mutations that change icon attributes (e.g., show/hide password)

### CSS Baseline
```css
body { background-color: #090A14; }
input:-webkit-autofill {
    -webkit-box-shadow: 0 0 0 1000px #0F1222 inset;
    -webkit-text-fill-color: #E2E8F0;
}
```

---

## Validation Pattern

Every form follows this pattern:

**HTML:**
- `<form id="..." novalidate>` to disable browser default
- Each field gets an error `<p id="fieldNameError" class="hidden ...">` below it
- Error element contains Lucide `alert-circle` icon + `<span>` for the message

**JS — setFieldError:**
```js
input.classList.add('border-red-400', 'focus:border-red-400', 'focus:ring-red-400');
input.classList.remove('border-dark-border', 'focus:border-primary', 'focus:ring-primary');
errorEl.querySelector('span').textContent = '...';
errorEl.classList.remove('hidden');
lucide.createIcons();
```

**JS — clearFieldError:**
```js
input.classList.remove('border-red-400', 'focus:border-red-400', 'focus:ring-red-400');
input.classList.add('border-dark-border', 'focus:border-primary', 'focus:ring-primary');
errorEl.classList.add('hidden');
```

**Realtime clear:** each field listens to `input` event and clears its own error on type.

**Password toggle:** separate `inputId` + `iconId` parameters so one function handles all password fields.

---

## Future Pages

Apply these rules to any new page:

1. **Theme** — always use the shared Tailwind config colors (`dark.*`, `primary`, `primaryHover`) and font families (`font-poppins`, `font-inter`).
2. **Responsive** — use `sm:` breakpoint variants; test on mobile, tablet, desktop.
3. **Icons** — use Lucide via CDN; call `lucide.createIcons()` after dynamic icon changes.
4. **Validation** — follow the shared `setFieldError` / `clearFieldError` pattern for all required fields.
5. **Cross-links** — include navigation links between related pages (e.g., login ↔ register, login → forgot-password).
6. **Language** — match the language of the page (Vietnamese for Vietnamese pages, English for English pages).
7. **No dead UI** — every link `href` and button `onclick` must point to a real target or `#`.
