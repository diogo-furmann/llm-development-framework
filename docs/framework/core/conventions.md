# Code Conventions

## Naming Conventions

### Files and Directories
- **Components**: PascalCase (`Button.tsx`, `DataTable.tsx`)
- **Hooks**: camelCase starting with "use" (`useAuth.ts`, `useApiData.ts`)
- **Services**: camelCase (`authService.ts`, `apiClient.ts`)
- **Utils**: camelCase (`formatDate.ts`, `validation.ts`)
- **Types**: camelCase (`user.ts`, `apiTypes.ts`)

### Variables and Functions
- **Variables**: camelCase (`userData`, `isLoading`)
- **Functions**: camelCase (`handleSubmit`, `fetchUserData`)
- **Constants**: SCREAMING_SNAKE_CASE (`API_BASE_URL`, `DEFAULT_TIMEOUT`)
- **Interfaces**: PascalCase (`UserData`, `ApiResponse`)
- **Enums**: PascalCase (`UserRole`, `ApiStatus`)

### Component Conventions
```typescript
// Component file structure
interface ComponentProps {
  // Props interface named after component + Props
}

const Component: React.FC<ComponentProps> = ({ prop1, prop2 }) => {
  // Component implementation
};

export default Component;
```

## TypeScript Conventions

### Interface Definitions
```typescript
// Use interfaces for object shapes
interface User {
  id: string;
  name: string;
  email: string;
  role: UserRole;
}

// Use type for unions and computed types
type Status = 'loading' | 'success' | 'error';
type UserWithStatus = User & { status: Status };
```

### Generic Types
```typescript
// Use descriptive generic names
interface ApiResponse<TData> {
  data: TData;
  success: boolean;
}

// Use T only for simple cases
function identity<T>(arg: T): T {
  return arg;
}
```

### Function Signatures
```typescript
// Prefer arrow functions for consistency
const handleSubmit = (data: FormData): Promise<void> => {
  // Implementation
};

// Use function declarations for hoisted utilities
function formatDate(date: Date | string | dayjs.Dayjs): string {
  // Use dayjs for date formatting in pt-BR format - see code-snippets.md
  return dayjs(date).tz('America/Sao_Paulo').format('DD/MM/YYYY');
}
```

## React Conventions

### Component Structure
```typescript
// 1. Imports (external libraries first, then internal)
import React, { useState, useEffect } from 'react';
import { Button, Input } from '../components';
import { useAuth } from '../hooks';
import './Component.css'; // Optional custom styles

// 2. Types and interfaces
interface Props {
  title: string;
  onSubmit: (data: FormData) => void;
}

// 3. Component definition
const Component: React.FC<Props> = ({ title, onSubmit }) => {
  // 4. Hooks (state, effects, custom hooks)
  const [data, setData] = useState<FormData>({});
  const { user } = useAuth();

  // 5. Event handlers
  const handleInputChange = (field: string, value: string) => {
    setData(prev => ({ ...prev, [field]: value }));
  };

  // 6. Effects
  useEffect(() => {
    // Side effects
  }, []);

  // 7. Render
  return (
    <div className={styles.container}>
      {/* JSX */}
    </div>
  );
};

export default Component;
```

### Hook Usage
- Always use hooks at the top level
- Custom hooks must start with "use"
- Prefer multiple useState calls over single complex state
- Use useCallback for expensive function creation
- Use useMemo for expensive calculations

### Event Handlers
```typescript
// Prefix with "handle"
const handleClick = () => {};
const handleSubmit = (e: FormEvent) => {};
const handleInputChange = (value: string) => {};
```

## Styling Conventions

### Ant Design Usage
```typescript
// Import specific components
import { Button, Input, Form, Card } from 'antd';

// Use Ant Design's built-in props and styling
<Button type="primary" size="large" loading={isLoading}>
  Submit
</Button>

// Custom styling through className when needed
<Card className="custom-card-style">
  Content
</Card>
```

### Theme Configuration
```typescript
// Configure theme through ConfigProvider
import { ConfigProvider } from 'antd';
import { antdTheme } from '../styles/antd-theme';

<ConfigProvider theme={antdTheme}>
  <App />
</ConfigProvider>
```

## Import/Export Conventions

### Barrel Exports
```typescript
// components/index.ts
export { default as Button } from './Button';
export { default as Input } from './Input';
export { default as Modal } from './Modal';
```

### Import Order
1. React and external libraries
2. Internal hooks and services
3. Components (from most general to most specific)
4. Types and interfaces
5. Styles

```typescript
import React, { useState } from 'react';
import axios from 'axios';

import { useAuth } from '../hooks';
import { apiService } from '../services';

import { Button, Input } from '../components';
import { Header } from './Header';

import { User, ApiResponse } from '../types';

import './Component.css'; // Optional custom styles
```

## Error Handling Conventions

### Try-Catch Blocks
```typescript
const fetchData = async (): Promise<Data | null> => {
  try {
    const response = await apiService.get('/data');
    return response.data;
  } catch (error) {
    console.error('Failed to fetch data:', error);
    return null;
  }
};
```

### Error Types
```typescript
interface ApiError {
  message: string;
  code: string;
  status: number;
}
```


## Git Conventions

### Commit Messages
- Format: `type(scope): description`
- Types: feat, fix, docs, style, refactor, chore
- Examples:
  - `feat(auth): add user login functionality`
  - `fix(button): resolve click event propagation`
  - `docs(api): update endpoint documentation`