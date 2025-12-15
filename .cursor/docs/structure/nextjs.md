# Front-End Project Structure Template

This document provides a comprehensive template for organizing a Next.js front-end project. **IMPORTANT**: Only install libraries and create folders when you have a specific need for them. Do not add dependencies or setup files "just in case" - start minimal and add as needed.

## 📁 Complete Directory Structure

```
front-end/
├── app/                                    # Next.js App Router (required)
│   ├── dashboard/                          # Protected dashboard routes
│   │   ├── displays/                       # Feature route example
│   │   │   ├── [displaySlug]/             # Dynamic route segment
│   │   │   │   └── page.tsx
│   │   │   ├── new/                        # Create new item route
│   │   │   │   └── page.tsx
│   │   │   └── page.tsx                   # List/index page
│   │   ├── my-profile/
│   │   │   └── page.tsx
│   │   ├── planograms/
│   │   │   └── page.tsx
│   │   ├── projects/
│   │   │   ├── [projectSlug]/             # Dynamic route with nested pages
│   │   │   │   └── [multiple files]
│   │   │   ├── new/
│   │   │   │   └── page.tsx
│   │   │   └── page.tsx
│   │   ├── stores/
│   │   │   ├── [storeSlug]/
│   │   │   │   └── [files]
│   │   │   ├── new/
│   │   │   │   └── page.tsx
│   │   │   └── page.tsx
│   │   ├── users/
│   │   │   ├── [userSlug]/
│   │   │   │   └── page.tsx
│   │   │   ├── invite/
│   │   │   │   └── page.tsx
│   │   │   └── page.tsx
│   │   ├── layout.tsx                     # Dashboard layout wrapper
│   │   └── page.tsx                       # Dashboard home page
│   ├── login/                              # Authentication routes
│   │   └── page.tsx
│   ├── register/
│   │   └── page.tsx
│   ├── layout.tsx                         # Root layout (required)
│   ├── page.tsx                            # Home/landing page (required)
│   ├── providers.tsx                       # Global providers wrapper
│   ├── globals.css                         # Global styles (required if using CSS)
│   └── favicon.ico                         # Site favicon
│
├── components/                             # Shared/reusable components
│   ├── ui/                                 # ⚠️ OPTIONAL: shadcn/ui components
│   │   ├── alert-dialog.tsx
│   │   ├── alert.tsx
│   │   ├── badge.tsx
│   │   ├── breadcrumb.tsx
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── checkbox.tsx
│   │   ├── dialog.tsx
│   │   ├── dropdown-menu.tsx
│   │   ├── form-field.tsx
│   │   ├── form-select-field.tsx
│   │   ├── input.tsx
│   │   ├── label.tsx
│   │   ├── popover.tsx
│   │   ├── radio-group.tsx
│   │   ├── select.tsx
│   │   ├── separator.tsx
│   │   └── textarea.tsx
│   ├── AdminOnly.tsx                       # Permission-based components
│   ├── AuthenticatedLayout.tsx             # Auth wrapper component
│   ├── AuthProvider.tsx                    # Auth context provider
│   ├── Breadcrumbs.tsx                     # Navigation breadcrumbs
│   ├── DetailField.tsx                     # Reusable detail display
│   ├── EmptyState.tsx                      # Empty state component
│   ├── InfoAlert.tsx                       # Alert/notification component
│   ├── Navbar.tsx                          # Navigation bar
│   ├── ThemeToggle.tsx                     # Theme switcher (if using themes)
│   └── UserMenu.tsx                        # User menu dropdown
│
├── features/                               # Feature-based organization (recommended)
│   ├── auth/                               # Authentication feature
│   │   ├── components/                     # Feature-specific components
│   │   │   ├── index.ts                    # Barrel export
│   │   │   └── login-form/                 # Component folder pattern
│   │   │       └── login-form.tsx
│   │   ├── hooks/                          # Feature-specific hooks
│   │   │   ├── index.ts
│   │   │   ├── use-is-admin.ts
│   │   │   └── use-require-admin.ts
│   │   ├── queries/                        # ⚠️ OPTIONAL: React Query hooks
│   │   │   ├── index.ts
│   │   │   ├── use-current-user-query.ts
│   │   │   ├── use-login-mutation.ts
│   │   │   ├── use-logout-mutation.ts
│   │   │   ├── use-register-mutation.ts
│   │   │   └── use-update-username-mutation.ts
│   │   ├── store/                          # ⚠️ OPTIONAL: Zustand store
│   │   │   ├── auth-slice.ts
│   │   │   └── index.ts
│   │   ├── types.ts                        # Feature TypeScript types
│   │   └── index.ts                        # Public exports
│   ├── dashboard/                          # Dashboard feature
│   │   ├── components/
│   │   │   ├── index.ts
│   │   │   ├── dashboard-content/
│   │   │   │   └── [component files]
│   │   │   └── dashboard-header/
│   │   │       └── [component files]
│   │   └── index.ts
│   ├── displays/                           # Displays feature
│   │   ├── components/
│   │   │   ├── display-card/
│   │   │   ├── display-detail/
│   │   │   ├── display-form/
│   │   │   ├── display-list/
│   │   │   ├── display-selector/
│   │   │   └── index.ts
│   │   ├── queries/                        # ⚠️ OPTIONAL: React Query
│   │   │   ├── use-create-display-mutation.ts
│   │   │   ├── use-delete-display-mutation.ts
│   │   │   ├── use-display-query.ts
│   │   │   ├── use-display-types-query.ts
│   │   │   ├── use-displays-query.ts
│   │   │   ├── use-standard-displays-query.ts
│   │   │   └── index.ts
│   │   ├── types.ts
│   │   └── index.ts
│   ├── planogram/                          # Planogram feature
│   │   ├── components/                     # Many component folders
│   │   │   ├── ai-overview-dialog/
│   │   │   ├── available-products-sidebar/
│   │   │   ├── category-select/
│   │   │   ├── grid/
│   │   │   ├── item-menu/
│   │   │   ├── name-input/
│   │   │   ├── planogram-actions/
│   │   │   ├── planogram-card/
│   │   │   ├── planogram-categories-selector/
│   │   │   ├── planogram-delete-button/
│   │   │   ├── planogram-download-button/
│   │   │   ├── planogram-form-fields/
│   │   │   ├── planogram-header/
│   │   │   ├── planogram-name-field/
│   │   │   ├── product-sidebar/
│   │   │   ├── project-display/
│   │   │   ├── row-header/
│   │   │   ├── shelves-table/
│   │   │   ├── three-js-view/              # ⚠️ OPTIONAL: Three.js component
│   │   │   └── index.ts
│   │   ├── hooks/
│   │   │   ├── index.ts
│   │   │   ├── use-grid-actions.ts
│   │   │   ├── use-planogram-data.ts
│   │   │   ├── use-planogram-form.ts
│   │   │   └── use-planogram-layout.ts
│   │   ├── queries/                        # ⚠️ OPTIONAL: React Query
│   │   │   └── [multiple query files]
│   │   ├── store/                          # ⚠️ OPTIONAL: Zustand
│   │   │   ├── planogram-slice.ts
│   │   │   └── index.ts
│   │   ├── types.ts
│   │   └── index.ts
│   ├── projects/                           # Projects feature
│   │   ├── components/
│   │   │   └── [component files]
│   │   ├── queries/                        # ⚠️ OPTIONAL: React Query
│   │   │   └── [query files]
│   │   ├── types.ts
│   │   └── index.ts
│   ├── stores/                             # Stores feature
│   │   ├── components/
│   │   │   ├── store-card/
│   │   │   ├── store-detail/
│   │   │   ├── store-form/
│   │   │   ├── store-list/
│   │   │   └── index.ts
│   │   ├── queries/                        # ⚠️ OPTIONAL: React Query
│   │   │   └── [query files]
│   │   ├── types.ts
│   │   └── index.ts
│   └── users/                              # Users feature
│       ├── components/
│       │   └── [component files]
│       ├── queries/                        # ⚠️ OPTIONAL: React Query
│       │   └── [query files]
│       ├── types.ts
│       └── index.ts
│
├── hooks/                                  # Global/shared React hooks
│   └── useCategories.ts                    # Example: shared hook
│
├── lib/                                    # Utility libraries and configurations
│   ├── axios.ts                            # ⚠️ OPTIONAL: Axios HTTP client setup
│   ├── utils.ts                            # General utility functions
│   ├── utils.test.ts                       # ⚠️ OPTIONAL: Tests (if using Vitest)
│   ├── navigation.ts                       # Navigation helpers
│   ├── planogramCSV.ts                     # Feature-specific utilities
│   ├── planogramCSV.test.ts                # ⚠️ OPTIONAL: Tests
│   ├── generated/                          # ⚠️ OPTIONAL: Auto-generated files
│   │   └── api-schemas.ts                  # Only if using OpenAPI code generation
│   ├── react-query/                        # ⚠️ OPTIONAL: React Query setup
│   │   ├── client.ts                       # QueryClient configuration
│   │   ├── hooks.ts                        # Custom query hook utilities
│   │   └── index.ts
│   ├── zustand/                            # ⚠️ OPTIONAL: Zustand utilities
│   │   └── create-selectors.ts             # Selector helper function
│   └── types/                              # Shared TypeScript types
│       └── index.ts
│
├── stores/                                 # ⚠️ OPTIONAL: Global Zustand stores
│   └── themeStore.ts                       # Example: theme management store
│
├── types/                                  # Global TypeScript types
│   └── categories.ts                       # Example: shared types
│
├── public/                                 # Static assets
│   ├── file.svg
│   ├── globe.svg
│   ├── next.svg
│   ├── undraw_groceries_4via.png
│   ├── undraw_groceries_4via.svg
│   ├── vercel.svg
│   └── window.svg
│
├── package.json                            # Dependencies and scripts (required)
├── package-lock.json                       # Lock file (required)
├── tsconfig.json                           # TypeScript configuration (required)
├── next.config.ts                          # Next.js configuration (required)
├── next-env.d.ts                           # Next.js TypeScript declarations (auto-generated)
├── components.json                         # ⚠️ OPTIONAL: shadcn/ui configuration
├── openapi-zod-client.config.ts            # ⚠️ OPTIONAL: OpenAPI code generation config
├── vitest.config.ts                        # ⚠️ OPTIONAL: Vitest test configuration
├── vitest.setup.ts                         # ⚠️ OPTIONAL: Vitest setup file
├── postcss.config.mjs                      # ⚠️ OPTIONAL: PostCSS config (if using Tailwind)
├── eslint.config.mjs                       # ESLint configuration (recommended)
└── README.md                               # Project documentation
```

## 🎯 Core Structure (Always Needed)

These folders and files are essential for a Next.js project:

- `app/` - Next.js App Router directory
- `components/` - Shared components (at least basic structure)
- `public/` - Static assets
- `package.json` - Dependencies
- `tsconfig.json` - TypeScript config
- `next.config.ts` - Next.js config

## ⚠️ Optional Libraries & When to Use Them

### 1. **React Query** (`@tanstack/react-query`)

**When to use**: Only if you need server state management, caching, background updates, and synchronization with your backend API.

**Required setup**:

- `lib/react-query/client.ts` - QueryClient configuration
- `lib/react-query/hooks.ts` - Custom hook utilities (optional)
- `app/providers.tsx` - QueryClientProvider wrapper
- `features/[feature]/queries/` - Query hooks in each feature

**If NOT using**:

- Remove `@tanstack/react-query` and `@tanstack/react-query-devtools` dependencies
- Delete `lib/react-query/` folder
- Remove QueryClientProvider from `app/providers.tsx`
- Delete all `queries/` folders in features
- Use fetch API or axios directly instead

---

### 2. **Zustand** (`zustand`)

**When to use**: Only if you need global client-side state management that's simpler than Redux.

**Required setup**:

- `lib/zustand/create-selectors.ts` - Selector helper (optional utility)
- `stores/[store-name].ts` - Global stores (only if needed)
- `features/[feature]/store/` - Feature-specific stores (only if needed)

**If NOT using**:

- Remove `zustand` dependency
- Delete `lib/zustand/` folder
- Delete `stores/` folder
- Remove all feature `store/` folders
- Use React Context API or component state instead

---

### 3. **shadcn/ui** (Radix UI + Tailwind CSS)

**When to use**: Only if you want a customizable component library built on Radix UI primitives with Tailwind styling.

**Required setup**:

- `components.json` - shadcn configuration file
- `components/ui/` - UI component primitives (add only components you need)
- Dependencies: `@radix-ui/*`, `tailwindcss`, `class-variance-authority`, `clsx`, `tailwind-merge`

**If NOT using**:

- Remove all `@radix-ui/*` dependencies
- Delete `components.json`
- Delete `components/ui/` folder
- Use a different UI library or build custom components

---

### 4. **Axios** (`axios`)

**When to use**: Only if you prefer axios over the native fetch API for HTTP requests.

**Required setup**:

- `lib/axios.ts` - Axios instance with interceptors and configuration

**If NOT using**:

- Remove `axios` dependency
- Delete `lib/axios.ts`
- Use native `fetch()` API or your chosen HTTP client

---

### 5. **Vitest** (`vitest`)

**When to use**: Only if you need unit testing, integration testing, or component testing.

**Required setup**:

- `vitest.config.ts` - Vitest configuration
- `vitest.setup.ts` - Test setup file (mocks, global config)
- Additional: `@testing-library/react`, `@testing-library/jest-dom`, `jsdom`

**If NOT using**:

- Remove `vitest` and related testing dependencies
- Delete `vitest.config.ts` and `vitest.setup.ts`
- Delete all `*.test.ts` and `*.test.tsx` files

---

### 6. **OpenAPI Code Generation** (`openapi-zod-client`)

**When to use**: Only if you have an OpenAPI/Swagger schema and want to generate TypeScript types and API clients automatically.

**Required setup**:

- `openapi-zod-client.config.ts` - Code generation configuration
- `lib/generated/api-schemas.ts` - Generated types (auto-generated, don't edit)
- Script in `package.json`: `"generate:schema": "openapi-zod-client ..."`

**If NOT using**:

- Remove `openapi-zod-client` dependency
- Delete `openapi-zod-client.config.ts`
- Delete `lib/generated/` folder
- Manually define TypeScript types for your API

---

### 7. **Three.js** (`three`, `@react-three/fiber`, `@react-three/drei`)

**When to use**: Only if you need 3D rendering, 3D visualizations, or interactive 3D scenes.

**Required setup**:

- Components using Three.js (e.g., `features/planogram/components/three-js-view/`)

**If NOT using**:

- Remove `three`, `@react-three/fiber`, `@react-three/drei` dependencies
- Delete any Three.js-related components

---

### 8. **React Grid Layout** (`react-grid-layout`)

**When to use**: Only if you need draggable, resizable grid layouts (like dashboard widgets).

**If NOT using**:

- Remove `react-grid-layout` and `@types/react-grid-layout` dependencies

---

### 9. **Font Awesome** (`@fortawesome/*`)

**When to use**: Only if you need Font Awesome icons.

**If NOT using**:

- Remove all `@fortawesome/*` dependencies
- Use alternative icon libraries (e.g., `lucide-react`, `@heroicons/react`)

---

### 10. **React Hot Toast** (`react-hot-toast`)

**When to use**: Only if you need toast notifications.

**If NOT using**:

- Remove `react-hot-toast` dependency
- Use alternative notification libraries or build custom solution

---

### 11. **Zod** (`zod`)

**When to use**: Only if you need runtime type validation and schema validation.

**If NOT using**:

- Remove `zod` dependency
- Use TypeScript types only or alternative validation libraries

---

### 12. **JOSE** (`jose`)

**When to use**: Only if you need JWT token encoding/decoding on the client side.

**If NOT using**:

- Remove `jose` dependency
- Handle JWT tokens on the backend or use alternative libraries

---

### 13. **Zodios** (`@zodios/core`)

**When to use**: Only if you're using Zodios for type-safe API clients (often paired with OpenAPI generation).

**If NOT using**:

- Remove `@zodios/core` dependency

---

### 14. **MSW** (`msw`)

**When to use**: Only if you need API mocking for development or testing.

**If NOT using**:

- Remove `msw` dependency
- Use alternative mocking solutions or mock directly in tests

---

### 15. **Faker.js** (`@faker-js/faker`)

**When to use**: Only if you need fake data generation for testing or development.

**If NOT using**:

- Remove `@faker-js/faker` dependency

---

## 📝 File Organization Patterns

### Feature-Based Structure

Each feature in `features/` should follow this pattern:

```
features/[feature-name]/
├── components/              # Feature-specific components
│   └── [component-name]/    # Component folder
│       ├── index.tsx        # Public export (barrel export)
│       └── [component-name].tsx  # Implementation
├── queries/                 # ⚠️ ONLY if using React Query
│   ├── use-[name]-query.ts
│   ├── use-[name]-mutation.ts
│   └── index.ts
├── store/                   # ⚠️ ONLY if using Zustand
│   ├── [feature]-slice.ts
│   └── index.ts
├── hooks/                   # Feature-specific hooks
│   └── use-[name].ts
├── types.ts                 # Feature TypeScript types
└── index.ts                 # Public exports (barrel export)
```

### Component Pattern

Components use a folder structure with barrel exports:

- `[component-name]/index.tsx` - Public export
- `[component-name]/[component-name].tsx` - Implementation

This enables clean imports:

```typescript
import { ComponentName } from '@/features/feature/components';
```

## 🔧 Configuration Files Explained

### `tsconfig.json`

- TypeScript compiler configuration
- Path aliases: `@/*` maps to root directory
- **Required** for TypeScript projects

### `next.config.ts`

- Next.js framework configuration
- Custom webpack, image optimization, redirects, etc.
- **Required** for Next.js projects

### `app/providers.tsx`

- Global React context providers wrapper
- Only include providers you're actually using:
  - React Query (if using)
  - Theme providers (if using)
  - Auth providers (if using)
  - Other context providers (if using)

### `components.json`

- **Optional**: shadcn/ui configuration
- Defines component paths and styling preferences
- Only needed if using shadcn/ui

### `postcss.config.mjs`

- **Optional**: PostCSS configuration
- Required if using Tailwind CSS
- Configures CSS processing

### `eslint.config.mjs`

- ESLint linting configuration
- **Recommended** for code quality

## 📦 Package.json Scripts

Common scripts (add only what you need):

```json
{
  "scripts": {
    "dev": "next dev", // Development server
    "build": "next build", // Production build
    "start": "next start", // Production server
    "lint": "eslint", // Lint code
    "test:typecheck": "tsc --noEmit", // Type check
    "test": "vitest", // ⚠️ Only if using Vitest
    "test:run": "vitest run", // ⚠️ Only if using Vitest
    "test:coverage": "vitest run --coverage", // ⚠️ Only if using Vitest
    "generate:schema": "..." // ⚠️ Only if using OpenAPI generation
  }
}
```

## ✅ New Project Checklist

When starting a new project:

1. ✅ **Start Minimal**

   - Install only: `next`, `react`, `react-dom`, `typescript`
   - Create basic structure: `app/`, `components/`, `public/`

2. ✅ **Add Libraries Only When Needed**

   - Need server state? → Add React Query
   - Need global state? → Add Zustand
   - Need UI components? → Add shadcn/ui
   - Need HTTP client? → Add axios (or use fetch)
   - Need testing? → Add Vitest
   - Need 3D? → Add Three.js
   - Need validation? → Add Zod

3. ✅ **Create Folders Only When Implementing**

   - Don't create empty `queries/` folders
   - Don't create empty `store/` folders
   - Create feature folders when you start building features

4. ✅ **Regular Cleanup**
   - Remove unused dependencies
   - Delete unused folders
   - Remove unused configuration files

## 🚫 Anti-Patterns to Avoid

- ❌ Installing React Query "just in case" - only add when you need server state management
- ❌ Setting up Zustand "for future use" - add when you actually need global state
- ❌ Creating empty `queries/` or `store/` folders - create them when you add the first query/store
- ❌ Installing UI libraries you don't use - only add what you need
- ❌ Keeping unused dependencies - regularly audit and remove
- ❌ Adding all shadcn/ui components - only add components you actually use
- ❌ Setting up testing framework without writing tests - add when you start testing

## 📖 Best Practices

1. **Start Minimal**: Begin with the smallest possible setup
2. **Add Incrementally**: Add libraries and folders as you need them
3. **Feature-First**: Organize by features, not by file type
4. **Barrel Exports**: Use `index.ts` files for clean imports
5. **Type Safety**: Keep TypeScript types close to where they're used
6. **Component Co-location**: Keep related files together
7. **Regular Cleanup**: Periodically remove unused dependencies and folders
8. **Document Decisions**: Note why you added each library in your README

## 🎨 Styling Approach

This template assumes **Tailwind CSS** for styling. If using a different approach:

- **CSS Modules**: Create `*.module.css` files alongside components
- **Styled Components**: Install `styled-components` and create styled components
- **CSS-in-JS**: Use your preferred solution (emotion, etc.)
- **Plain CSS**: Use `app/globals.css` and component-specific CSS files

Adjust the structure accordingly.

---

## 📚 Summary

**Remember**: This structure is a template. Adapt it to your project's actual needs.

**Golden Rule**: Less is more - only include what you use!

- ✅ Start with core Next.js structure
- ✅ Add libraries when you have a specific need
- ✅ Create folders when you start implementing features
- ✅ Regularly clean up unused code and dependencies
- ✅ Document why you added each major library

This approach keeps your project lean, maintainable, and easy to understand.
