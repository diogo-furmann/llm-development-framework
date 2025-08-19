# UserForm Component (Example)

## Purpose
Handles user creation and editing with form validation, loading states, and error handling using Ant Design Form component.

## Props Interface
```typescript
interface UserFormProps {
  initialValues?: Partial<User>;
  onSubmit: (values: UserFormData) => Promise<void>;
  loading?: boolean;
}

interface UserFormData {
  name: string;
  email: string;
  role: 'admin' | 'user';
  isActive: boolean;
}
```

## Ant Design Components Used
- **Form** (layout: 'vertical', validation rules)
- **Input** (text inputs for name and email)
- **Select** (role selection with options)
- **Switch** (active/inactive toggle)
- **Button** (submit button with loading state)
- **message** (success/error notifications)

## State Management
- **Local state**: Form instance managed by Ant Design Form.useForm()
- **Props received**: initialValues, onSubmit callback, loading state
- **No context consumed**: Self-contained component

## Usage
```typescript
// Create new user
<UserForm 
  onSubmit={createUser}
  loading={isCreating}
/>

// Edit existing user
<UserForm 
  initialValues={selectedUser}
  onSubmit={updateUser}
  loading={isUpdating}
/>
```

## Implementation Notes
- Uses Ant Design Form's built-in validation
- Automatically resets form on successful submission
- Displays success/error messages using Ant Design message
- Email validation using Ant Design's built-in email rule
- Submits data in format expected by userService
- Loading state disables form during submission