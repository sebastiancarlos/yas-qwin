<!-- Note: This README.md file was automatically generated. Plase run `make readme` to generate a new one. -->

# YAS-QWIN (Yet Another SQL-Query Writing Interface)

**YAS-QWIN** is a CLI tool for building (and optionally running) SQL queries.

#### Example:

```bash
yas-qwin --print-table your_table_name
```

#### Output: 

```sql
-- Print contents of table
SELECT * FROM your_table_name;
```

## Features

- **Single-file:** No complex installations, just a Python-powered CLI file
  (not even a `.py` extension)
- **Command palette:** Wide variety of commands. List them with `--help`.
- **Run on-the-fly or save for later:** Execute commands directly against your
  database by passing the `--run` flag. Or just print them out to learn or
  use.
- **Database agnostic (ish):** Currently it runs queries only in SQLite;
  fancier databases to be seduced later. But its raw SQL output can be used in
  **any database** (unless using options labeled "SQLite only", of course)
- **CLI Interface:** If you speak UNIX CLI, you and YAS-QWIN have a lot to
  talk about. It's as if Dennis Ritchie whispered SQL in your ear.
- **SQL Refresher:** If you run the commands without arguments, it
  will print out the SQL query with placeholders. This is ideal for learning
  SQL or refreshing your memory.

**Alpha Disclaimer:** Currenty, YAS-QWIN has limited support for selection
queries, and lacks data manipulation queries. Only DDL (creating and modifying
the database schema) queries are fully supported. Check the bottom of the
README for the full list of missing features.

## Installation

Grab your favorite shell and get going:

1. `git clone https://github.com/sebastiancarlos/yas-qwin`
2. `cd yas-qwin`
3. `./yas-qwin`

Optionally add to your `PATH`. (This can be done by running `make install`)

**Note:**
- `yas-qwin` is a python file without the `.py` extension (this is to call
  it easily without needing to create a shell alias).
- The development team recommends aliasing it anyway to something short like
  `q` for even faster SQL generation during interactive use.

## Usage

```bash
Usage: yas-qwin [OPTIONS] [COMMAND OPTIONS AND ARGS]
    
Two ways of passing commands are supported:
  1. Using -c/--command COMMAND 
  2. Using --COMMAND
    
Commands:
  list-tables	drop-index
  list-indexes	reindex
  print-schemas	create-table
  print-table	column-def
  rename-table	table-constr
  rename-column	foreign-key-clause
  add-column	returning-clause
  drop-column	with-clause
  create-index	cte-def
  drop-index

Options:
  -l, --list-commands    One per line
  -r, --run              Run it in your SQLite db
  -d, --database         Database file to use
  -c, --command          Command to run
  -h, --help             Run with any command

```

Or run it in your database directly. 

`yas-qwin -d your_database.db -c print-indexes --run`

**Note:** You don't need to pass a database file if there's a single `*.db`
file in your current directory.

*(Careful Icarus, do not fly close to the sun without sanitizing inputs)*

## Commandments

YAS-QWIN's commands shall lead the faithful:

- `list-tables`
- `list-indexes`
- `print-schemas`
- `print-table`
- `rename-table`
- `rename-column`
- `add-column`
- `drop-column`
- `create-index`
- `drop-index`
- `reindex`
- `create-table`
- `column-def`
- `table-constr`
- `foreign-key-clause`
- `returning-clause`
- `with-clause`
- `cte-def`

## Help

Every one of YAS-QWIN's commands comes with its own help message. Just type
`--help` after the command to get a detailed explanation of its usage.

But because you are busy, here they are:

### `list-tables`

```bash
Usage: yas-qwin --list-tables

- List names of all tables in database
- Note: SQLite only

```

#### Default output:

```sql
-- List tables in database
SELECT name FROM sqlite_schema WHERE type='table';
```

### `list-indexes`

```bash
Usage: yas-qwin --list-indexes
            
- List names of all indexes in database
- Note: SQLite only

```

#### Default output:

```sql
-- List indexes in database
SELECT name FROM sqlite_schema WHERE type='index';
```

### `print-schemas`

```bash
Usage: yas-qwin --print-schemas
            
- Print schema of all tables and indexes in database
- Note: SQLite only

```

#### Default output:

```sql
-- Print schema of all tables and indexes in database
SELECT sql FROM sqlite_schema;
```

### `print-table`

```bash
Usage: yas-qwin --print-table [TABLES]
        
- Print contents of tables
- Note: If no TABLES passed and an SQLite database is in range,
        fetch all tables and print them all

```

#### Default output:

```sql
-- Print contents of tables
SELECT * FROM sample-table;
```

### `rename-table`

```bash
Usage: yas-qwin --rename-table TABLE NEW_NAME
        
- Rename a table

```

#### Default output:

```sql
-- Rename table
ALTER TABLE old_table RENAME TO new_table;
```

### `rename-column`

```bash
Usage: yas-qwin --rename-column TABLE COLUMN NEW_NAME
        
- Rename a column

```

#### Default output:

```sql
-- Rename column in table
ALTER TABLE sample_table RENAME COLUMN old_column TO new_column;
```

### `add-column`

```bash
Usage: yas-qwin --add-column TABLE COLUMN_DEF
        
- Add a column

```

#### Default output:

```sql
-- Add column to table
ALTER TABLE sample_table ADD COLUMN column_def;
```

### `drop-column`

```bash
Usage: yas-qwin --drop-column TABLE COLUMN
        
- Drop a column

```

#### Default output:

```sql
-- Drop column from table
ALTER TABLE sample_table DROP COLUMN sample_column;
```

### `create-index`

```bash
Usage: yas-qwin --create-index [INDEX-NAME] [TABLE] [INDEXED-COLUMNS] [OPTIONS]
        
- Create an index on a column
- INDEXED-COLUMNS is a comma-separated list of columns
        
Options:
  -u/--unique: Create a unique index
  -w/--where EXPR: Create a partial index
  -i/--if-not-exists: Do not error if index already exists

```

#### Default output:

```sql
-- Create index
CREATE INDEX sample_index ON sample_table (sample_column);
```

### `drop-index`

```bash
Usage: yas-qwin --drop-index [INDEX-NAME] [OPTIONS]
        
- Drop an index
        
Options:
  -i/--if-exists: Do not error if index does not exist

```

#### Default output:

```sql
-- Drop index
DROP INDEX sample_index;
```

### `reindex`

```bash
Usage: yas-qwin --reindex [INDEX-NAME]
        
- Reindex a table
- If no index is specified, reindex all tables

```

#### Default output:

```sql
-- Reindex
REINDEX;
```

### `create-table`

```bash
Usage: yas-qwin --create-table [OPTIONS] TABLE_NAME COLUMN_DEFINITIONS [TABLE_CONSTRAINTS]
Options:
  -t, --temporary
  -i, --if-not-exists
  -a, --as-select SELECT_STATEMENT
SQLite only options:
  -s, --strict (defaults to true)
  -w, --without-rowid

```

#### Default output:

```sql
-- Create table
CREATE TABLE sample_table (sample_column_defs, sample_table_constraints) STRICT;
```

### `column-def`

```bash
Usage: yas-qwin --column-def COLUMN_NAME TYPE [OPTIONS]
Options:
  -p, --primary-key [CONFLICT-CLAUSE]
  -a, --autoincrement
  -d, --descending
  -n, --not-null [CONFLICT-CLAUSE]
  -u, --unique [CONFLICT-CLAUSE]
  -k, --check VALUE
  -d, --default VALUE
  -f, --foreign-key FOREIGN_KEY_CLAUSE
  -g, --generated-as VALUE
  -s, --stored

CONFLICT_CLAUSE can be one of:
  rollback
  abort
  fail
  ignore
  replace

```

#### Default output:

```sql
sample_column sample_type
```

### `table-constr`

```bash
Usage: yas-qwin --table-constr [COLUMN_NAMES] [OPTIONS]
Options:
  -p, --primary-key [CONFLICT_CLAUSE]
  -u, --unique [CONFLICT_CLAUSE]
  -k, --check VALUE
  -f, --foreign-key FOREIGN_KEY_CLAUSE
        
Note: COLUMN_NAMES are needed for PRIMARY KEY, UNIQUE, or
      FOREIGN KEY constraints.
        

CONFLICT_CLAUSE can be one of:
  rollback
  abort
  fail
  ignore
  replace

```

#### Default output:

```sql
<empty>
```

### `foreign-key-clause`

```bash
Usage: yas-qwin --foreign-key-clause FOREIGN_TABLE_NAME [FOREIGN_COLUMN_NAMES]
Options:
  -e, --on-delete VALUE
  -u, --on-update VALUE
  -f, --deferred

VALUE can be one of:
  cascade
  restrict
set null
set default

```

#### Default output:

```sql
REFERENCES foreign_table_name (foreign_column_names)
```

### `returning-clause`

```bash
Usage: yas-qwin --returning [COLUMN_NAMES]
        
  - Return the modified rows back to the application.
  - If COLUMN_NAMES is not specified, '*' is set and
    all columns are returned.

```

#### Default output:

```sql
RETURNING *
```

### `with-clause`

```bash
Usage: yas-qwin --with-clause CTE_DEFS [OPTIONS]
        
  - WITH_CLAUSE is a comma separated list of common table
    expressions (CTEs).
        
Options:
  -r, --recursive    Needed if at least one CTE is recursive.

```

#### Default output:

```sql
WITH cte_defs
```

### `cte-def`

```bash
Usage: yas-qwin --cte-def CTE_NAME SELECT-CLAUSE [OPTIONS]
        
  - CTE_NAME is the name of the common table expression.
  - To be used in a WITH clause.
Options:
  -m, --materialized

```

#### Default output:

```sql
cte_name AS (select_clause)
```

## Advanced Example

*"Roll up your sleeves Jesse, we need to cook!"*

```bash
#! /usr/bin/env bash

colId="$(yas-qwin --column-def id INTEGER --primary-key)";
colName="$(yas-qwin --column-def name TEXT --not-null)";
fk="$(yas-qwin --foreign-key-clause paper_sizes NAME --deferred --on-delete cascade --on-update cascade)";
colPage="$(yas-qwin --column-def paper_size TEXT --foreign-key "${fk}")";
yas-qwin --create-table document "$colId, $colName, $colPage";
```

Output:

```sql
-- Create table
CREATE TABLE document (
  id INTEGER PRIMARY KEY NOT NULL, 
  name TEXT NOT NULL, 
  paper_size TEXT REFERENCES paper_sizes (NAME) ON DELETE CASCADE ON UPDATE CASCADE DEFERRABLE INITIALLY DEFERRED
) STRICT
```

## Known Limitations

YAS-QWIN comes with some quirks:

- Not as database-engine diverse as could be yet.
- Might toss an "AYY LMAO" or "Under construction" at you. It's a work in
  progress, but it's getting there.
- Not all the SQL commands are supported. `SELECT`, `INSERT` and `UPDATE` are
  actually quite hard to implement. They are the main villains of the YAS-QWIN
  dev team, AYY LMAO.
- Inputs are not sanitized. Do not use with untrusted input.

## Disclaimer

While the idea of writing SQL through a CLI is mostly silly, no one knew for
sure until now. And while YAS-QWIN might be quirky, it stands as a
half-serious way of learning and automating your SQL.

If you want a fully feature SQL builder today, check out
[SQLGlot](https://github.com/tobymao/sqlglot).
