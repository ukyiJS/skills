# Migration Guide: Radix UI → Base UI v1.2.0

Base UI is built by the teams behind Radix, Floating UI, and Material UI. This guide covers migrating from `@radix-ui/react-*` to `@base-ui/react`.

---

## Package Consolidation

Radix uses separate packages per component. Base UI uses a single package.

```bash
# Remove Radix packages
pnpm remove @radix-ui/react-dialog @radix-ui/react-popover @radix-ui/react-tooltip @radix-ui/react-select @radix-ui/react-accordion @radix-ui/react-dropdown-menu @radix-ui/react-tabs @radix-ui/react-switch @radix-ui/react-checkbox @radix-ui/react-radio-group @radix-ui/react-slider @radix-ui/react-scroll-area @radix-ui/react-separator @radix-ui/react-toggle @radix-ui/react-toggle-group @radix-ui/react-avatar @radix-ui/react-collapsible @radix-ui/react-progress @radix-ui/react-toolbar @radix-ui/react-navigation-menu @radix-ui/react-context-menu @radix-ui/react-alert-dialog

# Install Base UI
pnpm add @base-ui/react
```

**Import changes**:
```tsx
// Radix (separate packages)
import * as Dialog from '@radix-ui/react-dialog';
import * as Popover from '@radix-ui/react-popover';
import * as Select from '@radix-ui/react-select';

// Base UI (single package, named exports)
import { Dialog } from '@base-ui/react/dialog';
import { Popover } from '@base-ui/react/popover';
import { Select } from '@base-ui/react/select';
```

---

## Core Pattern Change: asChild → render prop / direct children

Radix uses `asChild` to replace the default element. Base UI offers three patterns:

### Pattern 1: Direct Children (Simplest)

```tsx
// Radix
<Dialog.Trigger asChild>
  <button className="my-btn">Open</button>
</Dialog.Trigger>

// Base UI - just use className directly
<Dialog.Trigger className="my-btn">Open</Dialog.Trigger>
```

### Pattern 2: Render Element (Replace HTML Tag)

```tsx
// Radix
<Dialog.Trigger asChild>
  <a href="/link">Open</a>
</Dialog.Trigger>

// Base UI
<Dialog.Trigger render={<a href="/link" />}>Open</Dialog.Trigger>
```

### Pattern 3: Render Function (Full Control)

```tsx
// Radix
<Dialog.Trigger asChild>
  <MyButton variant="primary">Open</MyButton>
</Dialog.Trigger>

// Base UI
<Dialog.Trigger render={(props) => <MyButton {...props} variant="primary">Open</MyButton>} />
```

---

## Component Name Mapping

| Radix | Base UI |
|-------|---------|
| `Content` | `Popup` |
| `Overlay` | `Backdrop` |
| `DropdownMenu.*` | `Menu.*` |
| `Select.Item` | `Select.Item` + `Select.ItemText` + `Select.ItemIndicator` |
| `Select.ItemIndicator` | `Select.ItemIndicator` |
| N/A | `Viewport` (Dialog/AlertDialog) |
| N/A | `Positioner` (floating components) |

---

## Structural Differences

### 1. Explicit Positioner Required

Radix puts positioning props directly on `Content`. Base UI uses a separate `Positioner` wrapper.

```tsx
// Radix
<Popover.Content side="top" align="center" sideOffset={8}>
  Content
</Popover.Content>

// Base UI
<Popover.Portal>
  <Popover.Positioner side="top" align="center" sideOffset={8}>
    <Popover.Popup>
      Content
    </Popover.Popup>
  </Popover.Positioner>
</Popover.Portal>
```

### 2. Explicit Portal Required

Radix portals automatically. Base UI requires explicit `Portal`.

```tsx
// Radix (implicit portal)
<Dialog.Root>
  <Dialog.Trigger>Open</Dialog.Trigger>
  <Dialog.Overlay />
  <Dialog.Content>Content</Dialog.Content>
</Dialog.Root>

// Base UI (explicit portal)
<Dialog.Root>
  <Dialog.Trigger>Open</Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Backdrop />
    <Dialog.Viewport>
      <Dialog.Popup>Content</Dialog.Popup>
    </Dialog.Viewport>
  </Dialog.Portal>
</Dialog.Root>
```

### 3. Dialog Viewport Wrapper

Base UI requires a `Viewport` around `Popup` for Dialog, AlertDialog, and Drawer.

```tsx
// Radix
<Dialog.Portal>
  <Dialog.Overlay />
  <Dialog.Content>...</Dialog.Content>
</Dialog.Portal>

// Base UI
<Dialog.Portal>
  <Dialog.Backdrop />
  <Dialog.Viewport>
    <Dialog.Popup>...</Dialog.Popup>
  </Dialog.Viewport>
</Dialog.Portal>
```

---

## Prop Name Mapping

| Radix | Base UI |
|-------|---------|
| `align="center"` | `align="center"` (same) |
| `onOpenChange` | `onOpenChange` (same) |
| `defaultOpen` | `defaultOpen` (same) |
| `modal` | `modal` (same) |
| N/A | `disablePointerDismissal` |
| N/A | `disableHoverablePopup` |
| N/A | `disableAnchorTracking` |
| `loop` | `loopFocus` |

---

## Complete Migration Examples

### Dialog

```tsx
// Radix
import * as Dialog from '@radix-ui/react-dialog';

<Dialog.Root>
  <Dialog.Trigger asChild>
    <button>Open</button>
  </Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Overlay className="overlay" />
    <Dialog.Content className="content">
      <Dialog.Title>Title</Dialog.Title>
      <Dialog.Description>Description</Dialog.Description>
      <Dialog.Close asChild>
        <button>Close</button>
      </Dialog.Close>
    </Dialog.Content>
  </Dialog.Portal>
</Dialog.Root>

// Base UI
import { Dialog } from '@base-ui/react/dialog';

<Dialog.Root>
  <Dialog.Trigger className="trigger">Open</Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Backdrop className="overlay" />
    <Dialog.Viewport>
      <Dialog.Popup className="content">
        <Dialog.Title>Title</Dialog.Title>
        <Dialog.Description>Description</Dialog.Description>
        <Dialog.Close>Close</Dialog.Close>
      </Dialog.Popup>
    </Dialog.Viewport>
  </Dialog.Portal>
</Dialog.Root>
```

### Select

```tsx
// Radix
import * as Select from '@radix-ui/react-select';

<Select.Root value={value} onValueChange={setValue}>
  <Select.Trigger>
    <Select.Value placeholder="Select..." />
    <Select.Icon />
  </Select.Trigger>
  <Select.Portal>
    <Select.Content position="popper" sideOffset={8}>
      <Select.Viewport>
        <Select.Item value="react">
          <Select.ItemIndicator>✓</Select.ItemIndicator>
          <Select.ItemText>React</Select.ItemText>
        </Select.Item>
      </Select.Viewport>
    </Select.Content>
  </Select.Portal>
</Select.Root>

// Base UI
import { Select } from '@base-ui/react/select';

<Select.Root value={value} onValueChange={setValue}>
  <Select.Trigger>
    <Select.Value placeholder="Select..." />
    <Select.Icon />
  </Select.Trigger>
  <Select.Portal>
    <Select.Positioner sideOffset={8}>
      <Select.Popup>
        <Select.Item value="react">
          <Select.ItemIndicator>✓</Select.ItemIndicator>
          <Select.ItemText>React</Select.ItemText>
        </Select.Item>
      </Select.Popup>
    </Select.Positioner>
  </Select.Portal>
</Select.Root>
```

### Popover

```tsx
// Radix
import * as Popover from '@radix-ui/react-popover';

<Popover.Root>
  <Popover.Trigger asChild>
    <button>Open</button>
  </Popover.Trigger>
  <Popover.Portal>
    <Popover.Content side="top" align="center" sideOffset={8}>
      <Popover.Arrow />
      Content
      <Popover.Close>×</Popover.Close>
    </Popover.Content>
  </Popover.Portal>
</Popover.Root>

// Base UI
import { Popover } from '@base-ui/react/popover';

<Popover.Root>
  <Popover.Trigger>Open</Popover.Trigger>
  <Popover.Portal>
    <Popover.Positioner side="top" align="center" sideOffset={8}>
      <Popover.Popup>
        <Popover.Arrow />
        Content
        <Popover.Close>×</Popover.Close>
      </Popover.Popup>
    </Popover.Positioner>
  </Popover.Portal>
</Popover.Root>
```

### Tooltip

```tsx
// Radix
import * as Tooltip from '@radix-ui/react-tooltip';

<Tooltip.Provider delayDuration={200}>
  <Tooltip.Root>
    <Tooltip.Trigger asChild>
      <button>Hover</button>
    </Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Content side="top" sideOffset={4}>
        <Tooltip.Arrow />
        Tooltip text
      </Tooltip.Content>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>

// Base UI
import { Tooltip } from '@base-ui/react/tooltip';

<Tooltip.Provider delay={200}>
  <Tooltip.Root>
    <Tooltip.Trigger>Hover</Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Positioner side="top" sideOffset={4}>
        <Tooltip.Popup>
          <Tooltip.Arrow />
          Tooltip text
        </Tooltip.Popup>
      </Tooltip.Positioner>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>
```

### Dropdown Menu → Menu

```tsx
// Radix
import * as DropdownMenu from '@radix-ui/react-dropdown-menu';

<DropdownMenu.Root>
  <DropdownMenu.Trigger asChild>
    <button>Options</button>
  </DropdownMenu.Trigger>
  <DropdownMenu.Portal>
    <DropdownMenu.Content sideOffset={8}>
      <DropdownMenu.Item>Cut</DropdownMenu.Item>
      <DropdownMenu.Item>Copy</DropdownMenu.Item>
      <DropdownMenu.Separator />
      <DropdownMenu.Sub>
        <DropdownMenu.SubTrigger>More</DropdownMenu.SubTrigger>
        <DropdownMenu.Portal>
          <DropdownMenu.SubContent>
            <DropdownMenu.Item>Save As</DropdownMenu.Item>
          </DropdownMenu.SubContent>
        </DropdownMenu.Portal>
      </DropdownMenu.Sub>
    </DropdownMenu.Content>
  </DropdownMenu.Portal>
</DropdownMenu.Root>

// Base UI
import { Menu } from '@base-ui/react/menu';

<Menu.Root>
  <Menu.Trigger>Options</Menu.Trigger>
  <Menu.Portal>
    <Menu.Positioner sideOffset={8}>
      <Menu.Popup>
        <Menu.Item>Cut</Menu.Item>
        <Menu.Item>Copy</Menu.Item>
        <Menu.Separator />
        <Menu.SubmenuRoot>
          <Menu.SubmenuTrigger>More</Menu.SubmenuTrigger>
          <Menu.Portal>
            <Menu.Positioner>
              <Menu.Popup>
                <Menu.Item>Save As</Menu.Item>
              </Menu.Popup>
            </Menu.Positioner>
          </Menu.Portal>
        </Menu.SubmenuRoot>
      </Menu.Popup>
    </Menu.Positioner>
  </Menu.Portal>
</Menu.Root>
```

### Accordion

```tsx
// Radix
import * as Accordion from '@radix-ui/react-accordion';

<Accordion.Root type="single" defaultValue="item-1" collapsible>
  <Accordion.Item value="item-1">
    <Accordion.Header>
      <Accordion.Trigger>Section 1</Accordion.Trigger>
    </Accordion.Header>
    <Accordion.Content>Content 1</Accordion.Content>
  </Accordion.Item>
</Accordion.Root>

// Base UI
import { Accordion } from '@base-ui/react/accordion';

<Accordion.Root defaultValue={['item-1']}>
  <Accordion.Item value="item-1">
    <Accordion.Header>
      <Accordion.Trigger>Section 1</Accordion.Trigger>
    </Accordion.Header>
    <Accordion.Panel>Content 1</Accordion.Panel>
  </Accordion.Item>
</Accordion.Root>
```

Key differences:
- No `type="single"` / `type="multiple"` — use `multiple` boolean prop (default: `false`)
- `Content` → `Panel`
- `defaultValue` is always an array
- No `collapsible` prop — always collapsible

### Tabs

```tsx
// Radix
import * as Tabs from '@radix-ui/react-tabs';

<Tabs.Root defaultValue="tab1">
  <Tabs.List>
    <Tabs.Trigger value="tab1">Tab 1</Tabs.Trigger>
    <Tabs.Trigger value="tab2">Tab 2</Tabs.Trigger>
  </Tabs.List>
  <Tabs.Content value="tab1">Content 1</Tabs.Content>
  <Tabs.Content value="tab2">Content 2</Tabs.Content>
</Tabs.Root>

// Base UI
import { Tabs } from '@base-ui/react/tabs';

<Tabs.Root defaultValue="tab1">
  <Tabs.List>
    <Tabs.Tab value="tab1">Tab 1</Tabs.Tab>
    <Tabs.Tab value="tab2">Tab 2</Tabs.Tab>
    <Tabs.Indicator />
  </Tabs.List>
  <Tabs.Panel value="tab1">Content 1</Tabs.Panel>
  <Tabs.Panel value="tab2">Content 2</Tabs.Panel>
</Tabs.Root>
```

Key differences:
- `Trigger` → `Tab`
- `Content` → `Panel`
- Built-in `Indicator` component for animated tab indicator
- Data attribute: `[data-active]` instead of `[data-state="active"]`

---

## Styling Differences

### Data Attributes

Radix uses `data-state`:
```css
/* Radix */
.trigger[data-state="open"] { ... }
.tab[data-state="active"] { ... }
.checkbox[data-state="checked"] { ... }
```

Base UI uses specific data attributes:
```css
/* Base UI */
.trigger[data-popup-open] { ... }
.tab[data-active] { ... }
.checkbox[data-checked] { ... }
```

### className/style as Callbacks

Base UI supports callback functions for `className` and `style`:
```tsx
// Base UI only
<Tabs.Tab
  className={(state) => state.active ? 'tab-active' : 'tab'}
  style={(state) => ({ fontWeight: state.active ? 'bold' : 'normal' })}
/>
```

### Animation Data Attributes

Base UI uses `data-starting-style` and `data-ending-style` for CSS transitions:
```css
.Popup {
  transition: opacity 150ms, transform 150ms;
}
.Popup[data-starting-style],
.Popup[data-ending-style] {
  opacity: 0;
  transform: scale(0.9);
}
```

---

## Required CSS

Base UI requires `isolation: isolate` on the app root for proper portal stacking:

```css
#root {
  isolation: isolate;
}
```

---

## Features Only in Base UI

- `createHandle<T>()` for detached triggers and programmatic control
- `mergeProps` utility for safe prop merging
- `useRender` hook for building custom render-prop components
- `Toast` notification system
- `Combobox` and `Autocomplete` components
- `Drawer` component
- `NavigationMenu` component
- `Field` / `Fieldset` / `Form` validation system
- `Meter` component
- `NumberField` with formatting
- `ScrollArea` with custom scrollbars
- `Toolbar` component
- `CSPProvider` for nonce support
- `DirectionProvider` for RTL support
