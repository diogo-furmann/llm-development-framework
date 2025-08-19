# Project Documentation

This folder contains all documentation for the React development project and LLM-guided development framework.

## 📁 Documentation Structure

### [`framework/`](./framework/)
**LLM Development Framework** - The core documentation that guides Claude AI in implementing features consistently.

- [`architecture.md`](./framework/architecture.md) - System layers, data flow, and design patterns
- [`components.md`](./framework/components.md) - Ant Design component usage and patterns
- [`state.md`](./framework/state.md) - State management patterns and hooks
- [`api.md`](./framework/api.md) - Backend integration and data structures
- [`routing.md`](./framework/routing.md) - Navigation structure and route patterns
- [`conventions.md`](./framework/conventions.md) - Naming, TypeScript, and code organization
- [`workflows.md`](./framework/workflows.md) - Development processes and common tasks
- [`feature-template.md`](./framework/feature-template.md) - Step-by-step feature implementation guide
- [`decision-trees.md`](./framework/decision-trees.md) - Decision trees for eliminating choice paralysis
- [`code-snippets.md`](./framework/code-snippets.md) - Ready-to-use code patterns and templates
- [`implementation-guide.md`](./framework/implementation-guide.md) - How to document implementations
- [`ai-context-optimization.md`](./framework/ai-context-optimization.md) - AI context optimization for better LLM performance

### [`implementations/`](./implementations/)
**Implementation Documentation** - Detailed docs of everything that has been built.

- [`features/`](./implementations/features/) - Feature-level implementation documentation
- [`components/`](./implementations/components/) - Individual component documentation
- [`services/`](./implementations/services/) - API service implementation docs
- [`hooks/`](./implementations/hooks/) - Custom hooks documentation

### [`adrs/`](./adrs/)
**Architecture Decision Records** - Important architectural decisions and constraints.

- [`adr-template.md`](./adrs/adr-template.md) - Template and examples for ADRs

## 🚀 Quick Start for Claude AI

### Efficient Implementation Workflow:
1. **Start with** [`../CLAUDE.md`](../CLAUDE.md) - Main entry point with checklists
2. **Optimize context** - Read [`ai-context-optimization.md`](./framework/ai-context-optimization.md) for better LLM performance
3. **Check decision trees** - Use [`decision-trees.md`](./framework/decision-trees.md) for quick architectural decisions
4. **Use code snippets** - Copy patterns from [`code-snippets.md`](./framework/code-snippets.md)
5. **Follow feature template** if needed - Reference [`feature-template.md`](./framework/feature-template.md) for complex features
6. **Document implementation** - Use [`implementation-guide.md`](./framework/implementation-guide.md) framework

### Optimized LLM Workflow:
```
Context Optimization → Decision Tree → Code Snippet → Adapt → Implement → Document
```

This approach maximizes LLM performance by providing structured, prioritized context instead of overwhelming documentation.

## 🎯 Documentation Philosophy

This documentation system is designed to:
- **Guide LLM implementations** through consistent patterns
- **Capture implementation knowledge** for future reference
- **Maintain architectural consistency** across all features
- **Enable knowledge accumulation** over time

## 📝 How to Use This Documentation

### For Implementing Features
1. Read the relevant framework documentation
2. Follow the feature-template.md process
3. Implement following architectural patterns
4. Document the implementation in the implementations/ folder

### For Understanding Existing Code
1. Check implementations/ for what has been built
2. Reference framework/ for the patterns used
3. Look at adrs/ for architectural decisions made

## 🔄 Keeping Documentation Updated

- **Framework docs** change rarely (architectural patterns)
- **Implementation docs** grow with every feature built
- **ADRs** are added when architectural decisions are made
- **This README** should be updated when structure changes