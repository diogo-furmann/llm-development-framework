# User CRUD Implementation (Example)

## Summary
Complete user management system with create, read, update, delete operations using Ant Design components and custom hooks for API integration.

## Architecture Mapping
- **Components**: UserList, UserForm, UserDetails
- **Hooks**: useUsers, useUserOperations  
- **Services**: userService
- **State**: Local state + custom hooks for server state

## Data Flow
```
User Action → UserForm → useUserOperations → userService → API → useUsers refetch → UserList update
```

## Key Files Created
- `src/components/User/UserList.tsx` - Table displaying users with actions
- `src/components/User/UserForm.tsx` - Create/edit form with validation
- `src/components/User/UserDetails.tsx` - View user information
- `src/hooks/useUsers.ts` - Fetch and cache user data
- `src/hooks/useUserOperations.ts` - Create/update/delete operations
- `src/services/userService.ts` - API calls to /users endpoints
- `src/pages/UsersPage.tsx` - Main page combining all components

## Implementation Details

### Components
- **UserList**: Ant Design Table with edit/delete actions, pagination support
- **UserForm**: Ant Design Form with validation rules, loading states during submission
- **UserDetails**: Ant Design Descriptions component for read-only view

### State Management  
- Local state: Form data, modal visibility, selected user
- Server state: useUsers hook manages user list with loading/error states
- Operations: useUserOperations handles CRUD with optimistic updates

### API Integration
- Endpoints used: 
  - `GET /api/users` - Fetch user list
  - `POST /api/users` - Create new user
  - `PUT /api/users/:id` - Update user
  - `DELETE /api/users/:id` - Delete user
- Error handling: Toast notifications via Ant Design message component
- Loading states: Button loading, table loading, form submission states

## Usage Example
```typescript
// In UsersPage.tsx
const UsersPage: React.FC = () => {
  const { data: users, loading, error } = useUsers();
  const { createUser, updateUser, deleteUser } = useUserOperations();
  const [selectedUser, setSelectedUser] = useState<User | null>(null);
  const [isFormVisible, setIsFormVisible] = useState(false);

  return (
    <div>
      <Button type="primary" onClick={() => setIsFormVisible(true)}>
        Add User
      </Button>
      
      <UserList 
        users={users || []} 
        loading={loading} 
        onEdit={(user) => {
          setSelectedUser(user);
          setIsFormVisible(true);
        }}
        onDelete={deleteUser}
      />
      
      <Modal open={isFormVisible} onCancel={() => setIsFormVisible(false)}>
        <UserForm 
          initialValues={selectedUser}
          onSubmit={selectedUser ? updateUser : createUser}
          loading={false}
        />
      </Modal>
    </div>
  );
};
```

## Notes
- Follows architectural layers: Component → Hook → Service → API
- Uses Ant Design defaults for all styling and components
- Implements optimistic updates for better user experience
- Form validation handled by Ant Design Form rules
- Error states displayed using Ant Design message notifications
- Responsive table layout using Ant Design Table component