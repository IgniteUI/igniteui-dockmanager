# Minimum Size Constraints Feature - Complete Specification

## Overview

This feature adds the ability to set minimum width and height constraints for content panes and split panes in the IgcDockManager Web Component. This addresses a frequently requested enhancement where users need to prevent panes from being resized below certain dimensions.

## Problem Statement

Currently, the IgcDockManager allows panes to be freely resized to any size, similar to Visual Studio's behavior. While this provides flexibility, users have reported cases where they need to:

1. Prevent panes from becoming too small to display their content properly
2. Ensure certain panes maintain a minimum size for usability
3. Control the resize behavior with programmatic constraints that the splitter respects

A CSS-based workaround exists using `.sc-igc-split-pane-component-h { min-width: XXpx; }`, but this approach has a critical flaw: the splitter doesn't respect these CSS constraints. When a user drags the splitter past the CSS minimum, the CSS prevents the pane from shrinking, but the splitter position doesn't reflect this constraint, leading to a poor user experience.

## Solution Summary

Add new properties to the IgcDockManagerComponent and pane type definitions that allow developers to:

1. Set global default minimum sizes for all panes
2. Override minimum sizes on a per-pane basis
3. Have the splitter properly enforce these constraints during drag operations
4. Ensure initial layout and docking operations respect minimum sizes

## Key Features

### 1. Component-Level Properties (Global Defaults)

```typescript
dockManager.minPaneWidth = 150;        // Default for all content panes
dockManager.minPaneHeight = 100;       // Default for all content panes
dockManager.minSplitPaneWidth = 300;   // Default for all split panes
dockManager.minSplitPaneHeight = 200;  // Default for all split panes
```

### 2. Pane-Level Properties (Individual Overrides)

```typescript
{
  type: 'contentPane',
  contentId: 'editor',
  minWidth: 300,   // This pane requires at least 300px width
  minHeight: 200   // This pane requires at least 200px height
}
```

### 3. Splitter Constraint Enforcement

- Splitter drag operations respect minimum sizes
- Splitter stops at the position where a pane reaches its minimum
- Both adjacent panes' minimums are considered during resize

### 4. Layout Calculation Integration

- Initial layout respects minimum sizes
- Docking operations honor constraints
- Dynamic layout changes maintain minimums

## Documentation Structure

This feature is documented across four comprehensive documents:

### 1. [FEATURE_SPEC_MIN_SIZE.md](FEATURE_SPEC_MIN_SIZE.md)
**Purpose:** Complete feature specification and requirements
**Audience:** Product managers, designers, QA, and developers

**Contents:**
- Current behavior and limitations
- Proposed solution and API design
- Implementation requirements
- Edge cases and handling strategies
- Usage examples
- Testing requirements
- Documentation needs
- Performance considerations
- Accessibility requirements
- Implementation checklist

### 2. [API_DESIGN_MIN_SIZE.md](API_DESIGN_MIN_SIZE.md)
**Purpose:** Detailed API design with TypeScript interfaces and code examples
**Audience:** Developers, technical writers, framework wrapper maintainers

**Contents:**
- Complete TypeScript type definitions
- Property descriptions with JSDoc comments
- Helper types and interfaces
- Usage examples for various scenarios
- React, Angular, Blazor integration examples
- CSS variable integration (optional)
- Event integration (optional enhancement)
- Migration guide from CSS workaround
- Property precedence rules
- Framework-specific notes

### 3. [IMPLEMENTATION_GUIDE.md](IMPLEMENTATION_GUIDE.md)
**Purpose:** Technical implementation guidance for developers
**Audience:** Internal development team

**Contents:**
- Architecture overview
- Component structure and file organization
- Step-by-step implementation phases
- Code samples for each component
- Utility function implementations
- Splitter drag logic with constraint enforcement
- Layout manager updates
- CSS variable support (optional)
- Testing strategy with test cases
- Performance optimization tips
- Debugging support
- Framework wrapper updates
- Rollout plan and timeline

### 4. [ROADMAP.md](ROADMAP.md)
**Purpose:** Product roadmap and feature planning
**Audience:** All stakeholders

**Updates:**
- Added minimum size feature to "Going down the road" section
- Links to detailed specification

## Implementation Priority

**Priority:** High

**Rationale:**
- Frequently requested feature
- Addresses significant UX limitation
- Relatively isolated change (low risk)
- Provides immediate value to users
- No breaking changes (fully backward compatible)

## Timeline Estimate

| Phase | Duration | Details |
|-------|----------|---------|
| Design/Review | 1 week | API review, stakeholder approval |
| Implementation | 2-3 weeks | Core logic, properties, calculations |
| Testing | 1 week | Unit, integration, E2E tests |
| Documentation | 1 week | API docs, user guide, examples |
| **Total** | **5-6 weeks** | Complete feature delivery |

## Version Targeting

- **Recommended version:** 1.19.0
- **Breaking changes:** None (fully backward compatible)
- **Framework support:** Web Components, React, Angular, Blazor

## Related Issues

- Original request: https://github.com/IgniteUI/igniteui-react/issues/56
- This specification addresses the feature request for minimum size constraints

## Success Criteria

✅ **Functionality**
- Users can set minimum width/height both globally and per-pane
- Splitters respect minimum size constraints during drag operations
- Initial layout and docking operations honor minimum sizes
- All existing functionality continues to work (backward compatible)

✅ **Quality**
- All existing tests pass
- New tests provide >90% coverage
- No performance regression
- Works across all supported browsers
- Works with all framework wrappers

✅ **Documentation**
- Complete API documentation
- User guide with examples
- Migration guide from CSS workaround
- CHANGELOG entry and release notes

## Technical Highlights

### Constraint Resolution

When a user drags a splitter between two panes:

1. Calculate the proposed new sizes for both panes
2. Check if either pane would violate its minimum size
3. If a constraint would be violated, adjust the sizes:
   - Constrain the affected pane to its minimum
   - Adjust the other pane accordingly
4. Only apply the resize if both panes can meet their minimums

### Nested Split Panes

For nested split panes, the minimum size is calculated as:

- **Horizontal orientation:** Sum of children widths + splitter widths, max of children heights
- **Vertical orientation:** Max of children widths, sum of children heights + splitter heights

This ensures parent containers are sized appropriately to contain their children while respecting all constraints.

### Backward Compatibility

All new properties are optional with `undefined` defaults. When undefined:
- No minimum size constraints are applied
- Current behavior is maintained
- Existing applications work without modifications

## API Stability

This feature introduces new optional properties only. No existing properties or behaviors are modified. The API is designed to be:

- **Intuitive:** Similar to CSS min-width/min-height concepts
- **Flexible:** Support both global defaults and per-pane overrides
- **Consistent:** Works the same across all framework wrappers
- **Future-proof:** Can be extended with max-width/max-height in the future

## Migration from CSS Workaround

Users currently using the CSS workaround should migrate to the programmatic API:

**Before:**
```css
.sc-igc-split-pane-component-h {
  min-width: 200px;
}
```

**After:**
```typescript
dockManager.minPaneWidth = 200;
```

The programmatic approach provides:
- Proper splitter constraint enforcement
- Per-pane customization
- Consistent behavior across browsers
- Better integration with dynamic layouts

## Future Enhancements

While not part of this initial implementation, the foundation laid by this feature enables:

1. **Maximum size constraints** - `maxWidth` and `maxHeight` properties
2. **Aspect ratio constraints** - Maintain width/height ratios
3. **Size presets** - Named size profiles (small, medium, large)
4. **Responsive breakpoints** - Different minimums for different viewport sizes
5. **Validation events** - Notify when constraints cannot be satisfied
6. **Animation** - Smooth transitions when constraints are enforced

## Questions and Answers

**Q: What happens if minimum sizes exceed available space?**
A: The layout manager will try to satisfy constraints but may enable scrolling if necessary. A warning is logged in development mode.

**Q: Can I disable minimum size for specific panes?**
A: Yes, set `minWidth: 0` and `minHeight: 0` on the pane to disable constraints.

**Q: Do minimums apply to floating panes?**
A: Yes, floating windows respect minimum sizes just like docked panes.

**Q: How do minimums interact with the `size` property?**
A: If `size` is less than the minimum, it will be adjusted to meet the minimum.

**Q: Will this affect performance?**
A: No significant performance impact. Calculations are cached and only recalculated when necessary.

## Next Steps

1. **Review and approval** - Stakeholder review of specification
2. **Implementation planning** - Assign developers, set milestones
3. **Development** - Follow implementation guide
4. **Testing** - Comprehensive test coverage
5. **Documentation** - Update all docs and examples
6. **Release** - Include in version 1.19.0

## Contact

For questions about this specification:
- **Product questions:** Product management team
- **Technical questions:** Development team
- **Documentation questions:** Technical writing team

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-27  
**Status:** Proposed  
**Target Release:** 1.19.0
