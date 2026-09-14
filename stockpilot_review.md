# StockPilot — Architectural & Feature Review

---

## 1. Current Level: **Early Prototype (~25% complete)**

The project has a solid **database schema** and a handful of working API routes, but the frontend is largely scaffolded with empty pages. Here's the breakdown:

| Layer                | Status         | Notes                                                                   |
| -------------------- | -------------- | ----------------------------------------------------------------------- |
| **Database Schema**  | ✅ Mature      | 14 models, 7 enums, proper relations, indexes, cascades                 |
| **API Routes**       | 🟡 Partial     | 6/9 domains have routes; only inventory has advanced filtering          |
| **Auth System**      | 🟡 Partial     | Login/Register work, but no middleware, no session, no route protection |
| **Frontend Pages**   | 🔴 Skeletal    | 5 of 6 dashboard pages are **completely empty** (0 bytes)               |
| **UI Components**    | 🟡 Decent Base | 12 reusable components exist, but no layout shell (sidebar, navbar)     |
| **State Management** | 🔴 Empty       | `store/` and `hooks/` directories exist but are empty                   |
| **Testing**          | 🔴 None        | Zero test files, no test runner configured                              |
| **AI Features**      | 🔴 None        | Despite the "AI-Powered" project name, no AI integration exists         |
| **Payments/Stripe**  | 🔴 None        | `lib/stripe/` is empty, no Stripe SDK installed                         |

> [!IMPORTANT]
> **The .env file is committed with real database credentials and JWT secrets exposed.** This is a critical security issue that should be fixed immediately — rotate the Supabase password, move `.env` out of version control, and use `.env.example` with placeholder values.

---

## 2. What Is Better: Strong Design Patterns

### ✅ Prisma Schema — Production-Grade Data Model

The [schema.prisma](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/prisma/schema.prisma) is the strongest part of this project:

- **Comprehensive domain modeling**: User → Order → OrderItem → Product → Inventory → Warehouse, with StockMovements, Payments, AuditLog, Tags, Keywords, Categories
- **Proper use of `@@unique` composites** (e.g., `[warehouseId, productId]` on Inventory)
- **Strategic indexes** on all foreign keys for query performance
- **Cascading deletes** where appropriate (OrderItem, Inventory, ProductImage)
- **Enum-driven state machines**: `OrderStatus`, `PaymentStatus`, `StockMovementType`, `StockReservationStatus` — much better than magic strings
- **Audit trail built-in**: `AuditLog` model with actor, action, entity, and metadata JSON
- **`@@map` table naming** for clean PostgreSQL table names

### ✅ Stock Movements API — Best-in-Class Route

[stock-movements/route.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/api/stock-movements/route.ts) is the most mature API in the project:

- **Transactional writes** with `prisma.$transaction()` and configurable timeouts
- **E-commerce reservation logic** that correctly separates physical stock from reserved stock
- **Negative stock validation** preventing overselling
- **Granular error handling**: Distinguishes P2028 (timeout), P2003 (FK violation), business logic errors, and generic 500s
- **Clean GET with pagination meta** returning `{ data, meta: { totalCount, currentPage, totalPages } }`

### ✅ Inventory API — Smart Filtering Pattern

[inventory/route.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/api/inventory/route.ts):

- **Dynamic `whereClause` builder** that composes Prisma filters based on query params
- **Nested product filtering** (search by name/sku across related Product model)
- **Stock status enum** mapping (`OUT_OF_STOCK`, `IN_STOCK`, `LOW_STOCK`) to Prisma range queries
- **Transactional count + findMany** for consistent pagination

### ✅ Reusable UI Component Library

Several `components/ui/` files show strong design thinking:

- [SearchInput.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/SearchInput.tsx) — **Excellently documented** debounced search with `forwardRef`, `useImperativeHandle`, render-phase sync, and a11y (`aria-label`, `aria-hidden`)
- [DataTable.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/DataTable.tsx) — Clean generic `<DataTable<T>>` with typed columns and optional pagination
- [StatCard.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/StatCard.tsx) — Skeleton loading, trend indicators with dynamic icons, proper dark mode
- [StatusBadge.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/StatusBadge.tsx) — Semantic badge with configurable icon, size, status, and accessibility (`role="status"`)
- [Modal.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/Modal.tsx) — Dynamic form generation from field definitions, Escape-key dismiss, backdrop blur
- [InputBox.module.css](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/InputBox.module.css) — Beautiful glassmorphism input with floating label animation and autofill fix

### ✅ Prisma Singleton Pattern

[db.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/lib/db/db.ts) correctly prevents connection pool exhaustion in development with the `globalThis` singleton pattern.

---

## 3. What You Need to Complete: Critical Gaps

### 🔴 P0 — Security (Fix Immediately)

| Issue                                                  | Location                                                                                                                                                                                          | Fix                                                                        |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | --------------------------------- | ------- | ------ |
| **Credentials in `.env` committed to git**             | [.env](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/.  env)                                                                                                                           | Rotate Supabase password, use `.env.example` with placeholders             |
| **JWT secret is weak** (`"InventoryManagementSystem"`) | `.env` L26                                                                                                                                                                                        | Use a cryptographically random 256-bit secret                              |
| **No JWT verification function**                       | [jwt.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/lib/auth/jwt.ts)                                                                                                              | Only `generateToken` exists; `verifyToken` is missing                      |
| **No auth middleware**                                 | Entire project                                                                                                                                                                                    | All API routes and dashboard pages are publicly accessible                 |
| **Token never stored on client**                       | [LoginForm.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/forms/LoginForm.tsx) L37                                                                                    | Login succeeds but the JWT is discarded; never saved to cookies or headers |
| **Login returns wrong status** for bad password        | [login/route.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/api/auth/login/route.ts) L23                                                                                      | Returns 404 instead of 401 Unauthorized                                    |
| **Password not validated on register**                 | [register/route.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/api/auth/register/route.ts)                                                                                    | No minimum length, complexity, or input sanitization                       |
| **Role mismatch**                                      | [jwt.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/lib/auth/jwt.ts) L20 vs [schema.prisma](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/prisma/schema.prisma) L250 | JWT defines `'superadmin'                                                  | 'guest'`but Prisma enum has`Admin | Manager | Staff` |

### 🔴 P1 — Core Missing Logic

| Issue                                    | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **No dashboard layout**                  | `components/layout/` is empty — no sidebar, navbar, breadcrumbs, or user menu                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **5 empty dashboard pages**              | [inventory](<file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/(dashboard)/inventory/page.tsx>), [products](<file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/(dashboard)/products/page.tsx>), [orders](<file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/(dashboard)/orders/page.tsx>), [analytics](<file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/(dashboard)/analytics/page.tsx>), [settings](<file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/(dashboard)/settings/page.tsx>) are all 0 bytes |
| **No orders API**                        | `app/api/orders/` is an empty directory — no CRUD                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **No payments API**                      | `app/api/payments/` is an empty directory — no Stripe/webhook integration                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| **No analytics API**                     | `app/api/analytics/` is empty                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Empty features layer**                 | `features/auth/login.ts` and `features/auth/register.ts` are 0-byte files; `features/inventory/`, `features/orders/`, `features/payments/`, `features/products/`, `features/analytics/` are all empty                                                                                                                                                                                                                                                                                                                                                                      |
| **Empty store**                          | `store/` directory is empty — no Zustand, Redux, or Context state management                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **Empty hooks**                          | `hooks/` directory is empty — no custom hooks for data fetching, auth, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Empty types**                          | `types/` directory is empty — no shared TypeScript types                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Empty styles**                         | `styles/` directory is empty                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **Hardcoded localhost URL**              | [InventoryTable.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/inventory/InventoryTable.tsx) L63 uses `http://localhost:3000` instead of relative path                                                                                                                                                                                                                                                                                                                                                                                         |
| **URL query param bug**                  | [InventoryTable.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/inventory/InventoryTable.tsx) L63: `limit${limit}` is missing `=` sign → should be `limit=${limit}`                                                                                                                                                                                                                                                                                                                                                                             |
| **Category PATCH uses wrong field**      | [categories/[id]/route.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/app/api/categories/%5Bid%5D/route.ts) L43 references `parent_category_id` but the Prisma schema uses `parentCategoryId`                                                                                                                                                                                                                                                                                                                                                              |
| **Console.log left in production code**  | Login, register, and product routes have `console.log(data)` / `console.log(checkUser)` statements                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **Proxy.ts is empty**                    | [Proxy.ts](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/Proxy.ts) — 0 bytes, unclear purpose                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **No input validation library**          | `lib/validations/` is empty; no Zod, Yup, or any schema validation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **PasswordBox uses React.createElement** | [PasswordBox.tsx](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/components/ui/PasswordBox.tsx) uses imperative `React.createElement` instead of JSX — inconsistent with the rest of the codebase                                                                                                                                                                                                                                                                                                                                                              |
| **No error boundary**                    | No React Error Boundary to catch rendering failures                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **No loading states / Suspense**         | No loading.tsx or Suspense boundaries for streaming                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **tsconfig has trailing artifact**       | [tsconfig.json](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/tsconfig.json) L32 has a manually appended file in the `include` array                                                                                                                                                                                                                                                                                                                                                                                                                          |

### 🟡 P2 — Missing Infrastructure

| Item                      | Status                                                                                                                            |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Testing framework**     | No Jest/Vitest configured, zero test files                                                                                        |
| **CI/CD pipeline**        | No GitHub Actions, no Vercel config                                                                                               |
| **Linting/Formatting**    | ESLint exists but no Prettier, no pre-commit hooks                                                                                |
| **API documentation**     | No Swagger/OpenAPI spec                                                                                                           |
| **Error logging service** | No Sentry, no structured logging                                                                                                  |
| **Rate limiting**         | No rate limiting on auth endpoints                                                                                                |
| **CORS configuration**    | Default Next.js CORS (no custom config)                                                                                           |
| **next.config.ts**        | [Completely empty](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/next.config.ts) — no image domains, no redirects    |
| **README**                | [Default create-next-app README](file:///d:/Self%20Projects/Ai-Powered-Dashboard/stockpilot/README.md) — no project-specific docs |

---

## 4. Components & APIs to Create: Full Roadmap

### Phase 1: Security & Auth Foundation 🔐

```
Priority: CRITICAL — do this first
```

| Task                                    | Type    | Details                                                                    |
| --------------------------------------- | ------- | -------------------------------------------------------------------------- |
| `lib/auth/jwt.ts` → add `verifyToken()` | Backend | Decode + verify JWT, return typed payload or throw                         |
| `middleware.ts` (root)                  | Backend | Next.js middleware to protect `/dashboard/*` routes and `/api/*` endpoints |
| Auth cookie/session management          | Backend | Set `httpOnly` cookie on login, read from cookie in middleware             |
| `lib/validations/auth.ts`               | Backend | Zod schemas for login/register input validation                            |
| Role-based access control               | Backend | Middleware checks `role` claim for Admin-only routes                       |
| `.env.example`                          | Config  | Template file with placeholder values for all env vars                     |

---

### Phase 2: Dashboard Shell & Layout 🏗️

| Component                | Location                                | Details                                                                           |
| ------------------------ | --------------------------------------- | --------------------------------------------------------------------------------- |
| `DashboardLayout`        | `components/layout/DashboardLayout.tsx` | Sidebar + topbar + main content area wrapper                                      |
| `Sidebar`                | `components/layout/Sidebar.tsx`         | Navigation links with icons (lucide-react), active state, collapsible             |
| `Topbar`                 | `components/layout/Topbar.tsx`          | User avatar, notifications bell, search, role badge                               |
| `Breadcrumbs`            | `components/layout/Breadcrumbs.tsx`     | Auto-generated from route path                                                    |
| `(dashboard)/layout.tsx` | `app/(dashboard)/layout.tsx`            | Route group layout wrapping all dashboard pages with auth check + DashboardLayout |
| `(auth)/layout.tsx`      | `app/(auth)/layout.tsx`                 | Centered card layout for login/register                                           |

---

### Phase 3: Core Feature Pages 📄

#### Dashboard Home

| Task                                      | Details                                                                          |
| ----------------------------------------- | -------------------------------------------------------------------------------- |
| `(dashboard)/dashboard/page.tsx`          | Overview with StatCards: total products, total orders, revenue, low-stock alerts |
| `components/charts/RevenueChart.tsx`      | Line/area chart (use recharts or chart.js)                                       |
| `components/charts/OrdersChart.tsx`       | Bar chart of orders by status                                                    |
| `components/charts/InventoryPieChart.tsx` | Pie chart of stock status distribution                                           |
| Recent activity feed                      | Last 10 stock movements / orders                                                 |

#### Inventory Page

| Task                             | Details                                                                        |
| -------------------------------- | ------------------------------------------------------------------------------ |
| `(dashboard)/inventory/page.tsx` | Wire up existing `InventoryTable` + `InventoryStats`, add SearchInput, filters |
| Inventory adjustment modal       | Use `DynamicModal` to record stock IN/OUT/ADJUSTMENT                           |
| Low stock alerts list            | Filter inventory where `quantityOnHand <= reorderLevel`                        |
| Export to CSV                    | Download inventory snapshot                                                    |

#### Products Page

| Task                               | Details                                                                                    |
| ---------------------------------- | ------------------------------------------------------------------------------------------ |
| `(dashboard)/products/page.tsx`    | Product listing with DataTable, search, category filter                                    |
| `components/forms/ProductForm.tsx` | Create/Edit product form with image upload                                                 |
| Product detail view                | `(dashboard)/products/[id]/page.tsx` — show full product info, inventory across warehouses |
| Bulk actions                       | Select multiple products → activate/deactivate/delete                                      |

#### Orders Page

| Task                             | Details                                                                     |
| -------------------------------- | --------------------------------------------------------------------------- |
| `(dashboard)/orders/page.tsx`    | Order listing with status filters, search by order number                   |
| `components/forms/OrderForm.tsx` | Create order → select products, auto-calculate totals                       |
| Order detail view                | `(dashboard)/orders/[id]/page.tsx` — items, status timeline, payment status |
| Order status updates             | Buttons to transition: Pending → Processing → Shipped → Delivered           |

#### Analytics Page

| Task                             | Details                                          |
| -------------------------------- | ------------------------------------------------ |
| `(dashboard)/analytics/page.tsx` | Revenue trends, top products, inventory turnover |
| Date range picker                | Filter all analytics by custom period            |
| Export reports                   | PDF/CSV generation                               |

#### Settings Page

| Task                            | Details                                           |
| ------------------------------- | ------------------------------------------------- |
| `(dashboard)/settings/page.tsx` | Tab-based: Profile, Warehouses, Categories, Users |
| Warehouse management            | CRUD table using existing API                     |
| Category management             | Tree view for hierarchical categories             |
| User management (Admin only)    | CRUD users, assign roles, suspend accounts        |

---

### Phase 4: Missing API Endpoints 🔌

| Endpoint                          | Methods                   | Notes                                                        |
| --------------------------------- | ------------------------- | ------------------------------------------------------------ |
| `api/orders/route.ts`             | GET (list), POST (create) | With pagination, status filtering, auto-generate orderNumber |
| `api/orders/[id]/route.ts`        | GET, PATCH, DELETE        | Include items and payment status                             |
| `api/orders/[id]/status/route.ts` | PATCH                     | Dedicated status transition endpoint with validation         |
| `api/payments/route.ts`           | POST (create checkout)    | Stripe integration for payment processing                    |
| `api/payments/webhook/route.ts`   | POST                      | Stripe webhook handler to update PaymentStatus               |
| `api/analytics/route.ts`          | GET                       | Revenue, order count, top products, stock metrics            |
| `api/analytics/revenue/route.ts`  | GET                       | Revenue over time with date range filter                     |
| `api/users/route.ts`              | GET, POST                 | Admin: list users, create users                              |
| `api/users/[id]/route.ts`         | GET, PATCH, DELETE        | Admin: manage individual users                               |
| `api/audit-logs/route.ts`         | GET                       | Query audit trail with entity/actor filtering                |

---

### Phase 5: State Management & Hooks 🪝

| Item                    | Location                          | Purpose                                                                     |
| ----------------------- | --------------------------------- | --------------------------------------------------------------------------- |
| Auth context/store      | `store/authStore.ts`              | Zustand store for user session, role, token                                 |
| `useAuth` hook          | `hooks/useAuth.ts`                | Login, logout, check session, get user                                      |
| `useFetch` hook         | `hooks/useFetch.ts`               | Generic data fetching with loading/error states (or use SWR/TanStack Query) |
| `useDebounce` hook      | `hooks/useDebounce.ts`            | Extract debounce logic from SearchInput for reuse                           |
| Shared TypeScript types | `types/api.ts`, `types/models.ts` | Shared interfaces derived from Prisma schema                                |

---

### Phase 6: Polish & Production ✨

| Item                | Details                                                              |
| ------------------- | -------------------------------------------------------------------- |
| Toast notifications | Success/error toasts for all CRUD operations (replace `console.log`) |
| Loading skeletons   | Skeleton screens for every data-loading page                         |
| Error boundaries    | `error.tsx` and `not-found.tsx` at route group level                 |
| Responsive design   | Mobile sidebar collapse, responsive tables                           |
| Dark mode toggle    | Manual toggle instead of only `prefers-color-scheme`                 |
| SEO metadata        | Proper `<title>` and `<meta>` per page                               |
| Accessibility audit | Keyboard navigation, ARIA labels, focus management                   |
| Performance         | Image optimization config in next.config.ts, lazy loading            |

---

## File Tree Summary — What Exists vs. What's Needed

```
stockpilot/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx          ✅ Working
│   │   ├── register/page.tsx       ✅ Working
│   │   └── layout.tsx              ❌ MISSING
│   ├── (dashboard)/
│   │   ├── layout.tsx              ❌ MISSING (critical)
│   │   ├── dashboard/page.tsx      🟡 Placeholder only
│   │   ├── inventory/page.tsx      ❌ Empty (0 bytes)
│   │   ├── products/page.tsx       ❌ Empty (0 bytes)
│   │   ├── orders/page.tsx         ❌ Empty (0 bytes)
│   │   ├── analytics/page.tsx      ❌ Empty (0 bytes)
│   │   └── settings/page.tsx       ❌ Empty (0 bytes)
│   ├── api/
│   │   ├── auth/login/route.ts     ✅ Working
│   │   ├── auth/register/route.ts  ✅ Working
│   │   ├── inventory/route.ts      ✅ Advanced (filters + pagination)
│   │   ├── products/route.ts       ✅ CRUD
│   │   ├── products/[id]/route.ts  ✅ GET/PATCH/DELETE
│   │   ├── categories/route.ts     ✅ CRUD
│   │   ├── categories/[id]/route.ts ✅ PATCH/DELETE (has bug)
│   │   ├── warehouse/route.ts      ✅ CRUD
│   │   ├── warehouse/[id]/route.ts ✅ GET/PATCH/DELETE
│   │   ├── stock-movements/route.ts ✅ Advanced (transactions)
│   │   ├── orders/                  ❌ Empty
│   │   ├── payments/                ❌ Empty
│   │   └── analytics/               ❌ Empty
│   └── middleware.ts               ❌ MISSING
├── components/
│   ├── charts/                     ❌ Empty
│   ├── forms/
│   │   ├── LoginForm.tsx           ✅
│   │   └── RegisterForm.tsx        ✅
│   ├── inventory/
│   │   ├── InventoryTable.tsx      ✅ (has bugs)
│   │   └── InventoryStats.tsx      🟡 Shell only
│   ├── layout/                     ❌ Empty (critical)
│   ├── tables/                     ❌ Empty
│   └── ui/ (12 components)         ✅ Solid base
├── features/                       ❌ All empty
├── hooks/                          ❌ Empty
├── lib/
│   ├── auth/jwt.ts                 🟡 generateToken only
│   ├── db/db.ts                    ✅
│   ├── constants/                  ❌ Empty
│   ├── helpers/                    ❌ Empty
│   ├── stripe/                     ❌ Empty
│   └── validations/                ❌ Empty
├── store/                          ❌ Empty
├── styles/                         ❌ Empty
├── types/                          ❌ Empty
└── prisma/schema.prisma            ✅ Production-grade
```

> [!TIP]
> **Recommended build order:** Security (Phase 1) → Layout Shell (Phase 2) → Inventory page (Phase 3, easiest since APIs exist) → Products page → Orders APIs + page → Analytics → Settings → Polish.
