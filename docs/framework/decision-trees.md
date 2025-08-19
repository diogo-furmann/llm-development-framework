# Decision Trees - LLM Development Framework

## Overview
Decision trees eliminate choice paralysis by providing clear, step-by-step decisions for common implementation questions. Follow these trees to make consistent architectural decisions.

## 🎯 State Management Decision Tree

```
📊 STATE MANAGEMENT DECISION
│
├─ Q1: Is this data shared across multiple components?
│  ├─ NO → Use local useState
│  │     ✅ DONE: `const [data, setData] = useState(initialValue)`
│  │
│  └─ YES → Q2: What type of shared data?
│     │
│     ├─ User authentication/session data
│     │  └─ ✅ Use AuthContext (see code-snippets.md)
│     │
│     ├─ App-wide UI settings (sidebar, preferences)
│     │  └─ ✅ Use AppConfigContext (see code-snippets.md)
│     │
│     ├─ Server data (API responses)
│     │  └─ ✅ Use custom hook with useState (see code-snippets.md)
│     │
│     └─ Other shared data
│        └─ ✅ Create new Context (see code-snippets.md)
```

## 🎨 Component Structure Decision Tree

```
🏗️ COMPONENT DECISION
│
├─ Q1: What type of component are you building?
│  │
│  ├─ Form/Data Input
│  │  └─ ✅ Use Ant Design Form + validation
│  │     📁 See: code-snippets.md → Form Patterns
│  │
│  ├─ Data Display (read-only)
│  │  ├─ List/Table → ✅ Use Ant Design Table
│  │  ├─ Cards → ✅ Use Ant Design Card
│  │  ├─ Details → ✅ Use Ant Design Descriptions
│  │  └─ 📁 See: code-snippets.md → Display Patterns
│  │
│  ├─ Layout/Container
│  │  ├─ Page layout → ✅ Use Ant Design Layout (Header/Content/Footer)
│  │  ├─ Grid layout → ✅ Use Ant Design Row/Col
│  │  ├─ Spacing → ✅ Use Ant Design Space
│  │  └─ 📁 See: code-snippets.md → Layout Patterns
│  │
│  ├─ Navigation
│  │  ├─ Menu → ✅ Use Ant Design Menu
│  │  ├─ Breadcrumbs → ✅ Use Ant Design Breadcrumb
│  │  ├─ Pagination → ✅ Use Ant Design Pagination
│  │  └─ 📁 See: code-snippets.md → Navigation Patterns
│  │
│  └─ Modal/Overlay
│     ├─ Confirmation → ✅ Use Ant Design Modal.confirm
│     ├─ Form modal → ✅ Use Ant Design Modal + Form
│     ├─ Drawer → ✅ Use Ant Design Drawer
│     └─ 📁 See: code-snippets.md → Modal Patterns
```

## 🌐 API Integration Decision Tree

```
🔌 API INTEGRATION DECISION
│
├─ Q1: What type of API operation?
│  │
│  ├─ Data Fetching (GET)
│  │  ├─ Single record → ✅ Use useApiData hook
│  │  ├─ List of records → ✅ Use useApiData hook
│  │  └─ 📁 See: code-snippets.md → Data Fetching
│  │
│  ├─ Data Mutation (POST/PUT/DELETE)
│  │  ├─ Create → ✅ Use useApiOperation hook
│  │  ├─ Update → ✅ Use useApiOperation hook  
│  │  ├─ Delete → ✅ Use useApiOperation hook
│  │  └─ 📁 See: code-snippets.md → Data Mutations
│  │
│  └─ Q2: Where should the service be called?
│     │
│     ├─ Component needs loading/error states
│     │  └─ ✅ Use custom hook (useUsers, useProducts, etc.)
│     │
│     ├─ Form submission
│     │  └─ ✅ Call service directly in form onFinish
│     │
│     └─ Event handler
│        └─ ✅ Call service in handler, manage loading locally
```

## 📁 File Structure Decision Tree

```
📂 FILE ORGANIZATION DECISION
│
├─ Q1: What are you creating?
│  │
│  ├─ Reusable Component
│  │  └─ 📍 Location: src/components/{Category}/{ComponentName}/
│  │     ├─ index.ts (barrel export)
│  │     ├─ ComponentName.tsx
│  │     └─ ComponentName.css (only if needed for Ant Design overrides)
│  │
│  ├─ Page Component
│  │  └─ 📍 Location: src/pages/{PageName}/
│  │     ├─ index.ts
│  │     └─ {PageName}Page.tsx
│  │
│  ├─ Custom Hook
│  │  ├─ Data fetching → 📍 src/hooks/use{Resource}.ts
│  │  ├─ Operations → 📍 src/hooks/use{Resource}Operations.ts
│  │  └─ Utilities → 📍 src/hooks/use{Purpose}.ts
│  │
│  ├─ Service
│  │  └─ 📍 Location: src/services/{resource}Service.ts
│  │
│  ├─ Types/Interfaces  
│  │  ├─ API related → 📍 src/types/api.ts
│  │  ├─ Component props → 📍 Same file as component
│  │  └─ Domain types → 📍 src/types/{domain}.ts
│  │
│  └─ Utility Function
│     └─ 📍 Location: src/utils/{purpose}.ts
```

## 🔧 Error Handling Decision Tree

```
⚠️ ERROR HANDLING DECISION
│
├─ Q1: Where is the error occurring?
│  │
│  ├─ API Call in Hook
│  │  └─ ✅ Set error state, return in hook interface
│  │     📁 See: code-snippets.md → Error Handling in Hooks
│  │
│  ├─ Form Submission
│  │  ├─ Validation error → ✅ Let Ant Design Form handle it
│  │  └─ API error → ✅ Show message.error() notification
│  │
│  ├─ Component Rendering
│  │  └─ ✅ Use Error Boundary (see code-snippets.md)
│  │
│  └─ Event Handler
│     └─ ✅ Try-catch + message.error() notification
│        📁 See: code-snippets.md → Event Handler Errors
```

## 🎮 User Interaction Decision Tree

```
👆 USER INTERACTION DECISION
│
├─ Q1: What type of user interaction?
│  │
│  ├─ Button Click
│  │  ├─ Navigation → ✅ Use navigate() from useNavigation hook
│  │  ├─ API call → ✅ Call service, handle loading state
│  │  ├─ State change → ✅ Update local state with setter
│  │  └─ Modal open → ✅ Set modal visibility state to true
│  │
│  ├─ Form Submission
│  │  └─ ✅ Use Ant Design Form onFinish
│  │     📁 See: code-snippets.md → Form Handling
│  │
│  ├─ Input Change
│  │  ├─ Form input → ✅ Let Ant Design Form handle it
│  │  ├─ Search/Filter → ✅ Update state, trigger useEffect
│  │  └─ Toggle/Switch → ✅ Update boolean state directly
│  │
│  └─ Table Interaction
│     ├─ Row click → ✅ Navigate or set selected state
│     ├─ Edit button → ✅ Open modal with record data
│     ├─ Delete button → ✅ Show confirmation, call delete API
│     └─ Pagination → ✅ Let Ant Design Table handle it
```

## 🛠️ Utility Functions Decision Tree

```
🔧 UTILITY FUNCTION DECISION
│
├─ Q1: What type of utility do you need?
│  │
│  ├─ Data Storage/Persistence
│  │  ├─ Browser storage → ✅ Use localStorage helpers (see code-snippets.md)
│  │  ├─ Session data → ✅ Use sessionStorage pattern (modify localStorage helpers)
│  │  └─ In-memory cache → ✅ Use useState or useRef in custom hook
│  │
│  ├─ Data Formatting/Transformation
│  │  ├─ Date formatting → ✅ Use dayjs + formatDate utility (see code-snippets.md)
│  │  ├─ Number formatting → ✅ Use numberFormats utility (see code-snippets.md)
│  │  ├─ String manipulation → ✅ Create utility function in src/utils/
│  │  └─ Data validation → ✅ Create validator functions in src/utils/
│  │
│  ├─ API/HTTP Operations
│  │  ├─ HTTP requests → ✅ Use apiClient (see code-snippets.md)
│  │  ├─ Request interceptors → ✅ Configure in apiClient setup
│  │  ├─ Error handling → ✅ Use apiClient response interceptors
│  │  └─ Authentication → ✅ Use apiClient request interceptors
│  │
│  └─ UI/Component Helpers
│     ├─ Form validation → ✅ Create validation functions in src/utils/
│     ├─ Event handling → ✅ Create helper functions in component files
│     ├─ Conditional rendering → ✅ Use inline conditions or helper functions
│     └─ Style calculations → ✅ Create utility functions (minimal usage)
```

## 🗂️ Data Flow Decision Tree

```
🔄 DATA FLOW DECISION
│
├─ Q1: Where is the data coming from?
│  │
│  ├─ User Input (Forms)
│  │  └─ Flow: Form → onFinish → Service → API
│  │     ✅ Use Ant Design Form validation
│  │
│  ├─ API/Server
│  │  └─ Flow: Component → Hook → Service → API → State → Re-render
│  │     ✅ Use custom data-fetching hook
│  │
│  ├─ URL Parameters
│  │  └─ Flow: Route → useParams → Component
│  │     ✅ Use React Router hooks
│  │
│  ├─ Local Storage
│  │  └─ Flow: Component → usePersistentState hook
│  │     ✅ Use custom persistence hook
│  │
│  └─ Context/Global State
│     └─ Flow: Context Provider → Consumer Component
│        ✅ Use useContext hook
```

## 🎯 Quick Reference

### Most Common Decisions
1. **Need to share state?** → Check State Management tree
2. **Building a form?** → Use Ant Design Form (see snippets)
3. **Fetching API data?** → Use apiClient + useApiData pattern (see snippets)
4. **Need to store data locally?** → Use localStorage helpers (see snippets)
5. **Need to format dates/numbers?** → Use dayjs + formatting utilities (see snippets)
6. **Creating CRUD operations?** → Follow the full data flow pattern
7. **Not sure about file structure?** → Check File Structure tree

### Decision Flow Summary
1. **Start with the relevant decision tree**
2. **Follow the questions step by step**  
3. **Use the recommended solution**
4. **Reference code-snippets.md for implementation details**
5. **Document your implementation when done**

### Emergency Decision Rule
**When in doubt**: Use the simplest solution that follows our architectural patterns. You can always refactor later, but consistency is more important than optimization.