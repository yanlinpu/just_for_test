发现历史提交 54321 中包含错误，直接在当前工作区中针对这个错误进行修改，然后执行下面命令。

```
$ git commit --fixup 54321
$ git rebase -i --autosquash 54321^
$ git push origin master -f
```
