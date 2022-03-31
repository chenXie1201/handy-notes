当我们在 dev 分支上进行开发的时候，突然来了个紧急的 Bug ，但是你在 dev 上的编辑还不想进行提交

```bash
$ git status

On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

1、把当前工作现场存起来，等以后恢复现场后继续工作：

```bash
git stash

Saved working directory and index state WIP on dev: f52c633 add merge
```

现在 `git status` 就是干净的了，然后我们就可以创建一个 `Bug` 分支进行 `Bug` 修复然后提交

2、查看存储的工作列表

```bash
$ git stash list
```

3、将存储的工作内容恢复到工作区

1）`git stash apply` 恢复内容，`git stash drop` 删除存储的内容

2）`git stash pop` 恢复且删除

4、多次 stash，如何恢复

先用 `git stash list` 查看，然后恢复指定的 `stash` ，用命令：

```bash
$ git stash apply stash@{0}
```

5、合并 master 刚才修复的Bug，而不是合并整个 mater

```bash
$ git cherry-pick 4c805e2
```

6、强制删除一个没有合并的分支

当我们在一个分支上进行开发后又不想用这个分支的功能需要删除 

```bash
$ git branch -d feature-vulcan

error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

强行删除 

```bash
$ git branch -D feature-vulcan

Deleted branch feature-vulcan (was 287773e).
```