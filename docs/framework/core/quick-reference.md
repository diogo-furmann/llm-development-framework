# Quick Reference - Essential Patterns

## 🚀 Required Dependencies
```json
{
  "dependencies": {
    "antd": "^5.x.x",          // UI components (MANDATORY)
    "axios": "^1.x.x",         // HTTP client (MANDATORY)  
    "dayjs": "^1.x.x",         // Date lib, pt-BR (MANDATORY)
    "react": "^18.x.x",
    "react-router-dom": "^6.x.x",
    "react-error-boundary": "^4.x.x"
  }
}
```

## 🏗️ Architecture Layers
```
Component → Hook → Service → API
```

**Layer Responsibilities:**
- **Component**: UI rendering, user interaction (Ant Design only)
- **Hook**: State management, side effects
- **Service**: Business logic, data transformation  
- **API**: External communication (Axios only)

## 📁 File Structure Patterns
```
src/
├── components/       # Reusable UI (PascalCase)
├── pages/           # Route components (PascalCase)
├── hooks/           # Custom hooks (camelCase, use*)
├── services/        # Business logic (camelCase)
├── utils/           # Helpers (camelCase)
└── types/           # TypeScript definitions (camelCase)
```

## 🎨 Component Patterns

### Basic Component
```typescript
import { Button } from 'antd'

interface UserCardProps {
  user: User
  onEdit: (id: string) => void
}

const UserCard: React.FC<UserCardProps> = ({ user, onEdit }) => {
  return (
    <div>
      <h3>{user.name}</h3>
      <Button type="primary" onClick={() => onEdit(user.id)}>
        Edit
      </Button>
    </div>
  )
}

export default UserCard
```

### Form Component
```typescript
import { Form, Input, Button } from 'antd'

interface UserFormProps {
  onSubmit: (values: UserFormData) => void
  loading?: boolean
}

const UserForm: React.FC<UserFormProps> = ({ onSubmit, loading }) => {
  const [form] = Form.useForm()

  return (
    <Form form={form} onFinish={onSubmit} layout="vertical">
      <Form.Item 
        name="name" 
        label="Name" 
        rules={[{ required: true, message: 'Name is required' }]}
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

## 🔗 State Management Patterns

### Local State
```typescript
const Component = () => {
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)
  const [data, setData] = useState<User[]>([])
  
  return (/* JSX */)
}
```

### Custom Hook Pattern
```typescript
const useUsers = () => {
  const [users, setUsers] = useState<User[]>([])
  const [loading, setLoading] = useState(false)
  
  const fetchUsers = async () => {
    setLoading(true)
    try {
      const data = await userService.getAll()
      setUsers(data)
    } catch (error) {
      console.error('Failed to fetch users:', error)
    } finally {
      setLoading(false)
    }
  }
  
  useEffect(() => { fetchUsers() }, [])
  
  return { users, loading, refetch: fetchUsers }
}
```

## 🌐 Service Patterns

### API Service
```typescript
import apiClient from './apiClient'

interface User {
  id: string
  name: string
  email: string
}

const userService = {
  getAll: (): Promise<User[]> => 
    apiClient.get('/users').then(res => res.data),
    
  getById: (id: string): Promise<User> =>
    apiClient.get(`/users/${id}`).then(res => res.data),
    
  create: (user: Omit<User, 'id'>): Promise<User> =>
    apiClient.post('/users', user).then(res => res.data),
    
  update: (id: string, user: Partial<User>): Promise<User> =>
    apiClient.put(`/users/${id}`, user).then(res => res.data),
    
  delete: (id: string): Promise<void> =>
    apiClient.delete(`/users/${id}`)
}

export default userService
```

### API Client (axiOS)
```typescript
import axios from 'axios'

const apiClient = axios.create({
  baseURL: process.env.REACT_APP_API_URL || 'http://localhost:3001',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
})

// Request interceptor
apiClient.interceptors.request.use(config => {
  const token = localStorage.getItem('authToken')
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

export default apiClient
```

## 📅 Date Handling (Day.js)
```typescript
import dayjs from 'dayjs'
import 'dayjs/locale/pt-br'

dayjs.locale('pt-br')

// Format dates consistently
const formatDate = (date: string | Date) => dayjs(date).format('DD/MM/YYYY')
const formatDateTime = (date: string | Date) => dayjs(date).format('DD/MM/YYYY HH:mm')
```

## 📝 TypeScript Patterns

### Interface Definitions
```typescript
interface User {
  id: string
  name: string
  email: string
  createdAt: string
}

type UserStatus = 'active' | 'inactive' | 'pending'
type CreateUserRequest = Omit<User, 'id' | 'createdAt'>
```

### Component Props
```typescript
interface ComponentProps {
  data: User[]
  loading?: boolean
  onEdit: (user: User) => void
  onDelete: (id: string) => void
}
```

## 🚦 Error Handling
```typescript
// In components
try {
  await userService.create(formData)
  message.success('User created successfully')
} catch (error) {
  message.error('Failed to create user')
  console.error(error)
}

// In services  
const handleApiError = (error: any) => {
  if (error.response?.status === 401) {
    // Handle unauthorized
  }
  throw error
}
```

## 🎯 Naming Conventions
- **Files**: PascalCase components, camelCase others
- **Functions**: camelCase (`handleSubmit`, `fetchData`)
- **Constants**: SCREAMING_SNAKE_CASE (`API_BASE_URL`)
- **Interfaces**: PascalCase (`User`, `ApiResponse`)
- **Hooks**: camelCase with "use" prefix (`useUsers`, `useAuth`)

## ⚡ Implementation Checklist
- [ ] Use Ant Design components only
- [ ] Implement layer by layer (Data → Logic → UI)
- [ ] Add proper TypeScript interfaces
- [ ] Use Day.js for dates (pt-BR, DD/MM/YYYY)
- [ ] Handle loading/error states
- [ ] Add form validation with Ant Design
- [ ] Use Axios via apiClient service
- [ ] Create High Level Documentation under docs/project/