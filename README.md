# Git

## Git目录

- [git作用](#git作用)
- [配置](#git基本配置)
- [git基本操作](#git基本操作)
- [git分支](#git分支)
- [git远程仓库](#git远程仓库)
- [git扩展](#git扩展)

## git作用

- 版本控制：可以多个版本的内容同时存在，可以随时切换到某个版本

## git基本配置

```bash
git config --global user.name 自定义英文名
git config --global user.email 邮箱
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input

#查看刚才配置
git config --global --list
```

#### 注意事项

- 配置完后，执行code , 如果无法运行 VS code
  - 执行`git config --global --list`查看配置是否有误
  - VS Code 安装目录需要添加到环境变量中

## git基本操作

1. git初始化：`git init`
   - 作用：创建 .git 文件夹，用来保存版本
   - 注意事项：必须在文件所在目录执行该代码
2. 文件提交到暂存区：`git add 路径`
   - 注意事项：
     - 路径可以是`绝对路径` 、`相对路径` 、`.`(当前目录全部文件) 、 `*`
     - 暂存区只是临时存放，使用时必须配合
3. 查看未提交的，和暂存区的内容：`git status`
4. 确认提交文件：
   - 方式一：`git commit -m "描述"`
     - 注意事项：描述必须加英文状态的引号
   - 方式二：`git commit -v`
     - 会自动打开VS Code，描述内容直接书写在首行
     - 可跟上一个版本对比，快速查看修改的内容及位置
5. 查看提交历史：`git log`
   - commit ：版本号
6. 恢复到之前的版本：`git reset --hard 版本号前6位`
7. 查看所有版本(包括恢复的历史)：`git reflog`

## git分支

- 作用：可以同时存在多个版本，用到哪个版本就切换到对于的分支

- 创建分支：`git branch 分支名称`

- 切换分支：`git checkout 分支名称`

- 查看当前所在的分支：`git branch`

- 合并分支：`git merge 需要合并的分支名`

  - 注意事项：保留哪个分支，就到哪个分支执行代码

- 注意事项：

  - 提交文件时，当前是在的哪个分支，就提交哪个分支

  - 文件冲突时，

    1. 查看冲突文件，删除不需要的代码

    ```bash
    #查看冲突文件方式一：
    git status
    
    #查看冲突文件方式二：
    git status -sb
    ```

    2. ，重新提交

    ```bash
    #添加文件到暂存区
    git add 文件名
    
    #提交文件，此时不需要填写任何内容
    git commit    
    ```

### git远程仓库

- 作用：把本地代码上传到云端，

- [github帮助文档](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- HTTPS协议和SSH的区别

  - HTTPS：每次操作必须输入密码
  - SSH：电脑上的私钥（加密） 和GitHub上公钥（解密） 配对使用

- 创建公钥和私钥

  ```bash
  #方式一：
  ssh-keygen -t ed25519 -C 邮箱地址
  
  #方式二：
  ssh-keygen -t rsa -b 4096 -C 邮箱地址
  
  #.pub为公钥
  ```

- 绑定GitHub

  1. 查看公钥SSH：`cat 用户目录/.ssh/id_ed25519.pub `
  2. GitHub 中设置公钥：在设置中选择 SSH and GPG keys  => New SSH Key  => 名字自定义，ssh填写上方显示的公钥

  ```bash
  # GitHub 中设置公钥
  #在设置中选择 SSH and GPG keys  => New SSH Key  => 名字自定义，ssh填写上方显示的公钥
  ```

  3. 查看是否连接成功：`ssh -T git@github.com`
     - 注意事项：必须在 .ssh 目录下执行代码

- 上传至GitHub仓库

  ```bash
  #1. 创建仓库，得到ssh代码
  New repository    =>必要参数：仓库名，其他可默认
  
  #2. 上传代码  :
  	#git remote add origin 项目SSH ,添加仓库地址，并且设置仓库名为origin
  	#git push -u origin 分支名称x，把当前分支上传到远程仓库的分支x
  		#此代码只需执行一次，以后直接使用 git push
  git remote add origin 项目SSH
  git push -u origin master     
  
  ```

- GitHub下载到本地：`git clone HTTPS地址/SSH地址`

  ```bash
  #下载的三种方式
  	#1.在当前目录创建目录（目录名与仓库名相同）：
  	git clone HTTPS地址/SSH地址
  	
  	#2.克隆在自定义目录下
  	git clone HTTPS地址/SSH地址 目录名
  	
  	#3.在当前目录下下载代码
  	git clone HTTPS地址/SSH地址 .
  ```

- GitHub 更新到本地仓库：`git pull`

  

  ## 

## git扩展

1. 忽略某些文件不提交
   - 在当前文件夹创建 `.gitignore`文件
   - 在`.gitignore`文件中直接写入不想提交文件的文件名（要加后缀）
   
2. 设置 git 常见命令

   ```bash
   #1. 创建.bashrc文件
   touch ~/.bashrc
   
   #2. 打开文件并写入快捷键
   code ~/.bashrc
   
   #设置快捷键
   alias ga="git add"'
   alias gc="git commit -v"'
   alias gl="git pull"
   alias gp="git push"
   alias gco="git checkout"
   alias gst="git status -sb"
   
   #设置log的简化样式
   alias glog="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit -- | less"
   ```

   





