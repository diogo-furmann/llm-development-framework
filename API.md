# API Documentation

## Base Configuration
```typescript
const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:3001/api';
```

## Authentication
All authenticated requests require a Bearer token in the Authorization header:
```
Authorization: Bearer <token>
```

## Common Response Format
```typescript
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
  message?: string;
}
```

## Error Handling
Standard HTTP status codes:
- `200` - Success
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

## Endpoints

### Authentication
#### POST /auth/login
```typescript
// Request
interface LoginRequest {
  email: string;
  password: string;
}

// Response
interface LoginResponse {
  token: string;
  user: User;
  expiresAt: string;
}
```

#### POST /auth/register
```typescript
// Request
interface RegisterRequest {
  email: string;
  password: string;
  name: string;
}
```

### Users
#### GET /users/profile
Get current user profile (authenticated)

#### PUT /users/profile
Update user profile (authenticated)
```typescript
interface UpdateProfileRequest {
  name?: string;
  email?: string;
}
```

### Data Endpoints
#### GET /data
Fetch application data with optional filtering
```typescript
// Query Parameters
interface DataQuery {
  page?: number;
  limit?: number;
  search?: string;
  category?: string;
}
```

#### POST /data
Create new data entry (authenticated)
```typescript
interface CreateDataRequest {
  title: string;
  content: string;
  category: string;
}
```

#### PUT /data/:id
Update existing data entry (authenticated)

#### DELETE /data/:id
Delete data entry (authenticated)

## API Service Usage
```typescript
import { apiService } from './services/api';

// Example usage
const data = await apiService.get('/data', { params: { page: 1 } });
const newEntry = await apiService.post('/data', { title, content, category });
```