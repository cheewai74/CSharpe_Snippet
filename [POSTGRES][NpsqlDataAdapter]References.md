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
