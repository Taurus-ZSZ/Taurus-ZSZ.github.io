1. clone dev 分支

   ```shell
   #克隆github上指定的分支
   git clone -b dev ******
   ```

2. 安装hexo 插件
   npm install hexo-deployer-git --save
   如果图片不显示 安装图片加载插件
   npm install https://github.com/CodeFalling/hexo-asset-image --save

3. 编写blog之后,使用一下命令同步：

```shell
git add *
git commit -m "提交****"
git push origin dev 
```



## 更换电脑后如何重新部署hexo

1、安装nodejs

2、 npm install hexo

3、创建文件夹然后运行

```shell
hexo init

```

4、拉取远程dev分支

```shell
git clone -b dev git@github.com:Taurus-ZSZ/Taurus-ZSZ.github.io.git /e/200-Learning/240_myblog/123
```

 5、复制123文件下的所有到Taurus-ZSZ.github.io文件夹下，覆盖掉hexo init 生成的文件

就可以了



## 如何拉取最新的文件

```shell
#1.首先确保你当前处于dev分支，然后运行下面的命令
git pull origin dev 
```

