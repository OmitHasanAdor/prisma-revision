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
