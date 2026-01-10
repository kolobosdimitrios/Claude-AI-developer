# Database Migrations

## Naming Convention

Migration files should be named:
```
VERSION_description.sql
```

Examples:
- `2.27.0_add_user_preferences.sql`
- `2.28.0_create_audit_log.sql`
- `2.28.1_add_index_tickets.sql`

## Rules

1. **Version prefix**: Use the version number where this migration is introduced
2. **Idempotent**: Migrations should be safe to run multiple times (use `IF NOT EXISTS`, etc.)
3. **Forward only**: No rollback support - make sure migrations are correct before release
4. **Order**: Migrations are applied in version order (sorted by filename)

## Example Migration

```sql
-- 2.27.0_add_user_preferences.sql
-- Add user preferences table

CREATE TABLE IF NOT EXISTS user_preferences (
    user_id INT PRIMARY KEY,
    theme VARCHAR(20) DEFAULT 'dark',
    notifications_enabled TINYINT(1) DEFAULT 1,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES developers(id) ON DELETE CASCADE
);

-- Add column to existing table (with IF NOT EXISTS check)
SET @exist := (SELECT COUNT(*) FROM INFORMATION_SCHEMA.COLUMNS
               WHERE TABLE_SCHEMA = DATABASE()
               AND TABLE_NAME = 'tickets'
               AND COLUMN_NAME = 'estimated_hours');

SET @query := IF(@exist = 0,
    'ALTER TABLE tickets ADD COLUMN estimated_hours DECIMAL(5,2) DEFAULT NULL',
    'SELECT "Column already exists"');

PREPARE stmt FROM @query;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

## Tracking

Applied migrations are tracked in the `schema_migrations` table:
```sql
SELECT * FROM schema_migrations ORDER BY applied_at;
```
