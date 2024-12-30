SELECT * FROM pg_settings;

SELECT name, setting FROM pg_settings WHERE name  like 'sql_mode';