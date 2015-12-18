## 撒销一个合并

如果你觉得你合并后的状态是一团乱麻，想把当前的修改都放弃，你可以用下面的命令回到合并之前的状态：

    $ git reset --hard HEAD
或者你已经把合并后的代码提交，但还是想把它们撒销：

    $ git reset --hard ORIG_HEAD

## git reset 命令    
    git reset HEAD filename  #从暂存区中移除文件
    git reset –hard HEAD~3  #会将最新的3次提交全部重置，就像没有提交过一样。
    git reset –hard 38679ed709fd0a3767b79b93d0fba5bb8dd235f8(commit_id) #回退到 38679ed709fd0a3767b79b93d0fba5bb8dd235f8 版本

本地reset之后push + -f    

    git push origin master -f    
