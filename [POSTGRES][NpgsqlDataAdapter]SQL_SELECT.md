```
using System;
using System.Data;
using Npgsql;

class Program
{
    static void Main(string[] args)
    {
        var conn = new NpgsqlConnection("Host=127.0.0.1;Username=postgres;Password=password;Database=sandbox");
       
        conn.Open();

        Console.WriteLine("Version: {0}", conn.PostgreSqlVersion);

        string sql = "SELECT * FROM apress.customer";

        using var adapter = new NpgsqlDataAdapter(sql, conn);

        DataTable dt = new DataTable();
        adapter.Fill(dt);

        foreach (DataRow dr in dt.Rows)
        {
            Console.WriteLine("Name: {0}, {1}, {2}",
                dr["customer_id"], dr["fname"], dr["lname"]);
        }

        Console.ReadLine();
    }
}

```
