# MySQL Cheatsheet

## User
#### Select all users
```mysql
SELECT User, Host FROM mysql.user;
```


#### Show grants of a user

```mysql
SHOW GRANTS FOR @user ;
```

#### Create users
```mysql
CREATE USER 'myuser'@'TARGET IP/SERVER' identified by 'password';


CREATE USER 'myuser'@'localhost' identified by 'password';
```

#### Altering user password
```mysql
ALTER USER 'userName'@'localhost' IDENTIFIED BY 'New-Password-Here';
ALTER USER 'userName'@'%' IDENTIFIED BY 'New-Password-Here';
```

## Grants & Privileges

#### Revoking access to users
```mysql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'user'@'localhost';
```

#### Granting priv
```mysql
GRANT SELECT, ALTER ROUTINE, CREATE ROUTINE, EXECUTE ON database TO 'user'@'%'
```


## Database

#### Create database 
```mysql
CREATE DATABASE db_facility charset utf8mb4 collate utf8mb4_general_ci;
```

#### Get database table count
```mysql
select table_schema, count(*) from information_schema.TABLES
where table_schema in ('coaching_log')
group by table_schema
order by 1
```

#### Get database schema table row count and table list
```mysql
select table_schema, table_name, table_type, row_format, table_rows
from information_schema.TABLES
where table_schema in ('coaching_log') order by 1, 2
```
