# High-Level Development Plan - Resume-Account-UI

**Service Type**: Account Management Frontend Application (Next.js 14+ on Cloudflare Pages)
**Build Approach**: AI-Automated Development
**Priority**: HIGH - Critical for user account management and monetization

---

## Dependency Tree Overview

Resume-Account-UI (Account Management - Web)
├── DEPENDS ON: Resume-SSO (MVP-1.4: Login) → For authentication
├── DEPENDS ON: Resume-Account-API (All MVPs) → For backend operations
├── BLOCKS: None (this is an end-user application)

External Dependencies:
├── Resume-SSO (MVP-1.4: Login System) → For user authentication and sessions
├── Resume-Account-API (MVP-1: Core) → For profile, preferences, security
├── Resume-Account-API (MVP-2: Billing) → For subscriptions, payments, invoices
├── Resume-Account-API (MVP-3: Security) → For 2FA, sessions, API keys
└── Resume-Account-API (MVP-4: Teams) → For team management, RBAC, analytics

Integration Points:
├── Resume-UI → Shares authentication context
├── Resume-UI → Links to account management pages
└── Resume-UI → Subscription status for feature gating

---

## Application Routes Structure

```
/account
├── /dashboard                    # Account overview
├── /profile                      # Profile management
│   ├── /edit                    # Edit profile
│   └── /avatar                  # Avatar management
├── /security                     # Security settings
│   ├── /password                # Change password
│   ├── /2fa                     # Two-factor authentication
│   ├── /sessions                # Active sessions
│   └── /api-keys                # API key management
├── /subscription                 # Subscription management
│   ├── /overview                # Current plan details
│   ├── /upgrade                 # Upgrade plan
│   └── /cancel                  # Cancel subscription
├── /billing                      # Billing management
│   ├── /payment-methods         # Payment methods
│   ├── /history                 # Billing history
│   └── /invoices                # Invoice downloads
├── /preferences                  # User preferences
│   ├── /notifications           # Email notifications
│   ├── /privacy                 # Privacy settings
│   └── /data-export             # Export user data
├── /activity                     # Activity monitoring
│   ├── /log                     # Activity history
│   └── /audit                   # Audit trail (enterprise)
└── /team                         # Team management (enterprise)
    ├── /members                 # Team members
    ├── /roles                   # Role management
    ├── /invites                 # Pending invitations
    └── /settings                # Team settings
```

---

## MVP-1: Core Account Management (Foundation)

**Status**: MUST BUILD AFTER Resume-SSO and Resume-Account-API (MVP-1) are ready
**Dependencies**: Resume-SSO (MVP-1.4: Login), Resume-Account-API (MVP-1: Core Operations)
**Blocks**: All user account self-service features

### Epic 1.1: Authentication Integration
**What**: Integrate with Resume-SSO for user authentication
**Complexity**: Medium
**Dependencies**: Resume-SSO (MVP-1.4: Login with JWT)
**Blocks**: All authenticated features

**Tasks**:
- [ ] Next.js authentication setup with JWT
- [ ] Session management (httpOnly cookies)
- [ ] Protected route middleware
- [ ] Authentication context provider
- [ ] Token refresh mechanism
- [ ] Logout functionality across all tabs
- [ ] Session expiry handling
- [ ] Redirect to SSO for login
- [ ] Handle SSO callbacks
- [ ] CSRF protection implementation

**Acceptance Criteria**:
- [ ] JWT tokens stored securely in httpOnly cookies
- [ ] Protected routes redirect to SSO login
- [ ] Session persists across page reloads
- [ ] Token auto-refresh before expiry
- [ ] Logout clears all sessions
- [ ] CSRF protection active on all mutations

---

### Epic 1.2: Account Dashboard
**What**: Main account overview page showing key information
**Complexity**: Medium
**Dependencies**: Epic 1.1 (Authentication), Resume-Account-API (MVP-1.1: Account CRUD)

**Tasks**:
- [ ] `/account/dashboard` page layout
- [ ] Account summary card (name, email, plan)
- [ ] Quick stats widgets:
  - [ ] Current subscription tier
  - [ ] Storage usage
  - [ ] Resume count
  - [ ] Last login time
- [ ] Quick actions menu:
  - [ ] Edit profile
  - [ ] Change password
  - [ ] Manage subscription
  - [ ] View activity
- [ ] Recent activity feed (last 5 actions)
- [ ] Subscription status banner
- [ ] Account alerts/notifications
- [ ] Loading states with skeletons
- [ ] Error handling and retry

**Acceptance Criteria**:
- [ ] Dashboard displays account overview
- [ ] All quick stats update in real-time
- [ ] Quick actions navigate correctly
- [ ] Recent activity shows last 5 items
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] Loading states for all async data

---

### Epic 1.3: Profile Management
**What**: User profile editing interface
**Complexity**: Medium
**Dependencies**: Epic 1.2 (Dashboard), Resume-Account-API (MVP-1.1: Account CRUD)

**Tasks**:
- [ ] `/account/profile` page layout
- [ ] Profile form with fields:
  - [ ] Full name
  - [ ] Display name
  - [ ] Email (read-only, verified status)
  - [ ] Phone number (optional)
  - [ ] Bio/description
  - [ ] Location (country, timezone)
  - [ ] Language preference
- [ ] Avatar upload component:
  - [ ] Drag-and-drop upload
  - [ ] Image cropper
  - [ ] Default avatar generation
  - [ ] Size validation (< 5MB)
- [ ] Form validation (Zod schema)
- [ ] Save changes with optimistic updates
- [ ] Success/error toast notifications
- [ ] Unsaved changes warning

**Acceptance Criteria**:
- [ ] All profile fields editable except email
- [ ] Avatar upload with crop functionality
- [ ] Form validation prevents invalid data
- [ ] Optimistic updates provide instant feedback
- [ ] Unsaved changes prompt before navigation
- [ ] Success/error messages clear

---

### Epic 1.4: Security Settings
**What**: Basic security configuration interface
**Complexity**: High
**Dependencies**: Epic 1.1 (Authentication), Resume-Account-API (MVP-1.3: Security)

**Tasks**:
- [ ] `/account/security` page layout
- [ ] Change password form:
  - [ ] Current password field
  - [ ] New password with strength meter
  - [ ] Confirm password field
  - [ ] Password requirements display
- [ ] Email verification status:
  - [ ] Show verification badge
  - [ ] Resend verification email button
- [ ] Account deletion section:
  - [ ] Delete account button (danger zone)
  - [ ] Confirmation modal with typing confirmation
  - [ ] Data export before deletion option
- [ ] Security recommendations checklist
- [ ] Last password change timestamp
- [ ] Account creation date display

**Acceptance Criteria**:
- [ ] Password change requires current password
- [ ] Password strength meter works correctly
- [ ] Email verification status accurate
- [ ] Account deletion requires confirmation
- [ ] Security recommendations displayed
- [ ] All actions trigger email notifications

---

### Epic 1.5: Email Preferences
**What**: Email notification settings management
**Complexity**: Low
**Dependencies**: Epic 1.2 (Dashboard), Resume-Account-API (MVP-1.2: Preferences)

**Tasks**:
- [ ] `/account/preferences/notifications` page
- [ ] Email preference toggles:
  - [ ] Marketing emails
  - [ ] Product updates
  - [ ] Security alerts (non-optional)
  - [ ] Weekly digest
  - [ ] Resume tips
  - [ ] Feature announcements
- [ ] Email frequency settings:
  - [ ] Immediate
  - [ ] Daily digest
  - [ ] Weekly digest
- [ ] Unsubscribe from all (except security)
- [ ] Preview email templates
- [ ] Save preferences with confirmation
- [ ] Email preference import/export

**Acceptance Criteria**:
- [ ] All email types toggleable except security
- [ ] Frequency settings apply to eligible emails
- [ ] Unsubscribe all works correctly
- [ ] Preferences persist across sessions
- [ ] Changes confirmed via email

---

### Epic 1.6: Responsive Design & UI Components
**What**: Ensure account UI works on all devices
**Complexity**: Medium
**Dependencies**: All MVP-1 epics

**Tasks**:
- [ ] Mobile-optimized layouts (< 768px)
- [ ] Tablet-optimized layouts (768px - 1024px)
- [ ] Desktop layouts (> 1024px)
- [ ] Component library setup (shadcn/ui)
- [ ] Dark mode support with system preference
- [ ] Loading skeletons for all async operations
- [ ] Error boundaries and error pages
- [ ] Toast notification system
- [ ] Form components with validation
- [ ] Consistent design tokens
- [ ] Accessibility audit (WCAG 2.1 AA)

**Acceptance Criteria**:
- [ ] All pages responsive across devices
- [ ] Dark mode toggle persists
- [ ] Accessibility score > 95 (Lighthouse)
- [ ] Consistent UI across all pages
- [ ] Error states handled gracefully
- [ ] Loading states for all async operations

---

## MVP-2: Subscription & Billing UI (Monetization)

**Status**: Required for subscription management
**Dependencies**: MVP-1, Resume-Account-API (MVP-2: Billing)
**Blocks**: Payment and subscription self-service

### Epic 2.1: Subscription Overview
**What**: Current subscription status and details
**Complexity**: Medium
**Dependencies**: Epic 1.2 (Dashboard), Resume-Account-API (MVP-2.1: Subscription)

**Tasks**:
- [ ] `/account/subscription/overview` page
- [ ] Current plan card:
  - [ ] Plan name (Free, Pro, Premium)
  - [ ] Price and billing cycle
  - [ ] Next billing date
  - [ ] Auto-renewal status
- [ ] Feature comparison table:
  - [ ] Current plan features (checked)
  - [ ] Other plan features (locked/available)
  - [ ] Usage limits and current usage
- [ ] Plan benefits list
- [ ] Upgrade/downgrade CTA buttons
- [ ] Cancel subscription link
- [ ] Trial status indicator (if applicable)
- [ ] Subscription history timeline

**Acceptance Criteria**:
- [ ] Current plan details accurate
- [ ] Feature comparison clear
- [ ] Usage limits displayed with progress bars
- [ ] Upgrade/downgrade buttons contextual
- [ ] Trial countdown if applicable
- [ ] History shows all plan changes

---

### Epic 2.2: Payment Method Management
**What**: Add, update, and remove payment methods
**Complexity**: High
**Dependencies**: Epic 2.1 (Subscription), Resume-Account-API (MVP-2.2: Payment Methods)

**Tasks**:
- [ ] `/account/billing/payment-methods` page
- [ ] Payment methods list:
  - [ ] Card type and last 4 digits
  - [ ] Expiry date
  - [ ] Default payment method badge
  - [ ] Remove button (if not default)
- [ ] Add payment method:
  - [ ] Stripe Elements integration
  - [ ] Card number, expiry, CVC, ZIP
  - [ ] Save as default option
  - [ ] 3D Secure handling
- [ ] Set default payment method
- [ ] Update expiry date for existing cards
- [ ] Payment method validation
- [ ] PCI compliance notice

**Acceptance Criteria**:
- [ ] Stripe Elements integrated securely
- [ ] 3D Secure authentication works
- [ ] Can add multiple payment methods
- [ ] Default method clearly indicated
- [ ] Cannot remove last payment method if subscribed
- [ ] PCI compliant implementation

---

### Epic 2.3: Billing History
**What**: View all past transactions and charges
**Complexity**: Low
**Dependencies**: Epic 2.1 (Subscription), Resume-Account-API (MVP-2.3: Billing History)

**Tasks**:
- [ ] `/account/billing/history` page
- [ ] Transactions table:
  - [ ] Date column
  - [ ] Description (subscription, one-time)
  - [ ] Amount with currency
  - [ ] Status (paid, pending, failed)
  - [ ] Invoice link/download
- [ ] Filter options:
  - [ ] Date range picker
  - [ ] Transaction type
  - [ ] Status filter
- [ ] Pagination or infinite scroll
- [ ] Export to CSV option
- [ ] Search by invoice number
- [ ] Refund status indicators

**Acceptance Criteria**:
- [ ] All transactions displayed accurately
- [ ] Filters work correctly
- [ ] Invoices downloadable as PDF
- [ ] Export includes all filtered data
- [ ] Pagination handles large datasets
- [ ] Refunds clearly marked

---

### Epic 2.4: Invoice Management
**What**: View and download invoices
**Complexity**: Low
**Dependencies**: Epic 2.3 (Billing History), Resume-Account-API (MVP-2.3: Billing History)

**Tasks**:
- [ ] `/account/billing/invoices` page
- [ ] Invoice list view:
  - [ ] Invoice number
  - [ ] Date issued
  - [ ] Amount
  - [ ] Status (paid, due, overdue)
  - [ ] Download button
- [ ] Invoice preview modal
- [ ] Bulk download option (ZIP)
- [ ] Email invoice option
- [ ] Invoice search by number
- [ ] Tax information section

**Acceptance Criteria**:
- [ ] All invoices listed chronologically
- [ ] PDF download works correctly
- [ ] Preview shows full invoice
- [ ] Bulk download creates ZIP file
- [ ] Email sends invoice copy
- [ ] Tax information accurate

---

### Epic 2.5: Plan Upgrade/Downgrade
**What**: Change subscription tier interface
**Complexity**: High
**Dependencies**: Epic 2.1 (Subscription), Resume-Account-API (MVP-2.4: Plan Changes)

**Tasks**:
- [ ] `/account/subscription/upgrade` page
- [ ] Plan selection interface:
  - [ ] Current plan highlighted
  - [ ] Available plans with pricing
  - [ ] Feature comparison
  - [ ] Savings for annual billing
- [ ] Proration calculator:
  - [ ] Show credit/charge amount
  - [ ] Effective date
  - [ ] Next billing amount
- [ ] Downgrade warnings:
  - [ ] Feature loss warnings
  - [ ] Data retention policy
  - [ ] Confirmation modal
- [ ] Payment confirmation step
- [ ] Success page with receipt

**Acceptance Criteria**:
- [ ] Plan changes calculate proration correctly
- [ ] Downgrade warnings comprehensive
- [ ] Immediate plan activation after payment
- [ ] Email confirmation sent
- [ ] Feature access updated instantly
- [ ] Refunds processed for downgrades

---

## MVP-3: Advanced Security Features (Enhanced Security)

**Status**: Premium security enhancements
**Dependencies**: MVP-2, Resume-Account-API (MVP-3: Advanced Security)
**Blocks**: Advanced security features

### Epic 3.1: Two-Factor Authentication (2FA)
**What**: Setup and manage 2FA for account
**Complexity**: High
**Dependencies**: Epic 1.4 (Security Settings), Resume-Account-API (MVP-3.1: 2FA)

**Tasks**:
- [ ] `/account/security/2fa` page
- [ ] 2FA setup wizard:
  - [ ] QR code generation for authenticator apps
  - [ ] Secret key display for manual entry
  - [ ] Verification code input
  - [ ] Backup codes generation (10 codes)
  - [ ] Backup codes download as PDF
- [ ] 2FA management:
  - [ ] Enable/disable toggle
  - [ ] Regenerate backup codes
  - [ ] View remaining backup codes
  - [ ] Add multiple 2FA methods
- [ ] SMS 2FA option (premium):
  - [ ] Phone number verification
  - [ ] SMS code verification
  - [ ] Fallback to authenticator
- [ ] Recovery options configuration

**Acceptance Criteria**:
- [ ] QR code scans successfully in authenticator apps
- [ ] Backup codes work as fallback
- [ ] Can disable 2FA with password confirmation
- [ ] Multiple 2FA methods supported
- [ ] SMS 2FA works globally
- [ ] Recovery process documented

---

### Epic 3.2: Activity Log
**What**: View detailed account activity history
**Complexity**: Medium
**Dependencies**: Epic 1.2 (Dashboard), Resume-Account-API (MVP-3.2: Activity Logging)

**Tasks**:
- [ ] `/account/activity/log` page
- [ ] Activity feed with:
  - [ ] Timestamp (relative and absolute)
  - [ ] Activity type (login, logout, change, etc.)
  - [ ] IP address and location
  - [ ] Device/browser information
  - [ ] Success/failure status
- [ ] Filter options:
  - [ ] Date range picker
  - [ ] Activity type filter
  - [ ] Status filter (success/failed)
  - [ ] Device filter
- [ ] Search by IP or activity
- [ ] Export activity log (CSV/JSON)
- [ ] Suspicious activity alerts
- [ ] Real-time updates via WebSocket

**Acceptance Criteria**:
- [ ] All account activities logged
- [ ] Filters combine correctly
- [ ] Export includes filtered data
- [ ] Suspicious activity highlighted
- [ ] Real-time updates without refresh
- [ ] Location data approximate (city level)

---

### Epic 3.3: Active Sessions Management
**What**: View and manage all active login sessions
**Complexity**: Medium
**Dependencies**: Epic 3.2 (Activity Log), Resume-Account-API (MVP-3.3: Session Management)

**Tasks**:
- [ ] `/account/security/sessions` page
- [ ] Active sessions list:
  - [ ] Current session indicator
  - [ ] Device type and OS
  - [ ] Browser information
  - [ ] IP address and location
  - [ ] Last activity timestamp
  - [ ] Session start time
- [ ] Session actions:
  - [ ] Revoke individual session
  - [ ] Revoke all other sessions
  - [ ] Session expiry settings
- [ ] Session details modal
- [ ] Suspicious session warnings
- [ ] Map view of session locations

**Acceptance Criteria**:
- [ ] All active sessions displayed
- [ ] Can revoke any session except current
- [ ] Revoked sessions logged out immediately
- [ ] Suspicious sessions flagged
- [ ] Map shows approximate locations
- [ ] Session limit enforcement (5 concurrent)

---

### Epic 3.4: API Keys Management
**What**: Create and manage API keys for integrations
**Complexity**: High
**Dependencies**: Epic 1.4 (Security Settings), Resume-Account-API (MVP-3.4: API Keys)

**Tasks**:
- [ ] `/account/security/api-keys` page
- [ ] API keys list:
  - [ ] Key name and description
  - [ ] Created date
  - [ ] Last used date
  - [ ] Permissions/scopes
  - [ ] Status (active/revoked)
- [ ] Create API key:
  - [ ] Name and description fields
  - [ ] Permission scopes selection
  - [ ] Expiry date setting
  - [ ] IP whitelist option
  - [ ] Rate limit configuration
- [ ] Key display modal (show once only)
- [ ] Copy key to clipboard
- [ ] Revoke key with confirmation
- [ ] Usage statistics per key
- [ ] Webhook configuration

**Acceptance Criteria**:
- [ ] API keys generated securely
- [ ] Key shown only once after creation
- [ ] Permissions enforced on API calls
- [ ] Rate limits applied correctly
- [ ] Usage tracked accurately
- [ ] Revoked keys stop working immediately

---

### Epic 3.5: Account Recovery
**What**: Self-service account recovery options
**Complexity**: Medium
**Dependencies**: Epic 3.1 (2FA), Resume-Account-API (MVP-3.5: Recovery)

**Tasks**:
- [ ] Account recovery setup:
  - [ ] Recovery email configuration
  - [ ] Security questions setup
  - [ ] Recovery phone number
  - [ ] Trusted contacts
- [ ] Recovery methods priority
- [ ] Recovery codes generation
- [ ] Download recovery kit (PDF)
- [ ] Test recovery process
- [ ] Recovery audit log
- [ ] Disable account option
- [ ] Reactivation process

**Acceptance Criteria**:
- [ ] Multiple recovery methods configurable
- [ ] Recovery kit contains all needed info
- [ ] Test mode doesn't lock account
- [ ] Recovery attempts logged
- [ ] Account disable is reversible
- [ ] All methods work independently

---

## MVP-4: Enterprise & Team Features (Team Management)

**Status**: Enterprise tier features
**Dependencies**: MVP-3, Resume-Account-API (MVP-4: Teams)
**Blocks**: Team collaboration features

### Epic 4.1: Team Management
**What**: Create and manage team members
**Complexity**: Very High
**Dependencies**: Epic 1.2 (Dashboard), Resume-Account-API (MVP-4.1: Teams CRUD)

**Tasks**:
- [ ] `/account/team/members` page
- [ ] Team members table:
  - [ ] Member name and email
  - [ ] Role badge
  - [ ] Join date
  - [ ] Last active
  - [ ] Status (active/invited/suspended)
- [ ] Invite team members:
  - [ ] Email input (multiple)
  - [ ] Role selection
  - [ ] Custom invitation message
  - [ ] Invitation expiry setting
  - [ ] Bulk invite via CSV
- [ ] Member actions:
  - [ ] Change role
  - [ ] Suspend/reactivate
  - [ ] Remove from team
  - [ ] Resend invitation
- [ ] Team overview stats
- [ ] Seat usage indicator

**Acceptance Criteria**:
- [ ] Team members displayed with all info
- [ ] Invitations sent via email
- [ ] Role changes apply immediately
- [ ] Suspended users lose access
- [ ] Seat limits enforced
- [ ] Bulk operations work correctly

---

### Epic 4.2: Role-Based Access Control
**What**: Define and assign team roles
**Complexity**: High
**Dependencies**: Epic 4.1 (Team Management), Resume-Account-API (MVP-4.2: RBAC)

**Tasks**:
- [ ] `/account/team/roles` page
- [ ] Predefined roles:
  - [ ] Owner (full access)
  - [ ] Admin (manage team)
  - [ ] Editor (create/edit resumes)
  - [ ] Viewer (read-only)
- [ ] Custom role creation:
  - [ ] Role name and description
  - [ ] Permission matrix
  - [ ] Copy from existing role
- [ ] Permissions grid:
  - [ ] Resource types (resumes, billing, team)
  - [ ] Actions (view, create, edit, delete)
  - [ ] Granular permissions
- [ ] Role assignment interface
- [ ] Permission inheritance
- [ ] Role audit trail

**Acceptance Criteria**:
- [ ] Default roles work correctly
- [ ] Custom roles creatable
- [ ] Permissions enforced in UI
- [ ] Role changes logged
- [ ] Inheritance works as expected
- [ ] Cannot delete roles in use

---

### Epic 4.3: Team Billing
**What**: Consolidated billing for team subscriptions
**Complexity**: High
**Dependencies**: Epic 2.1 (Subscription), Resume-Account-API (MVP-4.3: Team Billing)

**Tasks**:
- [ ] `/account/team/billing` page
- [ ] Team subscription overview:
  - [ ] Current plan and seats
  - [ ] Price per seat
  - [ ] Total monthly/annual cost
  - [ ] Next billing date
- [ ] Seat management:
  - [ ] Add/remove seats
  - [ ] Seat assignment
  - [ ] Unutilized seats warning
- [ ] Cost breakdown:
  - [ ] Base subscription
  - [ ] Additional seats
  - [ ] Add-ons
  - [ ] Taxes
- [ ] Billing contact management
- [ ] Consolidated invoices
- [ ] Usage-based billing (if applicable)

**Acceptance Criteria**:
- [ ] Team billing separate from personal
- [ ] Seat changes prorated
- [ ] Consolidated invoices generated
- [ ] Usage tracked per member
- [ ] Billing contact receives invoices
- [ ] Cost predictions accurate

---

### Epic 4.4: Usage Analytics
**What**: Team usage statistics and insights
**Complexity**: Medium
**Dependencies**: Epic 4.1 (Team Management), Resume-Account-API (MVP-4.4: Analytics)

**Tasks**:
- [ ] `/account/team/analytics` page
- [ ] Usage dashboard:
  - [ ] Active users chart
  - [ ] Resume creation trends
  - [ ] Export statistics
  - [ ] Template usage
  - [ ] Storage consumption
- [ ] Member analytics:
  - [ ] Activity per member
  - [ ] Productivity metrics
  - [ ] Login frequency
  - [ ] Feature adoption
- [ ] Time-series charts
- [ ] Comparison periods
- [ ] Export reports (PDF/CSV)
- [ ] Scheduled reports
- [ ] Custom date ranges

**Acceptance Criteria**:
- [ ] Analytics update daily
- [ ] Charts interactive and filterable
- [ ] Reports exportable
- [ ] Scheduled reports delivered via email
- [ ] Member privacy maintained
- [ ] Historical data retained (1 year)

---

### Epic 4.5: Audit Logs
**What**: Comprehensive audit trail for compliance
**Complexity**: Medium
**Dependencies**: Epic 3.2 (Activity Log), Resume-Account-API (MVP-4.5: Audit)

**Tasks**:
- [ ] `/account/team/audit` page
- [ ] Audit log table:
  - [ ] Timestamp (microsecond precision)
  - [ ] Actor (user/system)
  - [ ] Action performed
  - [ ] Resource affected
  - [ ] Before/after values
  - [ ] IP address
  - [ ] Session ID
- [ ] Advanced filters:
  - [ ] User filter
  - [ ] Action type
  - [ ] Resource type
  - [ ] Date range
  - [ ] Success/failure
- [ ] Audit log retention settings
- [ ] Compliance report generation
- [ ] Immutable log storage
- [ ] Digital signatures for entries

**Acceptance Criteria**:
- [ ] All team actions logged
- [ ] Logs immutable after creation
- [ ] Retention policy configurable
- [ ] Compliance reports meet standards
- [ ] Search performance < 2s
- [ ] Export supports legal discovery

---

## Critical Path (Build Order)

1. **MVP-1.1**: Authentication Integration ⟶ Foundation (blocks all features)
2. **MVP-1.2**: Account Dashboard ⟶ Central hub
3. **MVP-1.3**: Profile Management ⟶ User identity
4. **MVP-1.4**: Security Settings ⟶ Basic security
5. **MVP-1.5**: Email Preferences ⟶ Communication control
6. **MVP-1.6**: Responsive Design ⟶ Mobile support
7. **MVP-2.1**: Subscription Overview ⟶ Plan visibility
8. **MVP-2.2**: Payment Methods ⟶ Payment capability
9. **MVP-2.3**: Billing History ⟶ Transaction transparency
10. **MVP-2.4**: Invoice Management ⟶ Financial records
11. **MVP-2.5**: Plan Changes ⟶ Subscription flexibility
12. **MVP-3.1**: Two-Factor Auth ⟶ Enhanced security
13. **MVP-3.2**: Activity Log ⟶ Security monitoring
14. **MVP-3.3**: Session Management ⟶ Access control
15. **MVP-3.4**: API Keys ⟶ Integration capability
16. **MVP-3.5**: Account Recovery ⟶ Self-service recovery
17. **MVP-4.1**: Team Management ⟶ Multi-user foundation
18. **MVP-4.2**: RBAC ⟶ Permission control
19. **MVP-4.3**: Team Billing ⟶ Consolidated payments
20. **MVP-4.4**: Usage Analytics ⟶ Team insights
21. **MVP-4.5**: Audit Logs ⟶ Compliance tracking

---

## Service Dependencies

| Dependency Type | Service & MVP | Required For | Purpose |
|-----------------|---------------|--------------|---------|
| **Authentication** | Resume-SSO (MVP-1.4) | Epic 1.1: Auth Integration | User login and sessions |
| **Core Account** | Resume-Account-API (MVP-1) | MVP-1: Core Management | Profile, preferences, security |
| **Billing** | Resume-Account-API (MVP-2) | MVP-2: Subscription UI | Payments, invoices, plans |
| **Security** | Resume-Account-API (MVP-3) | MVP-3: Advanced Security | 2FA, sessions, API keys |
| **Teams** | Resume-Account-API (MVP-4) | MVP-4: Enterprise | Team management, RBAC |

**Internal Component Dependencies**:
| This Component | Required For | Reason |
|----------------|--------------|---------|
| MVP-1.1 (Authentication) | All account features | User context required |
| MVP-1.2 (Dashboard) | User experience | Central account hub |
| MVP-2.1 (Subscription Overview) | Billing operations | Plan visibility required |
| MVP-2.2 (Payment Methods) | Subscription changes | Payment capability needed |
| MVP-3.1 (2FA) | Security features | Enhanced authentication |
| MVP-4.1 (Team Management) | Team features | Multi-user foundation |
| MVP-4.2 (RBAC) | Team permissions | Access control needed |

**Integration Dependencies**:
| Integration | Purpose | Data Shared |
|-------------|---------|------------|
| Resume-UI → Account-UI | Navigation links | User context, session |
| Account-UI → Resume-UI | Return navigation | Subscription status |
| Shared Auth Context | SSO integration | JWT tokens, user data |

---

## Key Components Architecture

### Server Components (React 19)
```typescript
// Layout components (server-side)
- AccountLayout
- DashboardLayout
- SecurityLayout
- BillingLayout
- TeamLayout

// Data fetching components
- AccountDataProvider
- SubscriptionProvider
- TeamDataProvider
```

### Client Components
```typescript
// Interactive components
- ProfileForm
- PaymentMethodForm
- TwoFactorSetup
- TeamInviteModal
- SessionManager
- ActivityFeed

// State management (Zustand)
- useAccountStore
- useSubscriptionStore
- useTeamStore
- useSecurityStore
```

### API Integration (TanStack Query)
```typescript
// Query hooks
- useAccount()
- useSubscription()
- usePaymentMethods()
- useTeamMembers()
- useActivityLog()

// Mutation hooks
- useUpdateProfile()
- useChangePassword()
- useAddPaymentMethod()
- useInviteTeamMember()
- useEnable2FA()
```

---

## State Management Strategy

### Zustand Stores
```typescript
// Account Store
interface AccountStore {
  user: User | null
  profile: Profile | null
  preferences: Preferences
  updateProfile: (data: ProfileUpdate) => Promise<void>
  updatePreferences: (prefs: Preferences) => void
}

// Subscription Store
interface SubscriptionStore {
  subscription: Subscription | null
  paymentMethods: PaymentMethod[]
  invoices: Invoice[]
  updateSubscription: (plan: Plan) => Promise<void>
}

// Team Store
interface TeamStore {
  team: Team | null
  members: TeamMember[]
  roles: Role[]
  inviteMembers: (emails: string[]) => Promise<void>
  updateMemberRole: (id: string, role: string) => Promise<void>
}
```

### TanStack Query Configuration
```typescript
// Query client setup
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      gcTime: 10 * 60 * 1000, // 10 minutes
      retry: 3,
      refetchOnWindowFocus: false,
    },
    mutations: {
      retry: 2,
      onError: handleGlobalError,
    },
  },
})
```

---

## Integration with Resume-UI

### Shared Components
- Navigation header with account menu
- Subscription status badge
- Feature gates based on plan
- Shared authentication context
- Common UI components (via package)

### Navigation Flow
```typescript
// Resume-UI → Account-UI
- Account menu dropdown → Account dashboard
- Upgrade prompts → Subscription page
- Settings link → Account settings

// Account-UI → Resume-UI
- Logo/home → Resume dashboard
- Create resume → Resume editor
- View resumes → Resume list
```

### Data Sharing
- JWT token in httpOnly cookie
- User context via React Context
- Subscription status in localStorage (cached)
- Team context for enterprise users

---

## Performance Targets

### Core Web Vitals
- First Contentful Paint (FCP): < 1.2s
- Time to Interactive (TTI): < 2.5s
- Largest Contentful Paint (LCP): < 2s
- Cumulative Layout Shift (CLS): < 0.05
- First Input Delay (FID): < 80ms

### Application Metrics
- Initial bundle size: < 150KB (gzipped)
- Lazy load all route bundles
- Image optimization (WebP with fallbacks)
- Static page generation where possible
- Edge caching on Cloudflare

### API Performance
- API response time: < 200ms (p95)
- Optimistic updates for better UX
- Request batching where applicable
- Aggressive caching strategies
- Background data refresh

---

## Accessibility Requirements

### WCAG 2.1 AA Compliance
- Color contrast ratio: 4.5:1 minimum
- Focus indicators on all interactive elements
- Keyboard navigation for all features
- Screen reader announcements
- ARIA labels and descriptions
- Semantic HTML structure

### Specific Considerations
- Form validation messages announced
- Loading states communicated
- Error states clearly indicated
- Success messages confirmed
- Modal focus management
- Skip navigation links

---

## Testing Strategy

### Unit Tests (Vitest)
- Component rendering tests
- Form validation logic
- State management stores
- Utility functions
- API response handlers
- Coverage target: > 80%

### Integration Tests (Testing Library)
- Authentication flow
- Profile update flow
- Payment method addition
- Team member invitation
- 2FA setup process

### E2E Tests (Playwright)
- Critical user journeys:
  - Login → Profile update → Save
  - Subscription upgrade flow
  - Team member invitation flow
  - 2FA enable/disable
  - Payment method management

### Performance Tests
- Lighthouse CI on every build
- Bundle size monitoring
- Runtime performance profiling
- Memory leak detection
- API response time tracking

---

## Security Considerations

### Data Protection
- PCI compliance for payment data
- No payment card numbers stored
- Sensitive data encrypted at rest
- API keys hashed with bcrypt
- Session tokens in httpOnly cookies

### Security Headers
```typescript
// Next.js security headers
{
  'Content-Security-Policy': "default-src 'self'",
  'X-Frame-Options': 'DENY',
  'X-Content-Type-Options': 'nosniff',
  'Referrer-Policy': 'strict-origin-when-cross-origin',
  'Permissions-Policy': 'camera=(), microphone=(), geolocation=()'
}
```

### Rate Limiting
- Login attempts: 5 per minute
- API key generation: 10 per day
- Password changes: 3 per hour
- Team invitations: 50 per day

---

## Environment Variables

```bash
# API Configuration
NEXT_PUBLIC_API_URL=https://account-api.resumebuilder.com
NEXT_PUBLIC_SSO_URL=https://sso.resumebuilder.com
NEXT_PUBLIC_APP_URL=https://app.resumebuilder.com

# Stripe Configuration
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=<stripe-publishable-key>

# Feature Flags
NEXT_PUBLIC_ENABLE_TEAMS=true
NEXT_PUBLIC_ENABLE_2FA=true
NEXT_PUBLIC_ENABLE_API_KEYS=true
NEXT_PUBLIC_ENABLE_AUDIT_LOGS=true

# Analytics
NEXT_PUBLIC_GA_TRACKING_ID=<google-analytics-id>
NEXT_PUBLIC_SENTRY_DSN=<sentry-dsn>

# Cloudflare
NEXT_PUBLIC_CLOUDFLARE_ACCOUNT_ID=<account-id>
NEXT_PUBLIC_CLOUDFLARE_SITE_ID=<site-id>
```

---

## Deployment Configuration

### Cloudflare Pages Setup
```yaml
# Build configuration
build:
  command: npm run build
  directory: .next
  environment:
    - NODE_VERSION: 20

# Routes
routes:
  - /account/*: serve from Pages
  - /api/*: proxy to Account-API Worker

# Headers
headers:
  /*:
    Cache-Control: public, max-age=3600
    X-Content-Type-Options: nosniff
```

---

## Success Metrics

- [ ] 100% test coverage for critical paths
- [ ] Lighthouse Performance Score > 95
- [ ] Accessibility Score > 95
- [ ] < 2s page load time (p95)
- [ ] < 3% error rate
- [ ] > 98% uptime SLA
- [ ] Zero security vulnerabilities
- [ ] Mobile usage > 30% of total
- [ ] User satisfaction score > 4.5/5
- [ ] Support ticket reduction > 40%