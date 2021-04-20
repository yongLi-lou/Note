# smptplib库发送邮箱

```python
import smtplib
from email.mime.text import MIMEText
from email.header import Header

# 用户密码等变量
sender = 'mgjterran@163.com'
reciver = '812261944@qq.com'
subject = 'test for python'
smtpserver = 'smtp.163.com'
username = 'mgjterran@163.com'
password = 'QIUYJOZFOYKENJOL'

# 构造邮件内容
msg = MIMEText('hello python') # 默认text格式
msg['Subject'] = Header(subject, 'utf-8')
msg['From'] = sender
msg['To'] = reciver

# 登录并发送邮件
try:
 smtp = smtplib.SMTP()
 smtp.connect(smtpserver)
 smtp.login(username, password)
 smtp.sendmail(sender, reciver, msg.as_string())
 print('发送成功')
except:
	print('发送失败')
finally:
 smtp.quit()
```

## password是smpt授权密码