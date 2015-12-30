## https://ruby-china.org/topics/28561
## https://github.com/Gazler/githug
#### no.18 tags_push
```
# Note: git push --all won't push your tags, only your branches.
git push --all
git push --tags
```
#### no.19 将新修改并入上一次提交  
```
git add forgotten_file.rb
git commit --amend
```
#### no.20 commit_in_future
```
env GIT_AUTHOR_DATE='Thu Dec 31 14:33:05 2015 -0800' git commit -m 'future!'
```
#### no.28 rebase   重定基底；复位基底
[rebase与pull区别](http://gitbook.liuhui998.com/4_2.html)
```
git rebase origin/master
git push
```
#### no.30 blame 整个文件的每一行的详细修改信息:包括SHA串,日期和作者
```
git blame config.rb
```
#### no.32&no.33 checkout_tags
```
git checkout v1.2
git checkout tags/v1.2   #v1.2 in both tags and branches
```
#### no.35 create_branch_with_commit_id
```
git branch test_branch -v 0310b847daea1dfbaf7fc6dad006f4ec543f6efa
```
#### no.41 repack
```
git gc
```
#### no.42 cherry-pick

在本地 master 分支上做了一个commit ( 38361a68138140827b31b72f8bbfd88b3705d77a ) ， 如何把它放到 本地 old_cc 分支上？ 

- $ git checkout old_cc
- $ git cherry-pick 38361a68 
```
git checkout master
git log README.md
git checkout master
git cherry-pick ca32a6dac7b6f97975edbe19a4296c2ee7682f68
```
#### no.43 grep 搜索
```
git grep TODO
```
#### no.44 rename commit

rebase -i, --interactive

Make a list of the commits which are about to be rebased. Let the user
edit that list before rebasing. This mode can also be used to split
commits (see SPLITTING COMMITS below).
```
git rebase -i HEAD~2
修改pick->reword coommit->commit
```
#### no.45 合并commits 
```
git rebase -i 9910ea2e406008cf42a966f89d09a44e21d143bf
修改pick->squash coommit->commit(except number.1)
```
#### no.46 分支合并时仅保存为一次提交
```
git merge --squash long-feature-branch
```
#### no.47 修改commit提交的顺序
```
git rebase -i HEAD~2
# 调换一二行顺序
```
#### no.48 bisect 查找问题的利器
```
git log --reverse -p  prog.rb
git bisect good f608824
git bisect bad
git bisect run make test
```
#### no.49 add 选择内容
```
git add -p feature.rb
# ? e
```
#### no.50 reflog
```
git reflog
```
#### no.51 取消commit
```
git revert HEAD~1 # 8f34f31a425b8b5e23cacb34e291c8f4a21fa71a
```
#### no.52 restore
```
git reflog  # 查看日志
git checkout ad05a1b
```
#### no.54 submodule
```
git submodule add https://github.com/jackmaney/githug-include-me ./githug-include-me
```
