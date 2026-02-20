# Base UI v1.2.0 Component Catalog

All components import from `@base-ui/react/<component>`.

```tsx
import { Dialog } from '@base-ui/react/dialog';
import { Select } from '@base-ui/react/select';
```

---

## Floating Components (Portal > Positioner > Popup)

These components use smart positioning via Floating UI. Always nest: `Portal > Positioner > Popup`.

### Select

Customizable dropdown select with keyboard navigation.

**Sub-parts**: `Root`, `Trigger`, `Value`, `Icon`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `List`, `Item`, `ItemText`, `ItemIndicator`, `Arrow`, `ScrollUpArrow`, `ScrollDownArrow`, `Separator`, `Group`, `GroupLabel`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`, `multiple`, `disabled`, `open`, `onOpenChange`, `modal`
- `Positioner`: `side`, `align`, `sideOffset`, `alignOffset`, `alignItemWithTrigger`
- `Item`: `value`, `disabled`, `label`

```tsx
import { Select } from '@base-ui/react/select';

<Select.Root defaultValue="react">
  <Select.Trigger>
    <Select.Value placeholder="Select..." />
    <Select.Icon />
  </Select.Trigger>
  <Select.Portal>
    <Select.Positioner sideOffset={8}>
      <Select.Popup>
        <Select.Item value="react">
          <Select.ItemIndicator />
          <Select.ItemText>React</Select.ItemText>
        </Select.Item>
        <Select.Item value="vue">
          <Select.ItemIndicator />
          <Select.ItemText>Vue</Select.ItemText>
        </Select.Item>
      </Select.Popup>
    </Select.Positioner>
  </Select.Portal>
</Select.Root>
```

### Popover

Floating content panel anchored to a trigger.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Viewport`, `Title`, `Description`, `Close`

**Key props**:
- `Root`: `open`, `onOpenChange`, `defaultOpen`, `delay`, `closeDelay`, `disablePointerDismissal`
- `Positioner`: `side`, `align`, `sideOffset`, `alignOffset`, `collisionAvoidance`, `collisionPadding`, `disableAnchorTracking`

```tsx
import { Popover } from '@base-ui/react/popover';

<Popover.Root>
  <Popover.Trigger>Open</Popover.Trigger>
  <Popover.Portal>
    <Popover.Positioner side="top" align="center" sideOffset={8}>
      <Popover.Popup>
        <Popover.Arrow />
        <Popover.Title>Title</Popover.Title>
        <Popover.Description>Description</Popover.Description>
        <Popover.Close>Close</Popover.Close>
      </Popover.Popup>
    </Popover.Positioner>
  </Popover.Portal>
</Popover.Root>
```

### Tooltip

Accessible tooltip on hover/focus.

**Sub-parts**: `Provider`, `Root`, `Trigger`, `Portal`, `Positioner`, `Popup`, `Arrow`, `Viewport`

**Key props**:
- `Provider`: `delay`, `closeDelay`, `timeout` (group-level defaults)
- `Trigger`: `delay`, `closeDelay`
- `Root`: `open`, `onOpenChange`, `defaultOpen`, `disableHoverablePopup`
- `Positioner`: `side`, `align`, `sideOffset`

```tsx
import { Tooltip } from '@base-ui/react/tooltip';

<Tooltip.Provider>
  <Tooltip.Root>
    <Tooltip.Trigger delay={200}>Hover me</Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Positioner side="top" sideOffset={8}>
        <Tooltip.Popup>
          <Tooltip.Arrow />
          Tooltip content
        </Tooltip.Popup>
      </Tooltip.Positioner>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>
```

### Menu

Dropdown menu with items, radio groups, checkboxes, and submenus.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Item`, `LinkItem`, `Separator`, `Group`, `GroupLabel`, `RadioGroup`, `RadioItem`, `RadioItemIndicator`, `CheckboxItem`, `CheckboxItemIndicator`, `SubmenuRoot`, `SubmenuTrigger`

**Key props**:
- `Root`: `open`, `onOpenChange`, `modal`, `disablePointerDismissal`
- `Trigger`: `openOnHover`, `delay`, `closeDelay`
- `SubmenuTrigger`: `openOnHover`, `delay` (default 100), `closeDelay` (default 0)
- Also: `Menu.createHandle<T>()`

```tsx
import { Menu } from '@base-ui/react/menu';

<Menu.Root>
  <Menu.Trigger>Options</Menu.Trigger>
  <Menu.Portal>
    <Menu.Positioner sideOffset={8}>
      <Menu.Popup>
        <Menu.Item>Cut</Menu.Item>
        <Menu.Item>Copy</Menu.Item>
        <Menu.Separator />
        <Menu.Item>Paste</Menu.Item>
      </Menu.Popup>
    </Menu.Positioner>
  </Menu.Portal>
</Menu.Root>
```

### Menubar

Horizontal menubar wrapping multiple `Menu.Root` instances.

**Sub-parts**: standalone component (wraps `Menu.*`)

```tsx
import { Menubar } from '@base-ui/react/menubar';
import { Menu } from '@base-ui/react/menu';

<Menubar>
  <Menu.Root>
    <Menu.Trigger>File</Menu.Trigger>
    <Menu.Portal>
      <Menu.Positioner>
        <Menu.Popup>
          <Menu.Item>New</Menu.Item>
          <Menu.Item>Open</Menu.Item>
        </Menu.Popup>
      </Menu.Positioner>
    </Menu.Portal>
  </Menu.Root>
</Menubar>
```

### Context Menu

Right-click context menu.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Item`, `LinkItem`, `Separator`, `Group`, `GroupLabel`, `RadioGroup`, `RadioItem`, `CheckboxItem`, `SubmenuRoot`, `SubmenuTrigger`

```tsx
import { ContextMenu } from '@base-ui/react/context-menu';

<ContextMenu.Root>
  <ContextMenu.Trigger>Right-click here</ContextMenu.Trigger>
  <ContextMenu.Portal>
    <ContextMenu.Positioner>
      <ContextMenu.Popup>
        <ContextMenu.Item>Cut</ContextMenu.Item>
        <ContextMenu.Item>Copy</ContextMenu.Item>
        <ContextMenu.Item>Paste</ContextMenu.Item>
      </ContextMenu.Popup>
    </ContextMenu.Positioner>
  </ContextMenu.Portal>
</ContextMenu.Root>
```

### Preview Card

Hoverable preview card anchored to a link.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`

```tsx
import { PreviewCard } from '@base-ui/react/preview-card';

<PreviewCard.Root>
  <PreviewCard.Trigger href="https://example.com">
    Hover to preview
  </PreviewCard.Trigger>
  <PreviewCard.Portal>
    <PreviewCard.Positioner side="top" sideOffset={8}>
      <PreviewCard.Popup>
        <PreviewCard.Arrow />
        Preview content here
      </PreviewCard.Popup>
    </PreviewCard.Positioner>
  </PreviewCard.Portal>
</PreviewCard.Root>
```

### Combobox

Searchable select with filtering.

**Sub-parts**: `Root`, `Input`, `Trigger`, `Icon`, `Clear`, `Value`, `Chips`, `Chip`, `ChipRemove`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Status`, `Empty`, `List`, `Row`, `Item`, `ItemIndicator`, `Group`, `GroupLabel`, `Separator`, `Collection`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`, `multiple`, `loopFocus`
- Also: `Combobox.useFilteredItems<T>()`

```tsx
import { Combobox } from '@base-ui/react/combobox';

<Combobox.Root>
  <Combobox.Input placeholder="Search..." />
  <Combobox.Portal>
    <Combobox.Positioner sideOffset={8}>
      <Combobox.Popup>
        <Combobox.Empty>No results</Combobox.Empty>
        <Combobox.Item value="react">
          <Combobox.ItemIndicator />
          React
        </Combobox.Item>
      </Combobox.Popup>
    </Combobox.Positioner>
  </Combobox.Portal>
</Combobox.Root>
```

### Autocomplete

Input with suggestion dropdown.

**Sub-parts**: `Root`, `Input`, `Trigger`, `Icon`, `Clear`, `Value`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Status`, `Empty`, `List`, `Row`, `Item`, `Separator`, `Group`, `GroupLabel`, `Collection`

```tsx
import { Autocomplete } from '@base-ui/react/autocomplete';

<Autocomplete.Root>
  <Autocomplete.Input placeholder="Type to search..." />
  <Autocomplete.Portal>
    <Autocomplete.Positioner sideOffset={8}>
      <Autocomplete.Popup>
        <Autocomplete.Empty>No results</Autocomplete.Empty>
        <Autocomplete.List>
          <Autocomplete.Item value="option1">Option 1</Autocomplete.Item>
        </Autocomplete.List>
      </Autocomplete.Popup>
    </Autocomplete.Positioner>
  </Autocomplete.Portal>
</Autocomplete.Root>
```

### Navigation Menu

Accessible navigation menu with animated content transitions.

**Sub-parts**: `Root`, `List`, `Item`, `Trigger`, `Icon`, `Content`, `Link`, `Portal`, `Backdrop`, `Positioner`, `Popup`, `Arrow`, `Viewport`

```tsx
import { NavigationMenu } from '@base-ui/react/navigation-menu';

<NavigationMenu.Root>
  <NavigationMenu.List>
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>
        Products
        <NavigationMenu.Icon />
      </NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <NavigationMenu.Link href="/products/a">Product A</NavigationMenu.Link>
        <NavigationMenu.Link href="/products/b">Product B</NavigationMenu.Link>
      </NavigationMenu.Content>
    </NavigationMenu.Item>
    <NavigationMenu.Item>
      <NavigationMenu.Link href="/about">About</NavigationMenu.Link>
    </NavigationMenu.Item>
  </NavigationMenu.List>
  <NavigationMenu.Portal>
    <NavigationMenu.Positioner>
      <NavigationMenu.Popup>
        <NavigationMenu.Viewport />
      </NavigationMenu.Popup>
    </NavigationMenu.Positioner>
  </NavigationMenu.Portal>
</NavigationMenu.Root>
```

---

## Overlay Components (Portal > Backdrop + Popup)

No Positioner needed. Use `Portal > Backdrop + Viewport > Popup`.

### Dialog

Modal dialog with focus trap and scroll lock.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Viewport`, `Popup`, `Title`, `Description`, `Close`

**Key props**:
- `Root`: `open`, `onOpenChange`, `defaultOpen`, `modal`, `disablePointerDismissal`
- Also: `Dialog.createHandle<T>()`

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

### Alert Dialog

Non-dismissable dialog requiring explicit user action.

**Sub-parts**: `Root`, `Trigger`, `Portal`, `Backdrop`, `Viewport`, `Popup`, `Title`, `Description`, `Close`

**Key props**: Same as Dialog but no pointer dismissal by default.
- Also: `AlertDialog.createHandle<T>()`

```tsx
import { AlertDialog } from '@base-ui/react/alert-dialog';

<AlertDialog.Root>
  <AlertDialog.Trigger>Delete</AlertDialog.Trigger>
  <AlertDialog.Portal>
    <AlertDialog.Backdrop />
    <AlertDialog.Popup>
      <AlertDialog.Title>Confirm Delete</AlertDialog.Title>
      <AlertDialog.Description>This action cannot be undone.</AlertDialog.Description>
      <AlertDialog.Close>Cancel</AlertDialog.Close>
      <button>Confirm</button>
    </AlertDialog.Popup>
  </AlertDialog.Portal>
</AlertDialog.Root>
```

### Drawer

Slide-in panel from screen edge.

**Sub-parts**: `Provider`, `IndentBackground`, `Indent`, `Root`, `Trigger`, `Portal`, `Backdrop`, `Viewport`, `Popup`, `Content`, `Title`, `Description`, `Close`

**Key props**:
- `Root`: `open`, `onOpenChange`, `direction` (`'left' | 'right' | 'top' | 'bottom'`), `modal`, `disablePointerDismissal`

```tsx
import { Drawer } from '@base-ui/react/drawer';

<Drawer.Root direction="right">
  <Drawer.Trigger>Open Drawer</Drawer.Trigger>
  <Drawer.Portal>
    <Drawer.Backdrop />
    <Drawer.Popup>
      <Drawer.Title>Drawer Title</Drawer.Title>
      <Drawer.Description>Content here</Drawer.Description>
      <Drawer.Close>Close</Drawer.Close>
    </Drawer.Popup>
  </Drawer.Portal>
</Drawer.Root>
```

---

## Form Components

### Field

Form field with label, description, validation, and error messages.

**Sub-parts**: `Root`, `Label`, `Control`, `Description`, `Item`, `Error`, `Validity`

**Key props**:
- `Root`: `name`, `invalid`, `disabled`
- `Error`: `match` (`'valueMissing' | 'typeMismatch' | 'tooShort' | ...`)
- `Validity`: render callback `(state: ValidityState) => ReactNode`

```tsx
import { Field } from '@base-ui/react/field';

<Field.Root>
  <Field.Label>Email</Field.Label>
  <Field.Control type="email" required placeholder="you@example.com" />
  <Field.Description>We'll never share your email.</Field.Description>
  <Field.Error match="valueMissing">Email is required.</Field.Error>
  <Field.Error match="typeMismatch">Enter a valid email.</Field.Error>
</Field.Root>
```

### Fieldset

Groups related fields with a legend.

**Sub-parts**: `Root`, `Legend`

```tsx
import { Fieldset } from '@base-ui/react/fieldset';

<Fieldset.Root>
  <Fieldset.Legend>Personal Information</Fieldset.Legend>
  {/* Field components here */}
</Fieldset.Root>
```

### Form

Form wrapper enabling custom validation and server-side error handling.

**Sub-parts**: standalone component

**Key props**: `errors` (record of field name to error messages), `onClearErrors`

```tsx
import { Form } from '@base-ui/react/form';

<Form errors={serverErrors} onClearErrors={setServerErrors}>
  {/* Field components here */}
  <button type="submit">Submit</button>
</Form>
```

### Input

Standalone text input component.

**Sub-parts**: standalone component

```tsx
import { Input } from '@base-ui/react/input';

<Input placeholder="Type here..." />
```

### Checkbox

Accessible checkbox with indeterminate support.

**Sub-parts**: `Root`, `Indicator`

**Key props**:
- `Root`: `checked`, `defaultChecked`, `onCheckedChange`, `indeterminate`, `disabled`, `name`, `value`

```tsx
import { Checkbox } from '@base-ui/react/checkbox';

<Checkbox.Root defaultChecked>
  <Checkbox.Indicator />
  Accept terms
</Checkbox.Root>
```

### Checkbox Group

Groups multiple checkboxes with shared state.

**Sub-parts**: standalone component (wraps `Checkbox.Root`)

**Key props**: `value`, `defaultValue`, `onValueChange`, `allValues` (for indeterminate parent)

```tsx
import { CheckboxGroup } from '@base-ui/react/checkbox-group';
import { Checkbox } from '@base-ui/react/checkbox';

<CheckboxGroup defaultValue={['react']}>
  <Checkbox.Root name="react" value="react">
    <Checkbox.Indicator /> React
  </Checkbox.Root>
  <Checkbox.Root name="vue" value="vue">
    <Checkbox.Indicator /> Vue
  </Checkbox.Root>
</CheckboxGroup>
```

### Radio

Accessible radio button.

**Sub-parts**: `Root`, `Indicator`

```tsx
import { Radio } from '@base-ui/react/radio';

<Radio.Root value="option1">
  <Radio.Indicator />
  Option 1
</Radio.Root>
```

### RadioGroup

Groups radio buttons for single selection.

**Sub-parts**: standalone component (wraps `Radio.Root`)

```tsx
import { RadioGroup } from '@base-ui/react/radio-group';
import { Radio } from '@base-ui/react/radio';

<RadioGroup defaultValue="react">
  <Radio.Root value="react"><Radio.Indicator /> React</Radio.Root>
  <Radio.Root value="vue"><Radio.Indicator /> Vue</Radio.Root>
</RadioGroup>
```

### Number Field

Number input with increment/decrement and formatting.

**Sub-parts**: `Root`, `ScrubArea`, `ScrubAreaCursor`, `Group`, `Decrement`, `Input`, `Increment`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`, `min`, `max`, `step`, `smallStep`, `largeStep`, `formatOptions` (Intl.NumberFormatOptions), `allowWheelScrub`

```tsx
import { NumberField } from '@base-ui/react/number-field';

<NumberField.Root defaultValue={0} min={0} max={100}>
  <NumberField.Group>
    <NumberField.Decrement>-</NumberField.Decrement>
    <NumberField.Input />
    <NumberField.Increment>+</NumberField.Increment>
  </NumberField.Group>
</NumberField.Root>
```

### Slider

Range slider with single or multiple thumbs.

**Sub-parts**: `Root`, `Value`, `Control`, `Track`, `Indicator`, `Thumb`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`, `onValueCommit`, `min`, `max`, `step`, `minStepsBetweenValues`, `orientation`

```tsx
import { Slider } from '@base-ui/react/slider';

<Slider.Root defaultValue={50}>
  <Slider.Value />
  <Slider.Control>
    <Slider.Track>
      <Slider.Indicator />
      <Slider.Thumb />
    </Slider.Track>
  </Slider.Control>
</Slider.Root>
```

### Switch

Toggle switch.

**Sub-parts**: `Root`, `Thumb`

**Key props**: `checked`, `defaultChecked`, `onCheckedChange`, `disabled`, `name`

```tsx
import { Switch } from '@base-ui/react/switch';

<Switch.Root defaultChecked>
  <Switch.Thumb />
</Switch.Root>
```

### Toggle

Standalone toggle button.

**Sub-parts**: standalone component

**Key props**: `pressed`, `defaultPressed`, `onPressedChange`, `disabled`

```tsx
import { Toggle } from '@base-ui/react/toggle';

<Toggle defaultPressed>Bold</Toggle>
```

### Toggle Group

Groups toggles for single or multiple selection.

**Sub-parts**: standalone component (wraps `Toggle`)

**Key props**: `value`, `defaultValue`, `onValueChange`, `multiple`, `loopFocus`

```tsx
import { ToggleGroup } from '@base-ui/react/toggle-group';
import { Toggle } from '@base-ui/react/toggle';

<ToggleGroup defaultValue={['bold']}>
  <Toggle value="bold">B</Toggle>
  <Toggle value="italic">I</Toggle>
  <Toggle value="underline">U</Toggle>
</ToggleGroup>
```

---

## Display Components

### Accordion

Collapsible sections.

**Sub-parts**: `Root`, `Item`, `Header`, `Trigger`, `Panel`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`, `multiple` (default: `false`), `disabled`, `orientation`, `loopFocus`

```tsx
import { Accordion } from '@base-ui/react/accordion';

<Accordion.Root defaultValue={['item1']}>
  <Accordion.Item value="item1">
    <Accordion.Header>
      <Accordion.Trigger>Section 1</Accordion.Trigger>
    </Accordion.Header>
    <Accordion.Panel>Content 1</Accordion.Panel>
  </Accordion.Item>
</Accordion.Root>
```

### Avatar

User avatar with image and fallback.

**Sub-parts**: `Root`, `Image`, `Fallback`

```tsx
import { Avatar } from '@base-ui/react/avatar';

<Avatar.Root>
  <Avatar.Image src="/avatar.jpg" alt="User" />
  <Avatar.Fallback>JD</Avatar.Fallback>
</Avatar.Root>
```

### Button

Standalone accessible button.

**Sub-parts**: standalone component

```tsx
import { Button } from '@base-ui/react/button';

<Button disabled>Click me</Button>
```

### Collapsible

Single collapsible section.

**Sub-parts**: `Root`, `Trigger`, `Panel`

**Key props**: `open`, `defaultOpen`, `onOpenChange`

```tsx
import { Collapsible } from '@base-ui/react/collapsible';

<Collapsible.Root>
  <Collapsible.Trigger>Toggle</Collapsible.Trigger>
  <Collapsible.Panel>Collapsible content</Collapsible.Panel>
</Collapsible.Root>
```

### Meter

Visual indicator of a value within a range (e.g., disk usage).

**Sub-parts**: `Root`, `Label`, `Track`, `Indicator`, `Value`

```tsx
import { Meter } from '@base-ui/react/meter';

<Meter.Root value={75} min={0} max={100}>
  <Meter.Label>Disk Usage</Meter.Label>
  <Meter.Value />
  <Meter.Track>
    <Meter.Indicator />
  </Meter.Track>
</Meter.Root>
```

### Progress

Progress indicator.

**Sub-parts**: `Root`, `Label`, `Track`, `Indicator`, `Value`

**Key props**: `value`, `min`, `max`, `getValueLabel`

```tsx
import { Progress } from '@base-ui/react/progress';

<Progress.Root value={60}>
  <Progress.Label>Loading</Progress.Label>
  <Progress.Value />
  <Progress.Track>
    <Progress.Indicator />
  </Progress.Track>
</Progress.Root>
```

### Scroll Area

Custom scrollbar overlay.

**Sub-parts**: `Root`, `Viewport`, `Content`, `Scrollbar`, `Thumb`, `Corner`

```tsx
import { ScrollArea } from '@base-ui/react/scroll-area';

<ScrollArea.Root style={{ height: 300 }}>
  <ScrollArea.Viewport>
    <ScrollArea.Content>
      {/* Long content */}
    </ScrollArea.Content>
  </ScrollArea.Viewport>
  <ScrollArea.Scrollbar orientation="vertical">
    <ScrollArea.Thumb />
  </ScrollArea.Scrollbar>
  <ScrollArea.Corner />
</ScrollArea.Root>
```

### Separator

Visual divider.

**Sub-parts**: standalone component

```tsx
import { Separator } from '@base-ui/react/separator';

<Separator orientation="horizontal" />
```

### Tabs

Tabbed interface.

**Sub-parts**: `Root`, `List`, `Tab`, `Panel`, `Indicator`

**Key props**:
- `Root`: `value`, `defaultValue`, `onValueChange`
- `List`: `loopFocus`, `activateOnFocus`
- `Tab`: uses `[data-active]` attribute (NOT `[data-selected]`)

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

### Toast

Toast notification system.

**Sub-parts**: `Provider`, `Portal`, `Viewport`, `Root`, `Content`, `Title`, `Description`, `Action`, `Close`, `Positioner`, `Arrow`

**Key props**:
- `Provider`: wrap app root
- Also: `Toast.useToastManager()` hook for adding/removing toasts

```tsx
import { Toast } from '@base-ui/react/toast';

// App root
<Toast.Provider>
  <App />
</Toast.Provider>

// Toast display component
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
              <Toast.Close>Dismiss</Toast.Close>
            </Toast.Content>
          </Toast.Root>
        ))}
      </Toast.Viewport>
    </Toast.Portal>
  );
}
```

### Toolbar

Accessible toolbar grouping buttons and controls.

**Sub-parts**: `Root`, `Button`, `Link`, `Separator`, `Group`, `Input`

```tsx
import { Toolbar } from '@base-ui/react/toolbar';

<Toolbar.Root>
  <Toolbar.Button>Cut</Toolbar.Button>
  <Toolbar.Button>Copy</Toolbar.Button>
  <Toolbar.Separator />
  <Toolbar.Button>Paste</Toolbar.Button>
</Toolbar.Root>
```

---

## Positioner CSS Variables

Available on all `Positioner` components for dynamic styling:

| Variable | Description |
|----------|-------------|
| `--anchor-height` | Height of the anchor element |
| `--anchor-width` | Width of the anchor element |
| `--available-height` | Available height in viewport |
| `--available-width` | Available width in viewport |
| `--transform-origin` | Transform origin for animations |

## Positioner Common Props

| Prop | Type | Description |
|------|------|-------------|
| `side` | `'top' \| 'right' \| 'bottom' \| 'left'` | Preferred side |
| `align` | `'start' \| 'center' \| 'end'` | Alignment along the side |
| `sideOffset` | `number` | Gap between anchor and popup |
| `alignOffset` | `number` | Shift along alignment axis |
| `collisionAvoidance` | `'flip' \| 'shift' \| 'none'` | How to handle collisions |
| `collisionBoundary` | `HTMLElement` | Collision boundary element |
| `collisionPadding` | `number` | Padding from boundary |
| `sticky` | `'always' \| 'container'` | Sticky behavior |
| `positionMethod` | `'absolute' \| 'fixed'` | CSS position method |
| `disableAnchorTracking` | `boolean` | Disable anchor position tracking |
| `arrowPadding` | `number` | Padding for arrow positioning |
