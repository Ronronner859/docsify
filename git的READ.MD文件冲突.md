# git的READ.MD文件冲突

### 1.本地通过git上传码云步骤

码云上创建了一个仓库，想要将本地的仓库链接到远程仓库

```she
git init 初始化本地仓库
```

```shell
git remote add origin 你的http仓库地址 //添加远程仓库的地址
```

添加完之后就执行

```she
git add .
git commit -m 'first commit'
git push -u origin master
```

那么就会出现这个问题(被拒绝)，所以在remote add后不要着急git add，一定要git pull origin master，出现这个原因是因为你在码云创建的仓库有**ReadMe**文件，而本地没有，造成本地和远程的不同步。

![image-20230107105025465](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230107105025465.png)

**原因：**

我们可以这样理解这个问题就是：别人上传到远程仓库后，你没有及时的同步（拉取）到本地，但是你同时又添加了一些内容（提交），以致于你在提交时，它会检测到你之前从远程仓库拉取的时候的仓库状态和现在的不一样。于是，它为了安全起见拒绝了你的提交（然后就报了这个错误）。

“不能快速前进”的原因是因为路不一样了，变得不好走了；体现在git里面就是提交历史出现分叉，主线不再是一条直线，而是在前端出现了分叉，git不知道该如何前进，所以报错了，让你来觉得走哪条路！说的简单点，就是远程仓库和本地仓库不同步了

### 2.解决方案

- 本地如果没有read.me文件就本地生成一个文件再push

  ```she
  git pull --rebase origin master     本地生成ReadMe文件
  git push -u origin master
  ```

  

- 强制上传覆盖文件

  ```
  git push -f origin master
  
  (这个命令在团队开发的时候最好不要用,否则可能会有生命危险)
  ```

  

