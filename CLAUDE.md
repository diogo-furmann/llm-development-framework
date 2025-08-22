# React Development Project - LLM Development Framework

## Project Overview
This is a React project featuring a comprehensive **LLM Development Framework** - a documentation-driven approach that guides AI agents in building consistent, maintainable applications. 

The project serves as both a working React application and a demonstration of how structured documentation can enable predictable, high-quality AI-assisted development.

## Quick Start
```bash
npm install
npm start
```

## Project Structure
```
src/
├── components/       # Reusable UI components
├── pages/           # Route components
├── hooks/           # Custom React hooks
├── services/        # API and business logic
├── utils/           # Helper functions
├── styles/          # Global styles (minimal, Antd handles most styling)
└── types/           # TypeScript definitions
```

## Key Technologies & Required Dependencies
- **React 18+** - UI framework
- **TypeScript** - Type safety
- **React Router** - Client-side routing
- **Ant Design (Antd)** - Enterprise-class UI component library (MANDATORY)
- **Axios** - HTTP client with interceptors and error handling (MANDATORY)
- **Day.js** - Date manipulation and formatting library (MANDATORY)
- **React Error Boundary** - Functional error boundary components (MANDATORY)

### Installation Command
```bash
npm install antd axios dayjs react-router-dom react-error-boundary
npm install -D typescript @types/react @types/react-dom prettier
```

### Prettier Configuration
Create a `.prettierrc` file in the project root with:
```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false
}
```

## Development Guidelines
- Follow component-driven development using Antd components
- Write TypeScript interfaces for all data structures
- Use functional components with hooks
- Leverage Antd's comprehensive component library (Button, Form, Table, Modal, etc.)
- Use Day.js for all date formatting and manipulation instead of native Date
- Use Axios via the centralized apiClient for all HTTP requests
- Use Antd's built-in responsive design system
- Focus on clean, maintainable code

## Development Commands
- Development server: `npm start`
- Type checking: `npm run type-check`
- Linting: `npm run lint`
- Code formatting: `npm run format`

## Documentation Index (Context-Optimized)
Claude should follow this priority hierarchy for maximum efficiency:

### ⭐ Essential (90% of implementations)
- **[docs/framework/core/quick-reference.md](./docs/framework/core/quick-reference.md)** - ⚡ **START HERE** - All patterns in one place
- **[docs/framework/core/architecture.md](./docs/framework/core/architecture.md)** - System layers, data flow, and design patterns  
- **[docs/framework/core/patterns.md](./docs/framework/core/patterns.md)** - Component, state, and service patterns
- **[docs/framework/core/conventions.md](./docs/framework/core/conventions.md)** - Naming, TypeScript, and code organization

### 🔧 Tools (Use as needed)
- **[docs/framework/tools/decision-trees.md](./docs/framework/tools/decision-trees.md)** - Quick architectural decisions
- **[docs/framework/tools/code-snippets.md](./docs/framework/tools/code-snippets.md)** - Copy-paste ready patterns
- **[docs/framework/tools/feature-template.md](./docs/framework/tools/feature-template.md)** - Step-by-step implementation guide
- **[docs/framework/tools/ai-context-optimization.md](./docs/framework/tools/ai-context-optimization.md)** - Context optimization strategies

### ⚡ Ultra-Fast Implementation Checklist
Before implementing any feature, Claude should:
1. 🚀 **Start with Quick Reference** - docs/framework/core/quick-reference.md (contains 90% of what you need)
2. 🎯 **Check Decision Trees** - docs/framework/tools/decision-trees.md for quick architectural decisions
3. 📦 **Copy Code Snippets** - docs/framework/tools/code-snippets.md for ready-to-use patterns
4. 🔧 **Follow Conventions** - docs/framework/core/conventions.md for naming and TypeScript patterns
5. 📝 **Document Implementation** - Create High Level Documentation under docs/project/

### Context-Optimized Workflow
```
quick-reference.md → decision-trees.md → code-snippets.md → implement → document
```

### After Every Implementation
Claude must create high level documentation in project/ folder

## Claude AI Integration Notes
This project is specifically designed to work with Claude AI through comprehensive documentation. Each aspect of the codebase should be well-documented to provide context for AI-assisted development.

### For Feature Implementation
When asked to implement any feature (like User CRUD), Claude should:

1. **Start with Quick Reference** - docs/framework/core/quick-reference.md contains 90% of needed patterns
2. **Use Decision Trees** - docs/framework/tools/decision-trees.md for quick architectural decisions
3. **Copy Code Snippets** - docs/framework/tools/code-snippets.md for ready-to-use patterns
4. **Implement layer-by-layer** - Data → Logic → UI → Integration → Validation → Documentation
5. **Follow constraints** - antd, axios, dayjs (pt-BR), DD/MM/YYYY format
6. **Validate TypeScript iteratively** - Run `npx tsc --noEmit` repeatedly until zero errors
7. **Format code** - Run `npm run format` to apply consistent formatting
8. **Create concise documentation** - Brief, essential high level implementation details only

### Context-Optimized Feature Types & Required Reading
- **CRUD Operations**: quick-reference.md + decision-trees.md + code-snippets.md
- **Forms**: quick-reference.md + patterns.md (form section)
- **Navigation**: quick-reference.md + architecture.md (routing section)
- **UI/Component Changes**: quick-reference.md + patterns.md
- **Complex Features**: Add tools/feature-template.md for step-by-step guidance

### Development Philosophy
This documentation focuses on **implementation patterns only**:
- ✅ **Architecture** - How to structure code
- ✅ **Components** - How to use Ant Design
- ✅ **State Management** - Simple, practical patterns
- ✅ **API Integration** - Data fetching and services
- ❌ **Testing** - **NEVER implement tests, test files, or testing-related code**
- ❌ **Performance** - Optimize when needed
- ❌ **Security** - Handle at infrastructure level  
- ❌ **Deployment** - Handle with DevOps tools

### ⚠️ Testing Exclusion Policy
**Claude must NEVER:**
- Create test files (.test.ts, .spec.ts, .test.tsx, .spec.tsx)
- Write test code (describe, it, expect blocks)
- Implement testing utilities or test helpers
- Add testing-related dependencies
- Suggest or mention testing in implementation docs

### Optimized Commands for Claude
```bash
# Ultra-fast context approach:
"Start with docs/framework/core/quick-reference.md, use tools/decision-trees.md for decisions, copy from tools/code-snippets.md"

# For CRUD features specifically:
"Context: CRUD | Quick ref: Service+Hook patterns | Constraints: antd+axios+dayjs pt-BR | Format: DD/MM/YYYY"

# For forms:
"Context: Forms | Quick ref: Form patterns | Validation: Ant Design rules | Format: DD/MM/YYYY"
```

### AI Performance Optimization
This framework includes specific optimizations for LLM performance:
- **Context prioritization** to reduce token usage
- **Structured reasoning patterns** to improve consistency  
- **Template-based implementation** to reduce hallucination
- **Performance metrics** to measure implementation quality