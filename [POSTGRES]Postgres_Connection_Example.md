Note: Stop using ODBC, it's not stable, use  Npgsql instead.</BR>
Precondition:</BR>
```
dotnet add package Npgsql
```
Example Code</BR>
```
// vs-connect.cs

using System;
using System.Data;
using Npgsql;

class Program
{
    static void Main(string[] args)
    {
        var conn = new NpgsqlConnection("Host=127.0.0.1;Username=postgres;Password=####;Database=#####");
        conn.Open();
        Console.WriteLine("Version: {0}", conn.ServerVersion);
        conn.Close();
        Console.ReadLine();

    }
}

```

Full Example</br>
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

            // string sql = "SELECT * FROM apress.customer";

            // using var adapter = new NpgsqlDataAdapter(sql, conn);

            // DataTable dt = new DataTable();
            // adapter.Fill(dt);

            // foreach (DataRow dr in dt.Rows)
            // {
            //     Console.WriteLine("Name: {0}, {1}, {2}",
            //         dr["customer_id"], dr["fname"], dr["lname"]);
            // }
            // Console.WriteLine();

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


            DataSet ds = new DataSet();
            NpgsqlDataAdapter da = new NpgsqlDataAdapter(
                "SELECT description, cost_price, sell_price FROM apress.item", 
                conn);

            da.InsertCommand = new NpgsqlCommand(
                "INSERT INTO apress.item(description, cost_price, sell_price) VALUES(@de, @cp, @sp)", 
                conn);

            // Set the data type of the parameters
            da.InsertCommand.Parameters.Add(new NpgsqlParameter(":de", DbType.String));
            da.InsertCommand.Parameters.Add(new NpgsqlParameter(":cp", DbType.Decimal));
            da.InsertCommand.Parameters.Add(new NpgsqlParameter(":sp", DbType.Decimal));

            // Bind the parameters to the coumn names in the item table.
            da.InsertCommand.Parameters[0].SourceColumn = "description";
            da.InsertCommand.Parameters[1].SourceColumn = "cost_price";
            da.InsertCommand.Parameters[2].SourceColumn = "sell_price";

            // Use DataAdapter to populate the DataSet that was created earlier.
            da.Fill(ds);

            // Create a DataTable as the first (and only) table in the DataSet
            DataTable  dt = ds.Tables[0];

            // Create a new row and populate its columns
            DataRow dr = dt.NewRow();
            dr["description"] = "Large Penguin";
            dr["cost_price"] = 7.23m;
            dr["sell_price"] = 9.99m;

            // Add the new row to our DataTable, and tell the DataAdapter to update, based on our 
            // DataSet with the new row

            dt.Rows.Add(dr);
            da.Update(ds);    
        }
        finally{
            conn.Close();
        }
    }
}

```
