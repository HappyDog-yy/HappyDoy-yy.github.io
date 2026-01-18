# Git实战
## 第一章 快速入门
### 1.1什么是Git

> Git是一款分布式版本控制软件。

### 1.2为什么要做版本控制

> 版本控制的必要性：可快速回滚到之前的某个状态

### 1.3安装Git
Linux系统
```bash
sudo apt-get install git
```
Windows系统
```
下载地址：https://git-scm.com/
```
下载成功之后，在任意文件夹中右键单击，可看到选项 `Open Git GUI here` 和 `Open Git Bash here`。
## 第二章 东北热创业史
### 2.1 第一阶段：单枪匹马开始干
- Git管理文件夹的步骤
```
1.进入要管理的文件夹
2.初始化（相当于给Git一个权限，文件夹下会生成.Git隐藏文件夹）
3.管理
4.生成版本
```
- 初始化
```bash
git init
```
- 管理
```bash
git add .(管理当前目录下的所有文件) 
git add 文件名(管理某文件)
```
- 生成版本
```
git commit -m '该版本的描述信息'
```
- 查看版本记录
```
git log
```
可看到每个提交记录的版本号，便于后续回滚操作

<img width="589" height="349" alt="Image" src="https://github.com/user-attachments/assets/ac64d86c-6422-4322-9141-87d9232b2962" />

- 查看管理状态
```
git status
```
```
红色-代表未管理的文件，包括新增文件、修改过内容的文件
绿色-代表已管理但还未提交至版本库
不显示-代表已管理并提交至版本库
```
- Git三大区域

> 工作区：实际编辑、修改文件的地方
> 暂存区：临时存放即将提交的修改
> 版本库：存储所有提交记录的数据库

<img width="2008" height="1528" alt="Image" src="https://github.com/user-attachments/assets/c43bc29d-5da4-4895-87fb-5b6ef2715ed4" />

### 2.2 第二阶段：拓展新功能
修改文件之后，文件状态变为未管理的红色，可重新 `add` 并 `commit` 至版本库。
### 2.3 第三阶段：回滚
- 回滚至之前版本
```bash
git reset --hard 版本号
```
版本号通过 `git log` 命令获取
- 回滚至之后版本

> 回滚至v2之后， `git log` 命令查看不到v3的版本号，此时再使用 `git reflog` 便可查看v3版本号，如下所示：

<img width="724" height="363" alt="Image" src="https://github.com/user-attachments/assets/25724509-7277-4d76-bbe4-73cedaf7270d" />

### 2.4 第四阶段：三大区域之间的转换
- 撤销对于某文件的修改
```bash
git checkout -- 文件名
```
该命令可用于撤销未 ```add``` 文件的修改，用版本库里的文件直接覆盖（直接丢弃工作区的修改），无法撤销。
- 撤销```add```暂存命令
```bash
git reset HEAD -- 文件名
```
仅撤销暂存命令，对于工作区的修改仍保留，文件状态由绿色变成红色。
## 第三章 分支
### 3.1初识分支

不同版本之间通过指针建立联系，v2在v1的基础上修改，增量保存，只保存相对于v1新增和修改的部分。这样不仅更节省空间，生成版本的速度也更快。
- 如何解决线上代码出现bug的问题？

> 创建一个新的分支，对代码进行紧急修复，修复完成之后合并到主分支（默认为master分支）。
eg.创建的每一个分支都可以起一个名字，在v3的基础上创建bug分支用于修复bug；创建dev分支用于开发新功能。最后将修复完bug的分支合并到master分支上。不同的分支之间可以做环境的隔离。

### 3.2分支命令
- 创建新的分支
```bash
git branch 新分支名
```
- 查看目前所处分支（*所在行的分支）
```bash
git branch
```
- 切换分支
```bash
git checkout 分支名
#或
git switch 分支名
```
创建新分支bug-->切换到分支bug-->修改代码--> ```commit``` 提交版本v4，可创建bug分支的版本v4。
- 合并分支
将bug分支的v4版本合并到master分支上： ```切换到master分支``` --> ```合并``` 
```bash
git merge 要合并的分支
```

> v4和v5都是基于v3进行修改，当v4和v5都修改了v3的同一行，且最后都要合并到v3时，会产生合并冲突，此时需要打开发生冲突的文件，**手动合并**并提交。

```bash
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

<img width="711" height="346" alt="Image" src="https://github.com/user-attachments/assets/663c91f1-5063-4ad6-9973-1790eb04a2d5" />

```bash
git add .
git commit -m 'v6，解决冲突，bug修复且dev分支新功能上线'
```
- 删除分支
```bash
git branch -d 分支名
```
### 3.3工作流
- Git开发规范
```master``` 分支存放相对稳定的版本，创建 ```dev``` 分支用于开发，版本的更替都在dev分支上，出现相对稳定的版本再合并到 ```master``` 分支。
- 代码托管仓库
创建仓库-->推送本地代码至仓库(所有的版本和分支)

> github：公共的仓库，private付费
gitlab：自己买服务器，自建仓库

上传和拉取代码