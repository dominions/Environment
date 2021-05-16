#### git重置代码
```bash
git reset --hard <commit-id>    #commit-it为可选
```

#### git解决本地修改后git pull冲突
如不想保留本地代码，可以用上面的重置命令，然后重新拉去。
否在可以按照如下命令
```bash
git stash # 把当前本地修改压入栈中
git pull #拉取更新
git stash pop #把本地修改恢复
```
