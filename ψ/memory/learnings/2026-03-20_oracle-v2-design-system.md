# Oracle-v2 Design System Reference

**Source**: oracle-v2/frontend/src/index.css + component .module.css files
**Learned from**: Dex (DM, Session 2) + direct codebase study

## Design Tokens

### Colors (Dark-only theme)
- **Backgrounds**: `#111110` (primary) → `#1a1917` (secondary) → `#211f1d` (card) → `#2a2825` (elevated)
- **Text**: `#e8e2d9` (primary) → `#a39b8f` (secondary) → `#6d6560` (muted)
- **Borders**: `#3a3632` (standard) / `#2d2a27` (subtle)
- **Accent**: `#d4943a` (warm amber), hover `#c07d2a`, glow `rgba(212,148,58,0.1)`
- **Status**: active `#3b82f6`, pending `#eab308`, answered `#22c55e`, closed `#6d6560`
- **Semantic**: success `#5a9a3e`, warning `#d4943a`, danger `#c44b3f`

### Typography
- Headings: `'Bitter', 'Georgia', serif` — 700 weight, -0.01em tracking
- Body: `'Inter', -apple-system, sans-serif` — line-height 1.6
- Mono: `'JetBrains Mono', 'Fira Code', monospace`
- Sizes: 20px (large heading), 16px (medium), 14px (standard), 13px (small), 10-12px (tiny)

### Spacing (4px base)
- xs: 4px, sm: 8px, md: 16px, lg: 24px, xl: 32px

### Border Radius
- sm: 6px, md: 10px, lg: 16px, 50% for avatars

### Content Widths
- narrow: 640px, medium: 800px, wide: 1000px, full: 1200px

### Transitions
- Standard: 0.2s, Fast: 0.15s, Slow: 0.3s

### Responsive
- Mobile: max-width 768px, Tablet: 900px, Small: 480px
- Min touch target: 44×44px

## Patterns
- CSS Modules with camelCase naming
- BEM-ish: `.blockName.modifier` for states (.active, .selected, .offline)
- All colors via CSS custom properties — never hardcode
- Minimal shadows — prefer borders and glow effects
- Each Beast has a theme color variable (--beast-*)
