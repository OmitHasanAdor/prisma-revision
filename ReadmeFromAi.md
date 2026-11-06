# Database Migration, Architecture & Relationships

## 1. Database Migration

### Q1. What is Database Migration?

**Answer:**

Database Migration is the process of tracking and applying changes to a database structure over time.

For example:
- Creating a new table.
- Adding or removing a column.
- Changing a column's data type.
- Adding or removing constraints.

### Q2. Why do we need Database Migration?

**Answer:**

- To keep database changes organized.
- To track changes over time.
- To maintain consistency across development and production databases.
- To work safely with other developers in a team.

### Q3. What is Prisma Migrate?

**Answer:**

Prisma Migrate is a database migration tool that synchronizes changes in the Prisma Schema with the actual database.

### Q4. How does Prisma Migration work?

**Answer:**

1. Define or modify a model in `schema.prisma`.
2. Generate a migration.
3. Prisma creates an SQL migration file.
4. Apply the migration to the database.

### Example: Prisma Migration

**Step 1: Define a model in `schema.prisma`.**

```prisma
model User {
  id   Int    @id @default(autoincrement())
  name String
}
```

**Step 2: Generate and apply migration (Prisma 7).**

```bash
npx prisma migrate dev --name create_user
```

**Step 3: Add a new field.**

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
}
```

**Step 4: Generate another migration.**

```bash
npx prisma migrate dev --name add_user_email
```

Now the database will have the new `email` column.

### Q5. What is the difference between `migrate dev` and `migrate deploy`?

**Answer:**

| Command | Purpose |
|---|---|
| `migrate dev` | Creates and applies migrations in development. |
| `migrate deploy` | Applies existing migrations in production. |

**Important Commands:**

```bash
# Create and apply migration
npx prisma migrate dev --name migration_name

# Apply pending migrations in production
npx prisma migrate deploy

# Reset development database
npx prisma migrate reset

# Check migration status
npx prisma migrate status
```

**Note:** Never use `migrate reset` in production because it resets the development database schema and can delete data.

---

## 2. Architecture

### Q1. What is Architecture in Software Development?

**Answer:**

Architecture is the overall structure of a software system. It defines how different components are organized and communicate with each other.

### Q2. What is Backend Architecture?

**Answer:**

Backend Architecture defines how the backend application is organized, including its controllers, services, database layer, and other components.

### Q3. What is a Layered Architecture?

**Answer:**

Layered Architecture divides an application into multiple layers, where each layer has a specific responsibility.

### Example: Backend Layered Architecture

```text
        Client
           |
           v
       Controller
           |
           v
        Service
           |
           v
       Data Access
           |
           v
       Prisma ORM
           |
           v
        Database
```

**Responsibilities:**

| Layer | Responsibility |
|---|---|
| Client | Sends requests to the server. |
| Controller | Handles requests and responses. |
| Service | Contains business logic. |
| Data Access | Handles database operations. |
| Prisma ORM | Communicates with the database. |
| Database | Stores application data. |

### Q4. What is Prisma's role in Backend Architecture?

**Answer:**

Prisma acts as the database access layer. It allows the application to perform database operations using TypeScript or JavaScript instead of writing SQL queries manually for every operation.

### Example: Prisma Client

```typescript
const users = await prisma.user.findMany();
```

This query retrieves all users from the database.

---
