# The instance of entity type ‘xxx‘ cannot be tracked because another instance with the same key value

asp.net core 3.1 环境：

场景： 一起执行几个方法体， 先在一个地方修改or添加一条记录；接着又在另个地方修改了该条记录，就会报如下错误：

具体详细错误信息：The instance of entity type 'xxx' cannot be tracked because another instance with the same key value for {'Id'} is already being tracked. When attaching existing entities, ensure that only one entity instance with a given key value is attached. Consider using 'DbContextOptionsBuilder.EnableSensitiveDataLogging' to see the conflicting key values.

代码片段：

foreach (var item in list)
                {
                    var func = this.GetCodeCheckFunction(item);
                    var gauge = UnitWork.FindSingle(func);

                    if (gauge == null)
                    {
                        //新增
                        UnitWork.Add(item);
                    }
                    else
                    {
                        //修改
                        gauge.GaugeType = item.GaugeType;
                        gauge.ModifiedBy = _auth.GetCurrentUser().User.Id;
                        gauge.ModifiedTime = DateTime.Now;
                        UnitWork.Update(gauge);
                    }
                    UnitWork.Save();
                }
介绍下业务场景：导入时excel中存在重复数据。更新数据用的UnitWork。

上面的报错信息很明显了，它告诉我们一条数据已经被trace了，不能重复trace，只要我们把这条被trace的数据取消就行了。

下面是修改后的代码：

foreach (var item in list)
                {
                    var func = this.GetCodeCheckFunction(item);
                    var gauge = _dbContext.MesInfoGauges.FirstOrDefault(func);

                    if (gauge == null)
                    {
                        //新增
                        _dbContext.MesInfoGauges.Add(item);
                        _dbContext.SaveChanges();
                        _dbContext.Entry(item).State = EntityState.Detached;
                    }
                    else
                    {
                        //修改
                        gauge.GaugeType = item.GaugeType;
                        gauge.ModifiedBy = user.Id;
                        gauge.ModifiedTime = DateTime.Now;
                        _dbContext.MesInfoGauges.Update(gauge);
                        _dbContext.SaveChanges();
                        _dbContext.Entry(entity: gauge).State = EntityState.Detached;
                    }
                }
保存后，修改当前数据的Entry状态为Detached。

若为.netcore 2.2,则写法为：



```c#
_dbContext.Entry(gauge).State = EntityState.Detached;
```




