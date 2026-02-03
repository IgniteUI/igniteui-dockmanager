# Ignite UI Dock Manager API Reference

This document provides a comprehensive reference of the Ignite UI Dock Manager Web Component API as of version 2.0.

## Table of Contents

- [Component](#component)
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Types and Interfaces](#types-and-interfaces)
- [Slots](#slots)
- [CSS Custom Properties](#css-custom-properties)
- [CSS Parts](#css-parts)

## Component

### `<igc-dockmanager>`

A powerful, flexible dock manager component for laying out, docking, undocking, pinning, and floating panes of content.

**Tag Name:** `igc-dockmanager`

**Module Import:**
```typescript
import { defineComponents, IgcDockManagerComponent } from 'igniteui-dockmanager';

defineComponents(IgcDockManagerComponent);
```

## Properties

### Layout and Configuration

#### `layout`
- **Type:** `IgcDockManagerLayout`
- **Description:** The layout configuration of the Dock Manager. Defines the structure of panes, including root pane, floating panes, and their relationships.

#### `activePane`
- **Type:** `IgcContentPane | null`
- **Default:** `null`
- **Description:** Determines the active content pane. The active pane typically has focus and may be visually highlighted.

#### `maximizedPane`
- **Type:** `IgcContentPane | IgcSplitPane | IgcTabGroupPane`
- **Description:** Determines the pane that is currently maximized. When set, the specified pane fills the entire dock manager area.

### Docking Behavior

#### `allowInnerDock`
- **Type:** `boolean`
- **Default:** `true`
- **Description:** Whether docking inside a pane is allowed. When true, panes can be docked into other panes to create nested layouts.

#### `allowSplitterDock`
- **Type:** `boolean`
- **Default:** `false`
- **Description:** Whether docking over splitter is allowed. Enables docking directly in a split pane by dragging a pane over one of its splitters.

#### `allowRootDock`
- **Type:** `boolean`
- **Default:** `true`
- **Description:** Determines whether docking into the root-level pane is allowed. When set to true (default), panes can be docked directly into the root container. This is done by creating a new root pane and repositioning the existing root pane as a sibling to the newly docked content pane.

#### `proximityDock`
- **Type:** `boolean`
- **Default:** `false`
- **Description:** Determines whether docking indicators are displayed based on proximity while docking.

#### `useFixedSizeOnDock`
- **Type:** `'none' | 'vertical' | 'horizontal' | 'both'`
- **Default:** `'none'`
- **Description:** Specifies which docking orientations should apply the `FixedSize` sizing mode when panes are dynamically created via docking. This setting affects only dynamically created panes via user docking actions. It does not apply to programmatically created panes or layout restorations.

### Pane Actions

#### `allowMaximize`
- **Type:** `boolean`
- **Default:** `true`
- **Description:** Whether maximize action button is displayed for the panes.

#### `closeBehavior`
- **Type:** `PaneActionBehavior`
- **Default:** `'allPanes'`
- **Description:** Which panes get affected by close operations. Determines whether clicking the close button closes only the selected pane or all panes in a tab group.

#### `unpinBehavior`
- **Type:** `PaneActionBehavior`
- **Default:** `'allPanes'`
- **Description:** Determines which panes are affected by particular pane action such as closing or unpinning. Controls whether the selected pane or all panes are unpinned when clicking the unpin button.

### Floating Panes

#### `allowFloatingPanesResize`
- **Type:** `boolean`
- **Default:** `true`
- **Description:** Whether floating panes can be resized.

#### `containedInBoundaries`
- **Type:** `boolean`
- **Default:** `false`
- **Description:** Whether pane dragging stops when any of the pane's sides goes outside the DockManager's bounds. When true, floating panes are constrained within the dock manager boundaries.

### Visual Behavior

#### `showPaneHeaders`
- **Type:** `'always' | 'onHoverOnly'`
- **Default:** `'always'`
- **Description:** Determines when to display the pane headers - always or on hover of the pane.

#### `showHeaderIconOnHover`
- **Type:** `'none' | 'closeOnly' | 'moreOptionsOnly' | 'all'`
- **Default:** `'none'`
- **Description:** Which header icons are shown on hover. Controls the visibility behavior of header action buttons.

#### `enableDragCursor`
- **Type:** `boolean`
- **Default:** `false`
- **Description:** Enables changing the mouse cursor when hovering over a tab or pane header. When set to true, the cursor changes from the default to pointer, indicating that the header can be dragged (e.g., to dock or float the pane).

### Auto-scrolling

#### `autoScrollConfig`
- **Type:** `{ edgeThreshold: number, scrollSpeed: number }`
- **Default:** `{ edgeThreshold: 20, scrollSpeed: 15 }`
- **Description:** Configuration for edge auto-scrolling behavior during drag & resize operations.
  - `edgeThreshold`: Distance in pixels from the container's edge that triggers scrolling.
  - `scrollSpeed`: Number of pixels to scroll per interval (affects scroll rate).

### Keyboard Navigation

#### `disableKeyboardNavigation`
- **Type:** `boolean`
- **Default:** `false`
- **Description:** Disables all keyboard navigation within the dock manager.

### Localization

#### `resourceStrings`
- **Type:** `IgcDockManagerResourceStrings | undefined`
- **Description:** The resource strings of the dock manager. Used for localizing UI text. In version 2.0+, localization should be configured using the `igniteui-i18n-core` package instead of this property.

### Context Menu

#### `contextMenuPosition`
- **Type:** `ContextMenuPosition`
- **Description:** Position to open the context menu.

### Internal/Advanced Properties

#### `dropPosition`
- **Type:** `IgcDockManagerPoint`
- **Description:** The drop position (pane) when docking.

#### `draggedPane`
- **Type:** `IgcContentPane | IgcSplitPane | IgcTabGroupPane | null`
- **Description:** Determines the pane that is currently dragged.

#### `direction`
- **Type:** `string` (readonly)
- **Description:** Gets the direction of the Dock Manager (ltr/rtl).

#### `isValidDrop`
- **Type:** `boolean`
- **Description:** Whether the last drop/drag target was valid.

## Methods

### `dropPane(): Promise<boolean>`
Completes the drop operation for the currently dragged pane.

**Returns:** Promise that resolves to true if the drop was successful, false otherwise.

### `removePane(pane: IgcDockManagerPane): Promise<void>`
Removes a pane from the layout.

**Parameters:**
- `pane`: The pane to remove from the layout.

**Returns:** Promise that resolves when the pane is removed.

### `focusPane(contentId: string): Promise<void>`
Sets focus to a specific content pane by its ID.

**Parameters:**
- `contentId`: The ID of the content pane to focus.

**Returns:** Promise that resolves when the pane receives focus.

### `scrollPaneIntoView(pane: IgcDockManagerPane): void`
Scrolls a pane into view within the dock manager container.

**Parameters:**
- `pane`: The pane to scroll into view.

### `getDockedPanesContainerRect(): DOMRect | undefined`
Gets the bounding rectangle of the docked panes container.

**Returns:** The DOMRect of the container, or undefined if not available.

### `focusElement(): void`
Sets focus to the dock manager element.

## Events

All events are `CustomEvent` instances that can be listened to using standard DOM event listeners or framework-specific event binding.

### `activePaneChanged`
- **Type:** `CustomEvent<IgcActivePaneEventArgs>`
- **Description:** Emitted when the active content pane changes.

**Event Args:**
```typescript
interface IgcActivePaneEventArgs {
  oldPane: IgcContentPane | null;
  newPane: IgcContentPane | null;
}
```

### `layoutChange`
- **Type:** `CustomEvent`
- **Description:** Emitted after the layout has been programmatically updated.

### Pane Events

#### `paneClose`
- **Type:** `CustomEvent<IgcPaneCloseEventArgs>`
- **Description:** Emitted when a pane is closed. Can be cancelled to prevent closing.

#### `panePinnedToggle`
- **Type:** `CustomEvent<IgcPanePinnedEventArgs>`
- **Description:** Emitted when a pane is pinned or unpinned. Can be cancelled to prevent the pin state change.

#### `paneScroll`
- **Type:** `CustomEvent<IgcPaneScrollEventArgs>`
- **Description:** Emitted when the user scrolls within a pane's content.

### Drag Events

#### `paneDragStart`
- **Type:** `CustomEvent<IgcPaneDragStartEventArgs>`
- **Description:** Emitted when a pane drag operation begins. Can be cancelled to prevent dragging.

#### `paneDragOver`
- **Type:** `CustomEvent<IgcPaneDragOverEventArgs>`
- **Description:** Emitted repeatedly as a pane is dragged.

#### `paneDragEnd`
- **Type:** `CustomEvent<IgcPaneDragEndEventArgs>`
- **Description:** Emitted when a pane drag operation ends. Can be cancelled to prevent the drop.

### Floating Pane Resize Events

#### `floatingPaneResizeStart`
- **Type:** `CustomEvent<IgcFloatingPaneResizeEventArgs>`
- **Description:** Emitted when a floating pane resize interaction begins. Can be cancelled to prevent resizing.

#### `floatingPaneResizeMove`
- **Type:** `CustomEvent<IgcFloatingPaneResizeMoveEventArgs>`
- **Description:** Emitted while a floating pane is being resized.

#### `floatingPaneResizeEnd`
- **Type:** `CustomEvent<IgcFloatingPaneResizeEventArgs>`
- **Description:** Emitted when a floating pane resize interaction ends.

### Splitter Events

#### `splitterResizeStart`
- **Type:** `CustomEvent<IgcSplitterResizeEventArgs>`
- **Description:** Emitted when a splitter resize starts. Can be cancelled to prevent resizing.

#### `splitterResizeEnd`
- **Type:** `CustomEvent<IgcSplitterResizeEventArgs>`
- **Description:** Emitted when a splitter resize ends.

### Header Connection Events

#### `paneHeaderConnected`
- **Type:** `CustomEvent<IgcPaneHeaderConnectionEventArgs>`
- **Description:** Emitted when an `<igc-pane-header>` is connected to the DOM.

#### `paneHeaderDisconnected`
- **Type:** `CustomEvent<IgcPaneHeaderConnectionEventArgs>`
- **Description:** Emitted when an `<igc-pane-header>` is disconnected from the DOM.

#### `tabHeaderConnected`
- **Type:** `CustomEvent<IgcTabHeaderConnectionEventArgs>`
- **Description:** Emitted when an `<igc-tab-header>` is connected to the DOM.

#### `tabHeaderDisconnected`
- **Type:** `CustomEvent<IgcTabHeaderConnectionEventArgs>`
- **Description:** Emitted when an `<igc-tab-header>` is disconnected from the DOM.

## Types and Interfaces

### Layout Types

#### `IgcDockManagerLayout`
The main layout configuration object.

```typescript
interface IgcDockManagerLayout {
  rootPane?: IgcDockManagerPane;
  floatingPanes?: IgcSplitPane[];
}
```

#### `IgcDockManagerPane`
Union type representing any type of pane.

```typescript
type IgcDockManagerPane = IgcContentPane | IgcSplitPane | IgcTabGroupPane | IgcDocumentHost;
```

#### `IgcContentPane`
Represents a content pane - the basic unit containing user content.

```typescript
interface IgcContentPane {
  type: 'contentPane';
  contentId: string;
  header?: string;
  tabHeaderId?: string;
  unpinnedHeaderId?: string;
  allowClose?: boolean;
  allowPinning?: boolean;
  allowDocking?: boolean;
  allowFloating?: boolean;
  allowMaximize?: boolean;
  hidden?: boolean;
  disabled?: boolean;
  isPinned?: boolean;
  isMaximized?: boolean;
  documentOnly?: boolean;
  acceptsInnerDock?: boolean;
  size?: number;
  floatingHeight?: number;
  floatingWidth?: number;
  floatingLocation?: IgcDockManagerPoint;
}
```

#### `IgcSplitPane`
Represents a split pane container that can hold multiple child panes.

```typescript
interface IgcSplitPane {
  type: 'splitPane';
  orientation: SplitPaneOrientation; // 'horizontal' | 'vertical'
  panes: IgcDockManagerPane[];
  size?: number;
  floatingHeight?: number;
  floatingWidth?: number;
  floatingLocation?: IgcDockManagerPoint;
  allowEmpty?: boolean;
  isMaximized?: boolean;
  useFixedSize?: boolean; // New in 1.16.0
}
```

#### `IgcTabGroupPane`
Represents a tab group that displays multiple panes as tabs.

```typescript
interface IgcTabGroupPane {
  type: 'tabGroupPane';
  panes: IgcContentPane[];
  size?: number;
  selectedIndex?: number;
  allowEmpty?: boolean;
}
```

#### `IgcDocumentHost`
Represents a document host area where documents are displayed as tabs.

```typescript
interface IgcDocumentHost {
  type: 'documentHost';
  rootPane?: IgcDockManagerPane;
  size?: number;
}
```

### Enums and String Union Types

Starting with version 2.0, TypeScript enums have been refactored to string union types. Const objects are provided for backward compatibility.

#### `DockManagerPaneType` / `IgcDockManagerPaneType`
```typescript
type DockManagerPaneType = 'splitPane' | 'contentPane' | 'tabGroupPane' | 'documentHost';

const IgcDockManagerPaneType = {
  splitPane: 'splitPane',
  contentPane: 'contentPane',
  tabGroupPane: 'tabGroupPane',
  documentHost: 'documentHost'
} as const;
```

#### `SplitPaneOrientation` / `IgcSplitPaneOrientation`
```typescript
type SplitPaneOrientation = 'horizontal' | 'vertical';

const IgcSplitPaneOrientation = {
  horizontal: 'horizontal',
  vertical: 'vertical'
} as const;
```

#### `PaneActionBehavior` / `IgcPaneActionBehavior`
```typescript
type PaneActionBehavior = 'selectedPane' | 'allPanes';

const IgcPaneActionBehavior = {
  selectedPane: 'selectedPane',
  allPanes: 'allPanes'
} as const;
```

#### `UnpinnedLocation` / `IgcUnpinnedLocation`
```typescript
type UnpinnedLocation = 'left' | 'right' | 'top' | 'bottom';

const IgcUnpinnedLocation = {
  left: 'left',
  right: 'right',
  top: 'top',
  bottom: 'bottom'
} as const;
```

#### `ContextMenuPosition`
```typescript
type ContextMenuPosition = 'start' | 'center' | 'end' | 'stretch';
```

### Helper Types

#### `IgcDockManagerPoint`
```typescript
interface IgcDockManagerPoint {
  x: number;
  y: number;
}
```

#### `IgcDockManagerResourceStrings`
Interface for localization strings. See the localization section in README.md for usage with version 2.0+.

## Slots

The Dock Manager provides several slots for customizing various UI elements:

### Button Slots

- **`paneHeaderCloseButton`** - Custom close button for pane headers
- **`tabHeaderCloseButton`** - Custom close button for tab headers
- **`closeButton`** - Alias for pane/tab close button
- **`moreTabsButton`** - Slot for the "more tabs" button
- **`maximizeButton`** - Slot for maximize buttons
- **`minimizeButton`** - Slot for minimize buttons
- **`pinButton`** - Slot for pin buttons
- **`unpinButton`** - Slot for unpin buttons
- **`moreOptionsButton`** - Slot for more-options buttons on tab headers

### Other Slots

- **`splitterHandle`** - Slot for custom splitter handle

## CSS Custom Properties

The Dock Manager exposes numerous CSS custom properties for styling:

### Colors

- `--igc-background-color` - Background color
- `--igc-active-color` - Active element color
- `--igc-border-color` - Border color
- `--igc-splitter-background` - Splitter background color
- `--igc-splitter-background-hover` - Splitter background color on hover

### Sizing (v1.18.0+)

- `--igc-splitter-thickness` - Thickness of splitters (replaces `--igc-splitter-width`)
- `--igc-resize-handle-size` - Size of resize handles (replaces `--igc-resize-handle-height`)
- `--igc-resize-handle-thickness` - Thickness of resize handles (replaces `--igc-resize-handle-width`)

### Resize Target Borders (v1.18.0+)

- `--igc-resize-target-border-color` - Color of the border highlighting the pane being resized
- `--igc-resize-target-border-width` - Width of the resize target border
- `--igc-resize-target-border-style` - Style of the resize target border (solid, dashed, etc.)

### Scrollbars (v1.15.0+)

CSS variables are available for styling scrollbars (exact variable names may vary by implementation).

## CSS Parts

The Dock Manager exposes several CSS parts for advanced styling:

### Pane Parts

- `pane` - Content panes
- `pane-header` - Pane headers
- `header-title` - Header title area

### Tab Parts

- `tab-header` - Tab headers
- `tab-header-close-button` - Close button on tabs
  - Options: `active`, `inactive`, `selected`, `hovered`, `hover`
- `tabs-more-menu-content` - More tabs menu content container
- `tabs-more-menu-item` - Individual items in the more tabs menu

### Splitter Parts

- `splitter` - Splitter elements
- `splitter-handle` - Splitter handle (drag area)
  - Options: `horizontal`, `vertical`

### Content ID Parts (v1.14.0+)

Each pane with a `contentId` also exposes that ID as a CSS part, allowing for per-pane styling.

---

## Migration Guide: v1.x to v2.0

### Breaking Changes

#### 1. Import Changes

**Before (v1.x):**
```typescript
import { defineCustomElements } from 'igniteui-dockmanager/loader';

defineCustomElements();
```

**After (v2.0+):**
```typescript
import { defineComponents, IgcDockManagerComponent } from 'igniteui-dockmanager';

defineComponents(IgcDockManagerComponent);
```

#### 2. Localization

**Before (v1.x):**
```typescript
import { IgcDockManagerResourceStringsES } from 'igniteui-dockmanager';

dockManager.resourceStrings = IgcDockManagerResourceStringsES;
```

**After (v2.0+):**
```typescript
import { registerI18n } from 'igniteui-i18n-core';
import { DockManagerResourceStringsES } from 'igniteui-i18n-resources';

registerI18n(DockManagerResourceStringsES, 'es');
```

### Recommended Updates

#### Enum to String Union Types

While backward compatibility is maintained, it's recommended to update to string literals:

**Before:**
```typescript
const layout: IgcDockManagerLayout = {
  rootPane: {
    type: IgcDockManagerPaneType.splitPane,
    orientation: IgcSplitPaneOrientation.horizontal,
    panes: []
  }
};
```

**After (recommended):**
```typescript
const layout: IgcDockManagerLayout = {
  rootPane: {
    type: 'splitPane',
    orientation: 'horizontal',
    panes: []
  }
};
```

---

## Version History

- **2.0.0** (Current) - Lit framework migration, localization refactoring
- **1.18.0** - Splitter docking, edge docking, fixed-size enhancements
- **1.17.0** - Fixed-size on dock, auto-scroll improvements
- **1.16.0** - Splitter docking, fixed-size split panes
- **1.15.0** - Close/unpin behaviors
- **1.14.0** - Proximity dock, contained boundaries, show pane headers
- **1.13.0** - Inner dock controls, focus pane method
- **1.12.5** - Pane scroll event

See [CHANGELOG.md](./CHANGELOG.md) for complete version history.
