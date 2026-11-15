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