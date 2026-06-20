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
