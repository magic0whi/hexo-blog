---
title: git
category: computer
date: 2020-02-08 16:41:45
tags:
toc: true
---

一些git的使用技巧

<!-- more -->

## ssh-agent 自启动

参考 [Archlinux Wiki](https://wiki.archlinux.org/index.php/SSH_keys#SSH_agents)

Windows用户见[Github Docs](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)

## Github SSH 测试

先添加ssh key: [快捷通道](https://github.com/settings/keys)

然后:

```console
$ ssh -T git@github.com
```

### GitHub 的 SSH 密钥指纹

以下是 MD5 格式(十六进制格式):

* 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 (RSA)
* ad:1c:08:a4:40:e3:6f:9c:f5:66:26:5d:4b:33:5d:8c (DSA)

以下是 SHA256 格式(base64 格式):

* SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 (RSA)
* SHA256:br9IjFspm1vxR3iA35FWE+4VTyz1hYVLIE2t1/CeyWQ (DSA)

## GIT 常用命令

1. 友好地输出日志:
   ```console
   $ git log --graph --oneline --decorate --all
   ```
2. 输出最近一次commit及diff格式的改动:
   ```console
   git log --cc -1
   ```
3. 查看commit间的修改
   ```console
   git diff HEAD HEAD~
   git diff HEAD^! --stat
   ```
4. 撤销对某个文件的更改
   ```console
   # 工作区
   $ git checkout -- <file> # unstaged changes
   $ git reset HEAD <file>  # staged changes
   # 缓存区
   $ git restore --staged <file>
   ```
5. 撤销合并
   * 移动HEAD指针(有人在此间隔提交可能会丢失更改) `git reset --hard HEAD~<n>`
   * 新建一个revert提交(适用于多人合作) `git revert <Commit ID>`

## Git SSH 协议代理

```bash ~/.ssh/config
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

# 或者 socat (http代理)
ProxyCommand=socat - PROXY:127.0.0.1:%h:%p,proxyport=1080
```

## 更好的Submodule

使用[git-subrepo](https://github.com/ingydotnet/git-subrepo)

然后使用`git subrepo`为开头

还可将子目录变为子仓库:

```console
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
```console
$ git format-patch <r1>..<r2>
```

2. 单个commit
```console
$ git format-patch -1 <r1>
```

3. 从某commit以来的修改(不包含该commit)
```console
$ git format-patch <r1>
```

4. 应用.patch文件
```console
$ git am 0001-xxxx.patch
```

## 限制 git-gc 的内存占用

```console
$ git config --global pack.windowMemory "100m"
$ git config --global pack.packSizeLimit "100m"
$ git config --global pack.threads "1"
```