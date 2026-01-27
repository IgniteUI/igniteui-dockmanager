# API Design: Minimum Size Constraints

## TypeScript Type Definitions

### 1. Dock Manager Component Interface Extensions

```typescript
/**
 * Interface for IgcDockManagerComponent with minimum size properties
 */
interface IgcDockManagerComponent {
  // ... existing properties ...
  
  /**
   * Default minimum width for content panes in pixels.
   * Applied to all content panes unless overridden at the pane level.
   * 
   * @default undefined
   * @example
   * ```typescript
   * dockManager.minPaneWidth = 150;
   * ```
   */
  minPaneWidth?: number;
  
  /**
   * Default minimum height for content panes in pixels.
   * Applied to all content panes unless overridden at the pane level.
   * 
   * @default undefined
   * @example
   * ```typescript
   * dockManager.minPaneHeight = 100;
   * ```
   */
  minPaneHeight?: number;
  
  /**
   * Default minimum width for split panes in pixels.
   * Applied to all split panes unless overridden at the pane level.
   * 
   * @default undefined
   * @example
   * ```typescript
   * dockManager.minSplitPaneWidth = 200;
   * ```
   */
  minSplitPaneWidth?: number;
  
  /**
   * Default minimum height for split panes in pixels.
   * Applied to all split panes unless overridden at the pane level.
   * 
   * @default undefined
   * @example
   * ```typescript
   * dockManager.minSplitPaneHeight = 150;
   * ```
   */
  minSplitPaneHeight?: number;
}
```

### 2. Pane Interface Extensions

```typescript
/**
 * Base interface for pane minimum size properties
 */
interface IgcPaneMinSize {
  /**
   * Minimum width for this pane in pixels.
   * Overrides the global default set on IgcDockManagerComponent.
   * 
   * @default undefined
   * @remarks
   * - For horizontal split containers, this constraint is enforced during resize
   * - For vertical split containers, this is used when pane can be resized horizontally
   * - Applies to both docked and floating states
   * 
   * @example
   * ```typescript
   * const pane: IgcContentPane = {
   *   type: 'contentPane',
   *   contentId: 'editor',
   *   minWidth: 300,
   *   minHeight: 200
   * };
   * ```
   */
  minWidth?: number;
  
  /**
   * Minimum height for this pane in pixels.
   * Overrides the global default set on IgcDockManagerComponent.
   * 
   * @default undefined
   * @remarks
   * - For vertical split containers, this constraint is enforced during resize
   * - For horizontal split containers, this is used when pane can be resized vertically
   * - Applies to both docked and floating states
   */
  minHeight?: number;
}

/**
 * Content pane interface with minimum size support
 */
interface IgcContentPane extends IgcPaneMinSize {
  type: 'contentPane';
  contentId: string;
  header?: string;
  // ... other existing properties ...
}

/**
 * Split pane interface with minimum size support
 */
interface IgcSplitPane extends IgcPaneMinSize {
  type: 'splitPane';
  orientation: 'horizontal' | 'vertical';
  panes: Array<IgcContentPane | IgcSplitPane | IgcTabGroupPane>;
  // ... other existing properties ...
}

/**
 * Tab group pane interface with minimum size support
 */
interface IgcTabGroupPane extends IgcPaneMinSize {
  type: 'tabGroupPane';
  panes: Array<IgcContentPane>;
  // ... other existing properties ...
}

/**
 * Document host interface with minimum size support
 */
interface IgcDocumentHost extends IgcPaneMinSize {
  type: 'documentHost';
  // ... other existing properties ...
}
```

### 3. Helper Types

```typescript
/**
 * Configuration object for minimum size constraints
 */
interface IgcMinSizeConfig {
  /**
   * Minimum width in pixels
   */
  width?: number;
  
  /**
   * Minimum height in pixels
   */
  height?: number;
}

/**
 * Result of minimum size validation
 */
interface IgcMinSizeValidationResult {
  /**
   * Whether the size meets minimum constraints
   */
  isValid: boolean;
  
  /**
   * Adjusted width to meet minimum constraint
   */
  adjustedWidth?: number;
  
  /**
   * Adjusted height to meet minimum constraint
   */
  adjustedHeight?: number;
  
  /**
   * List of panes that violated constraints
   */
  violations?: Array<{
    paneId: string;
    constraint: 'minWidth' | 'minHeight';
    requested: number;
    minimum: number;
  }>;
}
```

## Usage Examples

### Example 1: Basic Setup with Global Defaults

```typescript
import { defineCustomElements } from 'igniteui-dockmanager/loader';

defineCustomElements();

// Get reference to dock manager
const dockManager = document.querySelector('igc-dockmanager') as IgcDockManagerComponent;

// Set global minimum sizes
dockManager.minPaneWidth = 150;        // All content panes at least 150px wide
dockManager.minPaneHeight = 100;       // All content panes at least 100px tall
dockManager.minSplitPaneWidth = 300;   // All split panes at least 300px wide
dockManager.minSplitPaneHeight = 200;  // All split panes at least 200px tall

// Define layout (panes will respect global minimums)
dockManager.layout = {
  rootPane: {
    type: 'splitPane',
    orientation: 'horizontal',
    panes: [
      {
        type: 'contentPane',
        contentId: 'content1',
        header: 'Panel 1'
      },
      {
        type: 'contentPane',
        contentId: 'content2',
        header: 'Panel 2'
      }
    ]
  }
};
```

### Example 2: Per-Pane Minimum Sizes

```typescript
// Define layout with specific minimum sizes for each pane
dockManager.layout = {
  rootPane: {
    type: 'splitPane',
    orientation: 'horizontal',
    panes: [
      {
        type: 'contentPane',
        contentId: 'sidebar',
        header: 'Sidebar',
        minWidth: 200,   // Sidebar needs at least 200px
        minHeight: 300
      },
      {
        type: 'splitPane',
        orientation: 'vertical',
        minWidth: 400,   // Main area needs at least 400px
        panes: [
          {
            type: 'contentPane',
            contentId: 'editor',
            header: 'Editor',
            minWidth: 400,
            minHeight: 250  // Editor needs at least 250px height
          },
          {
            type: 'contentPane',
            contentId: 'console',
            header: 'Console',
            minWidth: 400,
            minHeight: 100  // Console needs at least 100px height
          }
        ]
      }
    ]
  }
};
```

### Example 3: Mixing Global and Pane-Specific Settings

```typescript
// Set conservative global defaults
dockManager.minPaneWidth = 100;
dockManager.minPaneHeight = 80;

dockManager.layout = {
  rootPane: {
    type: 'splitPane',
    orientation: 'horizontal',
    panes: [
      {
        type: 'contentPane',
        contentId: 'narrow',
        header: 'Narrow Panel'
        // Uses global defaults: 100px x 80px
      },
      {
        type: 'contentPane',
        contentId: 'wide',
        header: 'Wide Panel',
        minWidth: 300,  // Override: needs more width
        minHeight: 80   // Explicitly use same as global
      },
      {
        type: 'contentPane',
        contentId: 'nomin',
        header: 'No Minimum',
        minWidth: 0,    // Override: disable minimum width
        minHeight: 0    // Override: disable minimum height
      }
    ]
  }
};
```

### Example 4: React Integration (IgrDockManager)

```tsx
import { IgrDockManager } from 'igniteui-react';
import { IgrContentPane, IgrSplitPane } from 'igniteui-react';

function App() {
  return (
    <IgrDockManager
      id="dockManager"
      minPaneWidth={150}
      minPaneHeight={100}
      minSplitPaneWidth={300}
      minSplitPaneHeight={200}
      layout={{
        rootPane: {
          type: 'splitPane',
          orientation: 'horizontal',
          panes: [
            {
              type: 'contentPane',
              contentId: 'panel1',
              header: 'Panel 1',
              minWidth: 200,  // Override for this pane
            },
            {
              type: 'contentPane',
              contentId: 'panel2',
              header: 'Panel 2',
              // Uses component defaults
            }
          ]
        }
      }}
    />
  );
}
```

### Example 5: Dynamic Minimum Size Updates

```typescript
// Get reference to dock manager
const dockManager = document.querySelector('igc-dockmanager') as IgcDockManagerComponent;

// Function to update minimum sizes based on viewport
function updateMinimumSizes() {
  const width = window.innerWidth;
  
  if (width < 768) {
    // Mobile: smaller minimums
    dockManager.minPaneWidth = 80;
    dockManager.minPaneHeight = 60;
  } else if (width < 1024) {
    // Tablet: medium minimums
    dockManager.minPaneWidth = 150;
    dockManager.minPaneHeight = 100;
  } else {
    // Desktop: larger minimums
    dockManager.minPaneWidth = 200;
    dockManager.minPaneHeight = 150;
  }
}

// Update on load and resize
updateMinimumSizes();
window.addEventListener('resize', updateMinimumSizes);
```

### Example 6: Programmatic Access and Validation

```typescript
// Helper function to get effective minimum size for a pane
function getEffectiveMinSize(pane: IgcContentPane | IgcSplitPane | IgcTabGroupPane | IgcDocumentHost, 
                            dockManager: IgcDockManagerComponent): IgcMinSizeConfig {
  let defaultWidth: number | undefined;
  let defaultHeight: number | undefined;
  
  // Determine the appropriate default based on pane type
  switch (pane.type) {
    case 'contentPane':
      defaultWidth = dockManager.minPaneWidth;
      defaultHeight = dockManager.minPaneHeight;
      break;
    case 'splitPane':
    case 'tabGroupPane':
    case 'documentHost':
      defaultWidth = dockManager.minSplitPaneWidth;
      defaultHeight = dockManager.minSplitPaneHeight;
      break;
  }
  
  return {
    width: pane.minWidth ?? defaultWidth,
    height: pane.minHeight ?? defaultHeight
  };
}

// Validate a layout before applying
function validateLayout(layout: any, dockManager: IgcDockManagerComponent): IgcMinSizeValidationResult {
  // Validation logic implementation
  // Check if total minimum sizes fit in available space
  // Return validation result with any adjustments needed
}
```

### Example 7: CSS Variable Integration (Optional)

```html
<style>
  /* Set global minimum sizes via CSS */
  igc-dockmanager {
    --igc-min-pane-width: 150px;
    --igc-min-pane-height: 100px;
    --igc-min-split-pane-width: 300px;
    --igc-min-split-pane-height: 200px;
  }
  
  /* Different minimums for specific instance */
  #compactDockManager {
    --igc-min-pane-width: 100px;
    --igc-min-pane-height: 80px;
  }
  
  /* Responsive minimums */
  @media (max-width: 768px) {
    igc-dockmanager {
      --igc-min-pane-width: 80px;
      --igc-min-pane-height: 60px;
    }
  }
</style>

<igc-dockmanager id="dockManager"></igc-dockmanager>
<igc-dockmanager id="compactDockManager"></igc-dockmanager>
```

## Event Integration

### Suggested Events (Optional Enhancement)

```typescript
/**
 * Event fired when a resize operation is constrained by minimum size
 */
interface IgcMinSizeConstraintEvent extends CustomEvent {
  detail: {
    /**
     * The pane that triggered the constraint
     */
    pane: IgcContentPane | IgcSplitPane;
    
    /**
     * The dimension that was constrained
     */
    dimension: 'width' | 'height';
    
    /**
     * The requested size
     */
    requestedSize: number;
    
    /**
     * The minimum size that prevented the resize
     */
    minimumSize: number;
    
    /**
     * The orientation of the resize operation
     */
    orientation: 'horizontal' | 'vertical';
  };
}

// Usage
dockManager.addEventListener('minSizeConstraint', (e: IgcMinSizeConstraintEvent) => {
  console.log(`Resize constrained: ${e.detail.pane.contentId} ` +
              `cannot be smaller than ${e.detail.minimumSize}px`);
});
```

## Migration from CSS Workaround

### Before (CSS Workaround)

```css
/* Old approach - doesn't work properly with splitter */
.sc-igc-split-pane-component-h {
  min-width: 200px;
  min-height: 150px;
}
```

### After (New API)

```typescript
// New approach - properly enforced by splitter
const dockManager = document.querySelector('igc-dockmanager');
dockManager.minPaneWidth = 200;
dockManager.minPaneHeight = 150;
```

Or with CSS variables:

```css
igc-dockmanager {
  --igc-min-pane-width: 200px;
  --igc-min-pane-height: 150px;
}
```

## Property Precedence

When determining the minimum size for a pane, the following precedence applies:

1. **Pane-level property** (`pane.minWidth` / `pane.minHeight`) - Highest priority
2. **Component-level default** (`dockManager.minPaneWidth` / etc.) - Medium priority  
3. **CSS variable** (`--igc-min-pane-width` / etc.) - Low priority
4. **No constraint** (`undefined`) - Default

Example:
```typescript
// Component level: 100px
dockManager.minPaneWidth = 100;

// Pane level: 200px (overrides component level)
const pane = {
  type: 'contentPane',
  contentId: 'editor',
  minWidth: 200  // This wins
};

// Result: This pane will have minimum width of 200px
```

## Backward Compatibility

All new properties are optional. Existing code continues to work:

```typescript
// This continues to work exactly as before
const oldLayout = {
  rootPane: {
    type: 'splitPane',
    orientation: 'horizontal',
    panes: [/* ... */]
  }
};
dockManager.layout = oldLayout;
// No minimum sizes are enforced (current behavior maintained)
```

## Framework-Specific Notes

### React (IgrDockManager)
```tsx
<IgrDockManager 
  minPaneWidth={150}
  minPaneHeight={100}
  layout={...}
/>
```

### Angular
```html
<igc-dockmanager 
  [minPaneWidth]="150"
  [minPaneHeight]="100"
  [layout]="layout">
</igc-dockmanager>
```

### Blazor
```razor
<IgbDockManager 
  MinPaneWidth="150"
  MinPaneHeight="100"
  Layout="@layout">
</IgbDockManager>
```

### Web Components (Vanilla JS)
```javascript
const dockManager = document.getElementById('dockManager');
dockManager.minPaneWidth = 150;
dockManager.minPaneHeight = 100;
```

## Related Properties

### Interaction with `size` Property
```typescript
{
  type: 'contentPane',
  contentId: 'panel',
  size: 150,      // Initial size
  minWidth: 200   // Minimum size
}
// Result: Initial size will be adjusted to 200 (the minimum)
```

### Interaction with `floatingWidth` and `floatingHeight`
```typescript
{
  type: 'contentPane',
  contentId: 'panel',
  floatingWidth: 100,   // Desired floating width
  floatingHeight: 80,   // Desired floating height
  minWidth: 150,        // Minimum width
  minHeight: 100        // Minimum height
}
// Result: Floating window will be 150x100 (respecting minimums)
```

### Interaction with `useFixedSize`
When `useFixedSize` is enabled:
- Minimum sizes still apply
- Fixed-size panes should be at least their minimum size
- Resizing behavior constrained by minimum even in fixed-size mode
