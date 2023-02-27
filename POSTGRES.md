### What is A Database
1. A place to store, manipulate, and retrieve data inside a **computer server**
2. Information stored about us   
3. **Relational database is simply relationship between one or more tables**
	1. Instead of storing 

### Postgres
- Manage data held in a relational database. Postgres is very powerful and been around since 1974
- Used all over the internet

**How are data stored**
* Stores data in tables: in columns and rows*

*Table - Person*
| id  | first_name | last_name | gender | age | car_id |
| --- | ---------- | --------- | ------ | --- | ------ |
| 1   | Anne       | Smith     | Female | 44  | 313    |
| 2   | Jake       | Jones     | Male   | 21  |        |

*Car Table*
| id  | make | model | price |
| --- | ---- | ----- | ----- |
| 313 | Anne | Smith | 44000      |
















Table - Host
| ID  | first_name | last_name | email | stripe_id | zoom_id | room_id | 
| --- | ---------- | --------- | ----- | --------- | ------- | ------- |

Table - Guest 
| ID  | first_name | last_name | email | auth_key | stripe_id | room_id |     |
| --- | ---------- | --------- | ----- | -------- | --------- | ------- | --- |
|     |            |           |       |          |           |         |     |

Table - Room
| id  | start_date | title | fee | host_id | finished | zoom_link | 
| --- | ---------- | --- | ------- | -------- | --------- |



### Tips
```sql
- If you have an error or something is not happening, check if you added ;
- If you are having errors, try using '' instead of ""
- Use VSCode to run your sql instead of powershell
```
### Reserved Datatypes
```sql
char(5): fixed 5 characters
varchar: store any length of characters
varchar(50): store up to 50 characters
text: store any length of characters


serial: whole numbers that auto increment. Used for column ids
smallserial: 1 to 32,767
serial: 1 to 2147483647
bigserial: 1 to 9223372036854775807

%% numerical %%
smallInt: -32,768 to 32,767
integer: -2,147,583 to 2,174,483,647
bigInt
decimal | numeric
real
double precision (15 places of precision, used for )
true
false
null

date (yyyy-mm-dd)
time
timestamp
interval: represent duration of time, add/subtract intervals

currency
binary
json
range

```
### Reserved Keywords
```sql
SELECT id, email, country FROM host
WHERE country = 'US'
AND email like 'hello%' <-- these become slow if big dataset, use indexing
ORDER BY id DESC
LIMIT 2;
```

```sql
default '': --set default value
```

```sql
SELECT first_name FROM person



```