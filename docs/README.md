# Project Documentation

This folder contains all documentation for the React development project and LLM-guided development framework.

## 📁 Documentation Structure (Context-Optimized)

### [`core/`](./framework/framework/core/) 
**Essential Framework** - High-priority, minimal context for maximum AI efficiency.

- [`quick-reference.md`](./framework/core/quick-reference.md) - ⭐ **START HERE** - All essential patterns in one place
- [`architecture.md`](./framework/core/architecture.md) - System layers, data flow, and design patterns  
- [`patterns.md`](./framework/core/patterns.md) - Component, state, and service implementation patterns
- [`conventions.md`](./framework/core/conventions.md) - Naming, TypeScript, and code organization

### [`tools/`](./framework/tools/)
**Implementation Support** - Templates, decision support, and optimization guides.

- [`ai-context-optimization.md`](./framework/tools/ai-context-optimization.md) - AI context optimization for better LLM performance
- [`decision-trees.md`](./framework/tools/decision-trees.md) - Decision trees for eliminating choice paralysis  
- [`code-snippets.md`](./framework/tools/code-snippets.md) - Ready-to-use code patterns and templates
- [`feature-template.md`](./framework/tools/feature-template.md) - Step-by-step feature implementation guide

### [`project/`](./project/)
**Implementation Documentation** - High level docs of everything that has been built.

## 🚀 Quick Start for Claude AI (Context-Optimized)

### ⚡ Ultra-Fast Workflow:
1. **Start with** [`core/quick-reference.md`](./framework/core/quick-reference.md) - Everything you need in one file
2. **Use tools as needed**:
   - [`tools/decision-trees.md`](./framework/tools/decision-trees.md) for quick decisions
   - [`tools/code-snippets.md`](./framework/tools/code-snippets.md) for ready-to-use patterns
   - [`tools/ai-context-optimization.md`](./framework/tools/ai-context-optimization.md) for context efficiency

### 📋 Implementation Priority:
```
quick-reference.md → decision-trees.md → code-snippets.md → implement → document
```

### 🎯 Context Guidelines:
- **Essential**: core/ files only (90% of use cases)
- **As needed**: tools/ files for specific requirements
- **Reference**: project/ for learning from existing code

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
4. Document the implementation in the project/ folder

### For Understanding Existing Code
1. Check project/ for what has been built
2. Reference framework/ for the patterns used
3. Look at adrs/ for architectural decisions made

## 🔄 Keeping Documentation Updated

- **Framework docs** change rarely (architectural patterns)
- **Implementation docs** grow with every feature built
- **This README** should be updated when structure changes