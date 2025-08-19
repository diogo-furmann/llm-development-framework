# State Management Guide

## State Management Strategy

This application uses a simple, straightforward approach to state management:
1. **Local State** - Component-specific state with useState
2. **Context State** - Shared state across component trees (Auth only)
3. **Server State** - API data with custom hooks
4. **Form State** - Use Ant Design Form components
5. **Persistent State** - Browser storage for user preferences

## Local State Patterns

### Basic State
```typescript
const Component = () => {
  const [count, setCount] = useState(0);
  const [user, setUser] = useState<User | null>(null);
  
  return (/* JSX */);
};
```

### Multiple Related State Variables
```typescript
// When you have related loading/data/error state, group them logically
const Component = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  // Or use a custom hook to encapsulate this pattern
  const { data: users, loading, error, refetch } = useApiData<User[]>('/users');
  
  return (/* JSX */);
};
```

## Context State Management

### Auth Context
```typescript
interface AuthContextType {
  user: User | null;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  loading: boolean;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  const login = async (credentials: LoginCredentials) => {
    setLoading(true);
    try {
      const response = await authService.login(credentials);
      setUser(response.user);
    } catch (error) {
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    authService.logout();
    setUser(null);
  };

  useEffect(() => {
    const loadUser = async () => {
      try {
        const user = await authService.getCurrentUser();
        setUser(user);
      } catch (error) {
        console.error('Failed to load user:', error);
      } finally {
        setLoading(false);
      }
    };
    
    loadUser();
  }, []);

  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

### App Configuration Context (Optional)
```typescript
// Only if you need global app settings beyond auth
interface AppConfigContextType {
  sidebarCollapsed: boolean;
  setSidebarCollapsed: (collapsed: boolean) => void;
}

const AppConfigContext = createContext<AppConfigContextType | undefined>(undefined);

export const AppConfigProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [sidebarCollapsed, setSidebarCollapsed] = usePersistentState('sidebar-collapsed', false);

  return (
    <AppConfigContext.Provider value={{ sidebarCollapsed, setSidebarCollapsed }}>
      {children}
    </AppConfigContext.Provider>
  );
};

export const useAppConfig = () => {
  const context = useContext(AppConfigContext);
  if (!context) {
    throw new Error('useAppConfig must be used within AppConfigProvider');
  }
  return context;
};
```

## Server State Management

### API Data Hook
```typescript
interface UseApiDataOptions {
  enabled?: boolean;
  refetchInterval?: number;
  onError?: (error: Error) => void;
}

export const useApiData = <T>(
  endpoint: string, 
  options: UseApiDataOptions = {}
) => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  const refetch = useCallback(async () => {
    if (!options.enabled) return;
    
    setLoading(true);
    setError(null);
    
    try {
      const response = await apiService.get<T>(endpoint);
      setData(response.data);
    } catch (err) {
      const error = err as Error;
      setError(error);
      options.onError?.(error);
    } finally {
      setLoading(false);
    }
  }, [endpoint, options.enabled]);

  useEffect(() => {
    refetch();
  }, [refetch]);

  useEffect(() => {
    if (options.refetchInterval) {
      const interval = setInterval(refetch, options.refetchInterval);
      return () => clearInterval(interval);
    }
  }, [refetch, options.refetchInterval]);

  return { data, loading, error, refetch };
};
```

### API Operations Hook
```typescript
// Simple hook for create/update/delete operations
export const useApiOperation = <T>(operation: (data: T) => Promise<any>) => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const execute = async (data: T) => {
    setLoading(true);
    setError(null);

    try {
      const result = await operation(data);
      return result;
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : 'An error occurred';
      setError(errorMessage);
      throw err;
    } finally {
      setLoading(false);
    }
  };

  return { execute, loading, error };
};

// Usage example:
const { execute: createUser, loading, error } = useApiOperation(userService.create);
```

## Form State Management

### Use Ant Design Forms (Recommended)
```typescript
// Ant Design handles form state, validation, and submission
import { Form, Input, Button, message } from 'antd';
import { userService } from '../services';

interface UserFormData {
  name: string;
  email: string;
}

const UserForm: React.FC = () => {
  const [form] = Form.useForm<UserFormData>();
  const [loading, setLoading] = useState(false);

  const handleSubmit = async (values: UserFormData) => {
    setLoading(true);
    try {
      await userService.create(values);
      message.success('User created successfully!');
      form.resetFields();
    } catch (error) {
      message.error('Failed to create user');
    } finally {
      setLoading(false);
    }
  };

  return (
    <Form
      form={form}
      layout="vertical"
      onFinish={handleSubmit}
      initialValues={{ name: '', email: '' }}
    >
      <Form.Item
        name="name"
        label="Name"
        rules={[{ required: true, message: 'Please input the name!' }]}
      >
        <Input />
      </Form.Item>
      
      <Form.Item
        name="email"
        label="Email"
        rules={[
          { required: true, message: 'Please input the email!' },
          { type: 'email', message: 'Please enter a valid email!' }
        ]}
      >
        <Input />
      </Form.Item>
      
      <Form.Item>
        <Button type="primary" htmlType="submit" loading={loading}>
          Submit
        </Button>
      </Form.Item>
    </Form>
  );
};
```

### Simple Form Hook (Only if needed)
```typescript
// Use only for very simple forms that don't need Ant Design's features
export const useSimpleForm = <T extends Record<string, any>>(initialValues: T) => {
  const [values, setValues] = useState<T>(initialValues);
  const [isSubmitting, setIsSubmitting] = useState(false);

  const updateField = (field: keyof T, value: any) => {
    setValues(prev => ({ ...prev, [field]: value }));
  };

  const reset = () => setValues(initialValues);

  return { values, updateField, reset, isSubmitting, setIsSubmitting };
};
```

## State Persistence

### Persistent State Hook
```typescript
// Hook for persisting state in localStorage with better naming and error handling
export const usePersistentState = <T>(storageKey: string, defaultValue: T) => {
  const [persistedValue, setPersistedValue] = useState<T>(() => {
    try {
      const storedItem = window.localStorage.getItem(storageKey);
      return storedItem ? JSON.parse(storedItem) : defaultValue;
    } catch (error) {
      console.warn(`Failed to read from localStorage key "${storageKey}":`, error);
      return defaultValue;
    }
  });

  const updatePersistedValue = (newValue: T | ((prevValue: T) => T)) => {
    try {
      const valueToStore = typeof newValue === 'function' 
        ? (newValue as (prevValue: T) => T)(persistedValue)
        : newValue;
      
      setPersistedValue(valueToStore);
      window.localStorage.setItem(storageKey, JSON.stringify(valueToStore));
    } catch (error) {
      console.warn(`Failed to save to localStorage key "${storageKey}":`, error);
    }
  };

  const clearPersistedValue = () => {
    try {
      window.localStorage.removeItem(storageKey);
      setPersistedValue(defaultValue);
    } catch (error) {
      console.warn(`Failed to clear localStorage key "${storageKey}":`, error);
    }
  };

  return [persistedValue, updatePersistedValue, clearPersistedValue] as const;
};

// Usage examples:
const [sidebarCollapsed, setSidebarCollapsed] = usePersistentState('sidebar-collapsed', false);
const [userPreferences, setUserPreferences, clearPreferences] = usePersistentState('user-prefs', { theme: 'light' });
```

## State Guidelines

### Keep State Simple
```typescript
// Good: Simple, flat state
const UserList = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  const [selectedUserId, setSelectedUserId] = useState<string | null>(null);

  // Update specific user
  const updateUser = (userId: string, updates: Partial<User>) => {
    setUsers(prev => 
      prev.map(user => 
        user.id === userId ? { ...user, ...updates } : user
      )
    );
  };
};

// Avoid: Complex nested state
// Instead, use separate useState calls or custom hooks
```

### State Colocation
```typescript
// Keep state close to where it's used
const UserProfile = ({ userId }: { userId: string }) => {
  // This state only affects this component
  const [editing, setEditing] = useState(false);
  const [tempValues, setTempValues] = useState<Partial<User>>({});
  
  // This data might be needed elsewhere - use a custom hook
  const { user, loading, error } = useUser(userId);
  
  return (/* JSX */);
};
```

## Best Practices

### Avoid Unnecessary Re-renders
```typescript
// Split unrelated state
const UserDashboard = () => {
  // These don't need to be in the same state object
  const [user, setUser] = useState<User | null>(null);
  const [sidebarOpen, setSidebarOpen] = useState(false);
  
  return (/* JSX */);
};

// Memoize expensive calculations
const UserList = ({ users }: { users: User[] }) => {
  const sortedActiveUsers = useMemo(() => {
    return users
      .filter(user => user.active)
      .sort((a, b) => a.name.localeCompare(b.name));
  }, [users]);
  
  return (/* JSX */);
};
```

### Use Custom Hooks for Complex Logic
```typescript
// Instead of complex state in components, create custom hooks
export const useUsers = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  const fetchUsers = async () => {
    setLoading(true);
    try {
      const data = await userService.getAll();
      setUsers(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch users');
    } finally {
      setLoading(false);
    }
  };
  
  useEffect(() => {
    fetchUsers();
  }, []);
  
  return { users, loading, error, refetch: fetchUsers };
};
```

## Summary: When to Use Each Pattern

### ✅ **Always Use:**
- **useState** for local component state
- **Ant Design Forms** for all forms (handles state + validation)
- **useApiData** for fetching server data
- **usePersistentState** for saving user preferences

### ✅ **Use When Needed:**
- **Auth Context** for user authentication state
- **App Config Context** for global UI settings (optional)
- **Custom Hooks** to encapsulate complex state logic

### ❌ **Don't Use:**
- **useReducer** - Keep it simple with useState
- **Theme Context** - Use Ant Design's default styling only
- **Complex nested state** - Split into separate useState calls
- **Global state libraries** - Context + useState is sufficient for most apps

### 📝 **Quick Decision Guide:**
- Need to share user auth? → **Auth Context**
- Need a form? → **Ant Design Form**
- Need to save preferences? → **usePersistentState** 
- Need to fetch API data? → **useApiData custom hook**
- Everything else? → **useState in the component**