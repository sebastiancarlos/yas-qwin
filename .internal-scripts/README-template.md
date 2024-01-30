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
<!-- INSERT usage -->
```

Or run it in your database directly. 

`yas-qwin -d your_database.db -c print-indexes --run`

**Note:** You don't need to pass a database file if there's a single `*.db`
file in your current directory.

*(Careful Icarus, do not fly close to the sun without sanitizing inputs)*

## Commandments

Every one of YAS-QWIN's commands comes with its own help message. Just type
`--help` after the command to get a detailed explanation of its usage.

YAS-QWIN's commands shall lead the faithful:

<!-- INSERT commands_help -->
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
