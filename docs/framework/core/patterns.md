# Implementation Patterns

## 🎨 Component Patterns

### Basic Component Structure
```typescript
import { Button, Card } from 'antd'

interface ComponentProps {
  data: DataType
  onAction: (id: string) => void
  loading?: boolean
}

const Component: React.FC<ComponentProps> = ({ data, onAction, loading }) => {
  return (
    <Card title={data.title}>
      <p>{data.description}</p>
      <Button type="primary" loading={loading} onClick={() => onAction(data.id)}>
        Action
      </Button>
    </Card>
  )
}

export default Component
```

### Form Component Pattern
```typescript
import { Form, Input, Button, message } from 'antd'

interface FormData {
  name: string
  email: string
}

interface FormProps {
  onSubmit: (values: FormData) => Promise<void>
  initialValues?: Partial<FormData>
  loading?: boolean
}

const DataForm: React.FC<FormProps> = ({ onSubmit, initialValues, loading }) => {
  const [form] = Form.useForm()

  const handleSubmit = async (values: FormData) => {
    try {
      await onSubmit(values)
      message.success('Data saved successfully')
      form.resetFields()
    } catch (error) {
      message.error('Failed to save data')
    }
  }

  return (
    <Form
      form={form}
      layout="vertical"
      initialValues={initialValues}
      onFinish={handleSubmit}
    >
      <Form.Item
        name="name"
        label="Name"
        rules={[{ required: true, message: 'Name is required' }]}
      >
        <Input />
      </Form.Item>
      
      <Form.Item
        name="email"
        label="Email"
        rules={[
          { required: true, message: 'Email is required' },
          { type: 'email', message: 'Invalid email format' }
        ]}
      >
        <Input />
      </Form.Item>
      
      <Form.Item>
        <Button type="primary" htmlType="submit" loading={loading}>
          Save
        </Button>
      </Form.Item>
    </Form>
  )
}
```

### Table Component Pattern
```typescript
import { Table, Button, Space, Popconfirm } from 'antd'
import type { ColumnsType } from 'antd/es/table'

interface TableData {
  id: string
  name: string
  email: string
  createdAt: string
}

interface DataTableProps {
  data: TableData[]
  loading: boolean
  onEdit: (record: TableData) => void
  onDelete: (id: string) => void
}

const DataTable: React.FC<DataTableProps> = ({ data, loading, onEdit, onDelete }) => {
  const columns: ColumnsType<TableData> = [
    {
      title: 'Name',
      dataIndex: 'name',
      key: 'name',
    },
    {
      title: 'Email',
      dataIndex: 'email',
      key: 'email',
    },
    {
      title: 'Created',
      dataIndex: 'createdAt',
      key: 'createdAt',
      render: (date) => dayjs(date).format('DD/MM/YYYY'),
    },
    {
      title: 'Actions',
      key: 'actions',
      render: (_, record) => (
        <Space>
          <Button type="link" onClick={() => onEdit(record)}>
            Edit
          </Button>
          <Popconfirm
            title="Are you sure?"
            onConfirm={() => onDelete(record.id)}
          >
            <Button type="link" danger>
              Delete
            </Button>
          </Popconfirm>
        </Space>
      ),
    },
  ]

  return (
    <Table
      columns={columns}
      dataSource={data}
      loading={loading}
      rowKey="id"
      pagination={{ pageSize: 10 }}
    />
  )
}
```

## 🔗 State Management Patterns

### Local State Pattern
```typescript
const Component = () => {
  const [data, setData] = useState<DataType[]>([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)
  
  return (/* JSX */)
}
```

### Custom Hook Pattern
```typescript
interface UseDataReturn {
  data: DataType[]
  loading: boolean
  error: string | null
  refetch: () => void
  create: (item: CreateDataRequest) => Promise<void>
  update: (id: string, item: UpdateDataRequest) => Promise<void>
  delete: (id: string) => Promise<void>
}

const useData = (): UseDataReturn => {
  const [data, setData] = useState<DataType[]>([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)

  const fetchData = async () => {
    setLoading(true)
    setError(null)
    try {
      const result = await dataService.getAll()
      setData(result)
    } catch (err) {
      setError('Failed to fetch data')
      console.error(err)
    } finally {
      setLoading(false)
    }
  }

  const create = async (item: CreateDataRequest) => {
    const newItem = await dataService.create(item)
    setData(prev => [...prev, newItem])
  }

  const update = async (id: string, item: UpdateDataRequest) => {
    const updatedItem = await dataService.update(id, item)
    setData(prev => prev.map(x => x.id === id ? updatedItem : x))
  }

  const deleteItem = async (id: string) => {
    await dataService.delete(id)
    setData(prev => prev.filter(x => x.id !== id))
  }

  useEffect(() => { fetchData() }, [])

  return {
    data,
    loading,
    error,
    refetch: fetchData,
    create,
    update,
    delete: deleteItem
  }
}
```

### Context Pattern (Auth Only)
```typescript
interface AuthContextType {
  user: User | null
  login: (credentials: LoginCredentials) => Promise<void>
  logout: () => void
  loading: boolean
}

const AuthContext = createContext<AuthContextType | undefined>(undefined)

const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null)
  const [loading, setLoading] = useState(false)

  const login = async (credentials: LoginCredentials) => {
    setLoading(true)
    try {
      const response = await authService.login(credentials)
      setUser(response.user)
      localStorage.setItem('authToken', response.token)
    } finally {
      setLoading(false)
    }
  }

  const logout = () => {
    setUser(null)
    localStorage.removeItem('authToken')
  }

  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  )
}

const useAuth = () => {
  const context = useContext(AuthContext)
  if (!context) throw new Error('useAuth must be used within AuthProvider')
  return context
}
```

## 🌐 Service Patterns

### API Service Pattern
```typescript
import apiClient from './apiClient'

interface DataType {
  id: string
  name: string
  email: string
  createdAt: string
}

type CreateDataRequest = Omit<DataType, 'id' | 'createdAt'>
type UpdateDataRequest = Partial<Omit<DataType, 'id' | 'createdAt'>>

const dataService = {
  getAll: (): Promise<DataType[]> =>
    apiClient.get('/data').then(res => res.data),

  getById: (id: string): Promise<DataType> =>
    apiClient.get(`/data/${id}`).then(res => res.data),

  create: (data: CreateDataRequest): Promise<DataType> =>
    apiClient.post('/data', data).then(res => res.data),

  update: (id: string, data: UpdateDataRequest): Promise<DataType> =>
    apiClient.put(`/data/${id}`, data).then(res => res.data),

  delete: (id: string): Promise<void> =>
    apiClient.delete(`/data/${id}`)
}

export default dataService
```

### API Client Pattern (Axios)
```typescript
import axios from 'axios'

const apiClient = axios.create({
  baseURL: process.env.REACT_APP_API_URL || 'http://localhost:3001/api',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// Request interceptor for auth
apiClient.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('authToken')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => Promise.reject(error)
)

// Response interceptor for error handling
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('authToken')
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)

export default apiClient
```

## 📅 Date Handling Pattern
```typescript
import dayjs from 'dayjs'
import 'dayjs/locale/pt-br'

dayjs.locale('pt-br')

// Utility functions
export const formatDate = (date: string | Date) => dayjs(date).format('DD/MM/YYYY')
export const formatDateTime = (date: string | Date) => dayjs(date).format('DD/MM/YYYY HH:mm')
export const formatTimeAgo = (date: string | Date) => dayjs(date).fromNow()

// Usage in components
const dateDisplay = formatDate(user.createdAt) // "25/12/2023"
```

## 🎯 Error Handling Patterns

### Component Error Handling
```typescript
const Component = () => {
  const handleAction = async () => {
    try {
      await someApiCall()
      message.success('Operation completed successfully')
    } catch (error) {
      message.error('Operation failed')
      console.error('Error:', error)
    }
  }
  
  return (/* JSX */)
}
```

### Service Error Handling
```typescript
const dataService = {
  getAll: async (): Promise<DataType[]> => {
    try {
      const response = await apiClient.get('/data')
      return response.data
    } catch (error) {
      console.error('Failed to fetch data:', error)
      throw error
    }
  }
}
```

## 🚦 Loading States Pattern
```typescript
const Component = () => {
  const [loading, setLoading] = useState(false)
  
  const handleAction = async () => {
    setLoading(true)
    try {
      await someOperation()
    } finally {
      setLoading(false)
    }
  }
  
  return (
    <Button type="primary" loading={loading} onClick={handleAction}>
      Action
    </Button>
  )
}
```

## 📝 TypeScript Interface Patterns

### Data Interfaces
```typescript
interface User {
  id: string
  name: string
  email: string
  role: UserRole
  createdAt: string
  updatedAt: string
}

type UserRole = 'admin' | 'user' | 'moderator'
type CreateUserRequest = Omit<User, 'id' | 'createdAt' | 'updatedAt'>
type UpdateUserRequest = Partial<Omit<User, 'id' | 'createdAt' | 'updatedAt'>>

interface ApiResponse<T> {
  data: T
  success: boolean
  message?: string
}
```

### Component Props Interfaces
```typescript
interface ComponentProps {
  data: DataType[]
  loading?: boolean
  error?: string | null
  onEdit: (item: DataType) => void
  onDelete: (id: string) => void
  className?: string
  children?: React.ReactNode
}
```

## 🎨 Ant Design Component Usage

### Always Use These Components
- **Layout**: Layout, Header, Content, Sider, Footer
- **Forms**: Form, Input, Button, Select, DatePicker, Checkbox
- **Display**: Table, Card, List, Tag, Badge
- **Feedback**: Spin, Alert, message, notification, Modal
- **Navigation**: Menu, Breadcrumb, Pagination
- **Grid**: Row, Col, Space

### Default Styling Only
- No custom CSS classes
- Use Ant Design's default theme and colors
- Use built-in props for styling (size, type, variant)
- Leverage Space component for consistent spacing