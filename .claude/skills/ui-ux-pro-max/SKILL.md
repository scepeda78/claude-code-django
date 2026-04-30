---
name: ui-ux-pro-max
description: UI/UX design intelligence for Django web interfaces. Use when designing, implementing, reviewing, or improving Django templates, admin panels, landing pages, dashboards, forms, tables, charts, responsive layouts, color systems, typography, accessibility, motion, icons, visual polish, or design systems. Includes searchable data for UI styles, color palettes, product rules, font pairings, UX guidelines, chart guidance, and HTML/Tailwind stack patterns.
---

# UI/UX Pro Max for Django

Use this skill when a change affects how a Django feature looks, feels, moves, or is interacted with.

## Workflow

1. Read the current Django UI first: base templates, partials/components, static CSS, JS, icon system, and any existing design tokens.
2. For a new page, dashboard, landing page, or visual direction, generate a design system:

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<product type> <industry> <tone>" --design-system -f markdown -p "<Project Name>"
```

3. For targeted decisions, search the bundled data:

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "minimal dashboard" --domain style
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "healthcare dashboard" --domain color
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "professional data table" --domain ux
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "trend comparison" --domain chart
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "responsive forms" --stack html-tailwind
```

Available domains: `product`, `style`, `color`, `typography`, `landing`, `chart`, `ux`, `icons`, `react`, `web`, `google-fonts`.

Available stacks include: `html-tailwind`, `react`, `nextjs`, `vue`, `svelte`, `astro`, `flutter`, `swiftui`, `react-native`, `shadcn`, `threejs`, `angular`, `laravel`.

4. Optionally persist project guidance:

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system --persist -f markdown -p "<Project Name>" --output-dir .
```

This creates `design-system/<project>/MASTER.md`. For page-specific overrides, add `--page "<page name>"`.

## Django Implementation Rules

- Prefer Django templates, template inheritance, partials, inclusion tags, and static assets already used by the project.
- Do not introduce React, Vue, Tailwind, shadcn/ui, or a new frontend build chain unless the repository already uses it or the user explicitly asks.
- If Tailwind is not already configured, translate recommendations into normal CSS variables/classes instead of adding Tailwind as a dependency.
- Keep design decisions as tokens where practical: colors, spacing, radius, shadows, font families, and state colors.
- Use semantic HTML and Django `{% url %}`, `{% static %}`, `{% include %}`, and `{% block %}` patterns.
- Keep backend validation authoritative for forms; UI validation and helper text are supplementary.
- Use SVG/icon libraries already present in the project. Avoid emoji as functional icons.
- Provide loading, empty, error, disabled, hover, focus, and active states for interactive UI.

## Quality Checklist

- Responsive at 375px, 768px, 1024px, and 1440px with no horizontal scroll.
- Text contrast meets WCAG AA, focus states are visible, and keyboard navigation works.
- Forms have visible labels, nearby errors, helper text when needed, and clear submit feedback.
- Touch targets are at least 44x44px when used on mobile.
- Images reserve dimensions or aspect ratio and use appropriate `alt` text.
- Animations use transform/opacity where possible and respect `prefers-reduced-motion`.
- Tables and charts include labels, legends, useful empty states, and do not rely on color alone.
- After implementation, follow the repo Django flow: migrations if needed, relevant tests, and localhost review when an app exists.

## Bundled Resources

- `scripts/search.py`: no external Python dependencies.
- `data/*.csv`: styles, colors, typography, UX, charts, products, icons, and reasoning rules.
- `data/stacks/html-tailwind.csv`: the closest stack guidance for Django templates and static HTML/CSS.
