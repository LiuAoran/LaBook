#技术/Git  #技术/工具包 

## 背景

执行命令 git pull --tags（获取所有的标签），这个地方可能会报错，问题就在这里，would clobber existing tag 的意思是 会破坏现有的标签。

## 解决

执行命令 git pull --tags -f，这个命令可以覆盖本地存在的标签冲突。
```Git
git pull --tags -f
```