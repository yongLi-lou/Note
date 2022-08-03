# mysql修改表enum_在整个表中更改MySQL ENUM值的值
如果要更改枚举的值：

假设你的旧枚举是：

ENUM('English', 'Spanish', 'Frenchdghgshd', 'Chinese', 'German', 'Japanese')

改变使用方法：

-- Add a new enum value

ALTER TABLE `tablename` CHANGE `fieldname` `fieldname` ENUM

('English', 'Spanish', 'Frenchdghgshd', 'Chinese', 'German', 'Japanese', 'French');

-- Update the table to change all the values around.

UPDATE tablename SET fieldname = 'French' WHERE fieldname = 'Frenchdghgshd';

-- Remove the wrong enum from the definition

ALTER TABLE `tablename` CHANGE `fieldname` `fieldname` ENUM

('English', 'Spanish', 'Chinese', 'German', 'Japanese', 'French');

MySQL可能会通过表中的所有行尝试更新内容,我听说过有关计划优化的故事,但我不知道是否真的发生了这样的事情.
