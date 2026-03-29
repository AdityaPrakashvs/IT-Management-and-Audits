# Mobile UX Prototyping Toolkit for Financial Services

A comprehensive, production-ready mobile UX prototyping toolkit designed for banking and financial services applications. This toolkit provides a complete design system, interactive prototypes, accessibility guidelines, and documentation to accelerate the design and development of mobile banking experiences.

---

## Overview

Modern financial services demand mobile experiences that are secure, intuitive, and accessible to all users. This toolkit delivers a battle-tested foundation for building mobile banking prototypes that meet industry standards for usability, security perception, and regulatory compliance.

### What's Included

| Category | Contents |
|----------|----------|
| **Interactive Prototypes** | 10 fully functional HTML/CSS/JS screens covering core banking flows |
| **Design System** | Complete CSS-based design system with tokens, typography, spacing, and color scales |
| **Component Library** | Reusable UI components: buttons, cards, inputs, lists, navigation, modals |
| **Documentation** | UX best practices, accessibility checklists, user personas, journey maps |
| **Export Guides** | Instructions for translating prototypes to Figma and other design tools |

---

## Quick Start

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd mobile-ux-toolkit
   ```

2. **Open the prototype**
   ```bash
   # Simply open in any browser
   open prototype/index.html
   ```

3. **Navigate screens**
   Use the bottom navigation bar or the main menu to explore all prototype screens.

> No build tools, frameworks, or dependencies required. Pure HTML, CSS, and JavaScript.

---

## Design System

The design system is built on CSS custom properties (variables) for maximum flexibility and consistency.

### Color Palette

The color palette is optimized for trust, professionalism, and readability -- essential qualities for financial applications.

| Token | Value | Usage |
|-------|-------|-------|
| `--color-primary-600` | `#1a56db` | Primary actions, CTAs |
| `--color-primary-700` | `#1346b0` | Hover states, emphasis |
| `--color-primary-50` | `#eff6ff` | Backgrounds, highlights |
| `--color-neutral-900` | `#111827` | Primary text |
| `--color-neutral-500` | `#6b7280` | Secondary text |
| `--color-neutral-100` | `#f3f4f6` | Dividers, backgrounds |
| `--color-success-600` | `#059669` | Positive values, confirmations |
| `--color-danger-600` | `#dc2626` | Errors, negative values |
| `--color-warning-500` | `#f59e0b` | Warnings, pending states |

### Typography Scale

- **Display**: 32px / 700 weight -- Hero sections, large amounts
- **Heading 1**: 24px / 700 weight -- Screen titles
- **Heading 2**: 20px / 600 weight -- Section headers
- **Heading 3**: 18px / 600 weight -- Card titles
- **Body**: 16px / 400 weight -- General content
- **Body Small**: 14px / 400 weight -- Supporting text
- **Caption**: 12px / 400 weight -- Labels, metadata

### Spacing Scale

Based on a 4px grid system: `4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96`

---

## Prototype Screens

### Core Flows

| Screen | File | Description |
|--------|------|-------------|
| **Onboarding** | `screens/onboarding.html` | Welcome carousel with feature highlights |
| **Login** | `screens/login.html` | Authentication with PIN and biometric options |
| **Dashboard** | `screens/dashboard.html` | Account overview with balances and recent activity |
| **Transfer** | `screens/transfer.html` | Peer-to-peer and inter-account transfers |
| **Payments** | `screens/payments.html` | Bill payment flow with payee management |
| **History** | `screens/history.html` | Transaction history with search and filters |
| **Cards** | `screens/cards.html` | Card management, freeze/unfreeze, limits |
| **Profile** | `screens/profile.html` | User settings and preferences |
| **Notifications** | `screens/notifications.html` | Notification center with categorization |
| **Support** | `screens/support.html` | Help center with FAQs and contact options |

### Interaction Patterns

- Smooth screen-to-screen transitions
- Pull-to-refresh simulation
- Bottom sheet modals
- Toast notifications
- Swipe gestures on list items
- Loading state animations

---

## Accessibility

This toolkit is designed to meet **WCAG 2.1 Level AA** standards:

- All color combinations meet minimum contrast ratios (4.5:1 for text, 3:1 for large text)
- Focus indicators are visible on all interactive elements
- Screen reader landmarks and ARIA labels are applied throughout
- Touch targets meet the minimum 44x44px requirement
- Motion can be reduced via `prefers-reduced-motion` media query
- All form fields have associated labels
- Error messages are descriptive and programmatically associated

See `docs/ACCESSIBILITY_CHECKLIST.md` for the full compliance checklist.

---

## Documentation

| Document | Description |
|----------|-------------|
| [Design System Spec](docs/DESIGN_SYSTEM.md) | Complete design token reference and usage guidelines |
| [UX Best Practices](docs/UX_BEST_PRACTICES.md) | 15 essential practices for financial app UX |
| [Accessibility Checklist](docs/ACCESSIBILITY_CHECKLIST.md) | WCAG 2.1 AA compliance checklist |
| [User Personas](docs/USER_PERSONAS.md) | 3 detailed user personas for banking apps |
| [User Journeys](docs/USER_JOURNEYS.md) | Journey maps for key banking flows |
| [Figma Export Guide](docs/FIGMA_EXPORT_GUIDE.md) | Translating prototypes to Figma |

---

## Project Structure

```
mobile-ux-toolkit/
├── README.md
├── LICENSE
├── .gitignore
├── prototype/
│   ├── index.html              # Main entry point
│   ├── css/
│   │   ├── design-system.css   # Design tokens and base styles
│   │   └── components.css      # Reusable component styles
│   ├── js/
│   │   └── prototype.js        # Interactions and transitions
│   └── screens/
│       ├── login.html
│       ├── dashboard.html
│       ├── transfer.html
│       ├── payments.html
│       ├── history.html
│       ├── profile.html
│       ├── notifications.html
│       ├── cards.html
│       ├── support.html
│       └── onboarding.html
└── docs/
    ├── DESIGN_SYSTEM.md
    ├── UX_BEST_PRACTICES.md
    ├── ACCESSIBILITY_CHECKLIST.md
    ├── USER_PERSONAS.md
    ├── USER_JOURNEYS.md
    ├── FIGMA_EXPORT_GUIDE.md
    └── screenshots/
        └── SCREENSHOTS.md
```

---

## Browser Support

| Browser | Version |
|---------|---------|
| Chrome (Android) | 90+ |
| Safari (iOS) | 14+ |
| Firefox Mobile | 90+ |
| Samsung Internet | 14+ |
| Edge | 90+ |

---

## Usage Guidelines

### For Designers
- Use the prototype as a reference for interaction patterns and visual design
- Follow the Figma Export Guide to recreate screens in your design tool
- Reference the Design System spec for token values and component specifications

### For Developers
- Use the CSS custom properties directly in your production codebase
- Reference the component markup patterns for accessible HTML structure
- Use the prototype.js patterns for implementing screen transitions

### For Stakeholders
- Open `prototype/index.html` in a mobile browser to experience the prototype
- Use Chrome DevTools device emulation for the most accurate preview
- Navigate through key flows to evaluate the user experience

---

## Customization

### Theming

Override the CSS custom properties in `:root` to apply your brand colors:

```css
:root {
  --color-primary-600: #your-brand-color;
  --color-primary-700: #your-brand-dark;
  --color-primary-50: #your-brand-light;
}
```

### Adding Screens

1. Create a new HTML file in `prototype/screens/`
2. Use the existing screen template structure
3. Include the shared CSS and JS files
4. Add navigation links to `index.html`

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-screen`)
3. Commit your changes (`git commit -m 'Add new screen'`)
4. Push to the branch (`git push origin feature/new-screen`)
5. Open a Pull Request

---

*Built for teams shipping world-class financial mobile experiences.*
