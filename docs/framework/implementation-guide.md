# Implementation Documentation Framework

## Overview
Every code implementation must be accompanied by concise, structured documentation that captures what was built, why, and how it fits into the architecture.

## Documentation Structure

### Folder Organization
```
docs/
├── features/           # Feature-level implementation docs
│   ├── user-crud/     
│   ├── auth-system/   
│   └── dashboard/     
├── components/        # Component-level implementation docs
│   ├── forms/         
│   ├── tables/        
│   └── layouts/       
├── services/          # Service/API integration docs
│   ├── user-service/  
│   ├── auth-service/  
│   └── api-client/    
└── hooks/             # Custom hooks implementation docs
    ├── data-fetching/ 
    ├── state-management/
    └── utilities/     
```

## Documentation Requirements

### ⚠️ Concise Documentation Policy
- **Brief only**: 2-3 sentences per section maximum
- **Essential information**: Architecture decisions, key files, usage patterns
- **No testing content**: Never document tests, testing strategies, or test files
- **Focus**: What was built, why, and how to use it

### For Features (docs/features/{feature-name}/implementation.md)
```markdown
# {Feature Name} Implementation

## Summary
Brief description of what was implemented (1-2 sentences)

## Architecture Mapping
- **Components**: List of components created/modified
- **Hooks**: Custom hooks implemented
- **Services**: API services created/modified
- **State**: How state is managed for this feature

## Data Flow
```
User Action → Component → Hook → Service → API
```

## Key Files Created/Modified
- `src/components/{Component}/` - Description
- `src/hooks/use{Hook}.ts` - Description  
- `src/services/{service}.ts` - Description
- `src/pages/{Page}/` - Description

## Implementation Details
### Components
- **{ComponentName}**: Brief description and purpose
- **{AnotherComponent}**: Brief description and purpose

### State Management
- Local state: What's managed at component level
- Context state: What's shared across components (if any)
- Server state: How API data is fetched and cached

### API Integration
- Endpoints used: `GET /api/{resource}`, `POST /api/{resource}`
- Error handling approach
- Loading state management

## Usage Example
```typescript
// Basic usage example showing how to use the implemented feature
```

## Notes
- Any important implementation decisions
- Trade-offs made
- Future considerations
```

### For Components (docs/components/{category}/{component-name}.md)
```markdown
# {Component Name}

## Purpose
What this component does and why it exists

## Props Interface
```typescript
interface {ComponentName}Props {
  // Props with descriptions
}
```

## Ant Design Components Used
- Button (type, size, loading state)
- Form (layout, validation rules)
- Table (columns, pagination, sorting)

## State Management
- Local state managed
- Props received from parent
- Context consumed (if any)

## Usage
```typescript
<{ComponentName} 
  prop1={value1}
  prop2={value2}
/>
```

## Implementation Notes
- Key architectural decisions
- Ant Design patterns followed
```

### For Services (docs/services/{service-name}.md)
```markdown
# {Service Name}

## Purpose
What this service handles

## API Endpoints
- `GET {endpoint}` - Description
- `POST {endpoint}` - Description  
- `PUT {endpoint}` - Description
- `DELETE {endpoint}` - Description

## Error Handling
How errors are handled and propagated

## Usage
```typescript
// Service usage examples
```
```

### For Hooks (docs/hooks/{category}/{hook-name}.md)
```markdown
# {Hook Name}

## Purpose
What this hook manages and why it exists

## Interface
```typescript
// Hook signature and return type
```

## Usage
```typescript
// Usage example
```

## Implementation Details
- State managed
- Side effects handled
- Dependencies
```

## Documentation Task Instructions

### When to Document
After **every** implementation prompt that results in code being written, Claude must:
1. Create or update the appropriate documentation
2. Place it in the correct folder structure
3. Follow the template format

### Documentation Trigger Phrases
When the user says any of these, it means documentation should be created:

- "Implement [feature name]"
- "Create [component/service/hook]"
- "Build [functionality]"
- "Add [feature] to the app"
- After completing any implementation work

### Documentation Process
1. **Identify the scope**: Feature, Component, Service, or Hook?
2. **Create/update the appropriate doc file**
3. **Follow the template structure**
4. **Be concise but complete** - capture the essential information
5. **Include actual code snippets** from the implementation

## Example Documentation Task

**After implementing User CRUD:**

```markdown
# User CRUD Implementation

## Summary
Complete user management system with create, read, update, delete operations using Ant Design components and custom hooks for API integration.

## Architecture Mapping
- **Components**: UserList, UserForm, UserDetails
- **Hooks**: useUsers, useUserOperations  
- **Services**: userService
- **State**: Local state + custom hooks for server state

## Data Flow
```
User clicks → UserForm → useUserOperations → userService → API → useUsers refetch → UserList update
```

## Key Files Created
- `src/components/User/UserList.tsx` - Table displaying users with actions
- `src/components/User/UserForm.tsx` - Create/edit form with validation
- `src/hooks/useUsers.ts` - Fetch and cache user data
- `src/hooks/useUserOperations.ts` - Create/update/delete operations
- `src/services/userService.ts` - API calls to /users endpoints
- `src/pages/UsersPage.tsx` - Main page combining all components

## Implementation Details
### Components
- **UserList**: Ant Design Table with edit/delete actions, pagination
- **UserForm**: Ant Design Form with validation rules, loading states
- **UserDetails**: Ant Design Descriptions component for view mode

### State Management  
- Local state: Form data, modal visibility
- Server state: useUsers hook manages user list with loading/error states
- Operations: useUserOperations handles CRUD with optimistic updates

### API Integration
- Endpoints: GET/POST/PUT/DELETE `/api/users`
- Error handling: Toast notifications via Ant Design message
- Loading states: Button loading, table loading, form submission

## Usage Example
```typescript
// In UsersPage
const { data: users, loading } = useUsers();
const { createUser, updateUser, deleteUser } = useUserOperations();

return (
  <div>
    <UserForm onSubmit={createUser} />
    <UserList users={users} loading={loading} onEdit={updateUser} onDelete={deleteUser} />
  </div>
);
```

## Notes
- Follows architectural layers: Component → Hook → Service → API
- Uses Ant Design defaults for all styling
- Optimistic updates for better UX
- Form validation using Ant Design Form rules
```

## Integration with CLAUDE.md

Add this to the implementation checklist in CLAUDE.md:

```markdown
### Implementation Checklist
Before implementing any feature, Claude should:
1. 📋 **Read ARCHITECTURE.md** - Understand data flow and layer responsibilities
2. 🎨 **Check COMPONENTS.md** - Use appropriate Ant Design components and theming  
3. 🔧 **Follow CONVENTIONS.md** - Apply naming and TypeScript patterns
4. 🌐 **Review API.md** - Understand data structures and endpoints
5. 📱 **Consider ROUTING.md** - Plan navigation and URL structure
6. 🔄 **Apply STATE.md** - Use proper state management patterns
7. 🎯 **Reference WORKFLOWS.md** - Follow development processes
8. 📝 **Document Implementation** - Follow IMPLEMENTATION-DOCS.md framework

### After Every Implementation
Claude must create documentation following IMPLEMENTATION-DOCS.md:
- **Feature docs**: `docs/features/{feature-name}/implementation.md`
- **Component docs**: `docs/components/{category}/{component-name}.md`  
- **Service docs**: `docs/services/{service-name}.md`
- **Hook docs**: `docs/hooks/{category}/{hook-name}.md`
```