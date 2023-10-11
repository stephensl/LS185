## Connect to PostgreSQL database in Ruby using `pg` gem

### Method: `PG.connect`

  - this method will instantiate a new `PG::Connection` object and return it. 

#### Syntax: 

```ruby
PG.connect(dbname: "a_database")
```


## Execute a SQL query and return `PG::Result` object

### Method: `PG::Connection.exec`

  - the `exec` method is called on objects of the `PG::Connection` class. 
  - argument to `exec` is SQL query


#### Syntax: 

```ruby 
connection.exec("SELECT * FROM table_name")
```
  - returns `PG::Result` object

#

You can call methods on results object: can view all methods available to results object in `pry` using 
`ls name_of_results_object` then can use `show-method` to explore functionality.


### `result.values`

`values` returns array of arrays where each array represents a row in the database. 

#

### `result.fields`

`fields` returns array of all column names in the result set. 

#

### `result.ntuples`

`ntuples` returns number of rows in result set

#

### `result.each` yields hash to block

```ruby 
result.each do |tuple|
  puts "#{tuple["title"]} came out in #{tuple["year"]}"
end 
```

When we call `each` method on `PG::Result` object, will be yielding hash to the block (called tuple in this context).
  - keys are names of columns
  - values are values of data in that row

#

### `result.each_row` yields array to block

  - Use column indexes to access values. 

```ruby
result.each_row do |row|
  puts "#{row[1]} came out in #{row[2]}"
end 
```

#

### `result.field_values("name_of_column")`

   - Returns all values for all rows in specific column.

### `result.column_values("index_of_column")`

  - returns values for column referenced by index 


## Command Reference Table

### Connection Management

| Method                                         | Description                                 | Example                                 |
|------------------------------------------------|---------------------------------------------|-----------------------------------------|
| `PG.connect(dbname: "a_database")`              | Create a new `PG::Connection` object        | `conn = PG.connect(dbname: 'my_db')`     |

---

### Query Execution

| Method                                         | Description                                 | Example                                        |
|------------------------------------------------|---------------------------------------------|------------------------------------------------|
| `connection.exec("SELECT * FROM table_name")`  | Execute a SQL query                          | `result = conn.exec("SELECT * FROM users")`    |

---

### Result Handling

#### Basic Info

| Method                    | Description                                       | Example                                |
|---------------------------|---------------------------------------------------|----------------------------------------|
| `result.values`            | Array of Arrays with row values                    | `values = result.values`               |
| `result.fields`            | Column names as an Array of Strings                | `fields = result.fields`               |
| `result.ntuples`           | Number of rows in result                           | `row_count = result.ntuples`           |

#### Iterating Over Rows

| Method                    | Description                                       | Example                                |
|---------------------------|---------------------------------------------------|----------------------------------------|
| `result.each(&block)`      | Yield Hash of column names and values              | `result.each { |row| puts row['name'] }`|
| `result.each_row(&block)`  | Yield Array of values for each row                 | `result.each_row { |row| puts row[0] }` |

#### Accessing Specific Rows and Columns

| Method                    | Description                                       | Example                                |
|---------------------------|---------------------------------------------------|----------------------------------------|
| `result[index]`            | Hash of values for row at index                    | `row = result[0]`                      |
| `result.field_values(col)` | Array of values for a specific column              | `names = result.field_values('name')`  |
| `result.column_values(i)`  | Array of values for column at index                | `ids = result.column_values(0)`        |
