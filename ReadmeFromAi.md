# Prisma ORM Complete Notes

**From Beginner to Advanced | TypeScript + PostgreSQL**

A complete guide to learning Prisma ORM with questions, answers, syntax, practical examples, and important commands.

---

# Table of Contents

1. Introduction to Prisma
2. ORM Fundamentals
3. Prisma Architecture
4. Project Setup
5. Prisma Schema
6. Models and Fields
7. Data Types
8. Attributes
9. Database Migration
10. CRUD Operations
11. Filtering and Searching
12. Sorting and Pagination
13. Database Relationships
14. Nested Queries and Writes
15. Select and Include
16. Aggregation and GroupBy
17. Transactions
18. Indexes and Constraints
19. Enums and Default Values
20. Database Seeding
21. Raw Queries
22. Client Extensions
23. Error Handling
24. Connection Pooling
25. Performance Optimization
26. Prisma with TypeScript
27. Prisma with Next.js
28. Prisma with Express.js
29. Prisma with Neon
30. Production Deployment
31. Best Practices
32. Common Errors
33. Practice Questions
34. Real World Projects
35. Official Documentation

---

# 1. Introduction to Prisma

## Q1. What is Prisma?

**Answer:**

Prisma is a modern ORM (Object Relational Mapper) for Node.js and TypeScript that helps developers interact with databases using JavaScript or TypeScript instead of writing SQL queries manually.

Prisma provides:
- Type-safe database queries.
- Database migrations.
- Database schema management.
- Auto-generated Prisma Client.
- Visual database management through Prisma Studio.

## Q2. Why do we use Prisma?

**Answer:**

Without Prisma, we often need to write SQL queries manually. Prisma makes database operations easier, more organized, and type-safe.

**Without Prisma (Raw SQL):**

```typescript
const result = await pool.query(
  "SELECT * FROM users WHERE id = $1",
  [1]
);
```

**With Prisma:**

```typescript
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
});
```

## Q3. What are the main components of Prisma?

**Answer:**

Prisma ORM mainly provides three important components:

| Component | Purpose |
|---|---|
| Prisma Schema | Defines models and database structure. |
| Prisma Client | Performs database operations. |
| Prisma Migrate | Manages database migrations. |
| Prisma Studio | Visual interface for managing database records. |

## Q4. What are the advantages of Prisma?

**Answer:**

1. Type-safe database queries.
2. Less repetitive SQL code.
3. Easy database migrations.
4. Automatic TypeScript type generation.
5. Easier relationship management.
6. Better developer experience.

## Q5. What are the disadvantages of Prisma?

**Answer:**

1. Some complex queries may require raw SQL.
2. Developers still need SQL and database knowledge.
3. Generated Client types need to be updated after schema changes.
4. Query performance still depends on database design and optimization.

## Q6. What is the difference between Prisma and Raw SQL?

| Prisma | Raw SQL |
|---|---|
| Uses TypeScript or JavaScript methods. | Uses SQL statements. |
| Provides generated types. | Types need to be handled separately. |
| Easier for common CRUD operations. | Gives direct control over SQL. |
| Abstracts many database details. | Requires SQL knowledge. |

---

# 2. ORM Fundamentals

## Q1. What is ORM?

**Answer:**

ORM stands for Object Relational Mapping.

It is a technique that allows developers to interact with relational databases using objects and programming language methods instead of writing SQL queries manually.

**Example:**

```typescript
const users = await prisma.user.findMany();
```

This retrieves records from the User table.

## Q2. What is Object Relational Mapping?

**Answer:**

Object Relational Mapping connects programming language objects with relational database tables.

For example:

| Programming | Database |
|---|---|
| Class / Model | Table |
| Object | Row |
| Property | Column |
| Object ID | Primary Key |

## Q3. What is the difference between ORM and ODM?

**Answer:**

| ORM | ODM |
|---|---|
| Object Relational Mapping. | Object Document Mapping. |
| Commonly works with relational databases. | Commonly works with document databases. |
| Example: Prisma, TypeORM. | Example: Mongoose. |
| PostgreSQL, MySQL. | MongoDB. |

## Q4. What is Type Safety?

**Answer:**

Type Safety means checking whether the data being used matches the expected data types.

**Example:**

```typescript
const user = await prisma.user.create({
  data: {
    name: "Hasan",
    email: "hasan@example.com",
  },
});
```

If a required field is missing or has an incorrect type, TypeScript can identify the problem before runtime.

---

# 3. Prisma Architecture

## Q1. What is Architecture?

**Answer:**

Architecture is the overall structure of a software system. It describes how different components are organized and communicate with each other.

## Q2. How does Prisma work?

**Answer:**

Prisma uses a schema to define data models. Prisma Client is generated from those models and allows the application to communicate with the database.

**Basic Architecture:**

```text
       Application
            |
            v
       Prisma Client
            |
            v
      Driver Adapter
            |
            v
        PostgreSQL
```

Prisma Migrate manages changes to the database structure.

## Q3. What is Prisma Client?

**Answer:**

Prisma Client is an auto-generated, type-safe database client that allows developers to perform database operations.

**Example:**

```typescript
const users = await prisma.user.findMany();
```

## Q4. What is Prisma Studio?

**Answer:**

Prisma Studio is a graphical interface that allows developers to view, edit, create, and delete database records.

**Command:**

```bash
npx prisma studio
```

## Q5. What is Prisma Migrate?

**Answer:**

Prisma Migrate is a database migration tool that tracks schema changes and applies them to the database.

---

# 4. Project Setup

## Q1. How to install Prisma with TypeScript and PostgreSQL?

**Answer:**

Follow these steps.

### Step 1: Create a project

```bash
mkdir prisma-learning
cd prisma-learning
npm init -y
```

### Step 2: Install TypeScript and development tools

```bash
npm install -D typescript tsx @types/node
```

### Step 3: Install Prisma

```bash
npm install -D prisma
npm install @prisma/client @prisma/adapter-pg pg dotenv
npm install -D @types/pg
```

### Step 4: Initialize Prisma

```bash
npx prisma init --datasource-provider postgresql
```

### Step 5: Initialize TypeScript

```bash
npx tsc --init
```

### Step 6: Configure environment variables

Create a `.env` file:

```env
DATABASE_URL="postgresql://postgres:password@localhost:5432/prisma_db"
```

Replace the username, password, host, and database name with your actual PostgreSQL credentials.

### Step 7: Configure Prisma 7

Create `prisma.config.ts` in the project root:

```typescript
import "dotenv/config";
import { defineConfig, env } from "prisma/config";

export default defineConfig({
  schema: "prisma/schema.prisma",

  migrations: {
    path: "prisma/migrations",
  },

  datasource: {
    url: env("DATABASE_URL"),
  },
});
```

### Step 8: Configure schema

Open `prisma/schema.prisma`:

```prisma
generator client {
  provider = "prisma-client"
  output   = "../src/generated/prisma"
}

datasource db {
  provider = "postgresql"
}
```

### Step 9: Create a Prisma Client instance

Create `src/lib/prisma.ts`:

```typescript
import { PrismaPg } from "@prisma/adapter-pg";
import { PrismaClient } from "../generated/prisma/client";

const adapter = new PrismaPg({
  connectionString: process.env.DATABASE_URL!,
});

export const prisma = new PrismaClient({
  adapter,
});
```

### Step 10: Generate Prisma Client

```bash
npx prisma generate
```

### Project Structure

```text
prisma-learning/
│
├── prisma/
│   └── schema.prisma
│
├── src/
│   ├── generated/
│   │   └── prisma/
│   ├── lib/
│   │   └── prisma.ts
│   └── index.ts
│
├── .env
├── .gitignore
├── package.json
├── tsconfig.json
└── prisma.config.ts
```

**Important:** Prisma 7 uses driver adapters for database connections. The PostgreSQL example above uses `@prisma/adapter-pg`.

---

# 5. Prisma Schema

## Q1. What is Prisma Schema?

**Answer:**

Prisma Schema is a configuration and data modeling file that defines the database provider, generated client, models, fields, and relationships.

The default location is:

```text
prisma/schema.prisma
```

## Q2. What are the main blocks of Prisma Schema?

**Answer:**

There are two main configuration blocks and model blocks.

### Generator Block

Defines how Prisma Client is generated.

```prisma
generator client {
  provider = "prisma-client"
  output   = "../src/generated/prisma"
}
```

### Datasource Block

Defines which database provider is used.

```prisma
datasource db {
  provider = "postgresql"
}
```

### Model Block

Defines database models.

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
}
```

## Q3. What is the difference between Model and Table?

**Answer:**

A Prisma Model represents a database entity and generally maps to a database table.

Example:

```prisma
model User {
  id   Int    @id @default(autoincrement())
  name String
}
```

This model maps to a `User` table by default.

---

# 6. Models and Fields

## Q1. What is a Model?

**Answer:**

A Model represents the structure of a database table.

**Example:**

```prisma
model Product {
  id    Int    @id @default(autoincrement())
  name  String
  price Float
}
```

## Q2. What is a Field?

**Answer:**

A Field represents a column inside a model.

In the Product model:

- `id` is a field.
- `name` is a field.
- `price` is a field.

## Q3. What is an Optional Field?

**Answer:**

An optional field can contain `null` or have no value.

Use `?` after the data type.

```prisma
model User {
  id       Int     @id @default(autoincrement())
  name     String
  bio      String?
  imageUrl String?
}
```

Here, `bio` and `imageUrl` are optional.

## Q4. What is a Required Field?

**Answer:**

A required field must have a value unless it has a default value.

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String
}
```

Both `name` and `email` are required.

---

# 7. Prisma Data Types

## Q1. What are Scalar Data Types?

**Answer:**

Scalar types represent individual values stored in database fields.

| Prisma Type | Description |
|---|---|
| String | Text values. |
| Int | Integer numbers. |
| BigInt | Large integer numbers. |
| Float | Floating-point numbers. |
| Decimal | Precise decimal values. |
| Boolean | True or False. |
| DateTime | Date and time. |
| Json | JSON data. |
| Bytes | Binary data. |

## Example

```prisma
model Product {
  id          Int      @id @default(autoincrement())
  name        String
  description String?
  price       Decimal
  quantity    Int      @default(0)
  isAvailable Boolean  @default(true)
  createdAt   DateTime @default(now())
  metadata    Json?
}
```

**Important:**

Use `Decimal` instead of `Float` for financial values where decimal precision matters.

---

# 8. Attributes

## Q1. What are Attributes in Prisma?

**Answer:**

Attributes are special instructions added to models or fields to define database behavior and constraints.

There are two main types:

1. Field Attributes: Start with `@`.
2. Model Attributes: Start with `@@`.

## Common Field Attributes

| Attribute | Purpose |
|---|---|
| @id | Defines Primary Key. |
| @unique | Ensures unique values. |
| @default() | Sets a default value. |
| @updatedAt | Automatically updates modification time. |
| @map() | Maps a field to a database column name. |
| @relation() | Defines relationships. |

## Example

```prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String   @map("full_name")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

## Common Model Attributes

| Attribute | Purpose |
|---|---|
| @@id | Composite Primary Key. |
| @@unique | Composite Unique Constraint. |
| @@index | Creates an Index. |
| @@map | Maps a model to a database table. |

## Example

```prisma
model User {
  id        Int    @id @default(autoincrement())
  firstName String
  lastName  String
  email     String @unique

  @@index([firstName, lastName])
  @@map("users")
}
```

---

# 9. Database Migration

## Q1. What is Database Migration?

**Answer:**

Database Migration is the process of tracking and applying changes to a database structure over time.

Examples:
- Creating a new table.
- Adding a column.
- Removing a column.
- Changing a data type.
- Adding constraints.

## Q2. Why do we need Migration?

**Answer:**

- To track database structure changes.
- To maintain consistency between development and production.
- To collaborate with other developers.
- To safely update database structures.

## Q3. How does Prisma Migration work?

**Answer:**

1. Define a model in `schema.prisma`.
2. Run a migration command.
3. Prisma generates an SQL migration file.
4. Prisma applies the migration to the database.

## Example

**Step 1: Create a model.**

```prisma
model User {
  id   Int    @id @default(autoincrement())
  name String
}
```

**Step 2: Run migration.**

```bash
npx prisma migrate dev --name create_user
```

**Step 3: Add another field.**

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
}
```

**Step 4: Run another migration.**

```bash
npx prisma migrate dev --name add_email
```

Now the database will have the new email column.

## Q4. What is the difference between migrate dev and migrate deploy?

| Command | Purpose |
|---|---|
| migrate dev | Creates and applies migrations during development. |
| migrate deploy | Applies existing migrations in production. |
| migrate status | Checks migration status. |
| migrate reset | Resets the development database and reapplies migrations. |

## Important Migration Commands

```bash
# Create and apply migration
npx prisma migrate dev --name migration_name

# Apply migrations in production
npx prisma migrate deploy

# Check migration status
npx prisma migrate status

# Reset development database
npx prisma migrate reset

# Generate Prisma Client
npx prisma generate

# Open Prisma Studio
npx prisma studio
```

**Important:** Never run `migrate reset` in production. It can delete database data.

---

# 10. CRUD Operations

CRUD stands for:

- Create
- Read
- Update
- Delete

These are the four basic database operations.

Assume we have this model:

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
  age   Int?
}
```

## 10.1 Create

### Q1. How to create a single record?

**Answer:**

Use `create()`.

```typescript
const user = await prisma.user.create({
  data: {
    name: "Hasan",
    email: "hasan@example.com",
    age: 22,
  },
});
```

### Q2. How to create multiple records?

**Answer:**

Use `createMany()`.

```typescript
const users = await prisma.user.createMany({
  data: [
    {
      name: "Rahim",
      email: "rahim@example.com",
    },
    {
      name: "Karim",
      email: "karim@example.com",
    },
  ],
});
```

## 10.2 Read

### Q3. How to find all records?

**Answer:**

Use `findMany()`.

```typescript
const users = await prisma.user.findMany();
```

### Q4. How to find a record by ID?

**Answer:**

Use `findUnique()`.

```typescript
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
});
```

### Q5. What is findFirst()?

**Answer:**

`findFirst()` returns the first record matching the given conditions.

```typescript
const user = await prisma.user.findFirst({
  where: {
    age: {
      gte: 18,
    },
  },
});
```

### Q6. What is the difference between findUnique() and findFirst()?

| findUnique() | findFirst() |
|---|---|
| Searches using unique fields. | Searches using general filters. |
| Returns one unique record. | Returns the first matching record. |
| Can search by ID or unique email. | Can search by age, name, etc. |

## 10.3 Update

### Q7. How to update a record?

**Answer:**

Use `update()`.

```typescript
const user = await prisma.user.update({
  where: {
    id: 1,
  },
  data: {
    name: "Updated Hasan",
  },
});
```

### Q8. How to update multiple records?

**Answer:**

Use `updateMany()`.

```typescript
const result = await prisma.user.updateMany({
  where: {
    age: {
      lt: 18,
    },
  },
  data: {
    age: 18,
  },
});
```

## 10.4 Delete

### Q9. How to delete a record?

**Answer:**

Use `delete()`.

```typescript
const user = await prisma.user.delete({
  where: {
    id: 1,
  },
});
```

### Q10. How to delete multiple records?

**Answer:**

Use `deleteMany()`.

```typescript
const result = await prisma.user.deleteMany({
  where: {
    age: {
      lt: 18,
    },
  },
});
```

## Q11. What is Upsert?

**Answer:**

`upsert()` means Update or Insert.

If a matching record exists, it updates the record. Otherwise, it creates a new record.

```typescript
const user = await prisma.user.upsert({
  where: {
    email: "hasan@example.com",
  },

  update: {
    name: "Updated Hasan",
  },

  create: {
    name: "Hasan",
    email: "hasan@example.com",
  },
});
```

---

# 11. Filtering and Searching

## Q1. How to filter records?

**Answer:**

Use the `where` option.

```typescript
const users = await prisma.user.findMany({
  where: {
    age: 22,
  },
});
```

## Q2. How to use comparison operators?

**Answer:**

Prisma provides comparison operators.

| Operator | Meaning |
|---|---|
| equals | Equal to. |
| not | Not equal to. |
| gt | Greater than. |
| gte | Greater than or equal. |
| lt | Less than. |
| lte | Less than or equal. |
| in | Matches any value in an array. |
| notIn | Does not match values in an array. |

**Example:**

```typescript
const users = await prisma.user.findMany({
  where: {
    age: {
      gte: 18,
      lte: 30,
    },
  },
});
```

## Q3. How to search using contains?

**Answer:**

Use `contains` to find records where a string field contains specific text.

```typescript
const users = await prisma.user.findMany({
  where: {
    name: {
      contains: "Has",
      mode: "insensitive",
    },
  },
});
```

`mode: "insensitive"` performs case-insensitive filtering on supported providers such as PostgreSQL.

## Q4. What are AND, OR and NOT?

**Answer:**

These are logical operators used to combine conditions.

**AND Example:**

```typescript
const users = await prisma.user.findMany({
  where: {
    AND: [
      { age: { gte: 18 } },
      { age: { lte: 30 } },
    ],
  },
});
```

**OR Example:**

```typescript
const users = await prisma.user.findMany({
  where: {
    OR: [
      { name: "Hasan" },
      { name: "Rahim" },
    ],
  },
});
```

**NOT Example:**

```typescript
const users = await prisma.user.findMany({
  where: {
    NOT: {
      name: "Hasan",
    },
  },
});
```

---

# 12. Sorting and Pagination

## Q1. What is Sorting?

**Answer:**

Sorting means arranging database records in ascending or descending order.

Use `orderBy`.

```typescript
const users = await prisma.user.findMany({
  orderBy: {
    name: "asc",
  },
});
```

- `asc` = Ascending.
- `desc` = Descending.

## Q2. What is Pagination?

**Answer:**

Pagination divides a large number of records into smaller groups or pages.

For example, displaying 10 products per page.

## Q3. How to implement Offset Pagination?

**Answer:**

Use `skip` and `take`.

```typescript
const page = 2;
const limit = 10;

const products = await prisma.product.findMany({
  skip: (page - 1) * limit,
  take: limit,
  orderBy: {
    id: "asc",
  },
});
```

## Q4. What is Cursor-based Pagination?

**Answer:**

Cursor-based Pagination uses a unique record as a reference point to retrieve the next set of records.

```typescript
const products = await prisma.product.findMany({
  take: 10,

  cursor: {
    id: 20,
  },

  skip: 1,

  orderBy: {
    id: "asc",
  },
});
```

**Difference:**

| Offset Pagination | Cursor Pagination |
|---|---|
| Uses skip and take. | Uses cursor and take. |
| Useful for page numbers. | Useful for next/previous navigation. |
| Can become expensive at large offsets. | Often more efficient for large datasets. |

---

# 13. Database Relationships

## Q1. What is a Database Relationship?

**Answer:**

A Database Relationship defines how two or more tables are connected to each other.

There are three main types:

1. One-to-One.
2. One-to-Many.
3. Many-to-Many.

## Q2. What is One-to-One Relationship?

**Answer:**

One record in a table is associated with only one record in another table.

**Example:** One User has one Profile.

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

  user User @relation(fields: [userId], references: [id])
}
```

**Important:**

`@unique` ensures that one user cannot have multiple profiles.

## Q3. What is One-to-Many Relationship?

**Answer:**

One record in a table can be associated with multiple records in another table.

**Example:** One User can create multiple Posts.

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

  author User @relation(fields: [authorId], references: [id])
}
```

**Important:**

- `posts Post[]` means one user can have multiple posts.
- `authorId` is the Foreign Key.
- `@relation()` defines the relationship.

## Q4. What is Many-to-Many Relationship?

**Answer:**

Multiple records in one table can be associated with multiple records in another table.

**Example:** Students can enroll in multiple Courses, and each Course can have multiple Students.

### Explicit Many-to-Many

```prisma
model Student {
  id          Int          @id @default(autoincrement())
  name        String
  enrollments Enrollment[]
}

model Course {
  id          Int          @id @default(autoincrement())
  title       String
  enrollments Enrollment[]
}

model Enrollment {
  id        Int @id @default(autoincrement())
  studentId Int
  courseId  Int

  student Student @relation(fields: [studentId], references: [id])
  course  Course  @relation(fields: [courseId], references: [id])

  @@unique([studentId, courseId])
}
```

**Important:**

- Enrollment is called a Junction Table.
- It connects Student and Course.
- `@@unique()` prevents duplicate enrollment.

### Implicit Many-to-Many

When the junction table does not need additional fields, Prisma can manage the relation table automatically.

```prisma
model Post {
  id         Int        @id @default(autoincrement())
  title      String
  categories Category[]
}

model Category {
  id    Int    @id @default(autoincrement())
  name  String @unique
  posts Post[]
}
```

## Q5. What is Foreign Key?

**Answer:**

A Foreign Key is a field that references a unique or primary key in another table.

Example:

```prisma
model Post {
  id       Int @id @default(autoincrement())
  authorId Int

  author User @relation(fields: [authorId], references: [id])
}
```

Here, `authorId` is the Foreign Key.

## Q6. What are Referential Actions?

**Answer:**

Referential Actions define what happens to related records when a referenced record is updated or deleted.

Common actions:

| Action | Meaning |
|---|---|
| Cascade | Automatically affects related records. |
| Restrict | Prevents the operation when related records exist. |
| SetNull | Sets the foreign key to null. |
| SetDefault | Sets the foreign key to its default value. |
| NoAction | Does not automatically perform a referential action. |

**Example:**

```prisma
model Post {
  id       Int @id @default(autoincrement())
  authorId Int
  author   User @relation(
    fields: [authorId],
    references: [id],
    onDelete: Cascade
  )
}
```

When a user is deleted, their related posts are also deleted.

---

# 14. Nested Queries and Writes

## Q1. What is Nested Create?

**Answer:**

Nested Create allows us to create related records within a single database operation.

**Example: Create User and Posts together.**

```typescript
const user = await prisma.user.create({
  data: {
    name: "Hasan",

    posts: {
      create: [
        { title: "Prisma Basics" },
        { title: "Learning TypeScript" },
      ],
    },
  },

  include: {
    posts: true,
  },
});
```

## Q2. What is connect?

**Answer:**

`connect` connects an existing related record using a unique field.

```typescript
const post = await prisma.post.create({
  data: {
    title: "Learning Prisma",

    author: {
      connect: {
        id: 1,
      },
    },
  },
});
```

## Q3. What is disconnect?

**Answer:**

`disconnect` removes an existing relationship without necessarily deleting the related record.

```typescript
await prisma.post.update({
  where: {
    id: 1,
  },

  data: {
    author: {
      disconnect: true,
    },
  },
});
```

This requires an optional relation.

## Q4. What is connectOrCreate?

**Answer:**

It connects an existing record if it exists. Otherwise, it creates a new record.

```typescript
await prisma.post.create({
  data: {
    title: "Prisma Learning",

    author: {
      connectOrCreate: {
        where: {
          email: "hasan@example.com",
        },

        create: {
          name: "Hasan",
          email: "hasan@example.com",
        },
      },
    },
  },
});
```

---

# 15. Select and Include

## Q1. What is Select?

**Answer:**

`select` retrieves only the specified fields from the database.

```typescript
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },

  select: {
    name: true,
    email: true,
  },
});
```

## Q2. What is Include?

**Answer:**

`include` retrieves related records along with the main record.

```typescript
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },

  include: {
    posts: true,
  },
});
```

## Q3. What is the difference between Select and Include?

| Select | Include |
|---|---|
| Selects specific fields. | Includes related records. |
| Can select scalar and relation fields. | Loads relations alongside default scalar fields. |
| Useful for limiting returned data. | Useful for retrieving related data. |

**Important:** Prisma does not allow `select` and `include` at the same query level together. You can use nested selections.

---

# 16. Aggregation and GroupBy

## Q1. What is Aggregation?

**Answer:**

Aggregation performs calculations on multiple database records and returns a summarized result.

Common aggregation functions:

- count
- sum
- avg
- min
- max

## Q2. How to count records?

```typescript
const totalUsers = await prisma.user.count();
```

## Q3. How to calculate average price?

```typescript
const result = await prisma.product.aggregate({
  _avg: {
    price: true,
  },
});
```

## Q4. How to calculate total price?

```typescript
const result = await prisma.product.aggregate({
  _sum: {
    price: true,
  },
});
```

## Q5. What is GroupBy?

**Answer:**

`groupBy()` groups records based on one or more fields.

**Example:**

```typescript
const result = await prisma.product.groupBy({
  by: ["categoryId"],

  _count: {
    id: true,
  },

  _avg: {
    price: true,
  },
});
```

This groups products by category and calculates the count and average price.

---

# 17. Transactions

## Q1. What is a Database Transaction?

**Answer:**

A Transaction is a group of database operations that either all succeed or all fail together.

## Q2. What are ACID Properties?

**Answer:**

ACID represents four important database transaction properties.

| Property | Meaning |
|---|---|
| Atomicity | All operations succeed or none succeed. |
| Consistency | Database remains valid after the transaction. |
| Isolation | Concurrent transactions do not interfere improperly. |
| Durability | Committed changes are permanent. |

## Q3. How to use Prisma Transactions?

**Answer:**

Prisma supports multiple transaction approaches.

### Sequential Transactions

```typescript
const [users, posts] = await prisma.$transaction([
  prisma.user.findMany(),
  prisma.post.findMany(),
]);
```

### Interactive Transactions

```typescript
const result = await prisma.$transaction(async (tx) => {
  const user = await tx.user.create({
    data: {
      name: "Hasan",
      email: "hasan@example.com",
    },
  });

  const post = await tx.post.create({
    data: {
      title: "My First Post",
      authorId: user.id,
    },
  });

  return { user, post };
});
```

If an operation fails, the transaction is rolled back.

## Q4. When should we use Transactions?

**Answer:**

Use transactions when multiple database operations must succeed or fail together.

Examples:
- Bank transfers.
- Order creation.
- Payment processing.
- Inventory updates.
- Related record creation.

---

# 18. Indexes and Constraints

## Q1. What is an Index?

**Answer:**

An Index is a database structure that helps retrieve records faster.

It can improve query performance for frequently searched fields.

## Q2. How to create an Index in Prisma?

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique

  @@index([name])
}
```

## Q3. What is a Composite Index?

**Answer:**

A Composite Index is an index created using multiple fields.

```prisma
model Product {
  id         Int    @id @default(autoincrement())
  categoryId Int
  price      Decimal

  @@index([categoryId, price])
}
```

## Q4. What is a Composite Primary Key?

**Answer:**

A Primary Key created using multiple fields.

```prisma
model Enrollment {
  studentId Int
  courseId  Int

  @@id([studentId, courseId])
}
```

## Q5. What is a Unique Constraint?

**Answer:**

A Unique Constraint ensures that duplicate values are not stored in a field or combination of fields.

```prisma
model User {
  id    Int    @id @default(autoincrement())
  email String @unique
}
```

---

# 19. Enums and Default Values

## Q1. What is an Enum?

**Answer:**

An Enum is a predefined set of allowed values.

It helps restrict a field to specific values.

**Example:**

```prisma
enum UserRole {
  USER
  ADMIN
  SELLER
}

model User {
  id    Int      @id @default(autoincrement())
  name  String
  role  UserRole @default(USER)
}
```

## Q2. What is @default()?

**Answer:**

`@default()` assigns an automatic value when a record is created without specifying that field.

```prisma
model User {
  id        Int      @id @default(autoincrement())
  isActive  Boolean  @default(true)
  createdAt DateTime @default(now())
}
```

## Q3. What are common ID generators?

| Function | Purpose |
|---|---|
| autoincrement() | Automatically increases an integer ID. |
| uuid() | Generates a UUID. |
| cuid() | Generates a CUID. |
| now() | Returns current date and time. |

---

# 20. Database Seeding

## Q1. What is Database Seeding?

**Answer:**

Database Seeding is the process of inserting initial or sample data into a database.

It is useful for:
- Development.
- Testing.
- Creating default users.
- Adding initial categories.
- Preparing demo data.

## Q2. How to create seed data?

Create `prisma/seed.ts`:

```typescript
import { prisma } from "../src/lib/prisma";

async function main() {
  await prisma.user.createMany({
    data: [
      {
        name: "Hasan",
        email: "hasan@example.com",
      },
      {
        name: "Rahim",
        email: "rahim@example.com",
      },
    ],

    skipDuplicates: true,
  });
}

main()
  .catch((error) => {
    console.error(error);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

## Q3. How to configure seeding in Prisma 7?

Add the seed command to `prisma.config.ts`:

```typescript
import "dotenv/config";
import { defineConfig, env } from "prisma/config";

export default defineConfig({
  schema: "prisma/schema.prisma",

  migrations: {
    path: "prisma/migrations",
    seed: "tsx prisma/seed.ts",
  },

  datasource: {
    url: env("DATABASE_URL"),
  },
});
```

Run:

```bash
npx prisma db seed
```

**Important:** Prisma 7 requires explicitly running `db seed`. Seeding is no longer automatically triggered by `migrate dev` or `migrate reset`.

---

# 21. Raw Queries

## Q1. What are Raw Queries?

**Answer:**

Raw Queries allow developers to execute SQL statements directly through Prisma Client.

They are useful when a complex SQL query is difficult to express using Prisma's normal query API.

## Q2. What is $queryRaw?

**Answer:**

`$queryRaw` executes a SQL query and returns the selected data.

```typescript
const users = await prisma.$queryRaw`
  SELECT * FROM "User"
`;
```

## Q3. What is $executeRaw?

**Answer:**

`$executeRaw` executes SQL commands that modify database records and returns the number of affected rows.

```typescript
const result = await prisma.$executeRaw`
  UPDATE "User"
  SET name = 'Updated Hasan'
  WHERE id = 1
`;
```

## Q4. What is SQL Injection?

**Answer:**

SQL Injection is a security vulnerability where attackers manipulate SQL queries through untrusted input.

**Unsafe approach:**

```typescript
// Avoid building SQL strings from user input.
const query = `SELECT * FROM "User" WHERE name = '${name}'`;
```

**Safer approach:**

```typescript
const users = await prisma.$queryRaw`
  SELECT * FROM "User"
  WHERE name = ${name}
`;
```

Use parameterized queries and avoid unsafe raw SQL methods with untrusted input.

---

# 22. Prisma Client Extensions

## Q1. What are Prisma Client Extensions?

**Answer:**

Client Extensions allow developers to extend Prisma Client with custom functionality.

They can add reusable logic for models, queries, results, and client behavior.

## Q2. How to create an Extension?

```typescript
const extendedPrisma = prisma.$extends({
  query: {
    user: {
      async findMany({ args, query }) {
        console.log("Fetching users");

        return query(args);
      },
    },
  },
});
```

Now queries using `extendedPrisma.user.findMany()` will execute the custom logic.

---

# 23. Error Handling

## Q1. Why is Error Handling important?

**Answer:**

Error Handling helps applications handle unexpected database problems without crashing.

## Q2. How to handle Prisma errors?

```typescript
import { Prisma } from "../generated/prisma/client";

try {
  const user = await prisma.user.create({
    data: {
      name: "Hasan",
      email: "hasan@example.com",
    },
  });

} catch (error) {
  if (
    error instanceof Prisma.PrismaClientKnownRequestError
  ) {
    if (error.code === "P2002") {
      console.log("Email already exists");
    }
  }

  throw error;
}
```

## Q3. What are common Prisma Error Codes?

| Code | Meaning |
|---|---|
| P2002 | Unique constraint violation. |
| P2003 | Foreign key constraint violation. |
| P2004 | Database constraint violation. |
| P2025 | Required record not found. |
| P2014 | Required relation violation. |

---

# 24. Connection Pooling

## Q1. What is a Database Connection?

**Answer:**

A Database Connection is a communication channel between an application and a database.

## Q2. What is Connection Pooling?

**Answer:**

Connection Pooling manages a group of reusable database connections instead of creating a new connection for every query.

## Q3. Why do we need Connection Pooling?

**Answer:**

- Reduces connection creation overhead.
- Improves application performance.
- Controls the number of active connections.
- Helps prevent database connection exhaustion.

## Q4. How does PostgreSQL Pool work with Prisma 7?

```typescript
import { Pool } from "pg";
import { PrismaPg } from "@prisma/adapter-pg";
import { PrismaClient } from "../generated/prisma/client";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

const adapter = new PrismaPg(pool);

export const prisma = new PrismaClient({
  adapter,
});
```

For this setup, manage the pool lifecycle appropriately and close it during application shutdown.

---

# 25. Performance Optimization

## Q1. How to improve Prisma Query Performance?

**Answer:**

1. Retrieve only necessary fields.
2. Use appropriate database indexes.
3. Avoid unnecessary database queries.
4. Use pagination.
5. Avoid N+1 query problems.
6. Use transactions when appropriate.
7. Monitor slow database queries.

## Example: Select Only Required Fields

**Less efficient for a small response:**

```typescript
const users = await prisma.user.findMany();
```

**Better when only name and email are needed:**

```typescript
const users = await prisma.user.findMany({
  select: {
    name: true,
    email: true,
  },
});
```

## Q2. What is the N+1 Query Problem?

**Answer:**

The N+1 Query Problem occurs when an application performs one query to retrieve records and then executes another query for each individual record.

**Example of the problem:**

```typescript
const users = await prisma.user.findMany();

for (const user of users) {
  const posts = await prisma.post.findMany({
    where: {
      authorId: user.id,
    },
  });
}
```

If there are 100 users, this can execute 101 queries.

**Better approach:**

```typescript
const users = await prisma.user.findMany({
  include: {
    posts: true,
  },
});
```

This retrieves users and their related posts using Prisma's relation query capabilities.

---

# 26. Prisma with TypeScript

## Q1. How does Prisma support TypeScript?

**Answer:**

Prisma Client automatically generates TypeScript types based on the Prisma Schema.

This provides:
- Type-safe queries.
- Autocomplete.
- Compile-time error checking.
- Type inference.

## Q2. How to use Prisma-generated types?

```typescript
import { Prisma } from "../generated/prisma/client";

type UserWithPosts = Prisma.UserGetPayload<{
  include: {
    posts: true;
  };
}>;
```

## Q3. How to use Prisma model types?

```typescript
import { User } from "../generated/prisma/client";

const getUser = (user: User) => {
  return user.name;
};
```

## Q4. How to infer the return type of a Prisma query?

```typescript
import { Prisma } from "../generated/prisma/client";

const userArgs = {
  include: {
    posts: true,
  },
} satisfies Prisma.UserDefaultArgs;

type UserWithPosts = Prisma.UserGetPayload<typeof userArgs>;
```

---

# 27. Prisma with Next.js

## Q1. How to use Prisma in Next.js?

**Answer:**

Prisma can be used in Next.js Server Components, Server Actions, and Route Handlers to communicate with the database.

## Q2. Why use a Prisma Singleton?

**Answer:**

A Singleton helps reuse the Prisma Client instance instead of creating multiple instances unnecessarily.

**Example:**

```typescript
import { PrismaPg } from "@prisma/adapter-pg";
import { PrismaClient } from "@/generated/prisma/client";

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined;
};

const adapter = new PrismaPg({
  connectionString: process.env.DATABASE_URL!,
});

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({ adapter });

if (process.env.NODE_ENV !== "production") {
  globalForPrisma.prisma = prisma;
}
```

## Q3. How to fetch data in a Server Component?

```tsx
import { prisma } from "@/lib/prisma";

export default async function UsersPage() {
  const users = await prisma.user.findMany();

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

**Important:** Keep database access on the server. Do not expose database credentials or Prisma Client to the browser.

---

# 28. Prisma with Express.js

## Q1. How to use Prisma with Express?

**Answer:**

Prisma can be used in Express controllers or services to perform database operations.

**Example:**

```typescript
import express from "express";
import { prisma } from "./lib/prisma";

const app = express();

app.use(express.json());

app.get("/users", async (req, res) => {
  try {
    const users = await prisma.user.findMany();

    res.status(200).json({
      success: true,
      data: users,
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      message: "Something went wrong",
    });
  }
});

app.listen(5000);
```

## Q2. What is the recommended backend structure?

```text
src/
│
├── controllers/
│   └── user.controller.ts
│
├── services/
│   └── user.service.ts
│
├── routes/
│   └── user.route.ts
│
├── lib/
│   └── prisma.ts
│
├── middlewares/
│
└── app.ts
```

**Responsibilities:**

| Folder | Purpose |
|---|---|
| Controllers | Handle requests and responses. |
| Services | Contain business logic. |
| Routes | Define API endpoints. |
| Lib | Store shared configurations. |
| Middlewares | Handle reusable request processing. |

---

# 29. Prisma with Neon PostgreSQL

## Q1. What is Neon?

**Answer:**

Neon is a serverless PostgreSQL platform that provides cloud-hosted PostgreSQL databases.

## Q2. How to connect Prisma with Neon?

**Answer:**

1. Create a Neon account.
2. Create a PostgreSQL project.
3. Copy the connection strings.
4. Add the connection strings to `.env`.
5. Configure Prisma.
6. Run migrations.

**Example:**

```env
DATABASE_URL="YOUR_NEON_POOLED_CONNECTION_STRING"
```

For Prisma 7, the connection URL is configured through `prisma.config.ts`.

## Q3. What is the difference between Pooled and Direct Connection?

**Answer:**

| Pooled Connection | Direct Connection |
|---|---|
| Uses a connection pooler. | Connects directly to PostgreSQL. |
| Useful for applications with many short-lived connections. | Useful for direct database operations and migration workflows. |
| Helps manage connection limits. | Provides direct database access. |

Use the connection string recommended by Neon for the relevant operation.

---

# 30. Production Deployment

## Q1. What should we do before deploying Prisma?

**Answer:**

1. Configure production environment variables.
2. Generate Prisma Client.
3. Apply pending migrations.
4. Verify database connectivity.
5. Check application build.
6. Test production queries.

## Important Commands

```bash
# Generate Prisma Client
npx prisma generate

# Apply existing migrations
npx prisma migrate deploy

# Build TypeScript project
npx tsc
```

**Important:**

Use `migrate deploy` in production. Do not create development migrations directly against a production database.

---

# 31. Best Practices

1. Use a reusable Prisma Client instance.
2. Keep database logic organized.
3. Use TypeScript for type safety.
4. Use migrations to track schema changes.
5. Never commit `.env` files.
6. Validate user input before database operations.
7. Use indexes for frequently queried fields.
8. Avoid unnecessary database queries.
9. Use transactions for related operations.
10. Handle Prisma errors properly.
11. Use pagination for large datasets.
12. Never expose Prisma Client to frontend code.

---

# 32. Common Errors and Solutions

| Error | Possible Solution |
|---|---|
| Prisma Client not generated | Run `npx prisma generate`. |
| Database connection failed | Check DATABASE_URL. |
| Migration failed | Check schema and database state. |
| Unique constraint failed | Check duplicate values. |
| Foreign key constraint failed | Check referenced records. |
| TypeScript cannot find Prisma types | Generate Prisma Client again. |
| Too many connections | Configure connection pooling. |
| Record not found | Check the ID or unique field. |

---

# 33. Practice Questions

## Beginner

1. What is Prisma ORM?
2. What is Prisma Client?
3. What is Prisma Schema?
4. What is a Model?
5. What is a Field?
6. What is a Migration?
7. What is the difference between `@id` and `@unique`?
8. What is the difference between required and optional fields?

## Intermediate

9. What are CRUD operations?
10. What is the difference between `findUnique()` and `findFirst()`?
11. What is the difference between `create()` and `createMany()`?
12. What is the difference between `update()` and `updateMany()`?
13. What is a Foreign Key?
14. Explain One-to-One relationships.
15. Explain One-to-Many relationships.
16. Explain Many-to-Many relationships.
17. What is the difference between `select` and `include`?
18. What is Pagination?
19. What is Database Seeding?
20. What is an Index?

## Advanced

21. What are ACID properties?
22. What is a Database Transaction?
23. Explain Interactive Transactions.
24. What is the N+1 Query Problem?
25. What is Connection Pooling?
26. What are Raw Queries?
27. What is SQL Injection?
28. What are Client Extensions?
29. How does Prisma handle database errors?
30. How do you optimize Prisma queries?

---

# 34. Real-World Projects

Practice Prisma by building these projects.

## Project 1: Student Management System

**Features:**
- Student CRUD.
- Course CRUD.
- Student enrollment.
- One-to-Many and Many-to-Many relationships.

## Project 2: Blog API

**Features:**
- User registration.
- Post creation.
- Comments.
- Categories.
- Likes.
- Pagination.
- Search and filtering.

## Project 3: E-commerce Marketplace

**Features:**
- User and seller management.
- Product management.
- Categories.
- Cart and wishlist.
- Orders.
- Payments.
- Reviews.
- Admin dashboard.

## Project 4: Hospital Management System

**Features:**
- Patient management.
- Doctor management.
- Appointments.
- Prescriptions.
- Medical reports.
- Patient health records.

---

# 35. Official Documentation

| Topic | Documentation |
|---|---|
| Prisma Overview | https://www.prisma.io/docs/orm |
| Getting Started | https://www.prisma.io/docs/orm/getting-started |
| Prisma Schema | https://www.prisma.io/docs/orm/prisma-schema |
| Prisma Client | https://www.prisma.io/docs/orm/prisma-client |
| CRUD Operations | https://www.prisma.io/docs/orm/prisma-client/queries/crud |
| Filtering and Sorting | https://www.prisma.io/docs/orm/prisma-client/queries/filtering-and-sorting |
| Relations | https://www.prisma.io/docs/orm/prisma-schema/data-model/relations |
| Transactions | https://www.prisma.io/docs/orm/prisma-client/queries/transactions |
| Migrations | https://www.prisma.io/docs/orm/prisma-migrate |
| Seeding | https://www.prisma.io/docs/orm/prisma-migrate/workflows/seeding |
| Prisma Studio | https://www.prisma.io/docs/orm/tools/prisma-studio |

---

# Final Revision Checklist

- [ ] Understand ORM fundamentals.
- [ ] Set up Prisma with PostgreSQL.
- [ ] Understand Prisma Schema.
- [ ] Learn models, fields, and attributes.
- [ ] Perform all CRUD operations.
- [ ] Understand all three database relationships.
- [ ] Practice filtering and sorting.
- [ ] Implement pagination.
- [ ] Learn nested queries.
- [ ] Understand migrations and seeding.
- [ ] Learn transactions.
- [ ] Practice aggregation.
- [ ] Learn raw queries and error handling.
- [ ] Understand connection pooling.
- [ ] Optimize database queries.
- [ ] Integrate Prisma with Next.js and Express.
- [ ] Connect Prisma with Neon.
- [ ] Deploy a production-ready application.

**Learning Goal:** Become comfortable using Prisma ORM with TypeScript and PostgreSQL in real-world full-stack applications.

**Note:** These notes use Prisma ORM 7 syntax. Always check the official documentation for version-specific changes.