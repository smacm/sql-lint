# sql-lint

![Build Status](https://travis-ci.org/joereynolds/sql-lint.svg?branch=typescript)

`sql-lint` will do sanity checks on your query as well as bring errors back from the DB.
If you worry about forgetting `WHERE`s on a `DELETE` or borking your data with unexpected characters, then `sql-lint` is for you.

A complete list of the checks it does is below.

![Imgur](https://i.imgur.com/rJ3h34b.png)

![Imgur](https://i.imgur.com/nqi1MnT.gif)


## Installation

```
npm install -g sql-lint
```

Or download a [binary](https://github.com/joereynolds/sql-lint/releases)  

## Usage

`sql-lint` works on files, directories and stdin.

```
# Will lint all .sql files recursively from the current directory
sql-lint .

# Lints the create-person.sql file
sql-lint create-person.sql

# Lints stdin
echo 'DELETE FROM person;' | sql-lint
```

## Checks

`sql-lint` comes with its own suite of checks. It
also returns any errors from the SQL server you have connected to. Generally
you'll find that the errors from `sql-lint` are more informative than those from
the server. That said, you will still want errors from the server as it covers
more cases and will catch things that `sql-lint` does not.

### unmatched-parentheses

Shown when a query has an unbalanced amount of parentheses.

```
test/test-files//test.sql:16 [sql-lint: unmatched-parentheses] Unmatched parentheses.
```

### missing-where

Shown when a `DELETE` statement is missing a `WHERE` clause.

```
test/test-files/test.sql:20 [sql-lint: missing-where] DELETE statement missing WHERE clause.
```

### invalid-drop-option

Shown when an invalid option is given to the `DROP` statement.

```
test/test-files/test.sql:22 [sql-lint: invalid-drop-option] Option 'thing' is not a valid option, must be one of '["database","event","function","index","logfile","procedure","schema","server","table","view","tablespace","trigger"]'.
```

### invalid-create-option

Shown when an invalid option is given to the `CREATE` statement.

```
:24 [sql-lint: invalid-create-option] Option 'test' is not a valid option, must be one of '["algorithm","database","definer","event","function","index","or","procedure","server","table","tablespace","temporary","trigger","user","unique","view"]'.
```

### invalid-truncate-option

Shown when an invalid option is given to the `TRUNCATE` statement.

```
test/test-files/test.sql:26 [sql-lint: invalid-truncate-option] Option 'something' is not a valid option, must be one of '["table"]'.
```

### invalid-alter-option

Shown when an invalid option is given to the `ALTER` statement.

```
test/test-files/test.sql:28 [sql-lint: invalid-alter-option] Option 'mlady' is not a valid option, must be one of '["column","online","offline","ignore","database","event","function","procedure","server","table","tablespace","view"]'.
```

### odd-code-point

Shown when there are unsupported/unusual* code points in your code.

*<small>This check came about whilst working Microsoft Excel. Microsoft likes to
add a lot of zany characters which can subtly break your data without you
realising.</small>

```
test/test-files//test.sql:30 [sql-lint: odd-code-point] Unexpected code point.
```

### invalid-limit-quantifier

Shown when you specify something other than a number to the `LIMIT` statement.

```
test/test-files//test.sql:32 [sql-lint: invalid-limit-quantifier] Argument 'test' is not a valid quantifier for LIMIT clause.
```

### hungarian-notation

Shown when the string `sp_` or `tbl_` is present in the query.
```
test/test-files/test.sql:34 [sql-lint: hungarian-notation] Hungarian notation present in query
```


## Read more

To find out more, [read the documentation](https://sql-lint.readthedocs.io/)
