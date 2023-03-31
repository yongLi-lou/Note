# Freesql ExecuteIdentity()偶尔新增不成功

### 代码

```c#
var log_id = (int)_fsql.Insert(plan_log).ExecuteIdentity();
    
```

### 解决方案

```c#
var insertedPlanLog = _fsql.Insert(plan_log).ExecuteInserted().FirstOrDefault();
                int log_id = insertedPlanLog.item_control_plan_log_id;
```

