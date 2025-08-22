# System Architecture

## Overview
This React application follows a modular, component-driven architecture designed for scalability and maintainability.

## Required Dependencies
These libraries are mandatory for all projects using this framework:

```json
{
  "dependencies": {
    "antd": "^5.x.x",
    "axios": "^1.x.x", 
    "dayjs": "^1.x.x",
    "react": "^18.x.x",
    "react-dom": "^18.x.x",
    "react-router-dom": "^6.x.x",
    "react-error-boundary": "^4.x.x"
  },
  "devDependencies": {
    "typescript": "^5.x.x",
    "@types/react": "^18.x.x",
    "@types/react-dom": "^18.x.x"
  }
}
```

### Library Justifications
- **Ant Design (antd)** - Complete UI component library with consistent design system
- **Axios** - HTTP client with interceptors and request/response transformation
- **Day.js** - Lightweight date manipulation and formatting library (configured for pt-BR locale)
- **React Router** - Standard routing solution for React applications
- **React Error Boundary** - Functional error boundary components

### Locale Configuration
This framework is configured for the Brazilian market:
- **Locale**: Portuguese Brazil (pt-BR)
- **Timezone**: America/Sao_Paulo
- **Date Format**: DD/MM/YYYY (Brazilian standard)
- **Time Format**: HH:mm (24-hour format)
- **Currency**: Real Brasileiro (R$) when needed

## Architecture Layers

### Presentation Layer
- **Components**: Reusable UI components with clear interfaces
- **Pages**: Route-level components that compose smaller components
- **Layouts**: Wrapper components for consistent page structure

### Business Logic Layer
- **Hooks**: Custom React hooks for stateful logic
- **Services**: API communication and external integrations
- **Utils**: Pure functions and helper utilities

### Data Layer
- **State Management**: Context API for global state
- **API Client**: Centralized HTTP client configuration
- **Types**: TypeScript interfaces and type definitions

## Data Flow

```
User Input → Component → Hook → Service → API
                ↓
            State Update → Re-render
```

### State Management Pattern
1. **Local State**: useState for component-specific state
2. **Shared State**: Context API for cross-component state
3. **Server State**: don't worry
4. **Form State**: Controlled components with validation

### API Integration Pattern
1. **API Client**: Centralized wrapper around fetch/Axios with common headers, auth tokens, and error handling
2. **Service layer**: Business logic that uses the API client
3. **Custom hooks**: Manage loading states, caching, and data fetching
4. **Error boundaries**: Catch and display component errors

## Directory Structure

```
src/
├── components/          # Reusable UI components
│   ├── base/           # Basic building blocks (Button, Input)
│   ├── composite/      # Complex components (DataTable, Form)
│   └── layout/         # Layout components (Header, Sidebar)
├── pages/              # Route components
│   ├── Home/
│   ├── Dashboard/
│   └── Settings/
├── hooks/              # Custom React hooks
│   ├── useApi.ts       # API request hook
│   ├── useAuth.ts      # Authentication hook
│   └── useLocalStorage.ts
├── services/           # Business logic and API
│   ├── apiClient.ts    # HTTP client wrapper
│   ├── auth.ts         # Authentication service
│   └── userService.ts  # Example resource service
├── types/              # TypeScript definitions
│   ├── api.ts          # API response types
│   ├── auth.ts         # Authentication types
│   └── common.ts       # Common interfaces
├── utils/              # Helper functions
│   ├── validation.ts   # Form validation
│   ├── formatting.ts   # Date and data formatting
│   ├── localStorage.ts # LocalStorage helpers
│   └── constants.ts    # Application constants
└── styles/             # Global styles and themes
    ├── globals.css
    ├── variables.css
    └── components.css
```

## Design Patterns

### Component Composition
```typescript
// Container/Presenter pattern
const DataPage = () => {
  const { data, loading, error } = useApiData();
  
  if (loading) return <Loading />;
  if (error) return <ErrorMessage error={error} />;
  
  return <DataPresenter data={data} />;
};
```

### Custom Hooks
```typescript
// Encapsulate complex state logic
const useApiData = (endpoint: string) => {
  const [state, setState] = useState({ data: null, loading: true, error: null });
  
  useEffect(() => {
    const loadData = async () => {
      try {
        setState(prev => ({ ...prev, loading: true, error: null }));
        const response = await fetchData(endpoint);
        setState({ data: response, loading: false, error: null });
      } catch (error) {
        setState({ data: null, loading: false, error: error.message });
      }
    };
    
    loadData();
  }, [endpoint]);
  
  return state;
};
```

### Error Boundaries
```typescript
// Catch and handle component errors using react-error-boundary
import { ErrorBoundary } from 'react-error-boundary';

const ErrorFallback = ({ error, resetErrorBoundary }) => (
  <div>Something went wrong: {error.message}</div>
);

// Usage:
<ErrorBoundary FallbackComponent={ErrorFallback}>
  <App />
</ErrorBoundary>
```

## Utility Layer Patterns

### API Client
- **Single HTTP client**: Use one configured instance with common headers and error handling
- **Authentication integration**: Automatically include auth tokens in requests
- **Error handling**: Consistent error response parsing and logging

### LocalStorage Helpers
- **Centralized access**: Use helper functions instead of direct localStorage calls
- **Type safety**: Parse and validate data when reading from localStorage
- **Error handling**: Gracefully handle storage quota and parsing errors

### Data Transformation
- **Date formatting**: Consistent date display across the application
- **Data validation**: Transform and validate API responses
- **Number formatting**: Currency, percentages, and localized number formats

## Implementation Focus

This architecture is designed for simplicity and maintainability. Focus on:
- Clean separation between layers
- Consistent data flow patterns  
- Centralized utilities for common operations
- Proper use of Ant Design components
- TypeScript for type safety