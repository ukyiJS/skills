# Common Patterns & Code Examples

Reusable patterns for Base UI `@base-ui/react` v1.2.0.

---

## Dialog with Form

Controlled dialog that closes on successful form submission.

```tsx
import { Dialog } from '@base-ui/react/dialog';
import { Field } from '@base-ui/react/field';
import * as React from 'react';

function FormDialog() {
  const [open, setOpen] = React.useState(false);

  return (
    <Dialog.Root open={open} onOpenChange={setOpen}>
      <Dialog.Trigger>Add Item</Dialog.Trigger>
      <Dialog.Portal>
        <Dialog.Backdrop className="fixed inset-0 bg-black/40" />
        <Dialog.Viewport>
          <Dialog.Popup className="fixed inset-0 m-auto h-fit w-full max-w-md rounded-lg bg-white p-6 shadow-xl">
            <Dialog.Title className="text-lg font-semibold">New Item</Dialog.Title>
            <form
              onSubmit={async (e) => {
                e.preventDefault();
                const data = new FormData(e.currentTarget);
                await saveItem(Object.fromEntries(data));
                setOpen(false);
              }}
            >
              <Field.Root name="title" className="mt-4">
                <Field.Label className="text-sm font-medium">Title</Field.Label>
                <Field.Control required className="mt-1 w-full rounded border px-3 py-2" />
                <Field.Error match="valueMissing" className="text-sm text-red-600">
                  Title is required.
                </Field.Error>
              </Field.Root>

              <div className="mt-6 flex justify-end gap-2">
                <Dialog.Close className="rounded px-4 py-2 text-gray-600 hover:bg-gray-100">
                  Cancel
                </Dialog.Close>
                <button type="submit" className="rounded bg-blue-600 px-4 py-2 text-white">
                  Save
                </button>
              </div>
            </form>
          </Dialog.Popup>
        </Dialog.Viewport>
      </Dialog.Portal>
    </Dialog.Root>
  );
}
```

---

## Controlled Dialog with createHandle

Programmatic dialog control with typed payloads and detached triggers.

```tsx
import { Dialog } from '@base-ui/react/dialog';
import * as React from 'react';

interface ConfirmPayload {
  id: string;
  name: string;
}

const confirmDialog = Dialog.createHandle<ConfirmPayload>();

function DeleteButton({ id, name }: { id: string; name: string }) {
  return (
    <Dialog.Trigger handle={confirmDialog} payload={{ id, name }}>
      Delete {name}
    </Dialog.Trigger>
  );
}

function ConfirmDialog() {
  return (
    <Dialog.Root handle={confirmDialog}>
      {({ payload }) => (
        <Dialog.Portal>
          <Dialog.Backdrop className="fixed inset-0 bg-black/40" />
          <Dialog.Viewport>
            <Dialog.Popup className="fixed inset-0 m-auto h-fit w-full max-w-sm rounded-lg bg-white p-6">
              <Dialog.Title>Delete "{payload?.name}"?</Dialog.Title>
              <Dialog.Description>This action cannot be undone.</Dialog.Description>
              <div className="mt-4 flex justify-end gap-2">
                <Dialog.Close>Cancel</Dialog.Close>
                <button
                  onClick={async () => {
                    if (payload) await deleteItem(payload.id);
                    confirmDialog.close();
                  }}
                  className="rounded bg-red-600 px-4 py-2 text-white"
                >
                  Delete
                </button>
              </div>
            </Dialog.Popup>
          </Dialog.Viewport>
        </Dialog.Portal>
      )}
    </Dialog.Root>
  );
}

// Usage: place ConfirmDialog once, DeleteButton anywhere
function App() {
  return (
    <>
      <DeleteButton id="1" name="Project A" />
      <DeleteButton id="2" name="Project B" />
      <ConfirmDialog />
    </>
  );
}
```

---

## Select (Basic)

```tsx
import { Select } from '@base-ui/react/select';

const languages = { js: 'JavaScript', ts: 'TypeScript', py: 'Python' };

function LanguageSelect() {
  return (
    <Select.Root defaultValue="ts">
      <Select.Trigger className="flex items-center gap-2 rounded border px-3 py-2">
        <Select.Value />
        <Select.Icon>▾</Select.Icon>
      </Select.Trigger>
      <Select.Portal>
        <Select.Positioner sideOffset={4}>
          <Select.Popup className="rounded border bg-white shadow-lg">
            {Object.entries(languages).map(([value, label]) => (
              <Select.Item key={value} value={value} className="px-3 py-2 outline-none data-[highlighted]:bg-blue-50">
                <Select.ItemIndicator className="mr-2">✓</Select.ItemIndicator>
                <Select.ItemText>{label}</Select.ItemText>
              </Select.Item>
            ))}
          </Select.Popup>
        </Select.Positioner>
      </Select.Portal>
    </Select.Root>
  );
}
```

---

## Select (Multiple)

```tsx
import { Select } from '@base-ui/react/select';

function MultiSelect() {
  const options = ['React', 'Vue', 'Angular', 'Svelte'];

  function renderValue(selected: string[]) {
    if (selected.length === 0) return 'Select frameworks...';
    if (selected.length === 1) return selected[0];
    return `${selected[0]} (+${selected.length - 1} more)`;
  }

  return (
    <Select.Root multiple defaultValue={['React']}>
      <Select.Trigger className="flex items-center gap-2 rounded border px-3 py-2">
        <Select.Value>{renderValue}</Select.Value>
        <Select.Icon>▾</Select.Icon>
      </Select.Trigger>
      <Select.Portal>
        <Select.Positioner sideOffset={4} alignItemWithTrigger={false}>
          <Select.Popup className="rounded border bg-white shadow-lg">
            {options.map((opt) => (
              <Select.Item key={opt} value={opt} className="px-3 py-2 outline-none data-[highlighted]:bg-blue-50">
                <Select.ItemIndicator className="mr-2">✓</Select.ItemIndicator>
                <Select.ItemText>{opt}</Select.ItemText>
              </Select.Item>
            ))}
          </Select.Popup>
        </Select.Positioner>
      </Select.Portal>
    </Select.Root>
  );
}
```

---

## Combobox (Searchable Select)

```tsx
import { Combobox } from '@base-ui/react/combobox';

const items = ['Apple', 'Banana', 'Cherry', 'Date', 'Elderberry'];

function SearchableSelect() {
  const filteredItems = Combobox.useFilteredItems({ items, limit: 20 });

  return (
    <Combobox.Root>
      <Combobox.Input placeholder="Search fruit..." className="rounded border px-3 py-2" />
      <Combobox.Portal>
        <Combobox.Positioner sideOffset={4}>
          <Combobox.Popup className="rounded border bg-white shadow-lg">
            <Combobox.Empty className="px-3 py-2 text-gray-500">No results found</Combobox.Empty>
            {filteredItems.map((item) => (
              <Combobox.Item key={item} value={item} className="px-3 py-2 outline-none data-[highlighted]:bg-blue-50">
                <Combobox.ItemIndicator className="mr-2">✓</Combobox.ItemIndicator>
                {item}
              </Combobox.Item>
            ))}
          </Combobox.Popup>
        </Combobox.Positioner>
      </Combobox.Portal>
    </Combobox.Root>
  );
}
```

---

## Form with Validation

Client-side and server-side validation using Field, Fieldset, and Form.

```tsx
import { Field } from '@base-ui/react/field';
import { Fieldset } from '@base-ui/react/fieldset';
import { Form } from '@base-ui/react/form';
import * as React from 'react';

function RegistrationForm() {
  const [errors, setErrors] = React.useState<Record<string, string>>({});

  return (
    <Form
      errors={errors}
      onClearErrors={setErrors}
      onSubmit={async (e) => {
        e.preventDefault();
        const data = new FormData(e.currentTarget);
        const result = await registerUser(Object.fromEntries(data));
        if (result.errors) {
          setErrors(result.errors);
        }
      }}
    >
      <Fieldset.Root>
        <Fieldset.Legend className="text-lg font-semibold">Account</Fieldset.Legend>

        <Field.Root name="email" className="mt-4">
          <Field.Label>Email</Field.Label>
          <Field.Control type="email" required className="w-full rounded border px-3 py-2" />
          <Field.Error match="valueMissing">Email is required.</Field.Error>
          <Field.Error match="typeMismatch">Enter a valid email.</Field.Error>
          <Field.Error />  {/* Server-side errors */}
        </Field.Root>

        <Field.Root name="password" className="mt-4">
          <Field.Label>Password</Field.Label>
          <Field.Control type="password" required minLength={8} className="w-full rounded border px-3 py-2" />
          <Field.Error match="valueMissing">Password is required.</Field.Error>
          <Field.Error match="tooShort">At least 8 characters.</Field.Error>
        </Field.Root>
      </Fieldset.Root>

      <button type="submit" className="mt-6 rounded bg-blue-600 px-4 py-2 text-white">
        Register
      </button>
    </Form>
  );
}
```

---

## Toast Notifications

```tsx
import { Toast } from '@base-ui/react/toast';

// 1. Wrap app in Provider
function App() {
  return (
    <Toast.Provider>
      <MainContent />
      <ToastViewport />
    </Toast.Provider>
  );
}

// 2. Toast viewport renders all toasts
function ToastViewport() {
  const { toasts } = Toast.useToastManager();

  return (
    <Toast.Portal>
      <Toast.Viewport className="fixed bottom-4 right-4 z-50 flex flex-col gap-2">
        {toasts.map((toast) => (
          <Toast.Root key={toast.id} toast={toast}>
            <Toast.Content className="rounded-lg border bg-white p-4 shadow-lg">
              <Toast.Title className="font-semibold" />
              <Toast.Description className="text-sm text-gray-600" />
              <Toast.Close className="absolute right-2 top-2 text-gray-400">×</Toast.Close>
            </Toast.Content>
          </Toast.Root>
        ))}
      </Toast.Viewport>
    </Toast.Portal>
  );
}

// 3. Add toasts from any component
function SaveButton() {
  const toastManager = Toast.useToastManager();

  return (
    <button
      onClick={async () => {
        await save();
        toastManager.add({
          title: 'Saved',
          description: 'Changes saved successfully.',
          type: 'success',
        });
      }}
    >
      Save
    </button>
  );
}
```

---

## Animated Transitions (CSS)

Base UI uses `data-starting-style` and `data-ending-style` for enter/exit transitions.

```css
/* Popup animation */
.Popup {
  transform-origin: var(--transform-origin);
  transition: opacity 150ms, transform 150ms;
}

.Popup[data-starting-style],
.Popup[data-ending-style] {
  opacity: 0;
  transform: scale(0.9);
}

/* Backdrop animation */
.Backdrop {
  opacity: 0.4;
  transition: opacity 200ms;
}

.Backdrop[data-starting-style],
.Backdrop[data-ending-style] {
  opacity: 0;
}

/* Collapsible/Accordion panel animation */
.Panel {
  overflow: hidden;
  transition: height 200ms;
}

.Panel[data-starting-style],
.Panel[data-ending-style] {
  height: 0;
}
```

For JavaScript-based animation libraries (e.g., Motion):

```tsx
import { Popover } from '@base-ui/react/popover';
import { motion, type HTMLMotionProps } from 'motion/react';

<Popover.Portal keepMounted>
  <Popover.Positioner>
    <Popover.Popup
      render={(props, state) => (
        <motion.div
          {...(props as HTMLMotionProps<'div'>)}
          initial={false}
          animate={{
            opacity: state.open ? 1 : 0,
            scale: state.open ? 1 : 0.9,
          }}
        />
      )}
    >
      Content
    </Popover.Popup>
  </Popover.Positioner>
</Popover.Portal>
```

---

## Nested Menus (Submenus)

```tsx
import { Menu } from '@base-ui/react/menu';

function FileMenu() {
  return (
    <Menu.Root>
      <Menu.Trigger className="rounded px-3 py-1.5 hover:bg-gray-100">File</Menu.Trigger>
      <Menu.Portal>
        <Menu.Positioner sideOffset={4}>
          <Menu.Popup className="min-w-[180px] rounded border bg-white py-1 shadow-lg">
            <Menu.Item className="px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
              New File
            </Menu.Item>
            <Menu.Item className="px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
              Open
            </Menu.Item>
            <Menu.Separator className="my-1 h-px bg-gray-200" />

            <Menu.SubmenuRoot>
              <Menu.SubmenuTrigger className="flex w-full items-center justify-between px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
                Recent Files
                <span>›</span>
              </Menu.SubmenuTrigger>
              <Menu.Portal>
                <Menu.Positioner side="right" sideOffset={-4} alignOffset={-4}>
                  <Menu.Popup className="min-w-[180px] rounded border bg-white py-1 shadow-lg">
                    <Menu.Item className="px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
                      document.tsx
                    </Menu.Item>
                    <Menu.Item className="px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
                      config.ts
                    </Menu.Item>
                  </Menu.Popup>
                </Menu.Positioner>
              </Menu.Portal>
            </Menu.SubmenuRoot>

            <Menu.Separator className="my-1 h-px bg-gray-200" />
            <Menu.Item className="px-3 py-1.5 outline-none data-[highlighted]:bg-blue-50">
              Exit
            </Menu.Item>
          </Menu.Popup>
        </Menu.Positioner>
      </Menu.Portal>
    </Menu.Root>
  );
}
```

---

## Tooltip on Disabled Element

Disabled elements don't fire pointer events. Wrap in a span:

```tsx
import { Tooltip } from '@base-ui/react/tooltip';

<Tooltip.Root>
  <Tooltip.Trigger render={<span />}>
    <button disabled className="rounded bg-gray-200 px-3 py-1.5 text-gray-400">
      Submit
    </button>
  </Tooltip.Trigger>
  <Tooltip.Portal>
    <Tooltip.Positioner side="top" sideOffset={4}>
      <Tooltip.Popup className="rounded bg-gray-900 px-2 py-1 text-sm text-white">
        Complete the form first
      </Tooltip.Popup>
    </Tooltip.Positioner>
  </Tooltip.Portal>
</Tooltip.Root>
```

---

## Number Field with Currency Formatting

```tsx
import { NumberField } from '@base-ui/react/number-field';

<NumberField.Root
  defaultValue={9.99}
  min={0}
  step={0.01}
  formatOptions={{ style: 'currency', currency: 'USD' }}
>
  <NumberField.ScrubArea>
    <label>Price</label>
    <NumberField.ScrubAreaCursor />
  </NumberField.ScrubArea>
  <NumberField.Group className="flex items-center">
    <NumberField.Decrement className="rounded-l border px-2 py-1">−</NumberField.Decrement>
    <NumberField.Input className="w-24 border-y px-3 py-1 text-center" />
    <NumberField.Increment className="rounded-r border px-2 py-1">+</NumberField.Increment>
  </NumberField.Group>
</NumberField.Root>
```

---

## Scroll Area

```tsx
import { ScrollArea } from '@base-ui/react/scroll-area';

<ScrollArea.Root className="h-64 w-full overflow-hidden rounded border">
  <ScrollArea.Viewport className="h-full w-full">
    <ScrollArea.Content className="p-4">
      {/* Long content here */}
    </ScrollArea.Content>
  </ScrollArea.Viewport>
  <ScrollArea.Scrollbar
    orientation="vertical"
    className="flex w-2.5 touch-none select-none p-0.5"
  >
    <ScrollArea.Thumb className="relative flex-1 rounded-full bg-gray-300" />
  </ScrollArea.Scrollbar>
  <ScrollArea.Corner />
</ScrollArea.Root>
```

---

## Tabs with Animated Indicator

```tsx
import { Tabs } from '@base-ui/react/tabs';

<Tabs.Root defaultValue="tab1">
  <Tabs.List className="relative flex border-b">
    <Tabs.Tab
      value="tab1"
      className="px-4 py-2 outline-none data-[active]:text-blue-600"
    >
      Overview
    </Tabs.Tab>
    <Tabs.Tab
      value="tab2"
      className="px-4 py-2 outline-none data-[active]:text-blue-600"
    >
      Settings
    </Tabs.Tab>
    {/* Animated indicator slides to the active tab */}
    <Tabs.Indicator className="absolute bottom-0 h-0.5 bg-blue-600 transition-all duration-200" />
  </Tabs.List>
  <Tabs.Panel value="tab1" className="p-4">Overview content</Tabs.Panel>
  <Tabs.Panel value="tab2" className="p-4">Settings content</Tabs.Panel>
</Tabs.Root>
```

---

## Composing Multiple Components

Nest `render` props to combine triggers from different components:

```tsx
import { Dialog } from '@base-ui/react/dialog';
import { Tooltip } from '@base-ui/react/tooltip';

<Dialog.Root>
  <Tooltip.Root>
    <Tooltip.Trigger render={<Dialog.Trigger />}>
      Open Dialog
    </Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Positioner>
        <Tooltip.Popup>Click to open dialog</Tooltip.Popup>
      </Tooltip.Positioner>
    </Tooltip.Portal>
  </Tooltip.Root>
  <Dialog.Portal>
    <Dialog.Backdrop />
    <Dialog.Viewport>
      <Dialog.Popup>
        <Dialog.Title>Dialog with Tooltip</Dialog.Title>
        <Dialog.Close>Close</Dialog.Close>
      </Dialog.Popup>
    </Dialog.Viewport>
  </Dialog.Portal>
</Dialog.Root>
```

---

## Integrating Menu with Toolbar

```tsx
import { Toolbar } from '@base-ui/react/toolbar';
import { Menu } from '@base-ui/react/menu';

<Toolbar.Root className="flex gap-1 rounded border p-1">
  <Toolbar.Button>Cut</Toolbar.Button>
  <Toolbar.Button>Copy</Toolbar.Button>
  <Toolbar.Separator className="mx-1 w-px bg-gray-200" />

  <Menu.Root>
    <Toolbar.Button render={<Menu.Trigger />}>
      More ▾
    </Toolbar.Button>
    <Menu.Portal>
      <Menu.Positioner sideOffset={4}>
        <Menu.Popup className="rounded border bg-white py-1 shadow-lg">
          <Menu.Item className="px-3 py-1.5">Paste</Menu.Item>
          <Menu.Item className="px-3 py-1.5">Select All</Menu.Item>
        </Menu.Popup>
      </Menu.Positioner>
    </Menu.Portal>
  </Menu.Root>
</Toolbar.Root>
```
