# Token-based Authentication (Email & Password) with Hono, Prisma, and PostgreSQL

This tutorial is part of a series on building an authenticated backend with TypeScript, Hono, PostgreSQL, and Prisma. In this first part, we'll implement password authentication using Prisma for token storage and add authorization.

## What You Will Learn

- **Password Authentication**: Implement sign-up and login with JWT tokens
- **Authorization**: Control resource access based on user permissions
- **Secure API Endpoints**: Protect sensitive data like bank account balances

## Prerequisites

### Assumed Knowledge
- Basic TypeScript/Node.js
- Relational database concepts
- REST principles
- Some Prisma experience

### Development Environment
- Bun v1.x or later
- VS Code with Prisma extension (recommended)

## Getting Started

1. Clone the repository:
```bash
git clone https://github.com/rubcstswe/web102-hono-auth-jwt-prisma-forked.git
cd web102-hono-auth-jwt-prisma-forked
bun install
```

## Data Model

```prisma
model User {
  id      String    @id @default(uuid())
  email   String    @unique
  Account Account[]
}

model Account {
  id      String @id @default(uuid())
  userId  String
  user    User   @relation(fields: [userId], references: [id])
  balance Int    @default(0)
}
```

## Current Unsecured Endpoint

```http
GET /:userId/account/balance
```

**Problem**: Anyone can access any user's bank balance!

## Authentication vs Authorization

- **Authentication**: Verifies "Who are you?" (like a passport)
- **Authorization**: Determines "What can you do?" (like a boarding pass)

## Implementation Steps

### 1. Add Password Field to User Model

```prisma
model User {
  id           String    @id @default(uuid())
  email        String    @unique
  hashPassword String
  Account      Account[]
}
```

Update database:
```bash
bunx prisma db push
bunx prisma generate
```

### 2. Implement Registration

**Endpoint**:
```http
POST /register
{
  "email": "test@gmail.com",
  "password": "123456"
}
```

**Implementation**:
```typescript
app.post("/register", async (c) => {
  try {
    const body = await c.req.json();
    const bcryptHash = await Bun.password.hash(body.password, {
      algorithm: "bcrypt",
      cost: 4,
    });

    const user = await prisma.user.create({
      data: {
        email: body.email,
        hashedPassword: bcryptHash,
        Account: { create: { balance: 0 } },
      },
    });

    return c.json({ message: `${user.email} created successfully}` });
  } catch (e) {
    if (e instanceof Prisma.PrismaClientKnownRequestError) {
      if (e.code === 'P2002') {
        return c.json({ message: 'Email already exists' })
      }
    }
    throw e
  }
});
```

### 3. Implement Login with JWT

**Endpoint**:
```http
POST /login
{
  "email": "test@gmail.com",
  "password": "123456"
}
```

**Implementation**:
```typescript
app.post("/login", async (c) => {
  try {
    const body = await c.req.json();
    const user = await prisma.user.findUnique({
      where: { email: body.email },
      select: { id: true, hashedPassword: true },
    });

    if (!user) throw new HTTPException(401, { message: "Invalid credentials" });

    const match = await Bun.password.verify(
      body.password,
      user.hashedPassword,
      "bcrypt"
    );

    if (match) {
      const payload = {
        sub: user.id,
        exp: Math.floor(Date.now() / 1000) + 60 * 60,
      };
      const token = await sign(payload, "mySecretKey");
      return c.json({ message: "Login successful", token });
    } else {
      throw new HTTPException(401, { message: "Invalid credentials" });
    }
  } catch (error) {
    throw new HTTPException(401, { message: 'Invalid credentials' })
  }
});
```

### 4. Protect Endpoints with JWT Middleware

```typescript
app.use(
  "/protected/*",
  jwt({ secret: 'mySecretKey' })
);

app.get("/protected/account/balance", async (c) => {
  const payload = c.get('jwtPayload');
  if (!payload) throw new HTTPException(401, { message: "Unauthorized" });
  
  const user = await prisma.user.findUnique({
    where: { id: payload.sub },
    select: { Account: { select: { balance: true, id: true } } },
  });

  return c.json({ data: user });
});
```

## Key Security Notes

1. Always hash passwords (never store plaintext)
2. Use environment variables for secrets in production
3. Implement proper error handling
4. Set reasonable token expiration times

## Next Steps

- Add password reset functionality
- Implement role-based access control
- Add rate limiting to prevent brute force attacks
