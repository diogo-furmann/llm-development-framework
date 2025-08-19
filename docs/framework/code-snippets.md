# Code Snippets Library - LLM Development Framework

## Overview
Ready-to-use code patterns that follow our architectural guidelines. Copy, adapt, and use these snippets to maintain consistency across implementations.

## 📊 State Management Snippets

### Local Component State
```typescript
// Basic state for component-specific data
const MyComponent: React.FC = () => {
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState<DataType[]>([]);
  const [selectedItem, setSelectedItem] = useState<DataType | null>(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  return (/* JSX */);
};
```

### Auth Context (Copy-paste template)
```typescript
// src/contexts/AuthContext.tsx
import React, { createContext, useContext, useState, useEffect } from 'react';

interface User {
  id: string;
  name: string;
  email: string;
  role: string;
}

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

### App Config Context (For UI preferences)
```typescript
// src/contexts/AppConfigContext.tsx
import React, { createContext, useContext } from 'react';
import { usePersistentState } from '../hooks/usePersistentState';

interface AppConfigContextType {
  sidebarCollapsed: boolean;
  setSidebarCollapsed: (collapsed: boolean) => void;
  theme: 'light' | 'dark';
  setTheme: (theme: 'light' | 'dark') => void;
}

const AppConfigContext = createContext<AppConfigContextType | undefined>(undefined);

export const AppConfigProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [sidebarCollapsed, setSidebarCollapsed] = usePersistentState('sidebar-collapsed', false);
  const [theme, setTheme] = usePersistentState('theme', 'light');

  return (
    <AppConfigContext.Provider value={{ 
      sidebarCollapsed, 
      setSidebarCollapsed, 
      theme, 
      setTheme 
    }}>
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

### Persistent State Hook
```typescript
// src/hooks/usePersistentState.ts
import { useState } from 'react';

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
```

## 🌐 API Integration Snippets

### API Data Fetching Hook (Template)
```typescript
// src/hooks/use{Resource}.ts - Replace {Resource} with actual resource name
import { useState, useEffect, useCallback } from 'react';
import { {resource}Service } from '../services/{resource}Service';

interface UseApiDataOptions {
  enabled?: boolean;
  refetchInterval?: number;
}

export const use{Resource} = (options: UseApiDataOptions = { enabled: true }) => {
  const [data, setData] = useState<{ResourceType}[] | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    if (!options.enabled) return;
    
    setLoading(true);
    setError(null);
    
    try {
      const response = await {resource}Service.getAll();
      setData(response.data);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : 'Failed to fetch {resource}';
      setError(errorMessage);
    } finally {
      setLoading(false);
    }
  }, [options.enabled]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  useEffect(() => {
    if (options.refetchInterval) {
      const interval = setInterval(fetchData, options.refetchInterval);
      return () => clearInterval(interval);
    }
  }, [fetchData, options.refetchInterval]);

  return { data, loading, error, refetch: fetchData };
};

// Usage:
// const { data: users, loading, error, refetch } = useUsers();
```

### API Operations Hook (For mutations)
```typescript
// src/hooks/use{Resource}Operations.ts
import { useState } from 'react';
import { message } from 'antd';
import { {resource}Service } from '../services/{resource}Service';

export const use{Resource}Operations = () => {
  const [loading, setLoading] = useState(false);

  const createItem = async (data: Create{ResourceType}Request) => {
    setLoading(true);
    try {
      const result = await {resource}Service.create(data);
      message.success('{Resource} created successfully!');
      return result;
    } catch (error) {
      message.error('Failed to create {resource}');
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const updateItem = async (id: string, data: Update{ResourceType}Request) => {
    setLoading(true);
    try {
      const result = await {resource}Service.update(id, data);
      message.success('{Resource} updated successfully!');
      return result;
    } catch (error) {
      message.error('Failed to update {resource}');
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const deleteItem = async (id: string) => {
    setLoading(true);
    try {
      await {resource}Service.delete(id);
      message.success('{Resource} deleted successfully!');
    } catch (error) {
      message.error('Failed to delete {resource}');
      throw error;
    } finally {
      setLoading(false);
    }
  };

  return { createItem, updateItem, deleteItem, loading };
};
```

### API Client Template
```typescript
// src/services/apiClient.ts
import axios, { AxiosInstance, AxiosResponse } from 'axios';

// Create configured axios instance
const createApiClient = (): AxiosInstance => {
  const client = axios.create({
    baseURL: process.env.REACT_APP_API_BASE_URL || 'http://localhost:3001/api',
    timeout: 10000,
    headers: {
      'Content-Type': 'application/json',
    },
  });

  // Request interceptor for auth tokens
  client.interceptors.request.use(
    (config) => {
      const token = localStorage.getItem('auth_token');
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }
      return config;
    },
    (error) => Promise.reject(error)
  );

  // Response interceptor for error handling
  client.interceptors.response.use(
    (response: AxiosResponse) => response,
    async (error) => {
      if (error.response?.status === 401) {
        // Handle token expiration
        localStorage.removeItem('auth_token');
        window.location.href = '/login';
      }
      
      // Transform error for consistent handling
      const errorMessage = error.response?.data?.message || error.message || 'An error occurred';
      return Promise.reject(new Error(errorMessage));
    }
  );

  return client;
};

export const apiClient = createApiClient();
```

### Service Layer Template
```typescript
// src/services/{resource}Service.ts
import { apiClient } from './apiClient';

export interface {ResourceType} {
  id: string;
  name: string;
  // ... other properties
}

export interface Create{ResourceType}Request {
  name: string;
  // ... other properties (excluding id)
}

export interface Update{ResourceType}Request {
  name?: string;
  // ... other optional properties
}

const BASE_PATH = '/{resources}';

export const {resource}Service = {
  async getAll(): Promise<{ data: {ResourceType}[] }> {
    const response = await apiClient.get(BASE_PATH);
    return response.data;
  },

  async getById(id: string): Promise<{ data: {ResourceType} }> {
    const response = await apiClient.get(`${BASE_PATH}/${id}`);
    return response.data;
  },

  async create(data: Create{ResourceType}Request): Promise<{ data: {ResourceType} }> {
    const response = await apiClient.post(BASE_PATH, data);
    return response.data;
  },

  async update(id: string, data: Update{ResourceType}Request): Promise<{ data: {ResourceType} }> {
    const response = await apiClient.put(`${BASE_PATH}/${id}`, data);
    return response.data;
  },

  async delete(id: string): Promise<void> {
    await apiClient.delete(`${BASE_PATH}/${id}`);
  },
};
```

## 🎨 Component Snippets

### Basic Form Component (Ant Design)
```typescript
// Basic form using Ant Design
import React, { useState } from 'react';
import { Form, Input, Button, message } from 'antd';

interface FormData {
  name: string;
  email: string;
}

interface MyFormProps {
  initialValues?: Partial<FormData>;
  onSubmit: (values: FormData) => Promise<void>;
  loading?: boolean;
}

export const MyForm: React.FC<MyFormProps> = ({ initialValues, onSubmit, loading = false }) => {
  const [form] = Form.useForm<FormData>();

  const handleSubmit = async (values: FormData) => {
    try {
      await onSubmit(values);
      form.resetFields();
    } catch (error) {
      message.error('Failed to submit form');
    }
  };

  return (
    <Form
      form={form}
      layout="vertical"
      onFinish={handleSubmit}
      initialValues={initialValues}
    >
      <Form.Item
        name="name"
        label="Name"
        rules={[{ required: true, message: 'Please enter a name!' }]}
      >
        <Input />
      </Form.Item>
      
      <Form.Item
        name="email"
        label="Email"
        rules={[
          { required: true, message: 'Please enter an email!' },
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

### Data Table Component (Ant Design)
```typescript
// Table component for displaying data
import React from 'react';
import { Table, Button, Popconfirm, Space } from 'antd';
import { EditOutlined, DeleteOutlined } from '@ant-design/icons';
import type { ColumnsType } from 'antd/es/table';

interface DataItem {
  id: string;
  name: string;
  email: string;
  // ... other properties
}

interface DataTableProps {
  data: DataItem[];
  loading?: boolean;
  onEdit: (record: DataItem) => void;
  onDelete: (id: string) => void;
}

export const DataTable: React.FC<DataTableProps> = ({ 
  data, 
  loading = false, 
  onEdit, 
  onDelete 
}) => {
  const columns: ColumnsType<DataItem> = [
    {
      title: 'Name',
      dataIndex: 'name',
      key: 'name',
      sorter: (a, b) => a.name.localeCompare(b.name),
    },
    {
      title: 'Email',
      dataIndex: 'email',
      key: 'email',
    },
    {
      title: 'Actions',
      key: 'actions',
      render: (_, record) => (
        <Space>
          <Button
            type="link"
            icon={<EditOutlined />}
            onClick={() => onEdit(record)}
          >
            Edit
          </Button>
          <Popconfirm
            title="Are you sure you want to delete this item?"
            onConfirm={() => onDelete(record.id)}
            okText="Yes"
            cancelText="No"
          >
            <Button
              type="link"
              danger
              icon={<DeleteOutlined />}
            >
              Delete
            </Button>
          </Popconfirm>
        </Space>
      ),
    },
  ];

  return (
    <Table
      columns={columns}
      dataSource={data}
      loading={loading}
      rowKey="id"
      pagination={{
        showSizeChanger: true,
        showQuickJumper: true,
        showTotal: (total) => `Total ${total} items`,
      }}
    />
  );
};
```

### Modal Form Component
```typescript
// Modal containing a form
import React from 'react';
import { Modal } from 'antd';
import { MyForm } from './MyForm'; // Your form component

interface ModalFormProps {
  open: boolean;
  onCancel: () => void;
  onSubmit: (values: FormData) => Promise<void>;
  initialValues?: Partial<FormData>;
  title: string;
  loading?: boolean;
}

export const ModalForm: React.FC<ModalFormProps> = ({
  open,
  onCancel,
  onSubmit,
  initialValues,
  title,
  loading = false
}) => {
  const handleSubmit = async (values: FormData) => {
    await onSubmit(values);
    onCancel(); // Close modal on successful submit
  };

  return (
    <Modal
      title={title}
      open={open}
      onCancel={onCancel}
      footer={null} // Form handles its own submit button
      width={600}
    >
      <MyForm
        initialValues={initialValues}
        onSubmit={handleSubmit}
        loading={loading}
      />
    </Modal>
  );
};
```

### Page Component Template
```typescript
// src/pages/{Resource}Page.tsx
import React, { useState } from 'react';
import { Button, Space } from 'antd';
import { PlusOutlined } from '@ant-design/icons';
import { use{Resource} } from '../hooks/use{Resource}';
import { use{Resource}Operations } from '../hooks/use{Resource}Operations';
import { DataTable } from '../components/DataTable';
import { ModalForm } from '../components/ModalForm';

export const {Resource}Page: React.FC = () => {
  const { data, loading, error, refetch } = use{Resource}();
  const { createItem, updateItem, deleteItem, loading: operationLoading } = use{Resource}Operations();
  
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedItem, setSelectedItem] = useState<{ResourceType} | null>(null);

  const handleCreate = () => {
    setSelectedItem(null);
    setIsModalOpen(true);
  };

  const handleEdit = (item: {ResourceType}) => {
    setSelectedItem(item);
    setIsModalOpen(true);
  };

  const handleSubmit = async (values: FormData) => {
    if (selectedItem) {
      await updateItem(selectedItem.id, values);
    } else {
      await createItem(values);
    }
    refetch(); // Refresh the list
  };

  const handleDelete = async (id: string) => {
    await deleteItem(id);
    refetch(); // Refresh the list
  };

  if (error) {
    return <div>Error: {error}</div>;
  }

  return (
    <div>
      <Space style={{ marginBottom: 16 }}>
        <Button 
          type="primary" 
          icon={<PlusOutlined />} 
          onClick={handleCreate}
        >
          Add {Resource}
        </Button>
      </Space>

      <DataTable
        data={data || []}
        loading={loading}
        onEdit={handleEdit}
        onDelete={handleDelete}
      />

      <ModalForm
        open={isModalOpen}
        onCancel={() => setIsModalOpen(false)}
        onSubmit={handleSubmit}
        initialValues={selectedItem || {}}
        title={selectedItem ? 'Edit {Resource}' : 'Create {Resource}'}
        loading={operationLoading}
      />
    </div>
  );
};
```

## 🔧 Error Handling Snippets

### Error Boundary Component
```typescript
// src/components/ErrorBoundary.tsx
import React, { ReactNode, useState, useEffect } from 'react';
import { Result, Button } from 'antd';

interface ErrorBoundaryProps {
  children: ReactNode;
  fallback?: ReactNode;
}

// Note: Currently React doesn't have a hook equivalent for componentDidCatch
// This is a simplified version using react-error-boundary library approach
// Install: npm install react-error-boundary

import { ErrorBoundary as ReactErrorBoundary } from 'react-error-boundary';

const ErrorFallback: React.FC<{ error: Error; resetErrorBoundary: () => void }> = ({ 
  error, 
  resetErrorBoundary 
}) => {
  useEffect(() => {
    console.error('Uncaught error:', error);
  }, [error]);

  return (
    <Result
      status="error"
      title="Something went wrong"
      subTitle="An unexpected error occurred. Please try refreshing the page."
      extra={
        <Button type="primary" onClick={resetErrorBoundary}>
          Try again
        </Button>
      }
    />
  );
};

export const ErrorBoundary: React.FC<ErrorBoundaryProps> = ({ 
  children, 
  fallback 
}) => {
  return (
    <ReactErrorBoundary
      FallbackComponent={fallback ? () => <>{fallback}</> : ErrorFallback}
      onError={(error, errorInfo) => {
        console.error('Error caught by boundary:', error, errorInfo);
      }}
    >
      {children}
    </ReactErrorBoundary>
  );
};

// Alternative: Simple error state management within components
// Use this pattern for component-level error handling instead of boundaries
export const useErrorHandler = () => {
  const [error, setError] = useState<string | null>(null);

  const handleError = (error: unknown) => {
    const errorMessage = error instanceof Error ? error.message : 'An error occurred';
    setError(errorMessage);
    console.error('Error:', error);
  };

  const clearError = () => setError(null);

  return { error, handleError, clearError };
};

// Usage in components:
// const { error, handleError, clearError } = useErrorHandler();
// 
// if (error) {
//   return (
//     <Result
//       status="error"
//       title="Something went wrong"
//       subTitle={error}
//       extra={<Button onClick={clearError}>Try again</Button>}
//     />
//   );
// }
```

### Error Handling in Event Handlers
```typescript
// Error handling pattern for event handlers
const handleButtonClick = async () => {
  try {
    setLoading(true);
    await someApiCall();
    message.success('Operation completed successfully!');
  } catch (error) {
    console.error('Operation failed:', error);
    message.error('Operation failed. Please try again.');
  } finally {
    setLoading(false);
  }
};
```

## 🚦 Loading States Snippets

### Button Loading States
```typescript
// Button with loading state
const [isSubmitting, setIsSubmitting] = useState(false);

<Button 
  type="primary" 
  loading={isSubmitting}
  onClick={handleSubmit}
>
  {isSubmitting ? 'Submitting...' : 'Submit'}
</Button>
```

### Page Loading States
```typescript
// Full page loading
import { Spin } from 'antd';

if (loading) {
  return (
    <div style={{ textAlign: 'center', padding: '50px' }}>
      <Spin size="large" tip="Loading..." />
    </div>
  );
}
```

## 🛠️ Utility Snippets

### LocalStorage Helpers
```typescript
// src/utils/localStorage.ts
export const localStorage = {
  // Get item with type safety and error handling
  get<T>(key: string, defaultValue: T): T {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : defaultValue;
    } catch (error) {
      console.warn(`Failed to read from localStorage key "${key}":`, error);
      return defaultValue;
    }
  },

  // Set item with error handling
  set<T>(key: string, value: T): boolean {
    try {
      window.localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch (error) {
      console.warn(`Failed to save to localStorage key "${key}":`, error);
      return false;
    }
  },

  // Remove item with error handling
  remove(key: string): boolean {
    try {
      window.localStorage.removeItem(key);
      return true;
    } catch (error) {
      console.warn(`Failed to remove localStorage key "${key}":`, error);
      return false;
    }
  },

  // Clear all localStorage
  clear(): boolean {
    try {
      window.localStorage.clear();
      return true;
    } catch (error) {
      console.warn('Failed to clear localStorage:', error);
      return false;
    }
  },

  // Check if key exists
  has(key: string): boolean {
    return window.localStorage.getItem(key) !== null;
  },

  // Get all keys
  keys(): string[] {
    return Object.keys(window.localStorage);
  }
};

// Usage examples:
// const user = localStorage.get('user', null);
// localStorage.set('preferences', { theme: 'dark' });
// localStorage.remove('temp_data');
```

### Date Formatting Utilities (Using Day.js - pt-BR)
```typescript
// src/utils/formatting.ts
import dayjs from 'dayjs';
import relativeTime from 'dayjs/plugin/relativeTime';
import utc from 'dayjs/plugin/utc';
import timezone from 'dayjs/plugin/timezone';
import 'dayjs/locale/pt-br';

// Configure dayjs with Brazilian Portuguese locale and São Paulo timezone
dayjs.extend(relativeTime);
dayjs.extend(utc);
dayjs.extend(timezone);
dayjs.locale('pt-br');
dayjs.tz.setDefault('America/Sao_Paulo');

export const formatDate = (date: Date | string | dayjs.Dayjs, format: string = 'DD/MM/YYYY'): string => {
  const dayjsObj = dayjs(date).tz('America/Sao_Paulo');
  
  if (!dayjsObj.isValid()) {
    return 'Data Inválida';
  }

  return dayjsObj.format(format);
};

// Simplified date formatting presets for Brazilian market
export const dateFormats = {
  // Primary formats for Brazilian market
  date: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').format('DD/MM/YYYY'),
  time: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').format('HH:mm'),
  dateTime: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').format('DD/MM/YYYY HH:mm'),
  
  // Relative time in Portuguese
  relative: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').fromNow(),
  
  // Calendar view in Portuguese
  calendar: (date: Date | string | dayjs.Dayjs) => {
    const target = dayjs(date).tz('America/Sao_Paulo');
    const now = dayjs().tz('America/Sao_Paulo');
    
    if (target.isSame(now, 'day')) return 'Hoje';
    if (target.isSame(now.subtract(1, 'day'), 'day')) return 'Ontem';
    if (target.isSame(now.add(1, 'day'), 'day')) return 'Amanhã';
    
    return target.format('DD/MM/YYYY');
  },
  
  // API formats (keep ISO standard for backend)
  apiDate: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').format('YYYY-MM-DD'),
  apiDateTime: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').format('YYYY-MM-DD HH:mm:ss'),
  iso: (date: Date | string | dayjs.Dayjs) => dayjs(date).tz('America/Sao_Paulo').toISOString()
};

// Number formatting utilities
export const formatNumber = (num: number, options: Intl.NumberFormatOptions = {}): string => {
  return new Intl.NumberFormat('en-US', options).format(num);
};

export const numberFormats = {
  currency: (amount: number, currency = 'USD') => 
    formatNumber(amount, { style: 'currency', currency }),
  
  percentage: (value: number, decimals = 1) => 
    formatNumber(value / 100, { style: 'percent', minimumFractionDigits: decimals }),
  
  decimal: (num: number, decimals = 2) => 
    formatNumber(num, { minimumFractionDigits: decimals, maximumFractionDigits: decimals }),
  
  compact: (num: number) => 
    formatNumber(num, { notation: 'compact', compactDisplay: 'short' })
};

// Usage examples with dayjs (pt-BR):
// formatDate(new Date()) // "01/12/2023" (DD/MM/YYYY)
// dateFormats.date(new Date()) // "01/12/2023"
// dateFormats.time(new Date()) // "14:30"
// dateFormats.dateTime(new Date()) // "01/12/2023 14:30"
// dateFormats.relative(dayjs().subtract(1, 'hour')) // "há uma hora"
// dateFormats.calendar(dayjs().subtract(1, 'day')) // "Ontem"
// dateFormats.apiDate(new Date()) // "2023-12-01" (for API calls)
// numberFormats.currency(1234.56) // "$1,234.56"
// numberFormats.percentage(0.1234) // "12.3%"
```

### Updated Persistent State Hook (Using localStorage helpers)
```typescript
// src/hooks/usePersistentState.ts
import { useState } from 'react';
import { localStorage } from '../utils/localStorage';

export const usePersistentState = <T>(storageKey: string, defaultValue: T) => {
  const [persistedValue, setPersistedValue] = useState<T>(() => {
    return localStorage.get(storageKey, defaultValue);
  });

  const updatePersistedValue = (newValue: T | ((prevValue: T) => T)) => {
    const valueToStore = typeof newValue === 'function' 
      ? (newValue as (prevValue: T) => T)(persistedValue)
      : newValue;
    
    setPersistedValue(valueToStore);
    localStorage.set(storageKey, valueToStore);
  };

  const clearPersistedValue = () => {
    localStorage.remove(storageKey);
    setPersistedValue(defaultValue);
  };

  return [persistedValue, updatePersistedValue, clearPersistedValue] as const;
};
```

## 🎯 Quick Copy-Paste Checklist

When implementing a new feature:

1. **✅ Copy service template** - Replace {Resource} with your resource name
2. **✅ Copy data fetching hook** - Replace {Resource} with your resource name  
3. **✅ Copy operations hook** - Replace {Resource} with your resource name
4. **✅ Copy form component** - Adapt fields to your data structure
5. **✅ Copy table component** - Adapt columns to your data structure
6. **✅ Copy page component** - Wire everything together
7. **✅ Add error boundary** - Wrap your page component
8. **✅ Test all operations** - Create, read, update, delete

### Replacement Patterns
- `{Resource}` → Capitalized resource name (e.g., "User", "Product")
- `{resource}` → Lowercase resource name (e.g., "user", "product") 
- `{resources}` → Plural lowercase (e.g., "users", "products")
- `{ResourceType}` → TypeScript interface name
- `FormData` → Your specific form data interface name