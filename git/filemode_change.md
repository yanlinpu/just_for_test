git diff 
```
diff --git a/Android.mk b/Android.mk
old mode 100644
new mode 100755
```
切到源码的根目录下，执行

    git config core.fileMode false
  
这样你的所有的git库都会忽略filemode变更了～
