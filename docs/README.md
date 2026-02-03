# Documentation Files

This directory contains comprehensive API documentation that should be integrated into the GitHub Wiki.

## API-Reference.md

This file contains the complete API reference for Ignite UI Dock Manager v2.0+, including:

- Component properties (35+ properties with types, defaults, descriptions)
- Methods (6 methods with signatures)
- Events (17 events with event args)
- Type definitions (interfaces, enums, string unions)
- Slots (11 customization slots)
- CSS Custom Properties (20+ styling variables)
- CSS Parts (20+ parts for advanced styling)
- Migration Guide (v1.x → v2.0)

### Integrating into Wiki

To update the [Wiki Dock Manager Specification](https://github.com/IgniteUI/igniteui-dockmanager/wiki/Dock-Manager-Specification):

1. Clone the wiki repository:
   ```bash
   git clone https://github.com/IgniteUI/igniteui-dockmanager.wiki.git
   ```

2. Copy the content from `API-Reference.md` and integrate it into the **API** section of `Dock-Manager-Specification.md`

3. Update the revision history in the Wiki to note the v2.0 API updates

### Source

This documentation was generated from the official NPM package `igniteui-dockmanager@2.0.1` type definitions:
- `dockmanager-component.d.ts` (component API)
- `dockmanager.interfaces.d.ts` (types and interfaces)
- `custom-elements.json` (component metadata)
