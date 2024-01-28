# YAS-QWIN (Yet Another SQL-Query Writing Interface)

Welcome to the wild world of
**[YAS-QWIN](https://github.com/sebastiancarlos/yas-qwin)**, your zippy
sidekick for crafting SQL queries with swagger.

## Features 🌈

YAS-QWIN is the remarkably lightweight, command-line interface that gives you
some raw SQL to your prequel (that being your arguments and options). And hold
up, if you daily-drive SQLite, we run the query for you, homie!

Check these fabulous features:

- **Single-file beauty:** No complex installations, just a Python-powered CLI
  bad boi.
- **Command palette ecstasy:** Pop some commands and a fall in love with SQL.
- **Database agnostic (ish):** Currently playing nice with SQLite; fancier
  databases to be seduced later.
- **Damn intuitive:** If you speak UNIX CLI, you and YAS-QWIN have a lot to
  talk about. It's as if Dennis Ritchie whispered SQL in your ear.
- **Serve on-the-fly or save for later:** Execute commands directly against
  your database, or just print them out to learn, admire and play with.

## Installation ✨

Grab your favorite shell and get going:

1. **Clone YAS-QWIN**: `git clone https://github.com/sebastiancarlos/yas-qwin`
2. **Dive into the directory**: `cd yas-qwin`
3. **Find out what it can do for you**: `./yas-qwin --list-commands`

Customize your PATH to taste, and you're ready to engage in some serious query action!

## Usage

```bash
Usage: yas-qwin [OPTIONS] [COMMAND OPTIONS AND ARGS]

Two ways of passing commands are supported:
  1. Using -c/--command COMMAND
  2. Using --COMMAND

Options:
  -l, --list-commands
  -r, --run              Run it in your SQLite db
  -d, --database         Database file to use
  -c, --command          Command to run
  -h, --help             You know what dis do
```

Say you type this:

`yas-qwin --print-table your_table_name`

Out comes:

```sql
-- Print contents of table
SELECT * FROM your_table_name;
```

Or run it in your database, you hottie pipeliner you:

`yas-qwin -d your_database.db -c print-indexes --run`

## Commandments

YAS-QWIN's commands shall lead the faithful, AYY LMAO:

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
  - This one is so badass it has its own subcommands:
    - `column-def`
    - `table-constr`
    - `foreign-key-clause`
- ...plus many more to explore with `--list-commands`.
  - Not really... That's it.

## Help

Every one of YAS-QWIN's commands comes with its own help message. Just type
`--help` after the command to get a detailed explanation of its usage.

But because you are a busy sexy boi, you don't have time for that, so here are
all the motherloving help outputs:

### `create-table`

```bash
Usage: q -c create-table [OPTIONS] TABLE_NAME COLUMN_DEFINITIONS [TABLE_CONSTRAINTS]
Options:
  -t, --temporary
  -i, --if-not-exists
  -a, --as-select SELECT_STATEMENT
SQLite only options:
  -s, --strict (defaults to true)
  -w, --without-rowid
```

### `column-def`

```bash
Usage: q -c column-def COLUMN_NAME TYPE [OPTIONS]
Options:
  -p, --primary-key [CONFLICT-CLAUSE]
  -a, --autoincrement
  -d, --descending
  -n, --not-null [CONFLICT-CLAUSE]
  -u, --unique [CONFLICT-CLAUSE]
  -k, --check VALUE
  -d, --default VALUE
  -f, --foreign-key FOREIGN_KEY
  -g, --generated-as VALUE
  -s, --stored
```

### `table-constr`

```bash
Usage: q -c table-constr [COLUMN_NAMES] [OPTIONS]
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

### `foreign-key`

```bash
Usage: q -c foreign-key FOREIGN_TABLE_NAME [FOREIGN_COLUMN_NAMES]
Options:
  -e, --on-delete VALUE
  -u, --on-update VALUE
  -f, --deferred

VALUE can be one of:
  cascade
  restrict
  "set null"/"null"
  "set default"/"default
```

## Testing Grounds

For those of you who tread cautiously, YAS-QWIN skips the database execution
with a mere omission of `-r/--run`, printing the sacred scriptures (queries) to
your terminal for you to judge.

## Known Limitations

YAS-QWIN comes with some quirks:

- Not as diverse as could be yet. It do be a fierce CLI though.
- Might toss an "AYY LMAO" at you, for that's its way of saying "Under
  construction."
- Not all the SQL commands are supported. `SELECT`, `INSERT` and `UPDATE` are actually quite hard to implement. They are the main villains of the YAS-QWIN dev team, AYY LMAO.

## Disclaimer

While the idea of writing SQL through a CLI is mostly silly, no one knew for
sure until now. And while YAS-QWIN might be quirky, it stands as a half-serious
way of learning and automating your SQL.
