# Git的常用操作

### 提交代码

```
git add .
git commit -m "feat: xxxxxx"
```

### commit后如何撤销

```
git reset --soft HEAD^
```

这样就成功的撤销了commit。   
注意，仅仅是撤回commit操作，代码的修改仍然保留。

#### git reset的参数

`--mixed`
不删除工作空间改动代码，撤销commit，并且撤销`git add .`操作。这个为默认参数，`git reset --mixed HEAD^` 和 `git reset HEAD^` 效果是一样的。

`--soft`
不删除工作空间改动代码，撤销commit，不撤销`git add .`。

`--hard`
删除工作空间改动代码，撤销commit，撤销`git add .`。完成这个操作后，就恢复到了上一次的commit状态。

[参考：git使用情景2：commit之后，想撤销commit](https://blog.csdn.net/w958796636/article/details/53611133)