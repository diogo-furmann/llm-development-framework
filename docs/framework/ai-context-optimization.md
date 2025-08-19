# AI Context Optimization

## 🎯 Context Priority (% of Available Context)

```
┌─ TASK (10%) ────────────────────────────┐
│ What to build + success criteria        │
├─ DECISION (15%) ───────────────────────┤  
│ 1 relevant decision tree section only   │
├─ SNIPPETS (40%) ───────────────────────┤
│ 2-3 copy-paste code patterns           │
├─ CONSTRAINTS (10%) ────────────────────┤
│ antd + axios + dayjs + pt-BR format     │
└─ EXAMPLES (25%) ───────────────────────┘
│ Only if context space available         │
```

## 🔄 Implementation Pattern

### 1. Context Setup
```
TASK: [specific feature]
DECISION: [relevant decision tree section]  
SNIPPETS: [specific code patterns needed]
CONSTRAINTS: antd, axios, dayjs, pt-BR, DD/MM/YYYY
```

### 2. Execution Flow
```
Decision → Copy Snippet → Replace Placeholders → Implement → Document
```

## 📋 Task Templates

### CRUD Operations
```
CONTEXT: CRUD feature
DECISION: State Management decision tree
SNIPPETS: Service Layer template, CRUD Hooks template, Page Component template
CONSTRAINTS: pt-BR locale, DD/MM/YYYY format
```

### Forms
```
CONTEXT: Form component
DECISION: Component Structure decision tree
SNIPPETS: Form Component template, Modal Form template
CONSTRAINTS: Ant Design Form, pt-BR validation
```

### API Integration
```
CONTEXT: API service
DECISION: API Integration decision tree
SNIPPETS: API Client template, Service template
CONSTRAINTS: Axios interceptors, São Paulo timezone
```

## ⚡ Quick Rules

### DO:
- Read only relevant sections
- Copy exact code snippets
- Replace `{Resource}` placeholders systematically
- Follow template structure exactly
- Implement layer by layer (Data → Logic → UI)

### DON'T:
- Read entire documentation files
- Explain patterns (just use them)
- Deviate from templates
- Skip placeholder replacement
- Mix architectural layers

## ✅ Success Metrics

- **Pattern Compliance**: 100% (must follow exactly)
- **Code Quality**: Passes all framework constraints