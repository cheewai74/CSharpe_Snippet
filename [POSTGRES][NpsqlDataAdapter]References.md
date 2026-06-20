Connection String Options</BR>
|	Option	|	Meaning	|
| ----- 	| -----		|
|Server | The name or IP address of the server running PostgreSQL |
|Port | The port number to connect to; defaults to the standard port |
|Protocol | The protocol version number to use (2 or 3); if omitted, this will be chosen automatically |
|Database  | The name of the database; defaults to the same value as the User Id | 
|User Id | The username | 
|Password | The password |
|SSL | Sets the connection security as true or false; defaults to false |
|Pooling | Sets connection pooling as true or false; defaults to true |
|MinPoolSize | Sets the lower bound of the connection pool size |
|MaxPoolSize | Sets the upper bound of the connection pool size |
|Timeout | Sets the time to wait for a connection before timing out |

NpgsqlConnection Properties</BR>
|Property	|	Meaning	|
| ----- 	| -----		|
|ConnectionString	|	Gets or sets the connection string	|
|Database	|	Gets the name of the current database	|
|ServerVersion	|	Gets the version of the server currently connected to	|
|State	|	Gets the current state of the connection	|

Common NpgsqlCommand Properties</BR>
|Method	|	Meaning	|
| ----- 	| -----		|
|CommandText	|	Allows the command text to be retrieved or set|
|CommandTimeout	|	Sets how long the system will wait for the command to execute before terminating it|
|CommandType	|	Sets or gets the type of command; by default, this is Text for executing SQL statements, but can also be Stored Procedure when the command is to execute a stored procedure|
|Connection	|	Sets or gets the connection object to be used|
|Parameters	|	Allows access to parameters for prepared statements|
|Transaction |	Sets or gets the transaction in which the command is to execute|

Common NpgsqlConnection Methods</BR>
|Method	|	Meaning	|
| ----- 	| -----		|
|BeginTransaction	|	Starts a transaction and optionally passes an isolation level to use |
|ChangeDatabase	|	Closes the connection and reconnects to a different database |
|Close	|	Closes the connection, or, if using connection pooling, releases the connection back to the pool |
|Open	|	Opens the connection|

Common NpgsqlDataReader Properties</BR>
|Property	|	Meaning	|
| ----- 	| -----		|
|FieldCount	| Provides the number of columns in the data row |
|HasRows	| Set to true if there is one or more rows of data ready to be read |
|IsClosed |	Set to true if the data reader has been closed |
|Item	|	Retrieves the column in its native format |
|RecordsAffected	|Provides the number of rows affected by the SQL statement |
