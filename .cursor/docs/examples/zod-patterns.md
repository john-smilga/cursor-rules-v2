# Zod Implementation Patterns

This guide documents the recommended patterns for using Zod for runtime validation in TypeScript projects.

## Core Concepts

- **Runtime Validation**: Type assertions (`as`) only provide compile-time safety. Zod provides runtime validation.
- **Type Inference**: Derive TypeScript types from Zod schemas using `z.infer<typeof Schema>`
- **Error Handling**: Always handle validation failures appropriately

## Basic Schema Definition

```ts
import { z } from 'zod';

// Define the schema
const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().int().positive().min(13),
});

// Derive the type from the schema
type User = z.infer<typeof UserSchema>;
```

## Schema Organization

- Define schemas near related types or in dedicated schema files
- Group related schemas in feature directories: `features/auth/schemas/`
- Use kebab-case for schema file names: `login-schema.ts`, `user-schema.ts`

## Validation Methods

### `schema.parse()` - Throwing Validation

Use when you want validation errors to throw exceptions:

```ts
const fetchUser = async (id: string): Promise<User> => {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();

  // Throws ZodError if validation fails
  return UserSchema.parse(data);
};
```

### `schema.safeParse()` - Non-Throwing Validation

Use when you want to handle validation errors gracefully:

```ts
const fetchUserSafe = async (id: string): Promise<User | null> => {
  try {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();

    const result = UserSchema.safeParse(data);
    if (!result.success) {
      console.error('Invalid user data:', result.error.format());
      return null;
    }

    return result.data;
  } catch (error) {
    console.error('Error fetching user:', error);
    return null;
  }
};
```

## Advanced Schema Features

### Custom Validation with `.refine()`

```ts
const PasswordSchema = z.string().min(8).refine(
  (password) => /[A-Z]/.test(password),
  { message: "Password must contain at least one uppercase letter" }
);
```

### Complex Validation with `.superRefine()`

```ts
const FormSchema = z.object({
  email: z.string().email(),
  confirmEmail: z.string().email(),
}).superRefine((data, ctx) => {
  if (data.email !== data.confirmEmail) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: "Emails do not match",
      path: ["confirmEmail"],
    });
  }
});
```

### Default Values

```ts
const ConfigSchema = z.object({
  timeout: z.number().default(5000),
  retries: z.number().default(3),
});
```

### Transformations

```ts
const StringToNumberSchema = z.string().transform((val) => parseInt(val, 10));
```

## Common Patterns

### ❌ WRONG: Using Type Assertions

```ts
type User = {
  id: string;
  name: string;
  email: string;
  age: number;
}

const fetchUser = async (id: string): Promise<User> => {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();
  return data as User; // DANGEROUS: No runtime validation!
};
```

### ✅ RIGHT: Using Zod for Validation

```ts
import { z } from 'zod';

const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().int().positive().min(13),
});

type User = z.infer<typeof UserSchema>;

const fetchUser = async (id: string): Promise<User> => {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();

  // Runtime validation
  return UserSchema.parse(data);
};
```

## Integration Examples

### With HTTP Requests

See `docs/examples/useLoginMutation.md` for complete examples of:
- HTTP request wrapper with Zod validation
- Error normalization
- Form validation error handling

### With Zustand Stores

See `docs/examples/zustand-zod.md` for complete examples of:
- Zod schemas in Zustand slices
- Runtime validation in store actions
- Type inference from schemas

### With React Hook Form

```ts
import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';
import { z } from 'zod';

const loginSchema = z.object({
  email: z.string().email('Enter a valid email'),
  password: z.string().min(6, 'Password must be at least 6 characters'),
});

type LoginFormValues = z.infer<typeof loginSchema>;

function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<LoginFormValues>({
    resolver: zodResolver(loginSchema),
  });

  // ... rest of component
}
```

## Best Practices

1. **Always validate external data**: API responses, user inputs, environment variables
2. **Derive types from schemas**: Use `z.infer<typeof Schema>` instead of manually defining types
3. **Handle errors appropriately**: Use `safeParse()` when errors should be handled gracefully
4. **Add meaningful error messages**: Use `.refine()` and `.superRefine()` for custom validation messages
5. **Organize schemas logically**: Keep schemas near where they're used or in dedicated schema files
6. **Use transformations sparingly**: Prefer explicit validation over transformations when possible

## Summary

- Use Zod schemas for all external data validation
- Derive TypeScript types from schemas using `z.infer`
- Use `parse()` for throwing validation, `safeParse()` for graceful error handling
- Organize schemas in feature directories with kebab-case naming
- Reference integration examples in `docs/examples/` for complete patterns

