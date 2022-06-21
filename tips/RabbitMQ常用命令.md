# RabbitMQ常用命令

默认访问页面：http://127.0.0.1:15672

1. 启动RabbitMQ服务

service rabbitmq-server restart

2. 查看RabbitMQ服务状态

rabbitmqctl status

3. 启用web插件

rabbitmq-plugins enable rabbitmq_management

4. 重启RabbitMQ服务

service rabbitmq-server restart

5. 添加页面用户及密码

rabbitmqctl add_user admin 123456

6. 赋予其administrator角色

rabbitmqctl set_user_tags admin administrator

7. 设置权限

rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"

8. 查看所有用户

rabbitmqctl list_users

9. 查看用户权限

rabbitmqctl list_user_permissions admin

10. 删除用户

rabbitmqctl delete_user guest

11. 修改用户密码

rabbitmqctl change_password admin admin
