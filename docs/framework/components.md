# Component Library Documentation

## Component Structure
All components follow this structure:
```
ComponentName/
├── index.ts          # Barrel export
├── ComponentName.tsx # Main component (uses Ant Design components)
└── ComponentName.stories.tsx # Storybook stories (optional)
```

## Base Components

### Button
Uses Ant Design Button component with customized styling.

```typescript
import { Button as AntButton } from 'antd';

interface ButtonProps {
  type?: 'primary' | 'default' | 'dashed' | 'link' | 'text';
  size?: 'small' | 'middle' | 'large';
  disabled?: boolean;
  loading?: boolean;
  onClick?: () => void;
  children: React.ReactNode;
}

// Usage
<AntButton type="primary" size="middle" onClick={handleClick} loading={loading}>
  Click me
</AntButton>
```

### Input
Uses Ant Design Input component with Form.Item for validation.

```typescript
import { Input as AntInput, Form } from 'antd';

interface InputProps {
  type?: 'text' | 'email' | 'password' | 'number';
  placeholder?: string;
  value?: string;
  onChange?: (e: React.ChangeEvent<HTMLInputElement>) => void;
  disabled?: boolean;
}

// Usage with Form.Item for validation
<Form.Item
  label="Email"
  name="email"
  rules={[{ required: true, type: 'email' }]}
>
  <AntInput placeholder="Enter your email" />
</Form.Item>
```

### Modal
Uses Ant Design Modal component with built-in functionality.

```typescript
import { Modal as AntModal } from 'antd';

interface ModalProps {
  open: boolean;
  onCancel: () => void;
  title?: string;
  children: React.ReactNode;
  width?: number;
}

// Usage
<AntModal 
  open={isModalOpen} 
  onCancel={() => setIsModalOpen(false)} 
  title="Settings"
  width={600}
>
  <ModalContent />
</AntModal>
```

### Card
Uses Ant Design Card component for content grouping.

```typescript
import { Card as AntCard } from 'antd';

interface CardProps {
  title?: string;
  extra?: React.ReactNode;
  children: React.ReactNode;
  className?: string;
  bordered?: boolean;
}

// Usage
<AntCard title="Card Title" extra={<a href="#">More</a>} bordered={false}>
  Card content
</AntCard>
```

## Form Components

### Form
Uses Ant Design Form component with built-in validation and layout.

```typescript
import { Form, Input, Button } from 'antd';

interface FormData {
  username: string;
  email: string;
}

// Usage
<Form<FormData> layout="vertical" onFinish={onFinish}>
  <Form.Item
    label="Username"
    name="username"
    rules={[{ required: true, message: 'Please input your username!' }]}
  >
    <Input />
  </Form.Item>
  <Form.Item>
    <Button type="primary" htmlType="submit">
      Submit
    </Button>
  </Form.Item>
</Form>
```

### Select
Uses Ant Design Select component with options.

```typescript
import { Select as AntSelect } from 'antd';

interface SelectProps {
  options: { value: string; label: string; disabled?: boolean; }[];
  value?: string;
  onChange?: (value: string) => void;
  placeholder?: string;
  disabled?: boolean;
}

// Usage
<AntSelect
  placeholder="Select an option"
  options={[
    { value: 'option1', label: 'Option 1' },
    { value: 'option2', label: 'Option 2' },
  ]}
  onChange={handleChange}
/>
```

## Layout Components

### Layout
Uses Ant Design Layout components for page structure.

```typescript
import { Layout as AntLayout } from 'antd';
const { Header, Content, Sider, Footer } = AntLayout;

// Usage
<AntLayout>
  <Header>Header</Header>
  <AntLayout>
    <Sider>Sidebar</Sider>
    <Content>Main content</Content>
  </AntLayout>
  <Footer>Footer</Footer>
</AntLayout>
```

### Grid
Uses Ant Design Row and Col components for responsive grid.

```typescript
import { Row, Col } from 'antd';

// Usage
<Row gutter={[16, 16]}>
  <Col xs={24} sm={12} md={8} lg={6}>
    Content 1
  </Col>
  <Col xs={24} sm={12} md={8} lg={6}>
    Content 2
  </Col>
</Row>
```

### Space
Uses Ant Design Space component for consistent spacing.

```typescript
import { Space } from 'antd';

// Usage
<Space direction="vertical" size="middle" style={{ display: 'flex' }}>
  <div>Item 1</div>
  <div>Item 2</div>
</Space>
```

## Feedback Components

### Loading
Uses Ant Design Spin component for loading states.

```typescript
import { Spin } from 'antd';

// Usage
<Spin size="large" tip="Loading...">
  <div>Content being loaded</div>
</Spin>
```

### Notification
Uses Ant Design notification and message components.

```typescript
import { notification, message } from 'antd';

// Usage
notification.success({
  message: 'Success',
  description: 'Operation completed successfully!',
});

// Or for simple messages
message.success('Operation completed!');
```

## Data Display Components

### Table
Uses Ant Design Table component with built-in features.

```typescript
import { Table as AntTable } from 'antd';
import type { ColumnsType } from 'antd/es/table';

interface TableProps<T> {
  dataSource: T[];
  columns: ColumnsType<T>;
  loading?: boolean;
  pagination?: {
    current: number;
    total: number;
    pageSize: number;
    onChange: (page: number, pageSize: number) => void;
  };
}

// Usage
<AntTable 
  dataSource={data}
  columns={columns}
  loading={loading}
  pagination={paginationConfig}
  rowKey="id"
/>
```

## Component Guidelines
1. All components must have TypeScript interfaces
2. Use Ant Design components as building blocks
3. **Use Ant Design's default theme and colors** - no custom theming
4. Include error boundaries for complex components
5. Leverage Ant Design's built-in accessibility features
6. Use Ant Design's loading states (Spin, Button loading, etc.)
7. Document all props and usage examples
8. Follow Ant Design's design principles and patterns

## Styling Approach
- **No custom CSS** - Use Ant Design's default styling
- **No custom themes** - Use Ant Design's default theme and colors
- **Responsive design** - Use Ant Design's Grid system (Row/Col)
- **Icons** - Use Ant Design's icon library
- **Colors** - Use Ant Design's built-in color palette
- **Spacing** - Use Ant Design's Space component for consistent spacing

### Basic Setup
```typescript
// App.tsx - Simple setup, no custom theme
import { ConfigProvider } from 'antd';

function App() {
  return (
    <ConfigProvider>
      <div className="App">
        {/* Your app content */}
      </div>
    </ConfigProvider>
  );
}
```