# linux 中安装 Quartus 18.1

## ubuntu 16.04 install quartus 18.1
1、在intel 官网下载quarts liuux 版本
2、解压文件
3、进入解压的文件夹
```shell
zsz@zsz-pc:/opt/download/quartus$ ls
arriav-18.1.0.625.qdz    max-18.1.0.625.qdz
arriavgz-18.1.0.625.qdz  ModelSimSetup-18.1.0.625-linux.run
cyclone-18.1.0.625.qdz   QuartusSetup-18.1.0.625-linux.run
cyclonev-18.1.0.625.qdz  stratixiv-18.1.0.625.qdz
max10-18.1.0.625.qdz     stratixv-18.1.0.625.qdz

# 给ModelSimSetup-18.1.0.625-linux.run QuartusSetup-18.1.0.625-linux.run 可执行权限
chmod +x ModelSimSetup-18.1.0.625-linux.run
chmod +x QuartusSetup-18.1.0.625-linux.run

#直接 运行安装
./QuartusSetup-18.1.0.625-linux.run
#在gui中选择安装路径
#这里我选择 /opt/altera/18.1

```

### 启动出错
```shell
zsz@zsz-pc:~$ 
(quartus:669): Gtk-WARNING **: Error loading theme icon 'window-close' for stock: Unable to load image-loading module: /usr/lib/x86_64-linux-gnu/gdk-pixbuf-2.0/2.10.0/loaders/libpixbufloader-svg.so: /opt/altera/18.1/quartus/linux64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /usr/lib/x86_64-linux-gnu/libicuuc.so.55)

# 解决办法
cd <install_dir>/15.1/quartus/linux64
sudo mv libstdc++.so.6 libstdc++.so.6.quartus_distrib
sudo ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 libstdc++.so.6
```

参考连接：https://my.oschina.net/u/4681268/blog/4651673

