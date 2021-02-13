---
title: "我的第一个博客,记录我创建个人博客的流程和遇到的问题，看起来我也知道如何更新我的博客了"
date: 2019-12-09T16:58:07+08:00
draft: true

---

## 一、用到的安装包和资源包：     

1. [hugo windows-64bit.zip](https://github.com/gohugoio/hugo/releases)
2. [hugo themes 资源地址](https://themes.gohugo.io/)
3. [Git-64-bit.exe](https://gitforwindows.org/)
4. 代码编辑器，想用什么用什么，懒得写

## 二、步骤（所有命令全部在cmd控制台下）

1. 解压和安装上述资源，配置path环境变量（hugo的资源包的物理路径：C:\Users\Administrator\Desktop\hg）命令hugo version查看是否安装成功
2. - 安装git，默认路径git配置账户
   - 在桌面右键选择Git Bash Here输入命令(对应你 的github的账户名称和邮箱)
   - git config --global user.name "你的github账号"
   - git config --global user.email "你的github邮箱"
   - 此时在用户目录下已经生成配置文件,如我的是  C:\Users\Administrator\.gitconfig
3. 打开控制台：administrator命令：hugo new site myblog（在当前路径下创建一个名为myblog的文件夹）
4. 去themes.hugo.io下载一个资源，无所谓什么资源，自己喜欢就好，自己命名文件夹名字，我这里是m10c,放在myblog\themes\文件夹下
5. administrator命令：cd myblog
       myblog命令: hugo server -t m10c --buildDrafts(此处启动成功后可以在倒数第二行，看到自己的本地网址，打开浏览器可查看)ctrl+c可关闭
6. 创建自己的第一个博客文章，myblog命令：hugo new post/blog.md
7. 将个人博客部署到远端服务器：

- 打开https://github.com/new  Repository name填写前面你的账号名字.github.io
- 命令：hugo --theme=m10c --baseUrl="https://wangxuesong0910.github.io" --buildDrafts(我们需要的是静态的 HTML 文件。--theme 选项指定主题，--baseUrl 指定了项目的网站。最终静态文件生成在 public 目录中：）
- myblog命令:cd public
- public命令:git init（更新不需要此操作）
- public命令:git add .
- public命令:git commit -m "我的博客第一次提交"
- public命令:git remote add origin https://github.com/wangxuesong0910/wangxuesong0910.github.io.git
- public命令:git push -u origin master

8. 下面查看自己的博客：https://wangxuesong0910.github.io

——————————————————————————————————————————————————————————————————————

- Tip：删除已有的origin：git remote rm origin    解决问题： fatal: remote origin already exist
- Everything up-to-date，忘记写git commit -m ""
- on branch master    Your branch is up to date with ‘origin/master’  强制推送：git push -u origin -f
- 我的博客主页左侧下方小字体位置所在：C:\Users\Administrator\myblog\themes\m10c\layouts\_default \baseof.html
- 我的博客主页左侧大标题位置：C:\Users\Administrator\myblog\config.toml（注意编码，utf-8,否则中文出现乱码）

* 解决error: failed to push some refs to 'xxxx'

  合并分支：git pull --rebase origin master

```
fatal: 'master' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

————————————————
用 git pull origin master --allow-unrelated-histories 解决
```

