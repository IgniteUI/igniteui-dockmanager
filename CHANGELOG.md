# Ignite UI Dock Manager Change Log

All notable changes for each version of this project will be documented in this file.

## 2.1.0

### New features
- Added `paneFlyoutToggle` event that fires when an unpinned pane's flyout opens or closes (distinct from pin/unpin operations).
- Added `minResizeWidth` and `minResizeHeight` properties to `IgcContentPane`, `IgcSplitPane`, and `IgcTabGroupPane` to constrain resizing behavior during resize operations. These properties set minimum size constraints (in pixels, default: 42) that are enforced when a user interacts with splitters or resizes floating panes. **Note:** These constraints only apply during active resize operations and do not prevent docking into smaller containers or affect container sizing during docking operations.
- The `layoutChange` event now includes the updated layout in its `detail.layout` property.

### Bug Fixes
- The hover area for showing pane headers when `showPaneHeaders` is set to `onHoverOnly` now correctly accounts for the actual pane header height, including custom heights set via `::part(pane-header)` styles. Note that when customizing the header height, the `min-height` property should also be overridden, as it defaults to 40px to ensure header elements are properly displayed.

## 2.0.0

### Breaking Changes
- Localization resources are no longer bundled directly with this package. The previously exported resource string objects:
  - `IgcDockManagerResourceStringsEN`
  - `IgcDockManagerResourceStringsES`
  - `IgcDockManagerResourceStringsJP`
  - `IgcDockManagerResourceStringsKO`

  have been removed from this package and moved into dedicated localization packages (see the README "Localization" section for details). The corresponding resource string objects in the new localization packages no longer use the `Igc` prefix.
- Applications that relied on these exports must now:
  - Install the new localization package(s), and
  - Update imports to use the resource strings from the new packages.

### Major Changes
- Migrated the dock manager component from Stencil to Lit framework. This is an internal architectural change that maintains full API compatibility with previous versions.

### Enhancements
- Refactored TypeScript enums to string union types for better type safety and simpler codebase structure. Const objects are provided for backward compatibility.
  For example, the following:
  ```ts
  const layout1: IgcDockManagerLayout = {
    rootPane: {
      type: IgcDockManagerPaneType.splitPane,
      orientation: IgcSplitPaneOrientation.horizontal,
      panes: [
        // ...
      ]
    }
  };
  ```
  Can now also be written as:
  ```ts
  const layout1: IgcDockManagerLayout = {
    rootPane: {
      type: 'splitPane',
      orientation: 'horizontal',
      panes: [
        // ...
      ]
    }
  };
  ```

## 1.18.0

### New features
- Introduced a new docking behavior that allows docking directly in a split pane by dragging a pane over one of its splitters. It can be switched on via the new `allowSplitterDock` property of the `IgcDockManagerComponent`.
- Added edge docking functionality for docking panes to the edges of the dock manager. This docking mode is enabled when root docking is disabled via `allowRootDock` = false.
- Added automatic scrolling when resizing panes with `useFixedSize` enabled, allowing resizing beyond viewport bounds.
- Added visual feedback with a thin border highlighting the pane being resized. This can be customized via new CSS variables: `--igc-resize-target-border-color`, `--igc-resize-target-border-width`, and `--igc-resize-target-border-style`.
- Added `enableDragCursor` property to provide cursor feedback on hover for interactive panes and tabs - when enabled, the cursor changes to pointer when panes/tabs can be dragged.
- Exposed new CSS variables for splitter and resize handle customization:
  - `--igc-splitter-thickness` (replaces `--igc-splitter-width`)
  - `--igc-resize-handle-size` (replaces `--igc-resize-handle-height`)
  - `--igc-resize-handle-thickness` (replaces `--igc-resize-handle-width`)
- Improved joystick indicators to display on the visible portion of target panes.

### Enhancements
- Resize handle CSS variables renamed to be orientation-agnostic with backward compatibility maintained for old variable names.
- Auto-scroll behavior now triggers when dragging panes near the edges of the root container.
- Improved drop shadow positioning when docking in split panes with `useFixedSize` enabled.

### Bug fixes
- Removed obsolete start placeholder and splitter implementation.
- Fixed auto-scroll triggering incorrectly when dragging mouse upward at the top of the container.
- Fixed flyout behavior when clicking on splitters.
- Fixed splitter resizing to work without requiring mouse movement.
- Fixed splitter dragging preventing default browser behavior to avoid unwanted text selection and auto-scroll.
- Fixed dock indicators and preview incorrectly showing when context menu is displayed.
- Removed resize border from unpinned panes.
- Fixed bottom dock preview positioning.
- Fixed pane size calculation when `floatingWidth` or `floatingHeight` are undefined.
- Fixed root dock preview display for edge indicators.
- Fixed null reference errors during splitter docking.

## 1.17.0

### New features
- Adding two new Dock Manager properties:
   - `useFixedSizeOnDock`, which specifies which docking orientations should apply the `FixedSize` sizing mode when split panes are dynamically created via docking.
   - `allowRootDock`, which defaults to true, and enables or disables docking into the root-level container.
- Adding automatic scrolling of the root container during drag-and-drop operations.
  -  Introduced a new `autoScrollConfig` property for finer control over scroll behavior. 

### Bug fixes
- Placeholder panes are no longer present in Pane Navigator
- Dark theme is now properly applied to scrollbars

## 1.16.1

### Bug fixes
- Dock manager is not loading if layout has no floating panes collection

## 1.16.0

### New features
- Introduced a new docking behavior that allows docking directly in a split pane by dragging a pane over one of its splitters. It can be switched on via the new `allowSplitterDock` property of the `IgcDockManagerComponent`.
- Introduced the `useFixedSize` property of Split Panes - a new resizing behavior where child panes are sized in pixels and can be resized beyond the viewport, allowing scrollable overflow within the container. 

### Bug fixes
- The `showHeaderIconOnHover` property now defaults to `'none'` instead of `undefined` and has correct type

## 1.15.1

### Enhancements
- Added custom elements manifest file

## 1.15.0

### New features

- Added `closeBehavior` and `unpinBehavior` properties controlling whether the selected pane or all panes are closed/unpinned when clicking the respective button of a pane within a dockable TabGroup [#94](https://github.com/IgniteUI/igniteui-dockmanager/issues/94)

### Enhancements
- Added new CSS variables for styling the scrollbars

### Bug fixes
- TabGroupPane disappears when an unpinned pane is pinned [#84](https://github.com/IgniteUI/igniteui-dockmanager/issues/84)
- Unpinned panes on the right and bottom edges cannot be resized via the splitter after being sized to minimum width/height [#90](https://github.com/IgniteUI/igniteui-dockmanager/issues/90)
- Splitter cursor should be correct while resizing [#93](https://github.com/IgniteUI/igniteui-dockmanager/issues/93)
- Uncaught TypeError: Cannot read properties of undefined (reading 'focus') [#96](https://github.com/IgniteUI/igniteui-dockmanager/issues/96)
- Layout has more nesting levels then should [#97](https://github.com/IgniteUI/igniteui-dockmanager/issues/97)
- Reopening a pane in IgcDockManager after closing a tab group pane does not show content of pane [#103](https://github.com/IgniteUI/igniteui-dockmanager/issues/103)
- DockManager's maximized pane is not cleared when closing a maximized tabgroup pane

## 1.14.4

### Bug fixes
- Applying isMaximized on split pane breaks maximizing [#95](https://github.com/IgniteUI/igniteui-dockmanager/issues/95)

## 1.14.3

### Bug fixes
- Dock manager throws errors with Vite-based builds [#88](https://github.com/IgniteUI/igniteui-dockmanager/issues/88)
- Docking to edge in an empty root split pane throws errors [#89](https://github.com/IgniteUI/igniteui-dockmanager/issues/89)

## 1.14.2

### Bug fixes
- Constraining panels inside the dock manager boundaries with quick drags [#83](https://github.com/IgniteUI/igniteui-dockmanager/issues/83)

## 1.14.1

### Bug fixes
- Proximity dock rtl check

## 1.14.0

### New features

- Add `showPaneHeaders` property [#65](https://github.com/IgniteUI/igniteui-dockmanager/issues/65)
- Add `proximityDock` property [#66](https://github.com/IgniteUI/igniteui-dockmanager/issues/66)
- Add `containedInBoundaries` property [#68](https://github.com/IgniteUI/igniteui-dockmanager/issues/68)

### Enhancements
- Add `contentId` of elements as CSS parts [#32](https://github.com/IgniteUI/igniteui-dockmanager/issues/32) 

### Bug fixes
- Maximizing and unpinning panes leads to unclickable panes [#72](https://github.com/IgniteUI/igniteui-dockmanager/issues/72)
- Center dock is possible in a pane that has `acceptsInnerDock` to true if the `allowInnerDock` of `IgcDockManagerComponent` is set to false

## 1.13.0

### New features
- Add `focusPane` method [#58](https://github.com/IgniteUI/igniteui-dockmanager/issues/58), [#59](https://github.com/IgniteUI/igniteui-dockmanager/issues/59)
- Add `allowInnerDock` and `acceptsInnerDock` properties [#67](https://github.com/IgniteUI/igniteui-dockmanager/issues/67)

### Enhancements
- Save pane maximized state in layout [#60](https://github.com/IgniteUI/igniteui-dockmanager/issues/60)

### Bug fixes
- Tab selection order is not preserved [#31](https://github.com/IgniteUI/igniteui-dockmanager/issues/31)

## 1.12.5

### New features
- Add `paneScroll` event [#51](https://github.com/IgniteUI/igniteui-dockmanager/issues/51) 

### Bug fixes
- ТabGroupPane: Pinning one of several unpinned panes results in all the panes getting pinned [#62](https://github.com/IgniteUI/igniteui-dockmanager/issues/62)
- Context menu not positioning correctly in RTL mode
- Active pane is not retained when docking with keyboard

## 1.12.4

### Bug fixes
- Active pane incorrectly set when more than one Tab Group Pane is within a Floating Pane [#61](https://github.com/IgniteUI/igniteui-dockmanager/issues/61)

## 1.12.3

### Bug fixes
- Error is thrown when dropping pane in a separate window

## 1.12.2

### Bug fixes
- Docking indicator left/right arrows positions are reversed in RTL mode [#54](https://github.com/IgniteUI/igniteui-dockmanager/issues/54)
- Context menu not positioning correctly
- Missing overloads for `addEventListener` and `removeEventListener`

### Enhancements
- Add `tabs-more-menu-content` and `tabs-more-menu-item` CSS parts

## 1.12.1

### Enhancements
- Include pane information in `splitterResizeStart` and `splitterResizeEnd` events
- `IgcDockManagerComponent` is now exported as class

### Bug fixes
- Contents in slots with `unpinnedHeaderId` are not updated correctly [#53](https://github.com/IgniteUI/igniteui-dockmanager/issues/53)

## 1.12.0

### Bug fixes
- Docking not working with allowFloating: false [#40](https://github.com/IgniteUI/igniteui-dockmanager/issues/40)
- Flyout pane closing while active [#41](https://github.com/IgniteUI/igniteui-dockmanager/issues/41)
- Focusable elements does not receive focus [#42](https://github.com/IgniteUI/igniteui-dockmanager/issues/42)
- Navigating with pane navigator does not bring selected floating window on top [#48](https://github.com/IgniteUI/igniteui-dockmanager/issues/48)
- Event `splitterResizeStart` can't be cancelled
- Tabs context menu not positioning correctly

## 1.11.3

### New features
- Add `contextMenuPosition` property
- Add `selected` option for `tab-header-close-button` CSS part

## 1.11.2

### New features
- Add `hovered` option for `tab-header-close-button` CSS part

## 1.11.1

### Bug fixes
- CSS part fixes for `tab-header`

## 1.11.0

### New features
- Add options for `showHeaderIconOnHover` property for different buttons
- Add `horizontal` and `vertical` options for `splitter-handle` CSS part
- Add `header-title` CSS part
- Add `hover` option for `tab-header-close-button` CSS part in active/inactive states
- Add `paneHeaderCloseButton` and `tabHeaderCloseButton` slots

## 1.10.0

### New features
-  Add `showHeaderIconOnHover` property

### Bug fixes
- Active pane is not retained on float/dock
- Splitter styles are not applied [#36](https://github.com/IgniteUI/igniteui-dockmanager/issues/36)
- `click` event on customized header buttons is not working [#37](https://github.com/IgniteUI/igniteui-dockmanager/issues/37)
- Removed erroneous dock indicators while dragging over splitter

## 1.9.0

### Bug fixes
- Styles not applied [#33](https://github.com/IgniteUI/igniteui-dockmanager/issues/33)
- Resize in RTL mode [#24](https://github.com/IgniteUI/igniteui-dockmanager/issues/24)

## 1.8.0

### New features
- Customize dock manager buttons
- `layoutChange` event which fires when the layout updates [#17](https://github.com/IgniteUI/igniteui-dockmanager/issues/17)

## 1.7.0

### New features
- Customizable floating pane header
- `disabled` property per pane [#23](https://github.com/IgniteUI/igniteui-dockmanager/issues/23)
- `documentOnly` property which allows content pane to be docked only inside a document host
- `allowEmpty` property for split and tab group panes which allows displaying empty areas [#18](https://github.com/IgniteUI/igniteui-dockmanager/issues/18)
- `disableKeyboardNavigation` property on the dock manager [#22](https://github.com/IgniteUI/igniteui-dockmanager/issues/22)

### Bug fixes
- Docking indicators appear over the currently dragged floating pane

## 1.6.0

### New features
- Customize dock manager panes and tabs [#3](https://github.com/IgniteUI/igniteui-dockmanager/issues/3)

### Bug fixes
- A floating pane is draggable outside of the page [#14](https://github.com/IgniteUI/igniteui-dockmanager/issues/14)

## 1.5.0

### New features
- `allowMaximize` property per pane [#10](https://github.com/IgniteUI/igniteui-dockmanager/issues/10)

### Bug fixes
- Unpinned pane is closing automatically upon clicking on its content [#11](https://github.com/IgniteUI/igniteui-dockmanager/issues/11)
- Panes selected from the overflow menu are not activated if there is an unpinned pane from the same tab group

## 1.4.1

### Bug fixes
- Pane with allowPinning=false placed inside tab group can be unpinned [#9](https://github.com/IgniteUI/igniteui-dockmanager/issues/9)
- Normalize a maximized pane when navigating away from it via the keyboard

## 1.4.0

### New features
- Reorder tabs without creating floating pane
- Keyboard navigation
- Pane navigator
- Enable/disable floating pane resizing [#4](https://github.com/IgniteUI/igniteui-dockmanager/issues/4)
- Events for floating pane resizing

### Bug fixes
- Select pane when activated
- Flyout unpinned pane when activated
- Error thrown when hosting external popup inside pane [#5](https://github.com/IgniteUI/igniteui-dockmanager/issues/5)
- Tab selection is lost with nested Dock Manager components [#6](https://github.com/IgniteUI/igniteui-dockmanager/issues/6)
- Floating pane containing panes with disabled floating and docking cannot be moved
- Exception thrown when docking floating pane inside empty dock manager [#7](https://github.com/IgniteUI/igniteui-dockmanager/issues/7)

## 1.3.0

### New features
- More tabs menu appears when there is not enough space to display all tab headers
- Hide pane without removing it from the layout using its `hidden` property [#1](https://github.com/IgniteUI/igniteui-dockmanager/issues/1)
- Header slot properties for tab and unpinned pane - `tabHeaderId` and `unpinnedHeaderId` [#2](https://github.com/IgniteUI/igniteui-dockmanager/issues/2)

## 1.2.0

### New features
- Active pane
- Localization support

### Bug fixes
- Errors thrown when dragging the last document host tab and there is unpinned pane
- Tabs content disappears after docking a pane with allowFloating=false
- Exception thrown when quickly switching between docking indicators

## 1.1.0

### New features
- Maximizing panes
- Docking preview shadow
- ARIA support
- API for external drag/drop support
- Properties and events for user interactions such as closing, pinning, dragging
- Support for `ng update` for Angular projects

## 1.0.3

### Enhancements
- Resize splitter using the keyboard

## 1.0.2

### Bug fixes
- Pane goes out of view when resized to its minimum size

## 1.0.1

### Enhancements
- Add active color css variable
- Add keyboard support for context menu

### Bug fixes
- Selection is not working on first click when context menu is opened
- Single tab is not rendered correctly after pinning/unpinning its sibling

## 1.0.0
Initial release of Ignite UI Dock Manager
