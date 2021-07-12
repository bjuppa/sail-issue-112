# Reproduction of problem with parallel testing with Sail when database user is other than `root`

See discussion at https://github.com/laravel/sail/issues/112

This repo is a fresh Laravel install modified to put another `DB_USERNAME` than `root` into `.env`.

## Steps to reproduce

1. Clone this repo and enter the directory:

    ```sh
    git clone git@github.com:bjuppa/sail-issue-112.git && cd sail-issue-112
    ```

2. Install dependencies and copy `.env.example` to `.env`:

    ```sh
    composer create-project
    ```

3. Start sail:

    ```sh
    sail up
    ```

4. Run tests in parallel and recreate databases while doing so:

    ```sh
    sail test --parallel --recreate-databases
    ```

## Result

The test runner seems to drop & create a new database using the non-root db user configured in `.env` and it lacks permissions to do so:

```sql
SQLSTATE[42000]: Syntax error or access violation: 1044 Access denied for user 'sail_issue_112'@'%' to database 'sail_issue_112_test_1' (SQL: drop database if exists `sail_issue_112_test_1`)
```
