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

docs/
├── framework/       # LLM Development Framework documentation
│   ├── architecture.md, components.md, state.md, etc.
├── implementations/ # Documentation of implemented features/components
│   ├── features/    # Feature implementation docs
│   ├── components/  # Component documentation
│   ├── services/    # Service documentation
│   └── hooks/       # Custom hooks documentation
└── adrs/           # Architecture Decision Records
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

## Documentation Index
When implementing features, always consider these documentation files:

### Architecture & Patterns
- **[docs/framework/architecture.md](./docs/framework/architecture.md)** - System layers, data flow, and design patterns
- **[docs/framework/state.md](./docs/framework/state.md)** - State management patterns and hooks
- **[docs/framework/routing.md](./docs/framework/routing.md)** - Navigation structure and route patterns

### Implementation Guidelines
- **[docs/framework/components.md](./docs/framework/components.md)** - Ant Design component usage, patterns, and theming
- **[docs/framework/conventions.md](./docs/framework/conventions.md)** - Naming, TypeScript, and code organization
- **[docs/framework/api.md](./docs/framework/api.md)** - Backend integration and data structures
- **[docs/framework/implementation-guide.md](./docs/framework/implementation-guide.md)** - How to document every implementation

### Workflows & Templates
- **[docs/framework/workflows.md](./docs/framework/workflows.md)** - Development processes and common tasks
- **[docs/framework/feature-template.md](./docs/framework/feature-template.md)** - Step-by-step feature implementation guide
- **[docs/framework/decision-trees.md](./docs/framework/decision-trees.md)** - Decision trees for eliminating choice paralysis
- **[docs/framework/code-snippets.md](./docs/framework/code-snippets.md)** - Ready-to-use code patterns and templates
- **[docs/framework/ai-context-optimization.md](./docs/framework/ai-context-optimization.md)** - AI context optimization for better LLM performance

### Optimized Implementation Checklist
Before implementing any feature, Claude should:
1. 🧠 **Optimize Context** - Read docs/framework/ai-context-optimization.md to understand priority hierarchy
2. 📋 **Read Architecture** - docs/framework/architecture.md for data flow and layer responsibilities
3. 🎯 **Use Decision Trees** - docs/framework/decision-trees.md for quick architectural decisions
4. 📦 **Copy Code Snippets** - docs/framework/code-snippets.md for ready-to-use patterns
5. 🎨 **Check Components** - docs/framework/components.md for Ant Design usage
6. 🔧 **Follow Conventions** - docs/framework/conventions.md for naming and TypeScript patterns
7. 🌐 **Review API** - docs/framework/api.md for data structures and endpoints (if needed)
8. 📝 **Document Implementation** - docs/framework/implementation-guide.md

### After Every Implementation
Claude must create documentation following docs/framework/implementation-guide.md:
- **Feature docs**: `docs/implementations/features/{feature-name}/implementation.md`
- **Component docs**: `docs/implementations/components/{category}/{component-name}.md`  
- **Service docs**: `docs/implementations/services/{service-name}.md`
- **Hook docs**: `docs/implementations/hooks/{category}/{hook-name}.md`

## Claude AI Integration Notes
This project is specifically designed to work with Claude AI through comprehensive documentation. Each aspect of the codebase should be well-documented to provide context for AI-assisted development.

### For Feature Implementation
When asked to implement any feature (like User CRUD), Claude should:

1. **Optimize context first** - Read docs/framework/ai-context-optimization.md to understand context priority
2. **Use structured reasoning** - Follow Decision → Code → Adapt → Implement pattern
3. **Check decision trees** - Read only relevant decision tree section for the task
4. **Copy code snippets** - Use specific patterns, replace placeholders systematically
5. **Implement layer-by-layer** - Data → Logic → UI → Integration → Validation → Documentation
6. **Follow constraints** - antd, axios, dayjs (pt-BR), DD/MM/YYYY format
7. **Validate TypeScript iteratively** - Run `npx tsc --noEmit` repeatedly until zero errors
8. **Format code** - Run `npm run format` to apply consistent formatting
9. **Create concise documentation** - Brief, essential implementation details only

### Common Feature Types & Required Reading
- **CRUD Operations**: architecture.md + api.md + state.md + components.md
- **Forms**: components.md + state.md + conventions.md  
- **Navigation**: routing.md + components.md
- **UI/Component Changes**: components.md + conventions.md
- **New Pages**: All framework documentation files

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
# Context-optimized approach:
"Read ai-context-optimization.md first, then follow Decision→Code→Adapt→Implement pattern using specific decision trees and code snippets"

# For CRUD features specifically:
"Context: CRUD operation | Decision: State Management tree L15-23 | Snippets: Service+Hooks templates L298-450 | Constraints: antd+axios+dayjs pt-BR"

# For forms:
"Context: Form creation | Decision: Component Structure tree L35-65 | Snippets: Form template L328-390 | Format: DD/MM/YYYY"
```

### AI Performance Optimization
This framework includes specific optimizations for LLM performance:
- **Context prioritization** to reduce token usage
- **Structured reasoning patterns** to improve consistency  
- **Template-based implementation** to reduce hallucination
- **Performance metrics** to measure implementation quality