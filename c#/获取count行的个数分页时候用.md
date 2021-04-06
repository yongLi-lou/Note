# 获取count行的个数分页时候用

```c#
public static int SqlQueryCount(this DatabaseFacade facade, string sql, DbTransaction db = null, params object[] parameters)
        {
            var command = CreateCommand(facade, sql, out DbConnection conn, db, parameters);
            var reader = command.ExecuteReader();
            if (reader.Read())
            {
                var count = 0;
                int.TryParse(reader["count"].ToString(), out count);
                reader.Close();
                conn.Close();
                return count;
            }
            reader.Close();
            conn.Close();
            return 0;
        }
```

