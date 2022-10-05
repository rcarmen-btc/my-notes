# Learn Relational Databases

#Bash #PostgreSQL

### Key words | Questions

### Notes

Dump:

```bash
pg_dump -cC --inserts -U freecodecamp universe > universe.sql
```

Load: 

```bash
psql -U postgres < universe.sql
```

# Databases

---

- **List of databases**
    
    First thing to do is see what databases are here.
    
    Type `\l` into the prompt to **l**ist them.
    
- **Create database**
    
    ```sql
    CREATE DATABASE database_name;
    ```
    
    The capitalized words are keywords telling PostgreSQL what to do. The name of the database is the lowercase word. Note that **all commands need a semi-colon at the end.**
    
- **Connect to database**
    
    Enter `\c <database_name>` to **c**onnect to database.
    
- **Rename database**
    
    ```sql
    ALTER DATABASE database_name RENAME TO new_database_name;
    ```
    
- **Drop database**
    
    ```sql
    DROP DATABASE database_name;
    ```
    

# Tables

---

- **Display tables**
    
    Enter `\d` to **d**isplay the tables.
    
    You can view more details about a table by adding the table name after the **d**isplay command like this: `\d <table_name>`.
    
- **Create table**
    
    Create a table like this:
    
    ```sql
    CREATE TABLE table_name();
    ```
    
    Note that the parenthesis are needed for this one. It will create the table in the database you are connected to. Create a table named `first_table` in `second_database`.
    
    Inside those parenthesis you can put columns for a table so you don't need to add them with a separate command, like this:
    
    ```sql
    CREATE TABLE table_name(column_name DATATYPE CONSTRAINTS);
    ```
    
- **Drop table**
    
    Here's an example:
    
    ```sql
    DROP TABLE table_name;
    ```
    
- **Delete all data from table**
    
    You can use `TRUNCATE` to delete all data from a table. In the psql prompt, try to delete all the data in the majors table by entering `TRUNCATE majors;`
    
    ```sql
    TRUNCATE students, majors_courses, majors;
    ```
    

# Columns

---

- **Add column**
    
    ```sql
    ALTER TABLE table_name ADD COLUMN column_name DATATYPE;
    ```
    
    Add a column to `second_table` named `first_column`. Give it a data type of `INT`. `INT` stands for integer. Don't forget the semi-colon. 😄
    
- **Data types:**
    - `INT` - integer type
    - `VARCHAR(<maxinmum_length>)` - a short string of characters. You need to give it a maximum length.
    - `SERIAL` - will make your column an `INT` with a `NOT NULL` constraint, and automatically increment the integer when a new row is added.
    - `NUMERIC(4, 1)` - has up to four digits and one of them has to be right of the decimal.
    - `DATE` -
    - `FOREIGN KEY`  -
        
        ```sql
        ALTER TABLE table_name ADD FOREIGN KEY(column_name) REFERENCES 
        referenced_table(referenced_column);
        ```
        
- **Constraints**
    - `PRIMARY KEY` - a column that uniquely identifies each row in the table. Here's an example of how to set a `PRIMARY KEY`:
        
        ```sql
        ALTER TABLE table_name ADD PRIMARY KEY(column_name);
        
        ALTER TABLE table_name ADD PRIMARY KEY(column1, column2);
        ```
        
        The `name` column is pretty unique, why don't you set that as the primary key for this table.
        
    - `NOT NULL`
    - `UNIQUE`
- **Constraints**
    - `PRIMARY KEY` - a column that uniquely identifies each row in the table. Here's an example of how to set a `PRIMARY KEY`:
        
        ```sql
        ALTER TABLE table_name ADD PRIMARY KEY(column_name);
        
        ALTER TABLE table_name ADD PRIMARY KEY(column1, column2);
        ```
        
        The `name` column is pretty unique, why don't you set that as the primary key for this table.
        
    - `NOT NULL`
    - `UNIQUE`
- **Drop** **constraint**
    
    Here's an example of how to drop a constraint:
    
    ```sql
    ALTER TABLE table_name DROP CONSTRAINT constraint_name;
    ```
    
- **Create a column as a foreign key as one-to-one**
    
    ```sql
    ALTER TABLE table_name ADD COLUMN column_name (DATATYPE) [CONSTRAINT] REFERENCES 
    referenced_table_name(referenced_column_name);
    ```
    
    To set a row in `more_info`for Mario, you just need to set the `character_id` (foreign key) value to whatever it is in the `characters` table. Take a look at the details of `more_info` to see your foreign key.
    
- **Create a column as a foreign key as one-to-many**
    
    ```sql
    ALTER TABLE table_name ADD COLUMN column_name (DATATYPE) [CONSTRAINT] REFERENCES 
    referenced_table_name(referenced_column_name);
    ```
    

- **Enforce that by adding the `UNIQUE` constraint to your foreign key.**
    
    ```sql
    ALTER TABLE table_name ADD UNIQUE(column_name);
    ```
    
- **Drop column**
    
    You will probably need to know how to remove them. Here's an example:
    
    ```sql
    ALTER TABLE table_name DROP COLUMN column_name;
    ```
    
- **Rename a column**
    
    Here's how you can rename a column:
    
    ```sql
    ALTER TABLE table_name RENAME COLUMN column_name TO new_name;
    ```
    

# Rows

---

- **Add row**
    
    Rows are the actual data in the table. You can add one like this:
    
    ```sql
    INSERT INTO table_name(column_1, column_2) VALUES(value1, value2);xx`
    ```
    
    If column type is text type you need to put text in single quotes, like `‘Anya’`
    
- **Retrieve rows**
    
    You can view the data in a table by querying it with the `SELECT` statement. Here's how it looks:
    
    ```sql
    SELECT columns FROM table_name;
    ```
    
    Use a `SELECT` statement to view **all** the columns in `second_table`. Use an asterisk (`*`) to denote that you want to see all the columns.
    
    - **Order by**
        
        Actually, you should put that in order. Here's an example:
        
        ```sql
        SELECT columns FROM table_name ORDER BY column_name;
        ```
        
    - **Where**
        
        You can use it to view rows as well. Here's an example:
        
        ```sql
        SELECT columns FROM table_name WHERE condition;
        ```
        
    - **Join**
        
        You added that as a foreign key, that means you can get all the data from both tables with a `JOIN` command:
        
        ```sql
        SELECT columns FROM table_1 FULL JOIN table_2 ON table_1.primary_key_column = table_2.foreign_key_column;
        ```
        
        `FULL` - this will display all data even if the matched row is empty.
        
    
- **Delete a row**
    
    Here's an example of how to delete a row:
    
    ```sql
    DELETE FROM table_name WHERE condition;
    ```
    
- **Faster way to add new rows**
    
    Adding rows one at a time is quite tedious. Here's an example of how you could have added the previous three rows at once:
    
    ```sql
    INSERT INTO characters(name, homeland, favorite_color)
    VALUES('Mario', 'Mushroom Kingdom', 'Red'),
          ('Luigi', 'Mushroom Kingdom', 'Green'),
          ('Peach', 'Mushroom Kingdom', 'Pink');
    ```
    
- **Change row**
    
    You can change a value like this:
    
    ```sql
    UPDATE table_name SET column_name=new_value WHERE condition;
    ```
    

<aside>
📌 **SUMMARY:**

</aside>