# Architecture Decision Record (ADR) Template

Document important architectural decisions that affect how Claude should implement features.

## ADR-001: Data Flow Architecture
**Status**: Accepted  
**Date**: 2025-01-19

### Context
We need a consistent data flow pattern for all features to ensure maintainability and predictability.

### Decision
All features must follow this data flow:
```
User Action → Component (Ant Design) → Custom Hook → Service Layer → API → State Update → Component Re-render
```

### Consequences
- **Positive**: Consistent patterns, easier debugging, clear separation of concerns
- **Negative**: Slightly more boilerplate for simple features
- **Claude Impact**: Must always implement this pattern, never bypass layers

## ADR-002: Component Library and Styling Choice
**Status**: Accepted  
**Date**: 2025-01-19

### Context
Need consistent UI components and styling approach without complexity.

### Decision
Use Ant Design as the primary UI library with **default theme and colors only**. No custom CSS, no custom themes.

### Consequences  
- **Positive**: Consistent design, built-in accessibility, rapid development, no styling complexity
- **Negative**: Less design flexibility, must accept Ant Design's default appearance
- **Claude Impact**: Always use Ant Design components with default styling, never create custom CSS or themes

## ADR-003: State Management Strategy
**Status**: Accepted  
**Date**: 2025-01-19

### Context
Need predictable state management for different types of data.

### Decision
- **Local State**: useState for component-specific state
- **Shared State**: Context API for cross-component state  
- **Server State**: Custom hooks with loading/error states
- **Form State**: Ant Design Form or custom useForm hook

### Consequences
- **Claude Impact**: Must choose appropriate state management based on data scope and lifecycle

## Adding New ADRs
When making architectural decisions that Claude should follow:

1. Create ADR-XXX file
2. Document the decision and its impact on implementation
3. Update CLAUDE.md to reference the new ADR
4. Ensure all future implementations follow the ADR