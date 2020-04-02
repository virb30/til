# Truncate table and relations with Typeorm

## Problem

Delete from or Truncate a table which is related to another table was generating "can't delete foreign key..." error on postgres

## Scenario

The provided `clear` method of Typeorm could not truncate/drop tables that are related to another, in addition, the `delete` method sometimes could crash the application by trying to delete a record which has a foreign key.

The application has an util file named `truncate.ts`, in which iterates through an array of database entities and, on each iteration, it called the method delete with no parameters `.delete({})` to exclude the records:

```typescript
export default function truncate(): Promise<DeleteResult[]> {
  return Promise.all(
    Object.keys(models).map(key => {
      return models[Number(key)].delete({});
    });
  );
}
```

## Apply to

Typeorm on Postgres.

## Solution

The solution adopted was use entity `query` method instead `delete` and do a check for database type, in addition was included a cascade parameter to function to properly includes the `CASCADE` statement to query.

The updated version:

```typescript
export default function truncate(options?: TruncateOptions): Promise<any[]> {
  return Promise.all(
    Object.keys(models).map(key => {
      // get the typeorm active connection
      const connection = getConnection();

      // get tableName from connection metadata
      const tableName = connection.getMetadata(models[Number(key)]).tablePath;

      /*check it database is postgres*/
      const isPostgres =
        connection.options.type && connection.options.type === "postgres";

      // iterate over entity array calling the raw truncate query
      return models[Number(key)].query(
        `TRUNCATE TABLE ${tableName} ${
          isPostgres && options && options.cascade ? "CASCADE" : ""
        }`
      );
    })
  );
}
```

this function can be called this way:

```
await truncate({ cascade: true});
```
