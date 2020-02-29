---
title: git
category: computer
date: 2020-02-08 16:41:45
tags:
---

一些git的使用技巧

<!-- more -->

## Git SSH协议
 
1. 第一步创建SSH key

   rsa密钥
   
       $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   
   或者ed25519密钥
   
       $ ssh-keygen -t ed25519 -C "your_email@example.com"
   
   若设置了passphrase, 下面的命令可在需要时更改passphrase密码
   
       $ ssh-keygen -p

2. 启用SSH key

   首先以后台模式启动ssh-agent

   ```bash
   $ eval "$(ssh-agent -s)"
   > Agent pid 59566
   ```

   添加你SSH私钥到ssh-agent

   ```bash
   $ ssh-add ~/.ssh/id_rsa
   ```

### 设置自启动ssh-agent

参考 [Archlinux Wiki](https://wiki.archlinux.org/index.php/SSH_keys#SSH_agents)


```bash ~/.bashrc
if ! pgrep -u "$USER" ssh-agent > /dev/null; then
    ssh-agent > "$XDG_RUNTIME_DIR/ssh-agent.env"
fi
if [[ ! "$SSH_AUTH_SOCK" ]]; then
    eval "$(<"$XDG_RUNTIME_DIR/ssh-agent.env")"
fi
```

Windows用户见[Github Docs](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)

### Github测试

先添加ssh key: [快捷通道](https://github.com/settings/keys)

然后:

```bash
$ ssh -T git@github.com
```

### GitHub 的 SSH 密钥指纹

公钥指纹可用于验证与远程服务器的连接。

以下是 GitHub 的公钥指纹（十六进制格式）：

* 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 (RSA)
* ad:1c:08:a4:40:e3:6f:9c:f5:66:26:5d:4b:33:5d:8c (DSA)

以下是 OpenSSH 6.8 和更新版本中显示的 SHA256 哈希（base64 格式）：

* SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 (RSA)
* SHA256:br9IjFspm1vxR3iA35FWE+4VTyz1hYVLIE2t1/CeyWQ (DSA)

## 友好地输出日志

git log --graph --oneline --decorate --all

输出最近一次commit及diff格式的改动

git log --cc -1

查看commit间的修改

git diff HEAD HEAD~

## 撤销合并（2种方法）

1. 移动HEAD指针(有人在此间隔提交可能会丢失更改) `git reset --hard HEAD~`

2. 新建一个revert提交(适用于多人合作) git revert -m 1 HEAD，但如果你尝试再次合并 topic 到 master Git 会感到困惑：

```
$ git merge topic
Already up-to-date.
```

master分支并没有还未从topic合并的commit。更糟的是，如果你在 topic 中增加新的commit然后再次合并，Git **只会引入被还原的合并之后的改动** 而不会恢复被撤销的commit改动。

解决此问题的最好方式是再次**撤消之前的那个revert commit**

## git ssh协议下使用代理

```bash ~/.ssh/config
...
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
...
```

## 更好的Submodule

使用[git-subrepo](https://github.com/ingydotnet/git-subrepo)

然后使用`git subrepo`为开头

还可将子目录变为子仓库:

```bash
$ git subrepo init <subdir> [-r <remote>] [-b <branch>] [--method <merge|rebase>]
```

## Git LFS

https://zzz.buzz/zh/2016/04/19/the-guide-to-git-lfs/

## Git Cherry-pick

git cherry-pick用于把另一个本地分支的commit修改应用到当前分支

假设`dev`分支有一个hash为`38361a68`的commit
```
$ git checkout master
$ git cherry-pick 38361a68
```

## Git Format-patch

1. 两个commit间的修改(包含两个commit)
```bash
git format-patch <r1>..<r2>
```

2. 单个commit
```bash
git format-patch -1 <r1>
```

3. 从某commit以来的修改(不包含该commit)
```bash
$ git format-patch <r1>
```

4. 应用.patch文件
```bash
$ git am 0001-xxxx.patch
```