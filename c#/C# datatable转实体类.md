# C# DataTable转实体类

方法1:直接使用sqlQuery<T> 连表查询也可以用
```c#
var data = db.Database.SqlQuery<Order_Order>(sql);
```

方法2：

```c#
        /// <summary>  
        /// 填充对象列表：用DataTable填充实体类
        /// </summary>  
        public List<T> FillModel(DataTable dt)
        {
            if (dt == null || dt.Rows.Count == 0)
            {
                return null;
            }
            List<T> modelList = new List<T>();
            foreach (DataRow dr in dt.Rows)
            {
                //T model = (T)Activator.CreateInstance(typeof(T));  
                T model = new T();
                for (int i = 0; i < dr.Table.Columns.Count; i++)
                {
                    PropertyInfo propertyInfo = model.GetType().GetProperty(dr.Table.Columns[i].ColumnName, BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);
                    if (propertyInfo != null && dr[i] != DBNull.Value)
                    {
                        string value = dr[i] == null ? "" : dr[i].ToString();
                        //string typestr = dr[i].GetType().Name;
                        //if(typestr.Equals("DateTime"))
                            //value = dr[i].ToString();
                        propertyInfo.SetValue(model, value, null);
                    }
                        
                }

                modelList.Add(model);
            }
            return modelList;
        }
```

