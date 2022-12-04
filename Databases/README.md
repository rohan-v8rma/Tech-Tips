# Link between SQL Client and SQL Server

When we try to access the SQL server from our SQL client, we enter the required login details (IP address and port of the server, along with the password). Now, when we enter commands in our CLI, we are essentially running the queries directly in our SQL server. These commands are parsed by our SQL server, executed on our database, and the output is displayed.

When we try to access the SQL server from python, we use the mysql.connector API, to which we give the query we want to run as an argument. The SQL server then receives the request, executes the query and returns the output. This output can then be printed in python.