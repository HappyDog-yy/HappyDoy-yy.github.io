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
- 记录的图形展示+自定义格式
```
git log --graph --pretty=format:“%h %s”
```
```%h```：显示哈希值，即版本号
```%s```：显示提交说明
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
### 2.5 第五阶段：Git配置文件
- 项目配置文件：项目/.git/config
- 仅对当前项目有效
```bash
git config --local user.name 'HappyDog-yy'
git config --local user.email 'roseyy@xx.com'
```
- 全局配置文件：Git安装路径\etc\gitconfig
- 对当前用户的所有项目有效
```bash
git config --gobal user.name 'HappyDog-yy'
git config --gobal user.email 'roseyy@xx.com'
```
- 系统配置文件
- 对当前电脑系统的所有git用户和所有git项目有效
```bash
git config --system user.name 'HappyDog-yy'
git config --system user.email 'roseyy@xx.com'

!!!执行该命令需要root权限
```
- 查看当前生效的配置
```bash
git config --list
```
- 添加远程仓库
```bash
git remote add origin 仓库地址
```
该命令默认仅在当前项目生效，相当于加上了 ```--local```。
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

- Github创建仓库
```New repository``` --> ```name``` --> ```Description简介``` --> ``` visibility选择public``` --> ```Add README 打开``` --> ```.gitignore忽略管理.git隐藏文件夹``` --> ```Create respository``` 

创建成功之后，点击 ```code``` 可复制远程仓库地址，如 ```https://github.com/HappyDog-yy/Git_test.git```。
- 推送本地项目至远程仓库
```bash
1.给github仓库地址起别名origin
git remote add origin 远程仓库地址
2.将Git默认的分支名 ```master``` 重命名为 ```main```（Github默认为main）
git branch -M main
3.将本地 ```main``` 分支的代码推送到远程仓库 ```origin```
git push -u origin main
```
其中，```-u``` 代表默认参数，代表以后push时，若不传递参数，默认 ```push``` 到 ```origin``` 仓库的 ```main``` 分支。
- 拉取远程仓库代码至本地
```进入目标文件夹``` --> ```git clone 仓库地址``` 
- 家-github-公司协同开发

1.家代码开发上传至github
```bash
git checkout dev
git merge master
touch a1.cpp
git add .
git commit -m 'Home code a1.cpp'
手动提交dev分支代码至远程仓库
git push origin dev
```
2.公司拉取github代码(更新而非克隆)
```bash
git checkout dev
git pull origin dev
```
3.push公司代码
```bash
touch a2.cpp
git add .
git commit -m 'Company code a2.cpp'
git push origin dev
```
···
- 开发完毕，提交合并至master分支
```bash
git add .
git commit -m '开发完毕'
git push origin master
git checkout master
git merge dev
git push origin master
```
**注意！** 处于dev分支开发功能时，首先 **合并** master分支的最新代码，在此基础上开发，且**只执行一次**。
### 3.4 rebase的使用
#### 3.4.1同步上游分支
假设你在 ```origin/dev``` 分支上做开发，主分支 ```master``` 上有了新的提交，此时想同步到dev分支：
```bash
#切换主分支，拉取代码
git checkout master
git pull origin master
#dev分支变基
git checkout dev
git rebase master
```
- 这样做可以让dev分支上的提交，应用到 ```master``` 的最新提交之后。且提交历史为一条直线。
#### 3.4.2合并提交、修改记录
- 对最近三次提交记录进行整理
```bash
git rebase -i HEAD~3
```
执行后会打开一个vim编辑器，里面列出最近3次提交，以 ```pick``` 开头：

<img width="693" height="401" alt="Image" src="https://github.com/user-attachments/assets/e1677040-2dce-422b-8540-f15c07a03432" />

可通过修改pick为图片中 ```command``` 内容传递指令，常用的指令如下：
- ```s``` ：把该提交合并至上一个提交
- ```f``` ：合并该提交，并丢弃该提交的说明
- ```r``` ：修改提交说明
- ```e``` ：修改提交过的代码
- ```pick``` ：保留该提交

 ```Esc``` 退出编辑模式 --> ```:wq``` 退出交互式

- 注意：变基多用于自己的开发记录上，不要在已push的分支或公共分支上 ```rebase```。
- 若在 ```git rebase``` 的过程中遇到冲突，先解决冲突，后中断或继续 ```git rebase``` 的执行：
```bash
git rebase --continue
git rebase --abort
```
```Beyond Compare``` ：可帮助快速解决冲突的软件。
1.安装
2.在Git中配置
D:\App\Git\BeyondCompare 5
```bash
#设置当前git仓库的默认合并工具为 ```bc5```.
git config --local merge.tool bc5
#指定 ```bc5``` 程序的安装路径
git config --local mergetool.path 'D:\App\Git\BeyondCompare 5'
#关闭该工具处理完冲突自动生成的备份文件
git config --local mergetool.keepBackup false
```
```--local``` 表示只对当前项目有效。
3.应用Beyond Compare解决冲突
```bash
git mergetool
```
## 第四章 多人协同开发
### 4.1GIt Flow工作流
协同开发时，每一个开发者创建一个分支，开发完毕且通过测试之后合并分支。工作流如下图所示：

![Image](https://github.com/user-attachments/assets/2bbe5d34-add9-492f-88d6-54db69aab44e)

在Github上创建组织，之后邀请成员加入组织便可协同开发组织中的项目。
- 为版本添加标签 ```tag``` 并推送
```bash
git tag -a v1 -m '第一版'
git push origin --tags
```
- 邀请成员加入组织-->参与项目-->修改 ```Read/Write``` 权限
- leader做代码Review ：使用 ```pull request``` 功能
 ```Settings``` 设置--> ```Branches``` 分支--> ```Add branch ruleset``` 添加分支规则集--> ```Ruleset Name``` -->勾选 ```Require a pull request before merging``` 所有体检必须先在非目标分支上进行，并通过 ```pull request``` 提交，才能被合并。
- employee提交pull request
```new pull request ``` -->选择分支合并-->```dev```-->```dev```
 ```ddz```分支合并到 ```dev``` 分支： ```base```为 ```dev```，```compare```为 ```ddz```
### 4.2 开源仓库贡献代码流程
- fork源代码：将别人的源代码拷贝到自己的远程仓库
- 在自己仓库修改代码
- 给源代码的作者提交修复bug的申请（pull request）
### 4.3 Git免密码登录
- URL实现
```bash
仓库地址：https://github.com/HappyDog-yy/Git_test.git
添加用户信息：https://用户名：密码@github.com/HappyDog-yy/Git_test.git
```
- SSH实现
```bash
1.生成公钥和私钥
   ssh-keygen
   默认放在~/.ssh目录下，is_rsa.pub公钥、id_rsa.pub私钥
2.拷贝公钥内容，设置到github中
3.在git本地配置ssh地址
   git remote add origin git@github.com:HappyDog-yy/Git_test.git
4.使用
   git push origin master
```
- Git自动管理凭证
- Github CLI
使用GitHub官方命令行工具 ```gh``` 直接操作，命令为以下格式：
```bash
gh repo clone HappyDog-yy/Git_test
```
### 4.4 Git忽略文件
不希望Git管理文件夹中的所有文件时，创建 ```.gitignore``` 文件，在该文件中列出不希望被管理的文件名，此时使用 ```git status``` 查看文件时，检测不到已列文件。
- 忽略文件或目录
```bash
#忽略dist目录
dist/

#忽略pack.json文件
pack.json
```
- 通配符*
```bash
#忽略所有log后缀的文件
*.log
#但不忽略today.log-->取反
!today.log

#忽略所有temp开头的文件
temp*
```
- 递归匹配**：可匹配任意深度的目录
```bash
#忽略项目中任意位置的test.log文件
**/test.log

#忽略所有目录下的_pycache_文件夹
**/_pycache_/
```
由于我们的关注有限，可在[该地址](https://github.com/github/gitignore)找到各种语言的、较为全面的 ```.gitignore``` 文件
### 4.5 任务管理
- issues：文档以及任务管理
- wiki：对项目的介绍等，项目文档说明。

- [超全Git八股附上~](https://zhuanlan.zhihu.com/p/1935037099154842636)
