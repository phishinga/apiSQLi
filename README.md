# Simple Vulnerable Web App (SQLi)
Simple vulnerable to SQLi Python Flask web application that connects to a Microsoft SQL Server database and provides a simple API for searching and retrieving data.

![image](https://user-images.githubusercontent.com/121772502/231738945-201f9836-f6ab-45d1-8107-6d47413ea1af.png)
<sub>Please note: The repo contains only the app part, the underlying infrastructure needs to be deployed separately.</sub>

The app has five routes:

 1. **vulnerable():** 
    - Vulnerable to SQL injection attacks; accepts search term input as a string, concatenates it directly into the SQL query string, and returns the query results. Uses POST method. 
    - Simply go to: http://localhost:5000/vulnerable and play around.
 2. **home():** 
    - The home route ("/") serves a simple HTML form for searching the database by product ID. When the form is submitted, it sends a POST request to the same route, which queries the database and returns the results to a results.html template. 
    - Simply go to: http://localhost:5000/
3. **vulnerable_query():** 
    - Vulnerable to SQL injection attacks; accepts id parameter input as a string, concatenates it directly into the SQL query string, and returns the query results. Uses GET method. 
    - Simply go to http://localhost:5000/vulnerable_query?id=9 or use curl or Postman.
4. **secure_query():** 
    - Secure search route with input validation; only accepts id parameter input as a valid integer value, uses parameterized queries to prevent SQL injection attacks, and returns the query results. Uses GET method. 
    - Simply go to http://localhost:5000/secure_query?id=9 or use curl or Postman.
5. **The test_database()** 
    - This Function is used to test the database connection. 
    - Simply go to http://localhost:5000/test.

## Running the App

- Simply copy paste the whole repo and run ```python3 apiSQLi.py```
- The application listens on port 5000 and can be accessed via the URLs bellow.
- Please note: The repo contains only the app part, the underlying infrastructure needs to be deployed separately.

GET
- http://localhost:5000/secure_query?id=9
- http://localhost:5000/vulnerable_query?id=9
- Query that gets back all the results from table: 1 ```OR 1=1 --```

POST
- http://localhost:5000/
- http://localhost:5000/vulnerable
- Query that gets back all the results from table: ```' OR 1=1 --```

### Few POST Examples:

![image](https://user-images.githubusercontent.com/121772502/231851870-3b52f8d1-f183-4ea8-9cd2-122c5238639d.png)

![image](https://user-images.githubusercontent.com/121772502/231852002-f0abdd22-40cf-455b-9f80-84ac4f21ac0d.png)


## Dependencies and Requirements

Please note: The repo contains only the app part, the underlying infrastructure needs to be deployed separately.

- Flask: This is a web application framework for Python that provides tools for building web applications, including HTTP routing, template rendering, and request handling. It is used to create the web application and define the routes.
- Pyodbc: This is a Python module for accessing databases using ODBC drivers. It is used to connect to the SQL database and execute SQL queries.
- Microsoft ODBC Driver for SQL Server: This is a driver that allows applications to connect to a Microsoft SQL Server database. It is used by Pyodbc to establish a connection to the database. Available here: https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver16&tabs=alpine18-install%2Calpine17-install%2Cdebian8-install%2Credhat7-13-install%2Crhel7-offline
- Jinja2: This is a templating engine for Python that allows for the separation of application logic and presentation. It is used to render HTML templates for the web application.

To install these dependencies, you can use pip, the package installer for Python:

```
pip install flask pyodbc jinja2
```

- In addition to the above dependencies, you will also need to have a SQL Server instance to connect to, and a database with tables that match the SQL queries used in the application.
- Also, the application runs on port 5000 by default, so you will need to ensure that this port is open and available on your machine.
- Finally, if you want to understand the code check out this: https://auth0.com/blog/developing-restful-apis-with-python-and-flask/

## Variables

To successfully connect to the SQL Server database, you'll need to populate the following variables in the code with the appropriate values:

- server_name: This should be the name of the SQL Server instance that you want to connect to.
- database_name: This should be the name of the database that you want to query.
- username: This should be the username for the database user that you want to authenticate as.
- password: This should be the password for the database user that you want to authenticate as.
- odbc_driver: This should be the path to the ODBC driver that you want to use to connect to the SQL Server instance. 

    - Mine was: "/opt/microsoft/msodbcsql18/lib64/libmsodbcsql-18.0.so.1.1"

Additionally, the query variables in functions should be modified to match the table and column names in your own database schema.:

```
query = "SELECT * FROM [SalesLT].[Address] WHERE AddressID = ?"
```

- All values that need to be changed are marked in the code as: "XXXXXXXXXXXXXXXXXXXX"

## Securing the queries

-  The input validation checks whether the search_term parameter is a valid integer value. If it is not, the function returns a 400 Bad Request response with an error message.
- The query string now uses a placeholder (?) instead of string concatenation to indicate the position of the parameter in the query.
- The execute method now takes a tuple of parameter values as its second argument, and passes ('%' + search_term + '%',) as the parameter value. This ensures that the parameter is properly sanitized and validated as a string value.
- The function now returns a rendered template with the results of the query.
- With these changes, the function now uses parameterized queries to prevent SQL injection attacks, and input validation to ensure that the search_term parameter is a valid integer value.
- Also make sure to check this: https://portswigger.net/web-security/sql-injection

## SQLMap Results

![image](https://user-images.githubusercontent.com/121772502/231740531-17eff377-197d-437c-a758-74c4289a85b1.png)

## SQLi Results

![image](https://user-images.githubusercontent.com/121772502/231852130-71fe4527-7ec3-4adc-b58d-ad9fdf5d4f06.png)


## Disclaimer
Please note, that this project was created simply for demonstrating purposes and servers only as a playground for anyone who wants to learn more about SQLi. If you are looking for more robust product go search for Damn Vulnerable Web Apps as these provide more challanges to play with. 
