
## 如何在fork一个开源项目后，持续同步最新的代码
在github的使用过程中，fork一个开源项目是很常见的操作。但是，如何让自己fork出来的项目与源项目保持同步呢？  

可以利用git可以为本地仓库设置多个remote的特性，利用一个本地仓库作为中转，将fork之前的源项目的最新的commit，同步到自己的项目中。

以我自己fork的IoT项目[thingsboard](https://github.com/thingsboard/thingsboard.git)为例，操作如下：

- 在github中fork thingsboard项目，这样，我们在自己的github账户下就有一个thingsboard的fork版本了。

- 将自己的fork的thingsboard项目克隆到本地：
```
git clone https://github.com/leolovesmile/thingsboard.git
```

- 这个时候，运行`git remote -v`就可以看到本地的remote仅有下列两个列表项：
```
origin  https://github.com/leolovesmile/thingsboard.git (fetch)
origin  https://github.com/leolovesmile/thingsboard.git (push)
```

- 我们按照之前的设计，将源thingsboard项目添加到本地clone的remote中去`git remote add owner https://github.com/thingsboard/thingsboard.git`，这里的`owner`是我为这个新的remote起的别名，你可以任意命名。添加完后，再运行`git remote -v`就可以看到本地的remote列表发生了变化：
```
origin  https://github.com/leolovesmile/thingsboard.git (fetch)
origin  https://github.com/leolovesmile/thingsboard.git (push)
owner   https://github.com/thingsboard/thingsboard.git (fetch)
owner   https://github.com/thingsboard/thingsboard.git (push)
```

- 接下来要做的，就是在源项目的某分支（例如 *master* 分支）有你需要同步的内容时，先在本地运行 `get fetch owner`

- 然后将远程的owner库的master分支，合并到本地的master分支上(当然，合并时如果与本地自己的提交产生冲突，需要解决冲突)：
```
git checkout master
git merge owner/master
```

- 最后，将合并后的结果push到自己的远程分支，就完成了：
```
git commit -am "merge the lastest changes of the original project"
git push origin
```