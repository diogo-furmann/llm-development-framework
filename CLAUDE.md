# React Development Project - Claude AI Context

## Project Overview
This is a React project designed for documentation-driven development with Claude AI. The project serves as a sandbox for exploring how comprehensive documentation can improve AI-assisted development workflows.

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

## Key Technologies
- **React 18+** - UI framework
- **TypeScript** - Type safety
- **React Router** - Client-side routing
- **Ant Design (Antd)** - Enterprise-class UI component library

## Development Guidelines
- Follow component-driven development using Antd components
- Write TypeScript interfaces for all data structures
- Use functional components with hooks
- Leverage Antd's comprehensive component library (Button, Form, Table, Modal, etc.)
- Use Antd's built-in responsive design system
- Focus on clean, maintainable code

## Development Commands
- Development server: `npm start`
- Type checking: `npm run type-check`
- Linting: `npm run lint`

## Documentation Index
When implementing features, always consider these documentation files:

### Architecture & Patterns
- **ARCHITECTURE.md** - System layers, data flow, and design patterns
- **STATE.md** - State management patterns and hooks
- **ROUTING.md** - Navigation structure and route patterns

### Implementation Guidelines
- **COMPONENTS.md** - Ant Design component usage, patterns, and theming
- **CONVENTIONS.md** - Naming, TypeScript, and code organization
- **API.md** - Backend integration and data structures

### Workflows
- **WORKFLOWS.md** - Development processes and common tasks

### Implementation Checklist
Before implementing any feature, Claude should:
1. 📋 **Read ARCHITECTURE.md** - Understand data flow and layer responsibilities
2. 🎨 **Check COMPONENTS.md** - Use appropriate Ant Design components and theming
3. 🔧 **Follow CONVENTIONS.md** - Apply naming and TypeScript patterns
4. 🌐 **Review API.md** - Understand data structures and endpoints
5. 📱 **Consider ROUTING.md** - Plan navigation and URL structure
6. 🔄 **Apply STATE.md** - Use proper state management patterns
7. 🎯 **Reference WORKFLOWS.md** - Follow development processes

## Claude AI Integration Notes
This project is specifically designed to work with Claude AI through comprehensive documentation. Each aspect of the codebase should be well-documented to provide context for AI-assisted development.

### For Feature Implementation
When asked to implement any feature (like User CRUD), Claude should:

1. **Always read FEATURE-TEMPLATE.md first** - This provides a systematic approach
2. **Use the Task tool to read multiple documentation files** in parallel:
   ```
   Read: ARCHITECTURE.md, COMPONENTS.md, API.md, STATE.md, CONVENTIONS.md
   ```
3. **Plan before coding** - Map the feature to architectural layers
4. **Follow the data flow pattern**: User → Component → Hook → Service → API → State → Re-render
5. **Respect existing patterns** - Don't invent new approaches when documented patterns exist

### Common Feature Types & Required Reading
- **CRUD Operations**: ARCHITECTURE.md + API.md + STATE.md + COMPONENTS.md
- **Forms**: COMPONENTS.md + STATE.md + CONVENTIONS.md  
- **Navigation**: ROUTING.md + COMPONENTS.md
- **UI/Component Changes**: COMPONENTS.md + CONVENTIONS.md
- **New Pages**: All documentation files

### Development Philosophy
This documentation focuses on **implementation patterns only**:
- ✅ **Architecture** - How to structure code
- ✅ **Components** - How to use Ant Design
- ✅ **State Management** - Simple, practical patterns
- ✅ **API Integration** - Data fetching and services
- ❌ **Testing** - Handle separately later
- ❌ **Performance** - Optimize when needed
- ❌ **Security** - Handle at infrastructure level  
- ❌ **Deployment** - Handle with DevOps tools

### Quick Commands for Claude
```bash
# Before any feature implementation:
"Read all relevant documentation files first, then plan the implementation following the architectural layers"

# For User CRUD specifically:
"Read ARCHITECTURE.md, COMPONENTS.md, API.md, and STATE.md, then implement User CRUD following the documented patterns"
```