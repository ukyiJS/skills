---
name: base-ui-react
description: |
  Base UI (@base-ui/react v1.2.0) is a headless, accessible React component library built by
  the teams behind Radix, Floating UI, and Material UI. It provides 36+ unstyled compound
  components with full styling control, smart positioning via Floating UI, and three flexible
  render patterns.

  Use when: Building accessible UIs with @base-ui/react, implementing Dialog/Select/Popover/
  Tooltip/Menu/Tabs/Accordion/Toast/Combobox, migrating from Radix UI to Base UI, creating
  form validation with Field/Fieldset/Form, using compound component patterns with render props,
  styling with data attributes and CSS variables, or needing headless components with Floating UI
  positioning.
license: MIT
---

# Base UI React

**Package**: `@base-ui/react` v1.2.0
**Status**: Stable (production-ready)
**React**: 19+
**Components**: 36+

## Installation

```bash
pnpm add @base-ui/react
```

**Required CSS** — add to your app root. This creates a stacking context so portaled components (Dialog, Popover, Tooltip, etc.) render above page content:

```css
#root {
  isolation: isolate;
}
```

## Architecture

Base UI uses a **compound component** pattern. Each component is a namespace with sub-parts:

```tsx
import { Dialog } from '@base-ui/react/dialog';

<Dialog.Root>           {/* State container */}
  <Dialog.Trigger />    {/* Interaction element */}
  <Dialog.Portal>       {/* Renders in document.body */}
    <Dialog.Backdrop /> {/* Overlay */}
    <Dialog.Viewport>   {/* Scroll container */}
      <Dialog.Popup>    {/* Content container */}
        <Dialog.Title />
        <Dialog.Description />
        <Dialog.Close />
      </Dialog.Popup>
    </Dialog.Viewport>
  </Dialog.Portal>
</Dialog.Root>
```

All imports use sub-path pattern: `import { X } from '@base-ui/react/x'`.

## Three Render Patterns

### 1. Direct Children (Default)

Components render their default HTML element. Simplest pattern.

```tsx
<Dialog.Trigger className="btn">Open</Dialog.Trigger>
```

### 2. Render Element

Replace the default HTML tag by passing a JSX element to `render`.

```tsx
<Dialog.Trigger render={<a href="/contact" />}>Contact</Dialog.Trigger>
```

### 3. Render Function

Full control via callback receiving `(props, state)`.

```tsx
<Dialog.Popup
  render={(props, state) => (
    <div {...mergeProps<'div'>(props, { className: styles.popup })}>
      {state.open ? 'Open' : 'Closed'}
    </div>
  )}
/>
```

Additionally, `className` and `style` accept callbacks:

```tsx
<Tabs.Tab className={(state) => state.active ? 'tab-active' : 'tab'} />
```

## Floating Components: Portal > Positioner > Popup

Components that float (Select, Popover, Tooltip, Menu, Combobox, etc.) use Floating UI for positioning. The nesting order is always **Portal > Positioner > Popup**:

```tsx
import { Popover } from '@base-ui/react/popover';

<Popover.Root>
  <Popover.Trigger>Open</Popover.Trigger>
  <Popover.Portal>
    <Popover.Positioner side="top" align="center" sideOffset={8}>
      <Popover.Popup>
        <Popover.Arrow />
        Content here
        <Popover.Close>×</Popover.Close>
      </Popover.Popup>
    </Popover.Positioner>
  </Popover.Portal>
</Popover.Root>
```

**Positioner props**: `side` (top/right/bottom/left), `align` (start/center/end), `sideOffset`, `alignOffset`, `collisionAvoidance` (flip/shift/none), `collisionPadding`, `disableAnchorTracking`.

**CSS variables** on Positioner: `--anchor-width`, `--anchor-height`, `--available-width`, `--available-height`, `--transform-origin`.

## Styling with Data Attributes

Components expose state via data attributes for CSS styling:

```css
.trigger[data-popup-open] { background: var(--color-gray-100); }
.tab[data-active] { font-weight: bold; }
.item[data-highlighted] { background: var(--color-blue-50); }
.checkbox[data-checked] { border-color: var(--color-blue); }
.item[data-disabled] { opacity: 0.5; }
```

**Animations** use `data-starting-style` and `data-ending-style`:

```css
.Popup {
  transform-origin: var(--transform-origin);
  transition: opacity 150ms, transform 150ms;
}
.Popup[data-starting-style],
.Popup[data-ending-style] {
  opacity: 0;
  transform: scale(0.9);
}
```

## Component Categories

### Floating (Portal > Positioner > Popup)
Select, Popover, Tooltip, Menu, Menubar, ContextMenu, PreviewCard, Combobox, Autocomplete, NavigationMenu

### Overlay (Portal > Backdrop + Viewport > Popup)
Dialog, AlertDialog, Drawer

### Form
Field, Fieldset, Form, Input, Checkbox, CheckboxGroup, Radio, RadioGroup, NumberField, Slider, Switch, Toggle, ToggleGroup

### Display
Accordion, Avatar, Button, Collapsible, Meter, Progress, ScrollArea, Separator, Tabs, Toast, Toolbar

## Key Component Examples

### Dialog

Modal dialog with backdrop, viewport, focus trap, and scroll lock. Use `Dialog.Viewport` to wrap `Popup` for scroll containment. Use `disablePointerDismissal` to prevent closing on outside click. Use `Dialog.createHandle<T>()` for programmatic control with typed payloads from detached triggers.

```tsx
import { Dialog } from '@base-ui/react/dialog';

<Dialog.Root>
  <Dialog.Trigger>Open</Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Backdrop />
    <Dialog.Viewport>
      <Dialog.Popup>
        <Dialog.Title>Title</Dialog.Title>
        <Dialog.Description>Description</Dialog.Description>
        <Dialog.Close>Close</Dialog.Close>
      </Dialog.Popup>
    </Dialog.Viewport>
  </Dialog.Portal>
</Dialog.Root>
```

CSS variable `--nested-dialogs` is available on Popup for stacked dialog effects.

### Select

Dropdown select with keyboard navigation, type-ahead, and scroll arrows. Uses `Select.Item` (not `Select.Option`). Each item needs `Select.ItemText` for the display label and optional `Select.ItemIndicator` for the checkmark. Use `multiple` prop on Root for multi-select. Use `alignItemWithTrigger={false}` on Positioner for standard dropdown alignment instead of item-aligned behavior.

```tsx
import { Select } from '@base-ui/react/select';

<Select.Root defaultValue="react">
  <Select.Trigger>
    <Select.Value placeholder="Choose..." />
    <Select.Icon />
  </Select.Trigger>
  <Select.Portal>
    <Select.Positioner sideOffset={4}>
      <Select.Popup>
        <Select.Item value="react">
          <Select.ItemIndicator>✓</Select.ItemIndicator>
          <Select.ItemText>React</Select.ItemText>
        </Select.Item>
        <Select.Item value="vue">
          <Select.ItemIndicator>✓</Select.ItemIndicator>
          <Select.ItemText>Vue</Select.ItemText>
        </Select.Item>
      </Select.Popup>
    </Select.Positioner>
  </Select.Portal>
</Select.Root>
```

For multi-select, `Select.Value` children accepts a render function: `<Select.Value>{(values) => formatDisplay(values)}</Select.Value>`.

### Tooltip

Place `delay` and `closeDelay` on `Tooltip.Trigger`, not on Root. Wrap app in `Tooltip.Provider` to share `delay`, `closeDelay`, and `timeout` defaults across all tooltips. Use `disableHoverablePopup` on Root to prevent users from hovering over the popup to keep it open. For disabled elements, render the trigger as a `<span>` wrapping the disabled button.

```tsx
import { Tooltip } from '@base-ui/react/tooltip';

<Tooltip.Provider delay={200} closeDelay={0}>
  <Tooltip.Root>
    <Tooltip.Trigger delay={200}>Hover me</Tooltip.Trigger>
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

### Menu

Dropdown menu with items, separators, radio groups, checkbox items, and submenus. Place `openOnHover`, `delay`, and `closeDelay` on `Menu.Trigger` (not Root). Use `Menu.SubmenuRoot` and `Menu.SubmenuTrigger` for nested submenus. Use `Menu.createHandle<T>()` for programmatic control.

```tsx
import { Menu } from '@base-ui/react/menu';

<Menu.Root>
  <Menu.Trigger>Options</Menu.Trigger>
  <Menu.Portal>
    <Menu.Positioner sideOffset={8}>
      <Menu.Popup>
        <Menu.Arrow />
        <Menu.Item>Cut</Menu.Item>
        <Menu.Item>Copy</Menu.Item>
        <Menu.Separator />
        <Menu.SubmenuRoot>
          <Menu.SubmenuTrigger>More</Menu.SubmenuTrigger>
          <Menu.Portal>
            <Menu.Positioner side="right">
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

### Form Validation

Use `Field` for individual form fields with labels, descriptions, and error messages. `Field.Error` uses `match` prop to show errors for specific validity states (`valueMissing`, `typeMismatch`, `tooShort`, etc.). Use `Form` wrapper for server-side error handling via `errors` prop. Use `Fieldset` to group related fields.

```tsx
import { Field } from '@base-ui/react/field';
import { Form } from '@base-ui/react/form';

<Form errors={serverErrors} onClearErrors={setServerErrors}>
  <Field.Root name="email">
    <Field.Label>Email</Field.Label>
    <Field.Control type="email" required />
    <Field.Error match="valueMissing">Required.</Field.Error>
    <Field.Error match="typeMismatch">Invalid email.</Field.Error>
    <Field.Error />  {/* Server errors */}
  </Field.Root>
  <button type="submit">Submit</button>
</Form>
```

### Toast

Provider/Manager pattern. Wrap app in `Toast.Provider`. Use `Toast.useToastManager()` to add/remove toasts from any component. Render all active toasts in a `Toast.Viewport`.

```tsx
import { Toast } from '@base-ui/react/toast';

// App root
<Toast.Provider><App /></Toast.Provider>

// Toast viewport
function Toasts() {
  const { toasts } = Toast.useToastManager();
  return (
    <Toast.Portal>
      <Toast.Viewport>
        {toasts.map((toast) => (
          <Toast.Root key={toast.id} toast={toast}>
            <Toast.Content>
              <Toast.Title />
              <Toast.Description />
              <Toast.Close>×</Toast.Close>
            </Toast.Content>
          </Toast.Root>
        ))}
      </Toast.Viewport>
    </Toast.Portal>
  );
}

// Adding toasts
const manager = Toast.useToastManager();
manager.add({ title: 'Saved', description: 'Changes saved.' });
```

### Accordion

Collapsible sections. `multiple` defaults to `false` (single panel open at a time). Set `multiple` on Root to allow concurrent panels. Value is always an array. Use `loopFocus` on Root for keyboard focus looping.

```tsx
import { Accordion } from '@base-ui/react/accordion';

<Accordion.Root defaultValue={['section1']}>
  <Accordion.Item value="section1">
    <Accordion.Header>
      <Accordion.Trigger>Section 1</Accordion.Trigger>
    </Accordion.Header>
    <Accordion.Panel>Content 1</Accordion.Panel>
  </Accordion.Item>
  <Accordion.Item value="section2">
    <Accordion.Header>
      <Accordion.Trigger>Section 2</Accordion.Trigger>
    </Accordion.Header>
    <Accordion.Panel>Content 2</Accordion.Panel>
  </Accordion.Item>
</Accordion.Root>
```

### Tabs

Tabbed interface with optional animated indicator. Active tab uses `[data-active]` attribute (not `[data-selected]`). Use `loopFocus` on List for keyboard looping. Use `activateOnFocus` on List to activate tab on focus (automatic activation).

```tsx
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

## Utilities

### mergeProps

Import from `@base-ui/react/merge-props`. Safely merges two or more sets of React props. Event handlers are chained (all invoked), classNames are concatenated, styles are merged (later values override). Essential when using the render function pattern to combine Base UI's internal props with your own:

```tsx
import { mergeProps } from '@base-ui/react/merge-props';

<Component
  render={(props, state) => (
    <button
      {...mergeProps<'button'>(props, {
        className: styles.Button,
        onClick: () => trackAnalytics('click'),
      })}
    />
  )}
/>
```

Also supports a function form for accessing previous handlers:

```tsx
const merged = mergeProps(
  { onClick(e) { /* first */ } },
  (prev) => ({ onClick(e) { prev.onClick?.(e); /* then second */ } }),
);
```

### useRender

Import from `@base-ui/react/use-render`. Hook for building your own components that accept the `render` prop pattern (Base UI's equivalent of a Slot/asChild). Returns a React element. Use with `useRender.ComponentProps<'button', State>` for typing.

### CSPProvider

Import from `@base-ui/react/csp-provider`. Wraps your app to provide a `nonce` value for inline `<style>` and `<script>` tags generated by Base UI. Required for apps with strict Content Security Policy headers. Also accepts `disableStyleElements` to skip inline style rendering entirely.

### DirectionProvider

Import from `@base-ui/react/direction-provider`. Provides RTL/LTR direction context for components like Slider that need to reverse their layout in RTL mode. Wrap the relevant subtree with `<DirectionProvider direction="rtl">`.

## Critical Rules

### Always

- Use `Portal > Positioner > Popup` nesting for floating components
- Use `Portal > Backdrop + Viewport > Popup` for Dialog/AlertDialog/Drawer
- Add `isolation: isolate` to app root CSS
- Use `align` (not `alignment`) on Positioner
- Use `Select.Item` + `Select.ItemText` + `Select.ItemIndicator` (not `Select.Option`)
- Use `[data-active]` for Tabs (not `[data-selected]`)
- Place `delay`/`closeDelay` on `Tooltip.Trigger` (not Root)
- Use `mergeProps` when combining props in render functions

### Never

- Use `asChild` — Base UI uses `render` prop or direct children
- Skip `Portal` — portaling is explicit, not automatic
- Skip `Positioner` for floating components — required for positioning
- Use Radix naming: `Content`→`Popup`, `Overlay`→`Backdrop`
- Use `@base-ui-components/react` — old beta package name
- Assume `Accordion.Root` allows multiple panels by default — `multiple` defaults to `false`

## References

For detailed documentation, load these files when needed:

- **`references/component-catalog.md`** — All 36 components with sub-parts, props, and minimal examples
- **`references/migration-from-radix.md`** — Radix UI → Base UI migration with side-by-side comparisons
- **`references/known-patterns.md`** — Common patterns: Dialog+Form, createHandle, multi-select, Toast, animations, nested menus, composition
