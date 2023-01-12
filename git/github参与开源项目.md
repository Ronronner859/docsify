# github参与开源项目📕

>目的：跨团队开发
>
>团队的每个成员可以在服务器上或者自己开创一个github仓库，团队成员通过fork该仓库，进行下面的操作就可以跨团队开发。

## fork

### fork一下开源项目的仓库

![](https://img-blog.csdnimg.cn/7e58047f068c4ea6a4a63eba74898ac7.png#pic_center)

### 进入自己的github账户

![](https://img-blog.csdnimg.cn/6a104a877d434291b5da75f0616c2194.png#pic_center)

### 复制自己仓库的链接

![](https://img-blog.csdnimg.cn/844737962e8f49d097489643ed0cb3ed.png#pic_center)

点击复制

![](https://img-blog.csdnimg.cn/160cadd6b4b54216b6e8f4e70bd55515.png#pic_center)

> 复制地址后：
>
> - ​	可以直接打开终端输入git clone 地址 直接克隆到本地
> - ​	另一种就是下面的方法（通过工具打开）

### 这里用IDEA（idea首先登录github账户）

![](https://img-blog.csdnimg.cn/263040b1fe7e4185940e99ea0faa778d.jpeg#pic_center)

### 版本控制输入URL

![](https://img-blog.csdnimg.cn/ce2b7472bb81453489e252d12af46c51.jpeg#pic_center)

![](https://img-blog.csdnimg.cn/56a6de2873044754ade4b0c756e725e5.jpeg#pic_center)

### 打开终端,查看仓库链接

![](https://img-blog.csdnimg.cn/ee4f64520ef64b91b651632c13901686.jpeg#pic_center)

这里只能看到自己仓库的链接地址

### 添加上游仓库的地址

![](https://img-blog.csdnimg.cn/304606a8c3244625b2021038e77e4c17.png#pic_center)

![](https://img-blog.csdnimg.cn/e478f6babc2e45bea32e91acbaf64f96.jpeg#pic_center)

### 想给这个项目加一些功能

​	1.首先创建一个分支

```
git branch demo2.0
```

​	2.切换分支，在demo2.0这个分支进行创作不会影响master分支

```
git checkout demo2.0
```

​	3.熟练后可以合并一起写

>git checkout -b demo2.0

![](https://img-blog.csdnimg.cn/29033f15249d4e369a74b0038a9cfb7a.jpeg#pic_center)

  可以对文件增加一些功能

![](https://img-blog.csdnimg.cn/cb6fb4f4cd574f4fbfca873bc3b0490d.jpeg#pic_center)

### 功能完成后，在该分支在重复一遍基本提交

![](https://img-blog.csdnimg.cn/7aec3d96412c484683ca8c18bc1c418e.jpeg#pic_center)

![](https://img-blog.csdnimg.cn/343e5c2b4de84f789c643d5b15f83301.jpeg#pic_center)

然后就上传成功了

### 回到自己仓库

![](https://img-blog.csdnimg.cn/403cdc63c8fe445f82844879672c5a27.png#pic_center)

分支下的文件信息

![](https://img-blog.csdnimg.cn/b1be0bd39e474647bce192227c624aa7.jpeg#pic_center)

###  点击自己仓库的Pull request

![](https://img-blog.csdnimg.cn/14250835e0e84b8283ba5fa421638fa5.png#pic_center)

然后新建一个 New pull resquest

### 是否要将自己完成的分支发给用户

![](https://img-blog.csdnimg.cn/a58b9680c0154807a1f74c74625b1001.png#pic_center)

### 请求发送后，填写基本功能信息

![](https://img-blog.csdnimg.cn/c2fbdba1ffb343048c6cbb12beb67f00.png#pic_center)

### 回到用户的仓库，会受到对方发过来的请求

![](https://img-blog.csdnimg.cn/9f711695c8d0454c8b9a59d468c2a109.png#pic_center)

然后看到对方的提交信息

![](https://img-blog.csdnimg.cn/7837ef277dd744f9b78a6348d38ab9f6.png#pic_center)

### merge到自己的仓库，将对方的分支合并到自己的仓库

![](https://img-blog.csdnimg.cn/afb1c8185f4f4548ab63c912cde698a2.png#pic_center)

### 用户的仓库就更新了对方的实现功能

### 中途还会遇到合并冲突等问题..后续更新
