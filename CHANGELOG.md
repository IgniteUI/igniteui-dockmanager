# Ignite UI Dock Manager Change Log

All notable changes for each version of this project will be documented in this file.

## 1.6.0

### New features
- Customize dock manager panes and tabs [#3](https://github.com/IgniteUI/igniteui-dockmanager/issues/3)

### Bug fixes
- A floating pane is draggable outside of the page [#14](https://github.com/IgniteUI/igniteui-dockmanager/issues/14)

## 1.5.0

### New features
- "allowMaximize" property per pane [#10](https://github.com/IgniteUI/igniteui-dockmanager/issues/10)

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
