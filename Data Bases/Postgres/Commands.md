#database #postgres #commands 
# Postgres Commands
| Command | Description |
|---------|-------------|
| `psql -U [username] -d [database_name]` | Connect to a database |
| `\l` | List all databases |
| `\c [database_name]` | Connect to a specific database |
| `\dt` | List all tables in the current database |
| `\d [table_name]` | Describe a specific table |
| `CREATE DATABASE [database_name];` | Create a new database |
| `CREATE TABLE [table_name] ([column_name] [data_type] [constraints], [column_name] [data_type] [constraints], ...);` | Create a new table |
| `INSERT INTO [table_name] ([column_name1], [column_name2], ...) VALUES ([value1], [value2], ...);` | Insert data into a table |
| `UPDATE [table_name] SET [column_name1] = [new_value1], [column_name2] = [new_value2], ... WHERE [condition];` | Update data in a table |
| `DELETE FROM [table_name] WHERE [condition];` | Delete data from a table |
| `CREATE INDEX [index_name] ON [table_name] ([column_name]);` | Create an index on a table |
| `pg_dump [database_name] > [backup_file_name]` | Backup a database |
| `psql [database_name] < [backup_file_name]` | Restore a database |

