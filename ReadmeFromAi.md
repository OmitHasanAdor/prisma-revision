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

## 3. Database Relationships

### Q1. What is a Database Relationship?

**Answer:**

A Database Relationship defines how two or more tables are connected to each other.

Relationships help organize related data and maintain data consistency.

There are three main types of relationships:

### Q2. What is a One-to-One Relationship?

**Answer:**

One record in a table is associated with only one record in another table.

**Example:**

One User has one Profile.

```text
User
  |
  | 1:1
  |
Profile
```

**Prisma Example:**

```prisma
model User {
  id      Int     @id @default(autoincrement())
  name    String
  profile Profile?
}

model Profile {
  id     Int    @id @default(autoincrement())
  bio    String?
  userId Int    @unique
  user   User   @relation(fields: [userId], references: [id])
}
```

**Important:** `@unique` ensures that each user can have only one associated profile.

### Q3. What is a One-to-Many Relationship?

**Answer:**

One record in a table can be associated with multiple records in another table.

**Example:**

One User can create multiple Posts.

```text
User
  |
  | 1:N
  |
Posts
```

**Prisma Example:**

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[]
}

model Post {
  id       Int    @id @default(autoincrement())
  title    String
  authorId Int
  author   User   @relation(fields: [authorId], references: [id])
}
```

**Important:**
- `posts Post[]` means one user can have multiple posts.
- `authorId` is the Foreign Key.
- `@relation()` defines the relationship between User and Post.

### Q4. What is a Many-to-Many Relationship?

**Answer:**

Multiple records in one table can be associated with multiple records in another table.

**Example:**

A Student can enroll in multiple Courses, and a Course can have multiple Students.

```text
Students          Courses
    |                 |
    |                 |
    +---- Enrollments-+
```

**Prisma Example (Explicit Junction Table):**

```prisma
model Student {
  id         Int          @id @default(autoincrement())
  name       String
  enrollments Enrollment[]
}

model Course {
  id         Int          @id @default(autoincrement())
  title      String
  enrollments Enrollment[]
}

model Enrollment {
  id        Int     @id @default(autoincrement())
  studentId Int
  courseId  Int

  student Student @relation(fields: [studentId], references: [id])
  course  Course  @relation(fields: [courseId], references: [id])

  @@unique([studentId, courseId])
}
```

**Important:**

- `Enrollment` is called a Junction Table or Join Table.
- It connects the Student and Course tables.
- `@@unique([studentId, courseId])` prevents duplicate enrollment records for the same student and course.

---

## 4. Important Database Terms

| Term | Meaning |
|---|---|
| Primary Key (PK) | Uniquely identifies each record in a table. |
| Foreign Key (FK) | Connects one table to another table. |
| Unique | Prevents duplicate values. |
| Constraint | A rule applied to database columns or tables. |
| Schema | Defines the structure of a database. |
| Migration | Tracks and applies database structure changes. |
| Junction Table | Connects two tables in a many-to-many relationship. |
| ORM | Allows developers to interact with databases through programming language objects and methods. |

---

## 5. Quick Revision

| Question | Answer |
|---|---|
| What is Migration? | Tracking and applying database structure changes. |
| What is Architecture? | The overall structure of a software system. |
| What is Relationship? | A connection between database tables. |
| What is PK? | Primary Key. |
| What is FK? | Foreign Key. |
| What is One-to-One? | One record connects to one record. |
| What is One-to-Many? | One record connects to multiple records. |
| What is Many-to-Many? | Multiple records connect to multiple records. |
| What is Junction Table? | A table that connects two tables in a many-to-many relationship. |
| What is Prisma Migrate? | Prisma's database migration tool. |

---

## 6. Official Documentation

1. [Prisma ORM Overview](https://docs.prisma.io/docs/orm)
2. [Prisma Schema & Data Model](https://docs.prisma.io/docs/orm/prisma-schema)
3. [Prisma Migrate](https://docs.prisma.io/docs/orm/prisma-migrate)
4. [Development and Production Migrations](https://docs.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production)
5. [Prisma Relations](https://docs.prisma.io/docs/orm/prisma-schema/data-model/relations)

**Revision Tip:** Understand the concepts first, then practice writing the Prisma schema yourself. Do not just memorize the syntax.