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

Common NpgsqlDataReader Methods</br>
|Method	|	Meaning	|
| ----- 	| -----		|
|Close	|	Closes the data reader object |
|Dispose	|	Releases all the resources in use |
|GetBoolean	|Gets a column value as a Boolean value |
|GetDateTime	|	Gets a column value as a datetime value         |
|GetDecimal	|	Gets a column value as a decimal number |
|GetDouble	|	Gets a column value as a double |
|GetFieldType|	Returns the data type of the column at an index position |
|GetFloat	|	Gets a column value as a floating-point number |
|GetInt16	|	Gets a column value as a 16-bit integer |
|GetInt32	|	Gets a column value as a 32-bit integer  |
|GetInt64	|	Gets a column value as a 64-bit integer |
|GetName		|	Gets the column name of a column by index  |
|GetString	|	Gets a column value as a string  |
|IsDBNull	|	True if the value in a column is NULL  |
|Read	|	Advances the data reader to the next row   |

Sample Code</BR>
```
namespace postgres_test_connection;

using System;
using System.Data;
using Npgsql;

class Program
{
    static void Main(string[] args)
    {
        var conn = new NpgsqlConnection("Host=127.0.0.1;Username=postgres;Password=password;Database=sandbox");
        try{
            conn.Open();

            Console.WriteLine("Version: {0}", conn.PostgreSqlVersion);
            Console.WriteLine("State: {0}, Server Version: {1}", conn.State, conn.ServerVersion);

            string sql = "SELECT * FROM apress.customer";

            using var adapter = new NpgsqlDataAdapter(sql, conn);

            DataTable dt = new DataTable();
            adapter.Fill(dt);

            foreach (DataRow dr in dt.Rows)
            {
                Console.WriteLine("Name: {0}, {1}, {2}",
                    dr["customer_id"], dr["fname"], dr["lname"]);
            }
            Console.WriteLine();

            // NpgsqlCommand cmd = new NpgsqlCommand("SELECT * FROM apress.orderinfo", conn);
            // NpgsqlDataReader datard = cmd.ExecuteReader();
            // while (datard.Read()){
            //     for (int i=0; i < datard.FieldCount; i++){
            //         Console.Write("{0}, ",datard[i]);
            //     }
            // }

            // NpgsqlCommand cmd2 = new NpgsqlCommand(
            // "SELECT * FROM apress.customer WHERE customer_id = :cid OR fname = :fn", conn);
            // cmd2.Parameters.Add(new NpgsqlParameter("cid", DbType.Int32));
            // cmd2.Parameters.Add(new NpgsqlParameter("fn", DbType.String));
            // cmd2.Parameters[0].Value = 2;
            // cmd2.Parameters[1].Value = "Jenny";
            // NpgsqlDataReader datard2 = cmd2.ExecuteReader();

            // while (datard2.Read()) {
            //     for (int i = 0; i < datard2.FieldCount; i++) {
            //     Console.Write("{0}, ", datard2[i]);
            //     }
            // Console.WriteLine();
            // }

            // NpgsqlCommand cmd = new NpgsqlCommand(
            // "SELECT * FROM apress.customer WHERE customer_id = :cid OR fname = :fn", conn);
            // cmd.Parameters.Add(new NpgsqlParameter("cid", DbType.Int32));
            // cmd.Parameters.Add(new NpgsqlParameter("fn", DbType.String));
            // cmd.Prepare(); // We prepare the statement before executing it the first time, and we can then simply change the
            //                // values of parameters, without needing to rebind them to NpgsqlParameter objects, and reexecute
            //                // the statement.
            // cmd.Parameters[0].Value = 2;
            // cmd.Parameters[1].Value = "Jenny";
            // NpgsqlDataReader datard = cmd.ExecuteReader();
            // while (datard.Read()) {
            // for (int i = 0; i < datard.FieldCount; i++) {
            // Console.Write("{0}, ", datard[i]);
            // }
            // Console.WriteLine();
            // }
            // datard.Close();
            // cmd.Parameters[0].Value = 3;
            // cmd.Parameters[1].Value = "Adrian";
            // datard = cmd.ExecuteReader();
            // while (datard.Read()) {
            // for (int i = 0; i < datard.FieldCount; i++) {
            // Console.Write("{0}, ", datard[i]);
            // }
            // Console.WriteLine();
            // }


            // NpgsqlCommand cmd = new NpgsqlCommand("INSERT INTO apress.customer(title, fname, lname, addressline, town, zipcode, phone) VALUES('Mr.', 'Simoner', 'Bennett','1 Victoria Street', 'Nicetown', 'NT4 2WS', '342 6352')", conn);
            // int rowsaffected = cmd.ExecuteNonQuery();
            // Console.Write("Rows affected {0}", rowsaffected);
                
        }
        finally{
            conn.Close();
        }
    }
}

```


