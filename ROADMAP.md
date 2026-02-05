# Roadmap - Dock Manager Web Component

## Current Status (v2.0+)

The Dock Manager has undergone a major architectural migration from Stencil to Lit framework in version 2.0. This internal change maintains full API compatibility while improving maintainability and performance.

## Delivered Features

### Version 2.0.0
- ✅ Migration from Stencil to Lit framework
- ✅ Localization moved to separate packages (`igniteui-i18n-resources`)
- ✅ TypeScript enums refactored to string union types (with backward compatibility)
- ✅ Enhanced type safety and simpler codebase

### Version 1.18.0
- ✅ Splitter docking - dock directly in split panes by dragging over splitters
- ✅ Edge docking functionality (when root docking is disabled)
- ✅ Auto-scrolling when resizing panes with fixed size
- ✅ Visual feedback for pane resizing with customizable borders
- ✅ `enableDragCursor` property for drag feedback
- ✅ New CSS variables for splitter and resize handle customization

### Version 1.17.0
- ✅ `useFixedSizeOnDock` property for controlling fixed-size behavior on docking
- ✅ `allowRootDock` property to enable/disable root-level docking
- ✅ `autoScrollConfig` property for edge auto-scrolling during drag operations

### Version 1.16.0
- ✅ `allowSplitterDock` property
- ✅ `useFixedSize` property for split panes (pixel-based sizing with scrollable overflow)

### Version 1.15.0
- ✅ `closeBehavior` and `unpinBehavior` properties
- ✅ Custom elements manifest file
- ✅ CSS variables for scrollbar styling

### Version 1.14.0
- ✅ `showPaneHeaders` property (always or on hover only)
- ✅ `proximityDock` property
- ✅ `containedInBoundaries` property
- ✅ Content ID exposed as CSS parts for per-pane styling

### Version 1.13.0
- ✅ `focusPane` method
- ✅ `allowInnerDock` and `acceptsInnerDock` properties
- ✅ Save pane maximized state in layout

### Version 1.12.5
- ✅ `paneScroll` event

### Version 1.12.0 - 1.11.0
- ✅ Context menu positioning
- ✅ `showHeaderIconOnHover` property with multiple options
- ✅ CSS parts for tab headers and close buttons
- ✅ Slots for custom buttons (`paneHeaderCloseButton`, `tabHeaderCloseButton`)
- ✅ `header-title` CSS part

### Earlier Versions
- ✅ Customizable floating pane headers
- ✅ Keyboard navigation and pane navigator
- ✅ Maximize/minimize panes
- ✅ More tabs menu
- ✅ Tab reordering without floating
- ✅ ARIA support
- ✅ Localization support
- ✅ Active pane management
- ✅ Document host for document-style layouts
- ✅ All available slots exposed by the dock manager (#3, #4)
- ✅ `layoutChange` event (#17)
- ✅ Tabs and unpinned areas exposed for styling

## Ongoing Support

### Active Development (v2.0+, Lit-based)
- Active feature development
- Bug fixes and performance improvements
- Security updates

### Legacy Support (v1.18.x, Stencil-based)
- Security fixes only
- Critical bug fixes only
- No new features

## Future Considerations

### Under Evaluation
1. Additional customization options for tab appearance
2. Enhanced maximize/minimize event handling
3. Further performance optimizations for large layouts
4. Additional CSS parts and styling capabilities

### Community Requests
We actively monitor [GitHub Issues](https://github.com/IgniteUI/igniteui-dockmanager/issues) for feature requests and bug reports. Please:
- Request a [new feature](https://github.com/IgniteUI/igniteui-dockmanager/issues/new?assignees=igdmdimitrov&labels=feature-request%2C+status%3A+in-review&template=feature_request.md&title=)
- Upvote [existing feature requests](https://github.com/IgniteUI/igniteui-dockmanager/issues?q=is%3Aissue+is%3Aopen+label%3Afeature-request)
- [Report bugs](https://github.com/IgniteUI/igniteui-dockmanager/issues/new?assignees=igdmdimitrov&labels=bug%2C+status%3A+in-review&template=bug_report.md&title=)

## Documentation

For complete API documentation, see:
- [API.md](./API.md) - Comprehensive API reference
- [CHANGELOG.md](./CHANGELOG.md) - Version history and changes
- [README.md](./README.md) - Getting started guide

For usage examples and detailed documentation, visit the [Infragistics Dock Manager documentation](https://infragistics.com/products/ignite-ui-web-components/web-components/components/dock-manager.html).
