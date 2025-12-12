# React Testing Library Implementation Patterns

This guide documents the recommended patterns and best practices for using React Testing Library in your tests.

## Core Philosophy

Test your components the way users interact with them:
- Find elements by their accessible roles and names
- Interact with elements as users would (clicks, typing, keyboard navigation)
- Assert on visible behavior, not implementation details

## Query Selection Strategy

### 1. Prefer Role-Based Queries

Role-based queries (`getByRole`) are the most reliable because they match how screen readers and users identify elements.

```tsx
// ✅ GOOD: Using getByRole
import { render, screen } from '@testing-library/react';

test('renders login button', () => {
  render(<LoginForm />);
  const button = screen.getByRole('button', { name: /login/i });
  expect(button).toBeInTheDocument();
});
```

**Why**: Roles represent real user interaction. Screen readers announce elements by role, so testing by role ensures accessibility.

**Always include `name:` when applicable** to target specific elements:

```tsx
// ✅ GOOD: Specific button by name
const submitButton = screen.getByRole('button', { name: /submit/i });
const cancelButton = screen.getByRole('button', { name: /cancel/i });

// ❌ BAD: Ambiguous when multiple buttons exist
const button = screen.getByRole('button');
```

### 2. Avoid `getByLabelText` Unless Testing Accessibility

Modern UI patterns often break label associations. Prefer role + name instead.

```tsx
// ✅ GOOD: Using role + name
const emailInput = screen.getByRole('textbox', { name: /email/i });

// ⚠️ ACCEPTABLE: Only when explicitly testing accessibility
const emailInput = screen.getByLabelText(/email/i);
```

Use `getByLabelText` only when:
- You are explicitly validating that labels are properly associated
- Testing accessibility compliance

### 3. Avoid Non-Semantic Queries

Do NOT use unless absolutely necessary:
- `getByTestId` - Bypasses accessibility
- `getByPlaceholderText` - Placeholders are not accessible
- `getByDisplayValue` - Too implementation-specific
- `container.querySelector` - Bypasses all RTL utilities

**Only acceptable when:**
- Component has no semantic role (rare)
- You are testing low-level UI primitives
- No other query method works

```tsx
// ❌ BAD: Using test ID
const button = screen.getByTestId('submit-button');

// ✅ GOOD: Using role
const button = screen.getByRole('button', { name: /submit/i });
```

## Async Testing Patterns

### 4. Prefer `findBy*` for Async DOM Updates

Use `findBy*` queries when waiting for elements to appear or update. They automatically wait and retry.

```tsx
// ✅ GOOD: Using findByRole
test('shows data after loading', async () => {
  render(<DataComponent />);
  const data = await screen.findByRole('article', { name: /user data/i });
  expect(data).toBeInTheDocument();
});

// ❌ BAD: Using waitFor unnecessarily
test('shows data after loading', async () => {
  render(<DataComponent />);
  await waitFor(() => {
    expect(screen.getByRole('article')).toBeInTheDocument();
  });
});
```

**Why**: `findBy*` queries are built for async scenarios and handle retries automatically. Only use `waitFor` when `findBy*` cannot apply.

### 5. Use `waitFor` Only When `findBy*` Cannot Apply

Use `waitFor` for async effects that do not produce visible DOM nodes:

```tsx
// ✅ GOOD: waitFor for non-DOM assertions
test('updates state after async operation', async () => {
  render(<Component />);
  await waitFor(() => {
    expect(mockFunction).toHaveBeenCalled();
  });
});

// ❌ BAD: waitFor when findBy* works
test('shows message', async () => {
  render(<Component />);
  await waitFor(() => {
    expect(screen.getByText('Message')).toBeInTheDocument();
  });
  // Should use: await screen.findByText('Message')
});
```

## User Interaction Patterns

### 6. Use `userEvent` Instead of `fireEvent`

`userEvent` simulates real user actions. `fireEvent` should only be used for low-level or custom events.

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

// ✅ GOOD: Using userEvent
test('submits form on button click', async () => {
  const user = userEvent.setup();
  render(<LoginForm />);
  
  await user.type(screen.getByRole('textbox', { name: /email/i }), 'test@example.com');
  await user.click(screen.getByRole('button', { name: /submit/i }));
  
  expect(mockSubmit).toHaveBeenCalled();
});

// ❌ BAD: Using fireEvent
import { fireEvent } from '@testing-library/react';

test('submits form', () => {
  render(<LoginForm />);
  fireEvent.change(screen.getByRole('textbox'), { target: { value: 'test@example.com' } });
  fireEvent.click(screen.getByRole('button'));
});
```

**Why**: `userEvent` triggers the full event sequence (e.g., focus, input, blur) that real users generate.

## Assertion Patterns

### 7. Assert Visible Behavior, Not Internal Implementation

**Avoid:**
- Checking internal state or props
- Asserting classNames or DOM structure
- Mock function call counts unless it's a true integration point

**Prefer:**
- Assertions on visible text, roles, and what the user sees

```tsx
// ❌ BAD: Testing implementation
test('component has correct class', () => {
  const { container } = render(<Button />);
  expect(container.querySelector('.btn-primary')).toBeInTheDocument();
});

// ✅ GOOD: Testing behavior
test('button is visible and clickable', () => {
  render(<Button />);
  const button = screen.getByRole('button', { name: /click me/i });
  expect(button).toBeInTheDocument();
  expect(button).toBeEnabled();
});
```

### 8. Test Accessibility Interactions

Include keyboard navigation and focus behavior:

```tsx
import userEvent from '@testing-library/user-event';

test('navigates with keyboard', async () => {
  const user = userEvent.setup();
  render(<Navigation />);
  
  const firstLink = screen.getByRole('link', { name: /home/i });
  firstLink.focus();
  
  await user.keyboard('{Tab}');
  expect(screen.getByRole('link', { name: /about/i })).toHaveFocus();
});
```

## Query Organization Patterns

### 9. Use `within()` for Scoped Queries

Useful for tables, lists, repeated rows, or grouped elements to prevent selecting the wrong instance.

```tsx
import { within } from '@testing-library/react';

test('each row has edit button', () => {
  render(<UserTable />);
  const rows = screen.getAllByRole('row');
  
  rows.forEach((row) => {
    const { getByRole } = within(row);
    expect(getByRole('button', { name: /edit/i })).toBeInTheDocument();
  });
});
```

### 10. Avoid Re-Querying the Same Element

Query once, store the reference, reuse it:

```tsx
// ✅ GOOD: Query once, reuse
test('button state changes', async () => {
  const user = userEvent.setup();
  render(<Component />);
  const button = screen.getByRole('button', { name: /submit/i });
  
  expect(button).toBeEnabled();
  await user.click(button);
  expect(button).toBeDisabled();
});

// ❌ BAD: Re-querying
test('button state changes', async () => {
  const user = userEvent.setup();
  render(<Component />);
  
  expect(screen.getByRole('button')).toBeEnabled();
  await user.click(screen.getByRole('button'));
  expect(screen.getByRole('button')).toBeDisabled();
});
```

### 11. Prefer Screen Queries Over Destructuring From Render

Improves readability and consistency. Keeps tests flat and declarative.

```tsx
// ✅ GOOD: Using screen
import { render, screen } from '@testing-library/react';

test('renders form', () => {
  render(<LoginForm />);
  expect(screen.getByRole('textbox', { name: /email/i })).toBeInTheDocument();
});

// ⚠️ ACCEPTABLE: Destructuring when needed for within()
test('nested elements', () => {
  const { getByRole } = render(<Component />);
  const section = getByRole('region');
  const { getByText } = within(section);
  expect(getByText('Content')).toBeInTheDocument();
});
```

## Test Organization Patterns

### 12. Reset Mocks After Each Test

Prevents cross-test pollution. Always clear or reset mocks, timers, and modules.

```tsx
import { afterEach, vi } from 'vitest';

afterEach(() => {
  vi.clearAllMocks();
  // Reset any other test state
});
```

### 13. Avoid Snapshot Testing

Snapshots are brittle and high-maintenance. Prefer targeted, behavior-based assertions.

```tsx
// ❌ BAD: Snapshot testing
test('renders correctly', () => {
  const { container } = render(<Component />);
  expect(container).toMatchSnapshot();
});

// ✅ GOOD: Behavior-based assertions
test('displays user information', () => {
  render(<UserProfile user={mockUser} />);
  expect(screen.getByText(mockUser.name)).toBeInTheDocument();
  expect(screen.getByText(mockUser.email)).toBeInTheDocument();
});
```

### 14. Keep Tests Minimal and User-Focused

Test what matters to the actual user. Avoid testing internal render logic or permutations.

```tsx
// ❌ BAD: Testing implementation details
test('calls setState correctly', () => {
  const setState = vi.fn();
  render(<Component setState={setState} />);
  expect(setState).toHaveBeenCalledWith('value');
});

// ✅ GOOD: Testing user-visible behavior
test('displays updated value after interaction', async () => {
  const user = userEvent.setup();
  render(<Component />);
  await user.click(screen.getByRole('button'));
  expect(screen.getByText('Updated value')).toBeInTheDocument();
});
```

### 15. Avoid Mocking Timers When Possible

`userEvent` may break with mocked timers. If timers must be mocked, be explicit and careful.

```tsx
// ❌ BAD: Unnecessary timer mocking
vi.useFakeTimers();
// ... test code
vi.useRealTimers();

// ✅ GOOD: Let real timers run
test('shows message after delay', async () => {
  render(<Component />);
  await screen.findByText('Message', {}, { timeout: 2000 });
});
```

## Complete Example

Here's a complete example demonstrating best practices:

```tsx
import { render, screen, within } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect, vi, afterEach } from 'vitest';
import { LoginForm } from './login-form';

describe('LoginForm', () => {
  afterEach(() => {
    vi.clearAllMocks();
  });

  it('submits form with valid credentials', async () => {
    const user = userEvent.setup();
    const onSubmit = vi.fn();
    
    render(<LoginForm onSubmit={onSubmit} />);
    
    // Use role-based queries
    const emailInput = screen.getByRole('textbox', { name: /email/i });
    const passwordInput = screen.getByRole('textbox', { name: /password/i });
    const submitButton = screen.getByRole('button', { name: /login/i });
    
    // Interact as user would
    await user.type(emailInput, 'test@example.com');
    await user.type(passwordInput, 'password123');
    await user.click(submitButton);
    
    // Assert behavior, not implementation
    expect(onSubmit).toHaveBeenCalledWith({
      email: 'test@example.com',
      password: 'password123',
    });
  });

  it('shows validation errors', async () => {
    const user = userEvent.setup();
    render(<LoginForm />);
    
    const submitButton = screen.getByRole('button', { name: /login/i });
    await user.click(submitButton);
    
    // Wait for async validation messages
    expect(await screen.findByText(/email is required/i)).toBeInTheDocument();
    expect(await screen.findByText(/password is required/i)).toBeInTheDocument();
  });

  it('handles keyboard navigation', async () => {
    const user = userEvent.setup();
    render(<LoginForm />);
    
    const emailInput = screen.getByRole('textbox', { name: /email/i });
    emailInput.focus();
    
    await user.keyboard('{Tab}');
    expect(screen.getByRole('textbox', { name: /password/i })).toHaveFocus();
  });
});
```

## Summary

**Query Selection:**
- Prefer `getByRole` with `name` option
- Avoid `getByLabelText` unless testing accessibility
- Avoid non-semantic queries (`getByTestId`, `getByPlaceholderText`, etc.)

**Async Testing:**
- Use `findBy*` for waiting on DOM updates
- Use `waitFor` only when `findBy*` cannot apply

**User Interaction:**
- Use `userEvent` instead of `fireEvent`
- Test keyboard navigation and focus behavior

**Assertions:**
- Assert visible behavior, not implementation details
- Test accessibility interactions

**Organization:**
- Use `within()` for scoped queries
- Query once, reuse references
- Prefer `screen.*` queries
- Reset mocks after each test
- Avoid snapshots
- Keep tests minimal and user-focused
- Avoid mocking timers when possible

