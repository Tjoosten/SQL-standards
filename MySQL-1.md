SQL-1 Styleguide. 
=====================

## 1. MySQL naming conventions. 

#### 1.1 Indexes 

Constraint names must be suffixed by character codes to indicate the type of constraint it is. The character codes are defined in the following table:

| If the constraint is a: | it’s name must be suffixed with: |
| :---------------------: | :------------------------------: |
| Primary key             | `_pk`                            |
| Foreign key             | `_fk`                            |
| Check                   | `_ck`                            |
| Not null                | `_nn`                            |
| Unique                  | `_uq`                            |
| Index                   | `_idx`                           |

## 2. Comments 

- All comments must use the sql standards double dash method (`--`), even if the database supports c-style comments (`/* ... */`). This is because of portability across other databases, as not all db's support c-style comments.
- All tables must be commented. 
- All views must be commented. 
- Table and View comments are to exist immediately preceding the table or view definition, aligned with the first character of the definition.
- Column comments are optional, but encouraged. They should be used whenever there is a possibility it might help in understanding. 
- Column commands must be declared immediately preceding the column definition, aligned with the first character of the column type and/or constraints.

## Space formatting standards 

- Tabs are forbidden. 
- Lines may not be extended past 80 characters. 
- Indents must be 4 spaces for column names and table wide constraint definitions. 
- Column definitions must be separated by 1 blank line. 
- Column types declarations must be on the same line as the column name declaration. 
- Column types, inline column constraints, and table-wide constraint names must be declared 2 spaces after the longest column name in the table. 
- Inline column constraint definitions (primary keys, foreign keys, unique, etc) must be declared on the following line after the column type.
- Long column constraint definitions that don't fit within 80 character viewing width must be extended to the next line, intented 4 spaces further than the where column definition began.

## 3. I/O & Performance

#### 3.1 Do not use SELECT * in your queries

Always write the required column names after the SELECT statement, like: 

```sql 
SELECT CustomerID, CustomerFirstName, City from Emp;
```

This technique results in reduced disk I/O and better performance.
