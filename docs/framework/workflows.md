# Development Workflows

## Development Setup

### Initial Setup
```bash
# Clone and setup
git clone <repository>
cd tfront
npm install
npm start
```

### Environment Variables
Create `.env.local` file:
```
REACT_APP_API_URL=http://localhost:3001/api
REACT_APP_ENV=development
```

## Daily Development Workflow

### Starting Work
1. Pull latest changes: `git pull origin main`
2. Create feature branch: `git checkout -b feature/task-name`
3. Start development server: `npm start`

### During Development
1. Make small, focused commits
2. Run linter: `npm run lint`
3. Run type checker: `npm run type-check`
4. Update documentation as needed

### Before Committing
1. Check for type errors: `npm run type-check`
2. Fix linting issues: `npm run lint --fix`
3. Review changes: `git diff`
4. Commit with conventional message format

## Feature Development Process

### 1. Planning Phase
- Review requirements and acceptance criteria
- Update relevant documentation (API.md, COMPONENTS.md)
- Break down work into small tasks
- Identify dependencies and blockers

### 2. Implementation Phase
- Start with types and interfaces
- Create components following design system
- Implement business logic in services/hooks
- Add error handling and loading states

### 3. Verification Phase
- Check component functionality manually
- Verify error handling
- Check responsive design with browser dev tools

### 4. Validation Phase
- **TypeScript validation**: Run `npx tsc --noEmit` iteratively until zero errors
- **Code formatting**: Run `npm run format` for consistent styling
- **Self-review**: Check code changes for completeness

### 5. Documentation Phase
- Create concise, essential documentation only
- Focus on architecture decisions and usage patterns
- **Never document testing or create test-related content**

## Common Tasks

### Adding a New Component
1. Create component directory: `src/components/ComponentName/`
2. Create files:
   - `index.ts` - Barrel export
   - `ComponentName.tsx` - Main component (using Ant Design components)
   - `ComponentName.css` - Custom styles (only if needed for Ant Design overrides)
3. Update `src/components/index.ts`
4. Document in `COMPONENTS.md`

### Adding a New Page
1. Create page directory: `src/pages/PageName/`
2. Create component following page structure
3. Add route to router configuration
4. Update `ROUTING.md`
5. Add navigation links if needed

### Adding API Integration
1. Define types in `src/types/api.ts`
2. Add service methods in `src/services/api.ts`
3. Create custom hook in `src/hooks/`
4. Update `API.md` documentation
5. Add error handling

### UI/Component Updates
1. Use Ant Design components and built-in styling exclusively
2. Leverage Ant Design's default theme and colors
3. Use Ant Design's Grid system for responsive layouts
4. Update `COMPONENTS.md` if adding new component patterns

## Development Focus

Keep the development process simple and focused on implementation:
- Start development server: `npm start`
- Check types: `npm run type-check`
- Check code style: `npm run lint`
- Focus on clean, working code

## Troubleshooting

### Common Issues

#### Development Server Won't Start
1. Check Node.js version (requires 16+)
2. Clear node_modules: `rm -rf node_modules && npm install`
3. Check for port conflicts
4. Verify environment variables

#### TypeScript Errors
1. Run type checker: `npm run type-check`
2. Check for unused imports
3. Verify all dependencies are installed
4. Check interface definitions

#### UI/Component Issues
1. Check Ant Design component imports and usage
2. Verify components are using Ant Design's default styling
3. Check in multiple browsers
4. Check responsive breakpoints using Ant Design's Grid system

### Debug Commands
```bash
# Check outdated dependencies
npm outdated

# Clean install
rm -rf node_modules package-lock.json && npm install
```

## Git Workflow

### Branch Naming
- Features: `feature/description`
- Fixes: `fix/description` 
- Documentation: `docs/description`

### Commit Messages
Follow conventional commits:
- `feat: add user authentication`
- `fix: resolve button click issue`
- `docs: update API documentation`
- `test: add component tests`
- `refactor: simplify state management`

### Pull Request Process
1. Push feature branch to remote
2. Create PR with clear description
3. Request code review
4. Address feedback
5. Merge after approval