# VXUI 2

**Website**: [ui.vx.link](https://ui.vx.link) &nbsp;|&nbsp; **GitHub**: [tmplink/vxui_react](https://github.com/tmplink/vxui_react) &nbsp;|&nbsp; [中文文档](README.zh.md)

A general-purpose React UI component library for admin panels, ops dashboards, internal tools, and data-heavy interfaces. Built on CSS custom properties (design tokens) for theming and Radix UI primitives for accessibility. v1.0.0.

> **Security Notice**: 1.0.0 is a clean-slate release following the removal of a supply-chain attack vector discovered in the git history. The malicious `postinstall` script (commits `9c15fc0` / `fd8700d`) and self-referencing dependency (`c2bc8b9`) have been neutralized. This version also renames the package from `vxui-react` to `vxui-2`. See [CHANGELOG.md](CHANGELOG.md) for details.

## Installation

```bash
npm install vxui-2
```

`react` and `react-dom` (both ^19.0.0) must be provided by the host application.

## Quick Start

```tsx
import ReactDOM from 'react-dom/client';
import {
  ThemeProvider,
  ToastProvider,
  themePresets,
  AppShell,
  Button,
  Card,
  CardContent,
  CardHeader,
  CardTitle,
  Input,
} from 'vxui-2';
import 'vxui-2/styles.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <ThemeProvider themes={themePresets} defaultTheme="light">
    <ToastProvider>
      <AppShell
        brand="Acme Ops"
        title="Dashboard"
        navSections={[
          {
            title: 'Workspace',
            items: [{ key: 'overview', label: 'Overview', active: true }],
          },
        ]}
        headerActions={<Button size="sm">New Project</Button>}
      >
        <Card>
          <CardHeader>
            <CardTitle>Queue Health</CardTitle>
          </CardHeader>
          <CardContent>
            <Input label="Search orders" placeholder="PO-1024" />
          </CardContent>
        </Card>
      </AppShell>
    </ToastProvider>
  </ThemeProvider>,
);
```

## Theming

VXUI 2 uses CSS custom properties. Override any token to rebrand in minutes:

```css
:root {
  --vx-primary: #7c3aed;
  --vx-primary-strong: #6d28d9;
  --vx-primary-soft: rgba(124, 58, 237, 0.1);
  --vx-bg: #f8fafc;
  --vx-surface: #ffffff;
  --vx-border: #e2e8f0;
  --vx-text: #0f172a;
  --vx-radius: 10px;
  --vx-sidebar-width: 240px;
}
```

Dark mode tokens apply automatically when `<html>` has `class="dark"` or `data-theme="dark"`.

Create custom themes at runtime:

```tsx
import { ThemeProvider, createTheme, themePresets } from 'vxui-2';

const themes = {
  ...themePresets,
  ocean: createTheme('dark', {
    label: 'Ocean',
    tokens: {
      '--vx-primary': '#38bdf8',
      '--vx-primary-strong': '#0ea5e9',
      '--vx-primary-soft': 'rgba(56, 189, 248, 0.16)',
      '--vx-bg': '#06131f',
      '--vx-surface': '#0d2236',
    },
  }),
};

<ThemeProvider themes={themes} defaultTheme="ocean">
  <App />
</ThemeProvider>
```

Use `useTheme()` to read the current theme or call `setTheme('ocean')` to switch at runtime.

## Components

### Layout & Navigation
**AppShell**, **Shell** / ShellSidebar / ShellContent, **NavigationMenu**, **Breadcrumb**, **Pagination**, **Tabs**, **Menubar**, **Stepper**, **Resizable**, **ScrollArea**, **Separator**

### Buttons & Actions
**Button**, **Toggle**, **SegmentedControl**, **DropdownMenu**, **ContextMenu**

### Forms & Inputs
**Input**, **Textarea**, **Checkbox**, **Radio**, **Switch**, **Slider**, **NumberInput**, **TagInput**, **Rating**, **DatePicker**, **ColorPicker**, **Select**, **MultiSelect**, **PinInput**, **TimePicker**, **Form**, **FileUpload**

### Overlays & Popovers
**Dialog**, **Sheet**, **Popover**, **Tooltip**, **HoverCard**, **CommandPalette**

### Feedback & Status
**Toast**, **Notification**, **Alert**, **Progress**, **Spinner**, **Skeleton**, **EmptyState**, **Result**

### Data Display
**Table**, **Card**, **Badge**, **Avatar**, **Accordion**, **Calendar**, **Image**, **Carousel**, **TreeView**, **Timeline**, **Descriptions**

### Typography
**Heading**, **Text**, **Label**, **CodeBlock**, **Article**

### Mobile (`vxui-2/mobile`)
**MobileShell**, **MobileDrawer**, **BottomNav**, **MobileList**, **ActionSheet**

### Utility
**ThemeProvider**, **useTheme**, **Responsive**, **LanguageSwitcher**, **VXUIProvider**

## Design Principles

- **Token-first theming** — All visual decisions are CSS variables. No Tailwind, no CSS-in-JS.
- **Accessible by default** — Overlays and interactive components use Radix UI primitives with keyboard nav and ARIA.
- **No global side-effects** — CSS is scoped to `.vx-*` class names. Won't override your styles.
- **Mobile-first responsive** — Sidebar collapses to overlay on narrow viewports. Mobile components in a separate import path.
- **No runtime CSS injection** — Styles ship as a single static stylesheet. No FOUC.

## Development

```bash
npm install
npm run dev          # Start dev server
npm run typecheck    # TypeScript type check
npm run build        # Build library (for npm publish)
npm run test         # Run tests
npm run security:audit  # Security audit
```

## License

Apache-2.0