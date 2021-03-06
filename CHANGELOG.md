# Ignite UI Dock Manager Change Log

All notable changes for each version of this project will be documented in this file.

## 1.3.0

### New features
- More tabs menu appears when there is not enough space to display all tab headers
- Hide pane without removing it from the layout using its `hidden` property
- Header slot properties for tab and unpinned pane - `tabHeaderId` and `unpinnedHeaderId`

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
