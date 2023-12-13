# 重启API的BAT

```bash
@echo off
echo "API Restart"
taskkill /f /t /im CY_API.exe
echo "API Stop"
start /d "C:\Users\LYL\Work\Work\hy_api\CY_API\bin\Debug\net6.0" CY_API.exe
echo "API Start"
exit
```

