# MySQL schema + seed execution log (single-statement CLI)

Connection source: `database_mysql/db_connection.txt`

Connection command (as read):
```bash
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp
```

## Commands executed (one statement at a time)

```bash
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "SET time_zone = '+00:00';"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "CREATE TABLE IF NOT EXISTS tasks (uid BIGINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY, title VARCHAR(255) NOT NULL, description TEXT NULL, is_completed TINYINT(1) NOT NULL DEFAULT 0, due_date DATETIME NULL, created_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP, modified_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, INDEX idx_tasks_is_completed (is_completed), INDEX idx_tasks_due_date (due_date)) ENGINE=InnoDB;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks ENGINE=InnoDB;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY uid BIGINT UNSIGNED NOT NULL AUTO_INCREMENT;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY title VARCHAR(255) NOT NULL;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY description TEXT NULL;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY is_completed TINYINT(1) NOT NULL DEFAULT 0;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY due_date DATETIME NULL;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY created_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "ALTER TABLE tasks MODIFY modified_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "INSERT INTO tasks (title, description, is_completed) SELECT 'Sample task','First task',0 WHERE NOT EXISTS (SELECT 1 FROM tasks WHERE title='Sample task');"
```

## Verification queries executed

```bash
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "SHOW CREATE TABLE tasks;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "SELECT COUNT(*) AS task_count FROM tasks;"
mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp -e "SELECT * FROM tasks LIMIT 5;"
```

## Notes
- Indexes are created as part of the `CREATE TABLE IF NOT EXISTS ... INDEX ...` statement.
- Separate `CREATE INDEX ...` commands will fail with `ERROR 1061 (42000): Duplicate key name` if the indexes already exist.
- Seed insert is idempotent by checking `NOT EXISTS` on `title='Sample task'`.
