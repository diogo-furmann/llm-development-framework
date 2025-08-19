# Routing Structure

## Router Configuration

### Basic Setup
```typescript
// src/router/index.tsx
import { createBrowserRouter } from 'react-router-dom';
import { Layout } from '../components/Layout';
import { HomePage } from '../pages/Home';
import { DashboardPage } from '../pages/Dashboard';
import { SettingsPage } from '../pages/Settings';
import { NotFoundPage } from '../pages/NotFound';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <Layout />,
    children: [
      {
        index: true,
        element: <HomePage />
      },
      {
        path: 'dashboard',
        element: <DashboardPage />,
        children: [
          {
            path: 'analytics',
            element: <AnalyticsPage />
          },
          {
            path: 'reports',
            element: <ReportsPage />
          }
        ]
      },
      {
        path: 'settings',
        element: <SettingsPage />
      },
      {
        path: 'profile/:userId',
        element: <ProfilePage />
      },
      {
        path: '*',
        element: <NotFoundPage />
      }
    ]
  }
]);
```

## Route Structure

### Public Routes
```
/                    - Home page
/about              - About page
/contact            - Contact page
/login              - Login page
/register           - Registration page
```

### Protected Routes
```
/dashboard          - Main dashboard
/dashboard/analytics - Analytics view
/dashboard/reports  - Reports view
/settings           - User settings
/settings/profile   - Profile settings
/settings/security  - Security settings
/profile/:userId    - User profile page
```

### Route Patterns
```typescript
// Dynamic segments
/users/:id          - Single user
/posts/:postId/comments/:commentId - Nested resources

// Query parameters
/search?q=term&category=tech&page=1

// Optional segments
/docs/:section?     - Section is optional

// Wildcard routes
/admin/*           - Catches all admin sub-routes
```

## Route Components

### Layout Component
```typescript
// src/components/Layout/Layout.tsx
import { Outlet } from 'react-router-dom';
import { Header } from '../Header';
import { Sidebar } from '../Sidebar';
import { Footer } from '../Footer';

export const Layout: React.FC = () => {
  return (
    <div className="app-layout">
      <Header />
      <div className="main-content">
        <Sidebar />
        <main className="content">
          <Outlet />
        </main>
      </div>
      <Footer />
    </div>
  );
};
```

### Protected Route Component
```typescript
// src/components/ProtectedRoute/ProtectedRoute.tsx
import { Navigate, useLocation } from 'react-router-dom';
import { useAuth } from '../../hooks/useAuth';

interface ProtectedRouteProps {
  children: React.ReactNode;
  requireRole?: string;
}

export const ProtectedRoute: React.FC<ProtectedRouteProps> = ({ 
  children, 
  requireRole 
}) => {
  const { user, loading } = useAuth();
  const location = useLocation();

  if (loading) {
    return <div>Loading...</div>;
  }

  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  if (requireRole && user.role !== requireRole) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <>{children}</>;
};
```

## Navigation Hooks

### useNavigation Hook
```typescript
// src/hooks/useNavigation.ts
import { useNavigate, useLocation } from 'react-router-dom';

export const useNavigation = () => {
  const navigate = useNavigate();
  const location = useLocation();

  const goTo = (path: string, options?: { replace?: boolean; state?: any }) => {
    navigate(path, options);
  };

  const goBack = () => {
    navigate(-1);
  };

  const goForward = () => {
    navigate(1);
  };

  const refresh = () => {
    navigate(location.pathname, { replace: true });
  };

  const isActive = (path: string) => {
    return location.pathname === path;
  };

  const isParentActive = (path: string) => {
    return location.pathname.startsWith(path);
  };

  return {
    goTo,
    goBack,
    goForward,
    refresh,
    isActive,
    isParentActive,
    currentPath: location.pathname,
    search: location.search,
    state: location.state
  };
};
```

### useQueryParams Hook
```typescript
// src/hooks/useQueryParams.ts
import { useSearchParams } from 'react-router-dom';
import { useMemo } from 'react';

export const useQueryParams = () => {
  const [searchParams, setSearchParams] = useSearchParams();

  const queryParams = useMemo(() => {
    const params: Record<string, string> = {};
    searchParams.forEach((value, key) => {
      params[key] = value;
    });
    return params;
  }, [searchParams]);

  const setQueryParam = (key: string, value: string) => {
    const newParams = new URLSearchParams(searchParams);
    newParams.set(key, value);
    setSearchParams(newParams);
  };

  const removeQueryParam = (key: string) => {
    const newParams = new URLSearchParams(searchParams);
    newParams.delete(key);
    setSearchParams(newParams);
  };

  const clearQueryParams = () => {
    setSearchParams({});
  };

  return {
    queryParams,
    setQueryParam,
    removeQueryParam,
    clearQueryParams,
    searchParams
  };
};
```

## Route Guards

### Auth Guard
```typescript
// src/guards/AuthGuard.tsx
import { useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import { useAuth } from '../hooks/useAuth';

interface AuthGuardProps {
  children: React.ReactNode;
  redirectTo?: string;
}

export const AuthGuard: React.FC<AuthGuardProps> = ({ 
  children, 
  redirectTo = '/login' 
}) => {
  const { user, loading } = useAuth();
  const navigate = useNavigate();

  useEffect(() => {
    if (!loading && !user) {
      navigate(redirectTo);
    }
  }, [user, loading, navigate, redirectTo]);

  if (loading) {
    return <div>Loading...</div>;
  }

  if (!user) {
    return null;
  }

  return <>{children}</>;
};
```

### Role Guard
```typescript
// src/guards/RoleGuard.tsx
interface RoleGuardProps {
  children: React.ReactNode;
  allowedRoles: string[];
  fallback?: React.ReactNode;
}

export const RoleGuard: React.FC<RoleGuardProps> = ({ 
  children, 
  allowedRoles, 
  fallback = <div>Access denied</div> 
}) => {
  const { user } = useAuth();

  if (!user || !allowedRoles.includes(user.role)) {
    return fallback;
  }

  return <>{children}</>;
};
```

## Navigation Components

### NavLink Component
```typescript
// src/components/NavLink/NavLink.tsx
import { NavLink as RouterNavLink } from 'react-router-dom';
import { Menu } from 'antd';
import './NavLink.css'; // Minimal custom styles if needed

interface NavLinkProps {
  to: string;
  children: React.ReactNode;
  exact?: boolean;
}

export const NavLink: React.FC<NavLinkProps> = ({ 
  to, 
  children, 
  exact = false
}) => {
  return (
    <RouterNavLink
      to={to}
      className={({ isActive }) => 
        isActive ? 'nav-link active' : 'nav-link'
      }
      end={exact}
    >
      {children}
    </RouterNavLink>
  );
};

// Alternative using Ant Design Menu
export const AntNavLink: React.FC<NavLinkProps> = ({ to, children }) => {
  return (
    <Menu.Item key={to}>
      <RouterNavLink to={to}>
        {children}
      </RouterNavLink>
    </Menu.Item>
  );
};
```

### Breadcrumbs Component
```typescript
// src/components/Breadcrumbs/Breadcrumbs.tsx
import { Link, useLocation } from 'react-router-dom';
import { Breadcrumb } from 'antd';

export const Breadcrumbs: React.FC = () => {
  const location = useLocation();
  
  const generateBreadcrumbs = () => {
    const pathnames = location.pathname.split('/').filter(x => x);
    
    const breadcrumbItems = [
      {
        title: <Link to="/">Home</Link>
      }
    ];

    pathnames.forEach((pathname, index) => {
      const path = `/${pathnames.slice(0, index + 1).join('/')}`;
      const label = pathname.charAt(0).toUpperCase() + pathname.slice(1);
      const isLast = index === pathnames.length - 1;
      
      breadcrumbItems.push({
        title: isLast ? label : <Link to={path}>{label}</Link>
      });
    });

    return breadcrumbItems;
  };

  const breadcrumbItems = generateBreadcrumbs();

  if (breadcrumbItems.length <= 1) {
    return null;
  }

  return (
    <Breadcrumb items={breadcrumbItems} />
  );
};
```

## Route Data Loading

### Loader Functions
```typescript
// src/loaders/userLoader.ts
export const userLoader = async ({ params }: { params: any }) => {
  const { userId } = params;
  
  try {
    const user = await apiService.get(`/users/${userId}`);
    return { user: user.data };
  } catch (error) {
    throw new Response('User not found', { status: 404 });
  }
};

// In router configuration
{
  path: 'users/:userId',
  element: <UserPage />,
  loader: userLoader
}
```

### Using Loaded Data
```typescript
// src/pages/UserPage.tsx
import { useLoaderData } from 'react-router-dom';

interface LoaderData {
  user: User;
}

export const UserPage: React.FC = () => {
  const { user } = useLoaderData() as LoaderData;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
};
```

## Error Handling

### Error Boundary
```typescript
// src/components/ErrorBoundary/RouteErrorBoundary.tsx
import { useRouteError, isRouteErrorResponse } from 'react-router-dom';

export const RouteErrorBoundary: React.FC = () => {
  const error = useRouteError();

  if (isRouteErrorResponse(error)) {
    return (
      <div>
        <h1>{error.status} {error.statusText}</h1>
        <p>{error.data}</p>
      </div>
    );
  }

  return (
    <div>
      <h1>Something went wrong</h1>
      <p>An unexpected error occurred.</p>
    </div>
  );
};
```

