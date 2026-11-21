### Prisma Learning

- Database Migration = Database structure পরিবর্তন।
- Database Architecture = System-এর সামগ্রিক গঠন।
- Database Relationship = একাধিক table-এর মধ্যে সংযোগ।

## Prisma Schema
```prisma
model User {
  id   Int    @id @default(autoincrement())
  name String
}
```

**optional value shoho kare**
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

```prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String   @map("full_name")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

# CRUD Operations
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