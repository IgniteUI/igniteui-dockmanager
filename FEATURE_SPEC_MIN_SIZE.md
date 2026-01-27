# Feature Specification: Minimum Size for Content Panes and Split Panes

## Overview
Add the ability to set minimum width and height constraints for content panes and split panes in the IgcDockManager, ensuring that the splitter respects these constraints during resize operations.

## Current Behavior
- Panes are freely resizable to any size
- No minimum size constraints exist
- Modeled after Visual Studio's free resizing behavior
- CSS workaround exists (`.sc-igc-split-pane-component-h { min-width: XXpx; }`) but splitter doesn't respect it

## Proposed Solution

### 1. API Design

#### A. Component-Level Properties (Global Defaults)
Add properties to `IgcDockManagerComponent`:

```typescript
/**
 * Default minimum width for content panes (in pixels).
 * Applied to all content panes unless overridden at pane level.
 * Default: undefined (no minimum)
 */
minPaneWidth?: number;

/**
 * Default minimum height for content panes (in pixels).
 * Applied to all content panes unless overridden at pane level.
 * Default: undefined (no minimum)
 */
minPaneHeight?: number;

/**
 * Default minimum width for split panes (in pixels).
 * Applied to all split panes unless overridden at pane level.
 * Default: undefined (no minimum)
 */
minSplitPaneWidth?: number;

/**
 * Default minimum height for split panes (in pixels).
 * Applied to all split panes unless overridden at pane level.
 * Default: undefined (no minimum)
 */
minSplitPaneHeight?: number;
```

#### B. Pane-Level Properties (Individual Overrides)
Add properties to pane configurations (`IgcContentPane` and `IgcSplitPane`):

```typescript
/**
 * Minimum width for this specific pane (in pixels).
 * Overrides the global default set on IgcDockManagerComponent.
 * Default: undefined (uses global default or no minimum)
 */
minWidth?: number;

/**
 * Minimum height for this specific pane (in pixels).
 * Overrides the global default set on IgcDockManagerComponent.
 * Default: undefined (uses global default or no minimum)
 */
minHeight?: number;
```

### 2. Implementation Requirements

#### A. Splitter Resize Behavior
- When user drags a splitter, calculate the resulting sizes of affected panes
- If a pane would be resized below its minimum width/height:
  - Prevent the resize operation beyond that point
  - Stop the splitter at the position where the pane reaches its minimum size
  - Provide visual feedback (optional: cursor change, splitter resistance)
  
#### B. Initial Layout Calculation
- When calculating initial layout or after docking operations:
  - Respect minimum size constraints
  - If total available space is less than sum of minimum sizes:
    - Apply minimum sizes in priority order (primary panes first)
    - Remaining panes may be smaller or hidden if necessary
    - Consider adding a warning in development mode

#### C. Orientation-Aware Application
- Apply `minWidth` for panes in horizontal split containers
- Apply `minHeight` for panes in vertical split containers
- For panes that can be resized in both directions, apply both constraints

#### D. Nested Split Panes
- Minimum size of a split pane should be at least the sum of:
  - Minimum sizes of its children panes
  - Space for splitters between children
  - Any padding/margins

### 3. CSS Variable Support (Optional Enhancement)
In addition to programmatic API, support CSS variables for easier global styling:

```css
:root {
  --igc-min-pane-width: 100px;
  --igc-min-pane-height: 100px;
  --igc-min-split-pane-width: 200px;
  --igc-min-split-pane-height: 200px;
}
```

These would serve as fallback defaults if programmatic values are not set.

### 4. Backward Compatibility
- All new properties should be optional with `undefined` defaults
- When undefined, maintain current behavior (no minimum size constraints)
- Existing applications should work without any changes
- CSS workaround (`.sc-igc-split-pane-component-h`) should continue to work but be superseded by programmatic API

### 5. Edge Cases to Handle

#### A. Conflicting Constraints
- If minimum sizes of sibling panes exceed available space:
  - Option 1: Apply overflow scrolling to parent container
  - Option 2: Proportionally reduce all minimums
  - Option 3: Respect minimums and allow content to overflow
  - **Recommendation**: Option 1 (scrolling) for best UX

#### B. Floating Panes
- Apply minimum size constraints to floating windows
- Prevent window resize below minimum
- Set default floating window size to at least the minimum

#### C. Maximized/Minimized States
- Minimized panes: ignore minimum size (collapsed state)
- Maximized panes: minimum size still applies but usually irrelevant
- Restore state: ensure restored size respects minimum

#### D. Responsive Behavior
- When viewport shrinks below total minimum sizes:
  - Enable scrolling on dock manager container
  - Consider providing a responsive mode that temporarily relaxes constraints
  - Emit a warning event when constraints cannot be satisfied

### 6. Usage Examples

#### Example 1: Global Defaults
```typescript
const dockManager = document.querySelector('igc-dockmanager');
dockManager.minPaneWidth = 150;
dockManager.minPaneHeight = 100;
dockManager.minSplitPaneWidth = 200;
dockManager.minSplitPaneHeight = 150;
```

#### Example 2: Per-Pane Override
```typescript
const layout = {
  type: 'splitPane',
  orientation: 'horizontal',
  panes: [
    {
      type: 'contentPane',
      contentId: 'content1',
      minWidth: 200,  // Override for this pane
      minHeight: 150
    },
    {
      type: 'contentPane',
      contentId: 'content2'
      // Uses global defaults
    }
  ]
};
```

#### Example 3: CSS Variables
```css
igc-dockmanager {
  --igc-min-pane-width: 120px;
  --igc-min-pane-height: 100px;
}

/* Override for specific instance */
#myDockManager {
  --igc-min-pane-width: 200px;
}
```

### 7. Testing Requirements

#### Unit Tests
- Test minimum size enforcement during splitter drag
- Test minimum size with nested split panes
- Test override of global defaults
- Test edge cases (conflicting constraints, insufficient space)
- Test with different orientations (horizontal/vertical)

#### Integration Tests
- Test interaction with floating panes
- Test with maximize/minimize operations
- Test with docking/undocking operations
- Test responsive behavior on small viewports

#### Visual Regression Tests
- Capture screenshots of:
  - Splitter stopped at minimum size
  - Layout with various minimum sizes applied
  - Nested panes with cascading minimums

### 8. Documentation Updates

#### API Documentation
- Add property descriptions to component API docs
- Include examples in component documentation
- Update TypeScript definitions

#### User Guide
- Add section on "Controlling Pane Sizes"
- Provide common use cases and recipes
- Explain interaction with other size-related properties (e.g., `size`, `floatingWidth`, `floatingHeight`)

#### Migration Guide
- Document differences from CSS workaround approach
- Provide migration examples for existing CSS-based solutions

### 9. Performance Considerations
- Minimum size calculations should be cached and only recalculated when:
  - Layout structure changes
  - Minimum size properties change
  - Window/container resizes
- Avoid recalculating on every mouse move during drag

### 10. Accessibility
- Ensure keyboard-based pane resizing also respects minimum sizes
- Provide appropriate ARIA attributes if minimum size constraints affect user interaction
- Ensure screen readers announce when resize is constrained by minimum size

### 11. Related Properties
Consider how this feature interacts with existing properties:
- `size`: Initial size, should be >= minimum size
- `floatingWidth`/`floatingHeight`: Should respect minimums
- `useFixedSize`: How do minimums interact with fixed sizing?

## Implementation Checklist

- [ ] Add properties to IgcDockManagerComponent
- [ ] Add properties to pane type definitions
- [ ] Implement minimum size calculation logic
- [ ] Update splitter drag logic to enforce minimums
- [ ] Update layout calculation to respect minimums
- [ ] Add CSS variable support (optional)
- [ ] Implement edge case handling
- [ ] Write unit tests
- [ ] Write integration tests
- [ ] Update TypeScript definitions
- [ ] Update API documentation
- [ ] Update user guide
- [ ] Test with React wrapper (IgrDockManager)
- [ ] Test with Angular wrapper
- [ ] Test with Blazor wrapper
- [ ] Add visual regression tests
- [ ] Update CHANGELOG.md

## Success Criteria
1. Users can set minimum width/height both globally and per-pane
2. Splitters respect minimum size constraints during drag operations
3. Initial layout and docking operations honor minimum sizes
4. All existing functionality continues to work (backward compatible)
5. Performance impact is negligible
6. Feature works across all framework wrappers (React, Angular, Blazor)

## Priority
**High** - This is a frequently requested feature that addresses a significant limitation in the current UX.

## Related Issues
- Original request: https://github.com/IgniteUI/igniteui-react/issues/56
- This issue: (reference current issue number)

## Timeline Estimate
- Design/Review: 1 week
- Implementation: 2-3 weeks
- Testing: 1 week
- Documentation: 1 week
- Total: 5-6 weeks
