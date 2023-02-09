> [Official Docs](https://git-scm.com/docs/git-update-index)


## Assume files has unchanged
---
>When this flag is specified, the object names recorded for the paths are not updated. Instead, this option sets/unsets the "assume unchanged" bit for the paths. When the "assume unchanged" bit is on, the user promises not to change the file and allows Git to assume that the working tree file matches what is recorded in the index. If you want to change the working tree file, you need to unset the bit to tell Git. This is sometimes helpful when working with a big project on a file system that has very slow lstat(2) system call (e.g. cifs).

```bash
git update-index --assume-unchanged "dir/file_name.txt"
```

Undo:
```bash
git update-index --no-assume-unchanged "dir/file_name.txt"
```


## Skip file in work tree
---
>When one of these flags is specified, the object name recorded for the paths are not updated. Instead, these options set and unset the `skip-worktree` bit for the paths.

```bash
git update-index --skip-worktree "dir/file_name.txt"
```

Undo:
```bash
git update-index --no-skip-worktree "dir/file_name.txt"
```


## List files
---
> [Official Docs](https://git-scm.com/docs/git-ls-files)

```bash
git ls-files
```



## List files w/ files marked as `assume unchanged`
---
```bash
git ls-files -v
```


| Label | Description      |
|:-----:|:---------------- |
|   H   | cached           |
|   S   | skip-worktree    |
|   M   | unmerged         |
|   R   | removed/deleted  |
|   C   | modified/changed |
|   K   | to be killed     |
|   ?   | other            |

