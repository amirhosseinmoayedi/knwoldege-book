#database #sql #postgres 

![[_resources/aa2047b870024caaea0d2bd1328496c7.png]]

![[_resources/1e6153144d8900eca29c207dc5b78a2e.png]]
**<font color="#d99694">logical key</font> is normal info we save <font color="#d99694">like email and url </font>and etc not an intor float just string.**

<u>naming</u> <font color="#92d050">primary key</font> is normally called `id`.
<font color="#e36c09">foreign keys</font> naming convention is `tablename_id`. 

![[_resources/bd99d0ff5012ead2870a9dc9b34c4635.png]]

![[_resources/04dcc0b0a853470a46c895119efc26b3.png]]

<font color="#ffc000">Third Normal Form (3NF)</font> is a level of<font color="#ffc000"> database normalization</font> that aims to <font color="#ffc000">reduce data redundancy</font> and <font color="#ffc000">improve data integrity</font>. It builds upon the first two normal forms (1NF and 2NF)
![[_resources/8dc468203749f9eb6f5c6c241287dea1.png]]

## <font color="#76923c">Delete Policy</font>
When defining a<font color="#366092"> foreign key</font> in a database table, you can specify a delete policy that <font color="#366092">determines what should happen when a row in the referenced table is deleted</font>.
1.  `CASCADE`: When a row in the referenced table is deleted, all rows in the referencing table that reference that row are also deleted.    
2.  `SET NULL`: When a row in the referenced table is deleted, all rows in the referencing table that reference that row have their foreign key values set to `NULL`.
3.  `RESTRICT`: Prevents the deletion of a row in the referenced table if there are any referencing rows in the referencing table.    
4.  `NO ACTION`: Similar to `RESTRICT`, but only supported by some database systems. It behaves the same way as `RESTRICT`.
5.  `SET DEFAULT`: Sets the foreign key values in the referencing table to their default values when a row in the referenced table is deleted.

### <font color="#00b050">Unique Toghter</font>
A unique constraint can<u> be applied to a combination of columns in a database table</u> to ensure that the combination of values across those columns is unique.
```sql
CREATE TABLE users (
  user_id serial primary key,
  username varchar(50) not null unique,
  email varchar(100) not null,
  UNIQUE (username, email)
);
```

### <font color="#00b0f0">Join</font>
![[_resources/6ffcc2d4b9b95a06f7b27292dd7ec89f.png]]

1.  Inner join: Returns only the rows that have matching values in both tables being joined.
2.  Left join (or left outer join): Returns all the rows from the left table and the matching rows from the right table. If there is no match in the right table, the result will contain null values for the right table columns.  
3.  Right join (or right outer join): Returns all the rows from the right table and the matching rows from the left table. If there is no match in the left table, the result will contain null values for the left table columns.
4.  Full outer join: Returns all the rows from both tables, matching rows where possible and including null values for non-matching rows.
5.  Cross join (or cartesian join): Returns the Cartesian product of the two tables, which means it returns all possible combinations of rows from both tables.
![[Pasted image 20230502173917.png]]

```sql
-- Inner join
SELECT *
FROM table1
INNER JOIN table2
ON table1.column = table2.column;

-- Left join
SELECT *
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;

-- Right join
SELECT *
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;

-- Full outer join
SELECT *
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;

-- Cross join
SELECT *
FROM table1
CROSS JOIN table2;
```


### <font color="#95b3d7">many-to-many </font>
A many-to-many relationship in a database occurs when multiple records in one table are associated with multiple records in another table.
![[0c179b195ae79efd5865b00373689aa9.png]]
**in many to many table we don't have id for each row, some times it does not primary key.**

many to many table can have extra field for it's own.
