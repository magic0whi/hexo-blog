---
title: rsync
date: 2020-01-31 05:53:27
tags:
---
# Rsync常用命令

## 拷贝文件, 保留所有信息

`rsync -aXv -P $SOURCE_DIR/ $TARGET_DIR/`

## 仅复制, 不保留权限

`rsync -rlt -P $TARGET_DIR/ $SOURCE_DIR/`

## 同步文件夹(--delete参数)

`rsync -aXv -P --delete $SOURCE_DIR/ $TARGET_DIR/`

## 参数详解

```bash
-a, --archive               archive mode; equals -rlptgoD (no -H,-A,-X)
-X, --xattrs                preserve extended attributes
-v, --verbose               increase verbosity

-r, --recursive             recurse into directories
-l, --links                 copy symlinks as symlinks
-p, --perms                 preserve permissions
-t, --times                 preserve modification times
-g, --group                 preserve group
-o, --owner                 preserve owner (super-user only)
-D                          same as --devices --specials

--delete                delete extraneous files from destination dirs
-P                          same as --partial --progress
--partial               keep partially transferred files
--progress              show progress during transfer

-W, --whole-file            copy files whole (without delta-xfer algorithm)
```
