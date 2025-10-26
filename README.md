# Resume-Account-UI

**Account Management Frontend Application** for Resume Builder SaaS platform.

## Overview

Resume-Account-UI is a dedicated web application for comprehensive account management, billing, subscription management, and user preferences. Built with Next.js 14+ and modern React patterns, it provides a seamless experience for managing all aspects of user accounts.

## Technology Stack

- **Framework**: Next.js 14+ (App Router)
- **React**: React 19 with Server Components
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui
- **Forms**: React Hook Form + Zod validation
- **State Management**: Zustand + TanStack Query
- **Payments**: Stripe Elements & Checkout
- **Charts**: Recharts (usage analytics)
- **Tables**: TanStack Table
- **Deployment**: Cloudflare Pages

## Key Features

### MVP-1: Core Account Management
- User profile editing and management
- Avatar upload and management
- Email notification preferences
- Password change and security settings
- Account deletion and data export

### MVP-2: Subscription & Billing Management
- Current subscription display and management
- Plan upgrade/downgrade interface
- Payment method management (add/remove/update)
- Billing history and invoice downloads
- Usage tracking and limit monitoring

### MVP-3: Advanced Security Features
- Multi-factor authentication setup/management
- Trusted device management
- Login activity monitoring
- Security alerts and notifications
- Privacy settings and data controls

### MVP-4: Enterprise & Team Features
- Team member management and invitations
- Team billing and usage analytics
- Custom plan configuration
- API key management
- Advanced reporting and analytics

## Service Dependencies

**Depends On**:
- **Resume-SSO** - For user authentication and session management
- **Resume-Account-API** - For all account management operations

**No services depend on Resume-Account-UI** - This is an end-user application.

## Application Routes

### Authentication Routes
- `/login` - Sign in page
- `/register` - Sign up page
- `/forgot-password` - Password reset
- `/verify-email` - Email verification

### Account Management Routes (Protected)
- `/` - Account dashboard overview
- `/profile` - Profile information management
- `/profile/edit` - Edit profile details
- `/security` - Security settings and MFA
- `/notifications` - Email and push notification preferences
- `/privacy` - Privacy and data settings

### Billing & Subscription Routes
- `/subscription` - Current subscription management
- `/subscription/change` - Plan upgrade/downgrade
- `/billing` - Billing overview and payment methods
- `/billing/history` - Payment history and invoices
- `/billing/usage` - Usage tracking and analytics

### Team Management Routes (Enterprise)
- `/team` - Team member management
- `/team/invite` - Invite team members
- `/team/billing` - Team billing overview
- `/team/analytics` - Team usage analytics

### Data & Privacy Routes
- `/data/export` - Request data export
- `/data/delete` - Account deletion request
- `/api-keys` - API key management (Enterprise)

## Component Architecture

### Layout Components
```typescript
// Layout structure
- AccountLayout (main layout with sidebar)
- SidebarNavigation (account section navigation)
- HeaderBar (user info, notifications)
- BreadcrumbNavigation (page hierarchy)
- MobileNavigation (mobile-responsive menu)
```

### Profile Components
```typescript
// Profile management
- ProfileForm (basic profile information)
- AvatarUpload (profile picture management)
- BioEditor (rich text bio editing)
- ContactInfoForm (email, phone, location)
- SocialLinksForm (social media links)
```

### Security Components
```typescript
// Security features
- PasswordChangeForm (secure password update)
- MFASetupWizard (TOTP setup process)
- MFADisabledWarning (enable MFA prompt)
- TrustedDevicesList (device management)
- LoginActivityTable (login history)
- SecurityAlertsPanel (security notifications)
```

### Billing Components
```typescript
// Billing and subscription
- SubscriptionCard (current plan display)
- PlanComparisonTable (plan features comparison)
- UpgradePlanWizard (plan upgrade flow)
- PaymentMethodForm (add payment method)
- PaymentMethodsList (manage payment methods)
- InvoiceList (billing history)
- UsageChart (usage analytics visualization)
```

### Team Components
```typescript
// Team management (Enterprise)
- TeamMembersList (team member management)
- InviteMemberForm (send team invitation)
- TeamBillingOverview (team billing summary)
- TeamUsageAnalytics (team usage charts)
- RoleManagement (member role assignment)
```

## Screen-by-Screen UX Design

### Account Dashboard
- **Layout**: Overview cards with key metrics
- **Content**: Profile summary, subscription status, recent activity
- **Actions**: Quick links to common actions
- **Features**: Usage indicators, Security alerts, Billing reminders
- **UX**: Clean, scannable layout with clear CTAs

### Profile Management
- **Layout**: Tabbed interface (Basic Info, Contact, Social)
- **Content**: Form fields with validation, Avatar upload area
- **Actions**: Save changes, Cancel, Upload avatar
- **Features**: Real-time validation, Auto-save drafts
- **UX**: Progressive disclosure, Clear error states

### Security Settings
- **Layout**: Security checklist with expandable sections
- **Content**: Password change, MFA setup, Device list, Activity log
- **Actions**: Enable/disable features, Remove devices, View details
- **Features**: Security score indicator, Alert preferences
- **UX**: Security-focused design, Clear status indicators

### Subscription Management
- **Layout**: Current plan card + Upgrade options
- **Content**: Plan features, Usage limits, Billing cycle
- **Actions**: Upgrade, Downgrade, Cancel, Change plan
- **Features**: Usage meters, Feature comparison, Pro-rated pricing
- **UX**: Clear value proposition, Transparent pricing

### Billing History
- **Layout**: Table with filters and search
- **Content**: Invoice list with dates, amounts, status
- **Actions**: Download PDF, View details, Filter by date
- **Features**: Export to CSV, Payment status indicators
- **UX**: Easy navigation, Clear invoice organization

## State Management

### Zustand Stores
```typescript
// Account store
interface AccountStore {
  user: User | null;
  profile: UserProfile | null;
  subscription: Subscription | null;
  usage: UsageStats | null;
  loading: boolean;
  error: string | null;
  
  // Actions
  loadProfile: () => Promise<void>;
  updateProfile: (data: UpdateProfileData) => Promise<void>;
  loadSubscription: () => Promise<void>;
  updateSubscription: (planId: string) => Promise<void>;
}

// Security store
interface SecurityStore {
  securitySettings: SecuritySettings | null;
  trustedDevices: TrustedDevice[];
  loginActivity: LoginActivity[];
  mfaEnabled: boolean;
  
  // Actions
  loadSecuritySettings: () => Promise<void>;
  enableMFA: (secret: string) => Promise<void>;
  disableMFA: () => Promise<void>;
  removeDevice: (deviceId: string) => Promise<void>;
}

// Billing store
interface BillingStore {
  paymentMethods: PaymentMethod[];
  invoices: Invoice[];
  usage: UsageData[];
  billingSummary: BillingSummary | null;
  
  // Actions
  loadPaymentMethods: () => Promise<void>;
  addPaymentMethod: (method: PaymentMethodData) => Promise<void>;
  loadInvoices: () => Promise<void>;
  downloadInvoice: (invoiceId: string) => Promise<void>;
}
```

### TanStack Query Configuration
```typescript
// API queries with caching
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      cacheTime: 10 * 60 * 1000, // 10 minutes
      retry: 2,
      refetchOnWindowFocus: false,
    },
  },
});

// Custom hooks
export const useProfileQuery = () => {
  return useQuery({
    queryKey: ['profile'],
    queryFn: accountApi.getProfile,
    staleTime: 10 * 60 * 1000, // 10 minutes
  });
};

export const useSubscriptionQuery = () => {
  return useQuery({
    queryKey: ['subscription'],
    queryFn: accountApi.getSubscription,
    staleTime: 2 * 60 * 1000, // 2 minutes
  });
};
```

## Form Validation & UX

### Validation Schemas (Zod)
```typescript
// Profile validation
const profileSchema = z.object({
  firstName: z.string().min(1, 'First name is required').max(50),
  lastName: z.string().min(1, 'Last name is required').max(50),
  email: z.string().email('Invalid email address'),
  bio: z.string().max(500, 'Bio must be less than 500 characters'),
  timezone: z.string().optional(),
  language: z.string().optional(),
});

// Password change validation
const passwordChangeSchema = z.object({
  currentPassword: z.string().min(1, 'Current password is required'),
  newPassword: z.string()
    .min(8, 'Password must be at least 8 characters')
    .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/, 
      'Password must contain uppercase, lowercase, number, and special character'),
  confirmPassword: z.string().min(1, 'Please confirm your password'),
}).refine((data) => data.newPassword === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"],
});
```

### Form UX Patterns
- **Real-time Validation**: Immediate feedback on form fields
- **Progressive Disclosure**: Show advanced options only when needed
- **Auto-save**: Draft saving for long forms
- **Clear Error States**: Specific, actionable error messages
- **Success Feedback**: Clear confirmation of successful actions

## Payment Integration

### Stripe Elements Integration
```typescript
// Payment method form
const PaymentMethodForm = () => {
  const stripe = useStripe();
  const elements = useElements();
  
  const handleSubmit = async (event: FormEvent) => {
    event.preventDefault();
    
    if (!stripe || !elements) return;
    
    const { error, paymentMethod } = await stripe.createPaymentMethod({
      type: 'card',
      card: elements.getElement(CardElement),
    });
    
    if (error) {
      // Handle error
    } else {
      // Send payment method to backend
      await accountApi.addPaymentMethod(paymentMethod.id);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <CardElement options={cardElementOptions} />
      <button type="submit" disabled={!stripe}>
        Add Payment Method
      </button>
    </form>
  );
};
```

### Subscription Management
```typescript
// Plan upgrade flow
const PlanUpgradeWizard = () => {
  const [selectedPlan, setSelectedPlan] = useState<string>('');
  const [loading, setLoading] = useState(false);
  
  const handleUpgrade = async () => {
    setLoading(true);
    try {
      await accountApi.upgradeSubscription(selectedPlan);
      // Show success message
    } catch (error) {
      // Handle error
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <div>
      <PlanComparisonTable 
        plans={plans}
        selectedPlan={selectedPlan}
        onPlanSelect={setSelectedPlan}
      />
      <Button 
        onClick={handleUpgrade}
        disabled={!selectedPlan || loading}
        loading={loading}
      >
        Upgrade to {selectedPlan}
      </Button>
    </div>
  );
};
```

## Security Features Implementation

### MFA Setup Flow
```typescript
// TOTP setup wizard
const MFASetupWizard = () => {
  const [step, setStep] = useState(1);
  const [secret, setSecret] = useState('');
  const [verified, setVerified] = useState(false);
  
  const steps = [
    { title: 'Download Authenticator App', component: DownloadStep },
    { title: 'Scan QR Code', component: QRCodeStep },
    { title: 'Verify Code', component: VerifyStep },
    { title: 'Backup Codes', component: BackupCodesStep },
  ];
  
  return (
    <div className="mfa-setup-wizard">
      <Progress current={step} total={steps.length} />
      <h2>{steps[step - 1].title}</h2>
      {React.createElement(steps[step - 1].component, {
        secret,
        setSecret,
        verified,
        setVerified,
        onNext: () => setStep(step + 1),
        onBack: () => setStep(step - 1),
      })}
    </div>
  );
};
```

### Device Management
```typescript
// Trusted devices list
const TrustedDevicesList = () => {
  const { data: devices, removeDevice } = useTrustedDevices();
  
  return (
    <div className="devices-list">
      {devices?.map((device) => (
        <DeviceCard key={device.id} device={device}>
          <DeviceIcon type={device.type} />
          <div className="device-info">
            <h3>{device.name}</h3>
            <p>Last used: {formatDate(device.lastUsed)}</p>
            <p>IP: {device.ipAddress}</p>
          </div>
          <Button 
            variant="destructive" 
            onClick={() => removeDevice(device.id)}
          >
            Remove
          </Button>
        </DeviceCard>
      ))}
    </div>
  );
};
```

## Performance Optimizations

### Component Optimization
- **React.memo**: Prevent unnecessary re-renders
- **useMemo**: Cache expensive calculations
- **useCallback**: Stable function references
- **Code Splitting**: Lazy load route components
- **Image Optimization**: Next.js Image component

### Data Fetching Optimization
- **Query Deduplication**: Prevent duplicate requests
- **Background Refetching**: Keep data fresh
- **Optimistic Updates**: Immediate UI feedback
- **Pagination**: Efficient large dataset handling

### Bundle Optimization
```typescript
// Dynamic imports for heavy components
const PlanComparisonTable = dynamic(
  () => import('./components/PlanComparisonTable'),
  { 
    loading: () => <div>Loading plans...</div>,
    ssr: false 
  }
);

const UsageAnalyticsChart = dynamic(
  () => import('./components/UsageAnalyticsChart'),
  { 
    loading: () => <div>Loading analytics...</div>,
    ssr: false 
  }
);
```

## Accessibility Features

### WCAG 2.1 Compliance
- **Keyboard Navigation**: Full keyboard accessibility
- **Screen Reader Support**: Proper ARIA labels and roles
- **Color Contrast**: 4.5:1 minimum contrast ratio
- **Focus Management**: Clear focus indicators
- **Error Handling**: Accessible error announcements

### Accessibility Components
```typescript
// Accessible form field
const FormField = ({ label, error, ...props }) => {
  const id = useId();
  
  return (
    <div className="form-field">
      <label htmlFor={id} className="form-label">
        {label}
      </label>
      <input
        id={id}
        aria-describedby={error ? `${id}-error` : undefined}
        aria-invalid={!!error}
        className="form-input"
        {...props}
      />
      {error && (
        <div id={`${id}-error`} className="form-error" role="alert">
          {error}
        </div>
      )}
    </div>
  );
};
```

## Testing Strategy

### Unit Testing
```bash
# Run unit tests
npm test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch
```

### Integration Testing
```bash
# Run integration tests
npm run test:integration

# API integration tests
npm run test:api

# Form validation tests
npm run test:forms
```

### E2E Testing
```bash
# Run E2E tests (Playwright)
npm run test:e2e

# User journey tests
npm run test:journeys

# Accessibility tests
npm run test:a11y
```

## Development Setup

### Prerequisites
- Node.js 18+
- npm or pnpm
- Access to Resume-SSO and Resume-Account-API services

### Environment Variables

Create a `.env.local` file:

```bash
# API Configuration
NEXT_PUBLIC_SSO_URL=http://localhost:8787
NEXT_PUBLIC_ACCOUNT_API_URL=http://localhost:8790

# Stripe Configuration
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...

# Analytics
NEXT_PUBLIC_ANALYTICS_ID=<cloudflare-analytics-id>

# Feature Flags
NEXT_PUBLIC_ENABLE_TEAM_FEATURES=false
NEXT_PUBLIC_ENABLE_API_KEYS=false

# App Configuration
NEXT_PUBLIC_APP_NAME=Resume Builder Account
NEXT_PUBLIC_SUPPORT_EMAIL=support@resumebuilder.com
```

### Quick Start

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run tests
npm test

# Run E2E tests
npm run test:e2e
```

## Performance Targets

- **Initial Page Load**: < 2s (LCP)
- **Form Interactions**: < 100ms
- **API Response Handling**: < 300ms
- **Payment Processing**: < 5s
- **Data Export**: < 30s

## Development Plan

See [HIGH_LEVEL_PLAN.md](./HIGH_LEVEL_PLAN.md) for complete dependency-tree based development plan with MVPs, epics, and critical path.

## GitHub Issues

Track development progress in [GitHub Issues](https://github.com/MEYWD-Labs/Resume-Account-UI/issues):
- [MVP-1 Epics](https://github.com/MEYWD-Labs/Resume-Account-UI/issues?q=is%3Aissue+is%3Aopen+MVP-1) - Core Account Management
- [MVP-2 Epics](https://github.com/MEYWD-Labs/Resume-Account-UI/issues?q=is%3Aissue+is%3Aopen+MVP-2) - Subscription & Billing Management
- [MVP-3 Epics](https://github.com/MEYWD-Labs/Resume-Account-UI/issues?q=is%3Aissue+is%3Aopen+MVP-3) - Advanced Security Features
- [MVP-4 Epics](https://github.com/MEYWD-Labs/Resume-Account-UI/issues?q=is%3Aissue+is%3Aopen+MVP-4) - Enterprise & Team Features

## Deployment

```bash
# Build and deploy to Cloudflare Pages
npm run deploy

# Preview deployment
npm run deploy:preview

# Production deployment
npm run deploy:production
```

## Browser Support

- Chrome/Edge: Latest 2 versions
- Firefox: Latest 2 versions
- Safari: Latest 2 versions
- Mobile browsers: iOS Safari 14+, Chrome Android 90+

## Support

For issues or questions, create a GitHub issue in this repository.

## License

Proprietary - All rights reserved