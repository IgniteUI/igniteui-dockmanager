# Implementation Guide: Minimum Size Constraints for Panes

This document provides technical implementation guidance for adding minimum size constraints to the IgcDockManager component.

## Architecture Overview

### Components Involved

1. **IgcDockManagerComponent** - Main component that hosts panes and manages layout
2. **SplitPaneComponent** - Container that holds child panes with a splitter
3. **ContentPaneComponent** - Individual content pane 
4. **SplitterComponent** - Draggable element that resizes adjacent panes
5. **LayoutManager** - Calculates and applies pane sizes
6. **ResizeManager** - Handles resize operations

### Key Files (Estimated Locations)

```
src/
├── components/
│   ├── igc-dockmanager/
│   │   ├── igc-dockmanager.tsx          # Main component
│   │   └── igc-dockmanager.types.ts     # Type definitions
│   ├── split-pane/
│   │   ├── split-pane.tsx               # Split pane component
│   │   └── split-pane.types.ts
│   ├── content-pane/
│   │   ├── content-pane.tsx
│   │   └── content-pane.types.ts
│   └── splitter/
│       ├── splitter.tsx                  # Splitter component
│       └── splitter-drag-handler.ts      # Key file for resize logic
├── managers/
│   ├── layout-manager.ts                 # Layout calculations
│   └── resize-manager.ts                 # Resize operations
└── utils/
    └── size-calculator.ts                # Size calculation utilities
```

## Implementation Steps

### Phase 1: Type Definitions

**File: `igc-dockmanager.types.ts`**

```typescript
export interface IgcDockManagerComponent {
  // Add new properties
  minPaneWidth?: number;
  minPaneHeight?: number;
  minSplitPaneWidth?: number;
  minSplitPaneHeight?: number;
}

export interface IgcPaneMinSize {
  minWidth?: number;
  minHeight?: number;
}

export interface IgcContentPane extends IgcPaneMinSize {
  // Existing properties...
}

export interface IgcSplitPane extends IgcPaneMinSize {
  // Existing properties...
}

export interface IgcTabGroupPane extends IgcPaneMinSize {
  // Existing properties...
}
```

### Phase 2: Component Property Additions

**File: `igc-dockmanager.tsx`**

```typescript
@Component({
  tag: 'igc-dockmanager',
  // ...
})
export class IgcDockManager {
  /**
   * Default minimum width for content panes
   */
  @Prop() minPaneWidth?: number;
  
  /**
   * Default minimum height for content panes
   */
  @Prop() minPaneHeight?: number;
  
  /**
   * Default minimum width for split panes
   */
  @Prop() minSplitPaneWidth?: number;
  
  /**
   * Default minimum height for split panes
   */
  @Prop() minSplitPaneHeight?: number;
  
  /**
   * Watch for property changes and trigger layout recalculation
   */
  @Watch('minPaneWidth')
  @Watch('minPaneHeight')
  @Watch('minSplitPaneWidth')
  @Watch('minSplitPaneHeight')
  onMinSizeChanged() {
    this.recalculateLayout();
  }
}
```

### Phase 3: Minimum Size Calculation Utility

**File: `utils/size-calculator.ts`**

```typescript
// Constants for splitter dimensions
const SPLITTER_WIDTH = 4;  // Horizontal splitter width in pixels
const SPLITTER_HEIGHT = 4; // Vertical splitter height in pixels

/**
 * Gets the effective minimum width for a pane
 */
export function getEffectiveMinWidth(
  pane: IgcContentPane | IgcSplitPane,
  dockManager: IgcDockManagerComponent
): number | undefined {
  // Pane-level override takes precedence
  if (pane.minWidth !== undefined) {
    return pane.minWidth;
  }
  
  // Use component-level default
  if (pane.type === 'contentPane') {
    return dockManager.minPaneWidth;
  } else if (pane.type === 'splitPane') {
    return dockManager.minSplitPaneWidth;
  }
  
  return undefined;
}

/**
 * Gets the effective minimum height for a pane
 */
export function getEffectiveMinHeight(
  pane: IgcContentPane | IgcSplitPane,
  dockManager: IgcDockManagerComponent
): number | undefined {
  // Pane-level override takes precedence
  if (pane.minHeight !== undefined) {
    return pane.minHeight;
  }
  
  // Use component-level default
  if (pane.type === 'contentPane') {
    return dockManager.minPaneHeight;
  } else if (pane.type === 'splitPane') {
    return dockManager.minSplitPaneHeight;
  }
  
  return undefined;
}

/**
 * Calculates the minimum size for a split pane based on its children
 */
export function calculateSplitPaneMinimumSize(
  splitPane: IgcSplitPane,
  dockManager: IgcDockManagerComponent
): { minWidth: number; minHeight: number } {
  const orientation = splitPane.orientation;
  let minWidth = 0;
  let minHeight = 0;
  
  if (orientation === 'horizontal') {
    // For horizontal split: sum widths, max height
    splitPane.panes.forEach((child, index) => {
      const childMinWidth = getEffectiveMinWidth(child, dockManager) || 0;
      const childMinHeight = getEffectiveMinHeight(child, dockManager) || 0;
      
      minWidth += childMinWidth;
      minHeight = Math.max(minHeight, childMinHeight);
      
      // Add splitter width (except for last pane)
      if (index < splitPane.panes.length - 1) {
        minWidth += SPLITTER_WIDTH;
      }
    });
  } else {
    // For vertical split: max width, sum heights
    splitPane.panes.forEach((child, index) => {
      const childMinWidth = getEffectiveMinWidth(child, dockManager) || 0;
      const childMinHeight = getEffectiveMinHeight(child, dockManager) || 0;
      
      minWidth = Math.max(minWidth, childMinWidth);
      minHeight += childMinHeight;
      
      // Add splitter height (except for last pane)
      if (index < splitPane.panes.length - 1) {
        minHeight += SPLITTER_HEIGHT;
      }
    });
  }
  
  // Compare with explicitly set minimum on split pane itself
  const explicitMinWidth = getEffectiveMinWidth(splitPane, dockManager) || 0;
  const explicitMinHeight = getEffectiveMinHeight(splitPane, dockManager) || 0;
  
  return {
    minWidth: Math.max(minWidth, explicitMinWidth),
    minHeight: Math.max(minHeight, explicitMinHeight)
  };
}

/**
 * Validates if a size meets minimum constraints
 */
export function validateSize(
  requestedSize: number,
  minSize: number | undefined
): number {
  if (minSize === undefined || minSize === 0) {
    return requestedSize;
  }
  return Math.max(requestedSize, minSize);
}
```

### Phase 4: Splitter Resize Logic

**File: `splitter-drag-handler.ts`**

This is the most critical file for implementing minimum size constraints.

```typescript
export class SplitterDragHandler {
  private startPosition: number;
  private startSizes: number[];
  private minSizes: number[];
  private maxSizes: number[];
  
  /**
   * Called when drag starts
   */
  onDragStart(event: MouseEvent, splitPane: IgcSplitPane, splitterIndex: number) {
    this.startPosition = this.getPosition(event, splitPane.orientation);
    
    // Calculate starting sizes and constraints
    const panes = splitPane.panes;
    this.startSizes = panes.map(p => this.getCurrentSize(p, splitPane.orientation));
    
    // Calculate minimum sizes for each pane
    this.minSizes = panes.map(pane => {
      if (splitPane.orientation === 'horizontal') {
        return getEffectiveMinWidth(pane, this.dockManager) || 0;
      } else {
        return getEffectiveMinHeight(pane, this.dockManager) || 0;
      }
    });
    
    // Calculate maximum sizes (based on siblings' minimums)
    this.maxSizes = this.calculateMaxSizes(this.startSizes, this.minSizes, splitterIndex);
  }
  
  /**
   * Called during drag
   */
  onDrag(event: MouseEvent, splitPane: IgcSplitPane, splitterIndex: number) {
    const currentPosition = this.getPosition(event, splitPane.orientation);
    const delta = currentPosition - this.startPosition;
    
    // Calculate new sizes
    let newSizes = [...this.startSizes];
    
    // The splitter at index i separates panes i and i+1
    // Moving right/down: increase pane[i], decrease pane[i+1]
    // Moving left/up: decrease pane[i], increase pane[i+1]
    
    const leftPane = splitterIndex;
    const rightPane = splitterIndex + 1;
    
    // Calculate proposed sizes
    let newLeftSize = this.startSizes[leftPane] + delta;
    let newRightSize = this.startSizes[rightPane] - delta;
    
    // Apply minimum constraints
    const minLeft = this.minSizes[leftPane];
    const minRight = this.minSizes[rightPane];
    
    if (newLeftSize < minLeft) {
      // Left pane would be too small - constrain it
      newLeftSize = minLeft;
      newRightSize = this.startSizes[leftPane] + this.startSizes[rightPane] - minLeft;
    }
    
    if (newRightSize < minRight) {
      // Right pane would be too small - constrain it
      newRightSize = minRight;
      newLeftSize = this.startSizes[leftPane] + this.startSizes[rightPane] - minRight;
    }
    
    // Verify both constraints are met
    if (newLeftSize >= minLeft && newRightSize >= minRight) {
      newSizes[leftPane] = newLeftSize;
      newSizes[rightPane] = newRightSize;
      
      // Apply the new sizes
      this.applySizes(splitPane, newSizes);
      
      // Optional: Fire constraint event if at boundary
      if (newLeftSize === minLeft || newRightSize === minRight) {
        this.fireMinSizeConstraintEvent(splitPane, splitterIndex, {
          leftSize: newLeftSize,
          rightSize: newRightSize,
          minLeft,
          minRight
        });
      }
    }
  }
  
  /**
   * Calculate maximum sizes for each pane based on siblings' minimums
   */
  private calculateMaxSizes(
    currentSizes: number[],
    minSizes: number[],
    splitterIndex: number
  ): number[] {
    const maxSizes = new Array(currentSizes.length);
    
    // For the panes adjacent to the splitter being dragged
    const leftPane = splitterIndex;
    const rightPane = splitterIndex + 1;
    
    // Maximum left pane size = current left + (current right - min right)
    maxSizes[leftPane] = currentSizes[leftPane] + 
                         (currentSizes[rightPane] - minSizes[rightPane]);
    
    // Maximum right pane size = current right + (current left - min left)
    maxSizes[rightPane] = currentSizes[rightPane] + 
                          (currentSizes[leftPane] - minSizes[leftPane]);
    
    return maxSizes;
  }
  
  /**
   * Apply calculated sizes to panes
   */
  private applySizes(splitPane: IgcSplitPane, sizes: number[]) {
    splitPane.panes.forEach((pane, index) => {
      const size = sizes[index];
      if (splitPane.orientation === 'horizontal') {
        this.setPaneWidth(pane, size);
      } else {
        this.setPaneHeight(pane, size);
      }
    });
  }
}
```

### Phase 5: Layout Manager Updates

**File: `layout-manager.ts`**

```typescript
export class LayoutManager {
  /**
   * Calculate initial layout respecting minimum sizes
   */
  calculateLayout(
    rootPane: IgcSplitPane,
    availableWidth: number,
    availableHeight: number,
    dockManager: IgcDockManagerComponent
  ) {
    // Calculate minimum required space
    const minSize = calculateSplitPaneMinimumSize(rootPane, dockManager);
    
    // Check if we have enough space
    if (availableWidth < minSize.minWidth || availableHeight < minSize.minHeight) {
      console.warn(
        `Dock manager layout requires ${minSize.minWidth}x${minSize.minHeight} ` +
        `but only ${availableWidth}x${availableHeight} available. ` +
        `Some panes may not meet their minimum size constraints.`
      );
    }
    
    // Proceed with layout calculation
    this.calculatePaneLayout(rootPane, 0, 0, availableWidth, availableHeight, dockManager);
  }
  
  /**
   * Recursively calculate layout for a pane
   */
  private calculatePaneLayout(
    pane: IgcSplitPane | IgcContentPane,
    x: number,
    y: number,
    width: number,
    height: number,
    dockManager: IgcDockManagerComponent
  ) {
    if (pane.type === 'contentPane') {
      // Apply minimum size constraints
      const minWidth = getEffectiveMinWidth(pane, dockManager);
      const minHeight = getEffectiveMinHeight(pane, dockManager);
      
      const finalWidth = validateSize(width, minWidth);
      const finalHeight = validateSize(height, minHeight);
      
      this.applyPaneGeometry(pane, x, y, finalWidth, finalHeight);
    } else if (pane.type === 'splitPane') {
      this.calculateSplitPaneLayout(pane, x, y, width, height, dockManager);
    }
  }
  
  /**
   * Calculate layout for split pane children
   */
  private calculateSplitPaneLayout(
    splitPane: IgcSplitPane,
    x: number,
    y: number,
    width: number,
    height: number,
    dockManager: IgcDockManagerComponent
  ) {
    const panes = splitPane.panes;
    const orientation = splitPane.orientation;
    
    // Calculate sizes respecting minimums
    const sizes = this.calculatePaneSizes(panes, 
      orientation === 'horizontal' ? width : height,
      orientation,
      dockManager
    );
    
    // Position panes
    let offset = 0;
    panes.forEach((childPane, index) => {
      const size = sizes[index];
      
      if (orientation === 'horizontal') {
        this.calculatePaneLayout(childPane, x + offset, y, size, height, dockManager);
        offset += size + SPLITTER_WIDTH;
      } else {
        this.calculatePaneLayout(childPane, x, y + offset, width, size, dockManager);
        offset += size + SPLITTER_HEIGHT;
      }
    });
  }
  
  /**
   * Calculate sizes for panes respecting minimum constraints
   */
  private calculatePaneSizes(
    panes: Array<IgcPane>,
    availableSpace: number,
    orientation: 'horizontal' | 'vertical',
    dockManager: IgcDockManagerComponent
  ): number[] {
    // Get minimum sizes
    const minSizes = panes.map(pane => {
      if (orientation === 'horizontal') {
        return getEffectiveMinWidth(pane, dockManager) || 0;
      } else {
        return getEffectiveMinHeight(pane, dockManager) || 0;
      }
    });
    
    // Calculate total minimum required
    const splitterSpace = (panes.length - 1) * SPLITTER_WIDTH;
    const totalMinimum = minSizes.reduce((sum, min) => sum + min, 0) + splitterSpace;
    
    // Calculate available space for distribution
    const distributionSpace = availableSpace - splitterSpace;
    
    if (distributionSpace < totalMinimum - splitterSpace) {
      // Not enough space - use minimums (may cause overflow)
      return minSizes;
    }
    
    // Distribute space proportionally while respecting minimums
    // Use existing size or equal distribution as base
    const baseSizes = panes.map(pane => pane.size || (distributionSpace / panes.length));
    
    // Apply minimums
    const finalSizes = baseSizes.map((size, i) => Math.max(size, minSizes[i]));
    
    // Normalize to fit available space
    const total = finalSizes.reduce((sum, size) => sum + size, 0);
    if (total !== distributionSpace) {
      const scale = distributionSpace / total;
      return finalSizes.map((size, i) => Math.max(size * scale, minSizes[i]));
    }
    
    return finalSizes;
  }
}
```

### Phase 6: CSS Variables (Optional)

**File: `igc-dockmanager.scss`**

```scss
:host {
  // CSS variables for minimum sizes
  --igc-min-pane-width: var(--min-pane-width, unset);
  --igc-min-pane-height: var(--min-pane-height, unset);
  --igc-min-split-pane-width: var(--min-split-pane-width, unset);
  --igc-min-split-pane-height: var(--min-split-pane-height, unset);
}

.content-pane {
  min-width: var(--igc-min-pane-width);
  min-height: var(--igc-min-pane-height);
}

.split-pane {
  min-width: var(--igc-min-split-pane-width);
  min-height: var(--igc-min-split-pane-height);
}
```

**File: `igc-dockmanager.tsx`** (CSS variable integration)

```typescript
componentDidLoad() {
  // Read CSS variables if programmatic values not set
  if (this.minPaneWidth === undefined) {
    const cssValue = this.getCssVariableValue('--igc-min-pane-width');
    if (cssValue) {
      this.minPaneWidth = parseInt(cssValue);
    }
  }
  // Repeat for other properties...
}

private getCssVariableValue(variableName: string): string | null {
  const style = getComputedStyle(this.el);
  const value = style.getPropertyValue(variableName).trim();
  return value || null;
}
```

## Testing Strategy

### Unit Tests

```typescript
describe('Minimum Size Constraints', () => {
  describe('Size Calculation Utilities', () => {
    it('should return pane-level minWidth over component default', () => {
      const dockManager = { minPaneWidth: 100 } as IgcDockManagerComponent;
      const pane = { type: 'contentPane', minWidth: 200 } as IgcContentPane;
      expect(getEffectiveMinWidth(pane, dockManager)).toBe(200);
    });
    
    it('should return component default when pane has no override', () => {
      const dockManager = { minPaneWidth: 100 } as IgcDockManagerComponent;
      const pane = { type: 'contentPane' } as IgcContentPane;
      expect(getEffectiveMinWidth(pane, dockManager)).toBe(100);
    });
    
    it('should return undefined when no constraints set', () => {
      const dockManager = {} as IgcDockManagerComponent;
      const pane = { type: 'contentPane' } as IgcContentPane;
      expect(getEffectiveMinWidth(pane, dockManager)).toBeUndefined();
    });
  });
  
  describe('Splitter Drag Handler', () => {
    it('should prevent resizing below minimum width', () => {
      // Test implementation
    });
    
    it('should allow resizing above minimum width', () => {
      // Test implementation
    });
    
    it('should constrain both panes in a split', () => {
      // Test implementation
    });
  });
  
  describe('Layout Manager', () => {
    it('should respect minimum sizes in initial layout', () => {
      // Test implementation
    });
    
    it('should distribute extra space when minimums are met', () => {
      // Test implementation
    });
    
    it('should warn when available space is insufficient', () => {
      // Test implementation
    });
  });
});
```

### Integration Tests

```typescript
describe('Minimum Size Integration', () => {
  it('should prevent splitter from resizing pane below minimum', async () => {
    const dockManager = createDockManager({
      minPaneWidth: 150,
      layout: createHorizontalSplit(['pane1', 'pane2'])
    });
    
    const splitter = getSplitter(dockManager, 0);
    await dragSplitter(splitter, -500); // Try to drag far left
    
    const pane1Width = getPaneWidth(dockManager, 'pane1');
    expect(pane1Width).toBeGreaterThanOrEqual(150);
  });
  
  it('should respect per-pane overrides', async () => {
    const dockManager = createDockManager({
      minPaneWidth: 100,
      layout: {
        type: 'splitPane',
        orientation: 'horizontal',
        panes: [
          { type: 'contentPane', contentId: 'pane1', minWidth: 200 },
          { type: 'contentPane', contentId: 'pane2' }
        ]
      }
    });
    
    const splitter = getSplitter(dockManager, 0);
    await dragSplitter(splitter, -500);
    
    expect(getPaneWidth(dockManager, 'pane1')).toBeGreaterThanOrEqual(200);
    expect(getPaneWidth(dockManager, 'pane2')).toBeGreaterThanOrEqual(100);
  });
});
```

### E2E Tests

```typescript
describe('Minimum Size E2E', () => {
  it('should visually constrain splitter movement', async () => {
    await page.setContent(`
      <igc-dockmanager id="dm"></igc-dockmanager>
      <script>
        dm.minPaneWidth = 150;
        dm.layout = /* ... */;
      </script>
    `);
    
    const splitter = await page.$('.splitter');
    const box = await splitter.boundingBox();
    
    // Drag splitter left
    await page.mouse.move(box.x, box.y);
    await page.mouse.down();
    await page.mouse.move(box.x - 500, box.y);
    await page.mouse.up();
    
    // Verify pane width
    const paneWidth = await page.$eval('.content-pane', el => el.offsetWidth);
    expect(paneWidth).toBeGreaterThanOrEqual(150);
  });
});
```

## Performance Considerations

1. **Cache minimum size calculations**
   - Calculate once per layout change
   - Store in pane metadata
   - Invalidate only when properties change

2. **Optimize drag operations**
   - Debounce/throttle drag events if needed
   - Use RequestAnimationFrame for smooth updates
   - Avoid unnecessary layout recalculations

3. **Lazy evaluation**
   - Only calculate nested minimums when needed
   - Cache results until structure changes

## Debugging Support

Add development-mode warnings:

```typescript
if (process.env.NODE_ENV === 'development') {
  if (totalMinimum > availableSpace) {
    console.warn(
      '[IgcDockManager] Minimum size constraints cannot be satisfied:\n' +
      `  Required: ${totalMinimum}px\n` +
      `  Available: ${availableSpace}px\n` +
      `  Panes: ${panes.map((p, i) => `${i}: ${minSizes[i]}px`).join(', ')}`
    );
  }
}
```

## Framework Wrapper Updates

### React (igniteui-react)

```typescript
// File: IgrDockManager.tsx
export interface IgrDockManagerProps {
  minPaneWidth?: number;
  minPaneHeight?: number;
  minSplitPaneWidth?: number;
  minSplitPaneHeight?: number;
  // ... other props
}
```

### Angular (igniteui-angular)

```typescript
// File: igc-dockmanager.component.ts
@Input() minPaneWidth?: number;
@Input() minPaneHeight?: number;
@Input() minSplitPaneWidth?: number;
@Input() minSplitPaneHeight?: number;
```

### Blazor (igniteui-blazor)

```csharp
// File: IgbDockManager.razor.cs
[Parameter] public int? MinPaneWidth { get; set; }
[Parameter] public int? MinPaneHeight { get; set; }
[Parameter] public int? MinSplitPaneWidth { get; set; }
[Parameter] public int? MinSplitPaneHeight { get; set; }
```

## Rollout Plan

1. **Phase 1**: Internal implementation (2 weeks)
   - Add properties and type definitions
   - Implement core logic
   - Add unit tests

2. **Phase 2**: Integration and testing (1 week)
   - Integration tests
   - E2E tests
   - Manual testing across scenarios

3. **Phase 3**: Framework wrappers (1 week)
   - Update React wrapper
   - Update Angular wrapper
   - Update Blazor wrapper
   - Test each wrapper

4. **Phase 4**: Documentation and release (1 week)
   - API documentation
   - User guide updates
   - Migration guide
   - CHANGELOG entry
   - Release notes

## Version Targeting

Recommend for version: **1.19.0**

Breaking changes: **None** (fully backward compatible)

## Success Metrics

- ✅ All existing tests pass
- ✅ New tests provide >90% coverage of new code
- ✅ No performance regression (measure drag performance)
- ✅ Works across all supported browsers
- ✅ Works with all framework wrappers
- ✅ Documentation complete
