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




### Reserved Keywords
```sql

```

```sql
SELECT first_name FROM person
```