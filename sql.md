**[< Home](README.md)**
# SQL

`LEAD(column, offset) OVER()`
Return the value of the next row following the order given in the order

Ex : 
```sql
SELECT id, LEAD(id, 1) OVER(ORDER BY id) FROM table;
```
## JOIN
- INNER JOIN
- OUTER JOIN
  - LEFT JOIN
  - RIGHT JOIN
  - FULL JOIN
- NATURAL JOIN
- CROSS JOIN

# Postgresql

## Transaction :
xmin id de la transaction qui modifie la donnée
xmax quelle transaction vas invalider la donnée 

autobacklog (= garbage collector)

## Isolation Mode : 
### READ COMMITED (default) 
- commited data  = valid data 
- authorize the non reapetable read 
- authorize the phantom reads

non repeatable read ex :
GLOBAL SUM 
- update during the calc
faild sum...

### REPEATABLE READ
- snapshot of data taken at the begining of the transaction
- the posterior commit aren't visible 
- force to handle serialization failure

#### snapshot 
contain 3 information : 
- xmin : the smaller XID (transaction ID) active when the snapshot is taken
  - all XID < xmin is considered as visible and commited 
- xmax : the next XID that will be assign
  - all XID >= xmax will be invisible
- xip_list : the list of active XID when the snapshot is taken 
  - those transaction will be invisible even is XID e [xmin, xmax[

Can't update the same value in this mode (ERROR)


## [WAL](acronyme.md#wal) (for durability)
- Journal of all data modification
- Only written on small file
- Always writen before the real modification on disk


- Crash: postgresql rebuild based on WAL
- Periodic snapshot of datas
- Those small file equily serve the duplication
