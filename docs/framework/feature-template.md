# Feature Implementation Template

Use this template when implementing any new feature to ensure all architectural layers are considered.

## Feature: [Feature Name]
**Description**: [Brief description of the feature]

## Pre-Implementation Checklist

### 1. Architecture Review (ARCHITECTURE.md)
- [ ] **Presentation Layer**: Which components will be affected?
- [ ] **Business Logic Layer**: Which hooks/services will handle the logic?
- [ ] **Data Layer**: How will state be managed? What API endpoints are needed?
- [ ] **Data Flow**: User Input → Component → Hook → Service → API → State Update → Re-render

### 2. Component Planning (COMPONENTS.md)
- [ ] Which Ant Design components will be used?
- [ ] Are there existing components that can be reused?
- [ ] What new components need to be created?
- [ ] Component structure: Directory and files needed

### 3. Data & API Integration (API.md + STATE.md)
- [ ] What TypeScript interfaces are needed?
- [ ] Which API endpoints will be called?
- [ ] How will loading states be managed?
- [ ] How will errors be handled?
- [ ] What state management pattern fits best?

### 4. Routing Considerations (ROUTING.md)
- [ ] Are new routes needed?
- [ ] Are there navigation changes?
- [ ] Are route guards needed?
- [ ] How will URL parameters be handled?

### 5. Code Standards (CONVENTIONS.md)
- [ ] Naming conventions for files and components
- [ ] TypeScript interface patterns
- [ ] Import/export structure
- [ ] Error handling patterns

### 6. UI/Design Approach (COMPONENTS.md)
- [ ] Use Ant Design default styling and components
- [ ] Responsive design using Ant Design Grid system
- [ ] Accessibility is built-in with Ant Design components

## Implementation Plan

### Phase 1: Foundation
- [ ] Create TypeScript interfaces (follow CONVENTIONS.md)
- [ ] Set up API service methods (follow API.md patterns)
- [ ] Create custom hooks if needed (follow STATE.md patterns)

### Phase 2: Components
- [ ] Create/update components using Ant Design (follow COMPONENTS.md)
- [ ] Implement proper error boundaries
- [ ] Add loading states

### Phase 3: Integration
- [ ] Connect components to hooks/services
- [ ] Add routing if needed (follow ROUTING.md)
- [ ] Verify data flow through all layers

### Phase 4: Validation & Documentation
- [ ] **TypeScript validation**: Run `npx tsc --noEmit` until zero errors
- [ ] **Code formatting**: Run `npm run format`
- [ ] **Create concise documentation**: Essential implementation details only
- [ ] **Verify**: No test files or testing code created

## ⚠️ Final Step Enforcement
**Before considering implementation complete:**
1. ✅ Zero TypeScript errors: `npx tsc --noEmit` shows no issues
2. ✅ Code formatted: `npm run format` applied
3. ✅ No testing code: Confirmed no .test.* or .spec.* files created
4. ✅ Documentation: Brief, essential details documented

## Example: User CRUD Feature

### Architecture Mapping
```
User Action (Create/Read/Update/Delete)
    ↓
UserForm/UserList Components (Presentation Layer)
    ↓
useUsers/useUserMutation Hooks (Business Logic Layer)
    ↓
userService.create/get/update/delete (Service Layer)
    ↓
API /users endpoints (Data Layer)
    ↓
State Update → Component Re-render
```

### Required Files
```
src/
├── components/User/
│   ├── UserForm.tsx      # Create/Edit form using Ant Design Form
│   ├── UserList.tsx      # List using Ant Design Table
│   └── UserDetails.tsx   # View using Ant Design Descriptions
├── hooks/
│   ├── useUsers.ts       # Fetch and cache users
│   └── useUserMutation.ts # Create/Update/Delete operations
├── services/
│   └── userService.ts    # API calls to /users endpoints
├── types/
│   └── user.ts          # User interface definitions
└── pages/
    └── UsersPage.tsx    # Route component combining all User components
```

## Post-Implementation
- [ ] Update relevant documentation files
- [ ] Add feature to COMPONENTS.md if new components were created
- [ ] Update API.md if new endpoints were added
- [ ] Run linting and type checking
- [ ] Verify feature works correctly