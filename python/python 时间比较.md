# python 时间比较

python 中时间比较，

```python
def compare_time(time1, time2):
    d1 = datetime.datetime.strptime(time1, '%Y-%m-%d %H:%M:%S')
    d2 = datetime.datetime.strptime(time2, '%Y-%m-%d %H:%M:%S')
    delta = d1 - d2
    print("days is "+ delta.days)
    if delta.days >= 30:
        return True
    else:
        return False

time1 = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
time2 = '2019-10-07 12:12:12'
compare_time(time1,time2)
```

