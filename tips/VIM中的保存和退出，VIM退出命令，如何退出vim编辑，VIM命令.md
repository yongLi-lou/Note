# VIM中的保存和退出，VIM退出命令，如何退出vim编辑，VIM命令

在 Linux 中使用 vim 时，输入 `vim xxx.file` 输入好文件内容之后，怎么保存呢？

按 ESC，左下角就可以进行输入

`:w` 保存但不退出

`:wq` 保存并退出

`:q` 退出

`:q!` 强制退出，不保存

`:e!` 放弃所有修改，从上次保存文件开始再编辑命令历史