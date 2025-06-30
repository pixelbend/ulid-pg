# ULID Pg

## An Implementation of ULID for PostgreSQL

ULID (Universally Unique Lexicographically Sortable Identifier) is a modern alternative to UUIDs that provides **unique**, **sortable**, and **URL-safe** identifiers.

This PostgreSQL implementation allows you to generate ULIDs natively using a `plpgsql` function. It encodes a 128-bit value consisting of a timestamp and randomness using Crockford’s Base32 format.

## Structure of ULID

The ULID is a 26-character string composed of:

- **Timestamp Prefix** (48 bits): Encodes the current timestamp in milliseconds.
- **Random Suffix** (80 bits): Provides uniqueness via secure randomness.

```pseudo
01HXZ6Z9RY3X8DQ5FHP5G0JKYV
└─────┬─────┘ └─────┬─────┘
  Timestamp   Random Entropy
```

- **Lexicographically Sortable**: ULIDs sort by creation time.
- **Globally Unique**: 80 bits of randomness ensures uniqueness.
- **URL-Safe**: Uses Crockford's Base32, safe for file paths and URLs.

## Usage of ULID Pg

Once you’ve installed the function from `ulid.sql` in your PostgreSQL instance, you can use it as follows.

- Generating ULID

```sql
-- Generate a ULID
SELECT gen_ulid();
-- Result: 01HXZ6Z9RY3X8DQ5FHP5G0JKYV
```

- Defining ULID

```sql
-- Creating a users table with ULID as primary keys
CREATE TABLE users (
  id TEXT DEFAULT gen_ulid() PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now() NOT NULL,
  updated_at TIMESTAMPTZ
);
```
