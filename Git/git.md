# git操作

## 本地操作

### 基本操作

基本操作演示一般使用的三步提交法

1. git status  
  `git status` 该指令可以查看自从上次提交（git commit）之后产生了修改的文件
2. git add  
  `git add [具体文件名]` 该指令将会把产生了变化的文件提交修改到临时工作站
  或使用全部添加指令`git add .`
3. git commit -m/-v  
  `git commit -m "[描述语句]"` 该指令将临时工作站的内容（git add的文件）提交到本地git仓库
  或使用`git commit -v`可以查看详细的修改内容，并且添加详细的描述语

### 版本控制

1. git log/reflog  
  `git log`可以查看当前所在工作站的log（若使用时间穿梭回到之前的版本，也只能看到那时的log）  
  `git reflog`可以查看提交到git仓库的所有版本信息
2. git reset --hard [版本号]
  `git reset --hard [版本号]`可以使当前工作站到达指定版本（只add而未commit的内容将被覆盖）

## 分支控制

1. git branch [分支名]
  `git branch [分支名]`可以创建分支，该分支的分支名为[]内的内容。  
  分支使用完成后，使用`git branch -d [分支名]`来删除分支。
2. git checkout [分支名]
  `git checkout [分支名]`可以切换分支，使当前工作站到达目标分支，在目标分支下的操作（修改，提交等操作），都会记录在目标分支下，只对目标分支产生影响。
3. git merge [分支名]
  `git merge [分支名]`该指令用于合并分支，首先`git checkout`到达需要保留的分支，然后使用该指令，选择需要被合并的分支，进行分支合并。

## 远程操作

1. git pull
2. git push -u origin master (第一次)  
   git push (以后)
