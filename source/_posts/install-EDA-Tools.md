---
title: 安装EDA 工具
date: 2022-05-08 08:53:06
tags:	
      -EDA
---

# 安装EDA 工具

## Linux

### Install Questasim10.7

#### 安装

1. 目录设置，以及文件建立，文件在最后在使用lmgrd时会用到

   ```shell
   sudo mkdir /usr/tmp
   sudo touch /usr/tmp/.flexlm
   ```

2. 修改权限，然后安装，里面有一步选择：**全平台安装**，其他就没啥了，都是下一步

   ```shell
   chmod +x ./install.linux64
   ```

3. license文件产生（这里产生的license文件名为mentor.dat，如果修改了名字，下面要相应修改）下面第(1)、(2)步在windows环境下进行操作

   1. 修改license.src文件

      - 第一行：修改成自己主机名和mac地址 //用hostname查主机名，用ifconfig查mac地址
      - 第二行：mgcld <安装目录>/questasim/linux_x86_64

   2. 运行 run_me.bat 文件

   3. 将生成的mentor.dat文件拷贝到linux 的home目录，或者其他你准备放license的目录

      ```shell
      #License path
      /opt/IC_EDA/Questasim10.7/questasim/License/mentor.dat
      ```

   4. 修改文件格式，两种方式

      - 执行dos2unix ./mentor.dat 

      - vim 打开文件

        ```shell
        vim ./mentor.dat
        #查看当前格式
        :set ff
        # 显示为dos
        #更改格式为unix
        :set ff=unix
        ：wq
        # 完成格式转换。
        ```

4. 环境变量设置,在bashrc文件里设置（ ：vim ~/.bashrc）

   ```shell
   vim ~/.bashrc
   # export LM_LICENSE_FILE=/home/soft/mentor.dat  //设置license的环境变量,也可以把文件名修改为license.dat
   
   export LM_LICENSE_FILE=27000@waves-pc.localdomain:/opt/IC_EDA/Questasim10.7/questasim/License/mentor.dat
   
   #questasim   #export PATH=$PATH:<安装目录>/linux_x86_64
   PATH=$PATH:/opt/IC_EDA/Questasim10.7/questasim/linux_x86_64
   
   alias lmg_mentor="/opt/IC_EDA/Questasim10.7/questasim/linux_x86_64/lmgrd -c /opt/IC_EDA/Questasim10.7/questasim/License/mentor.dat"
   
   #设置完后，source ~/.bashrc，再输入env查看设置成功没有
   source ~/.bashrc
   ```

5. crack文件执行

   - 将sfk文件拷贝到<安装目录>/linux_x86_64/mgls/lib下

   - 将libstdc++.so.5文件拷贝到/usr/lib下  //有的可能不需要拷贝也能成功

     ```shell
     #出现如下错误：
     ./sfk: error while loading shared libraries: libstdc++.so.5: cannot open shared object file: No such file or directory
     #查看 sfk 的动态链接库，
     ldd sfk
     # 查看系统中是否存在libstdc++.so.5
     在/usr/lib/libstdc++.so.5 中找到。
     更新动态链接库
     ldconfig
     ldconfig -v
     删除原有动态链接库，重建软链接
     ```

     

   - 修改权限 sudo chmod 755 sfk

     ```shell
     sudo ./sfk rep -yes -pat -bin /5589E557565381ECD00000008B5508/31C0C357565381ECD00000008B5508/ -bin /5589E557565381ECD8000000E8000000005B81C3/33C0C357565381ECD8000000E8000000005B81C3/ -bin /41574989FF415641554154554889CD534489C3/33C0C389FF415641554154554889CD534489C3/ -dir . 
     ```

6. 开启license，可以设置开机自启

   ```shell
   lmg_mentor
   ```

7. 打开软件questasim

   ```shell
   vsim
   ```

   

   

## 多个LM_LICENSE_FILE变量的问题：

   Intel® 和第三方软件使用LM_LICENSE_FILE环境变量指定许可文件的位置或许可服务器。 例如， Intel® Quartus® Prime软件和ModelSim®软件都使用LM_LICENSE_FILE变量以指定其许可的位置。   

​    注： 开发计算机与许可服务器通信所花费的时间直接影响编译时间。如果您的 LM_LICENSE_FILE环境变量设置包括许多许可服务器的路径，又或者如果许可服务器由远程位置托管，则会发现编译时间显著增加。    

Linux或UNIX系统中，会在LM_LICENSE_FILE环境变量附带的每个许可文件或许可服务器位置后面插入一个冒号（:）。 而在Windows系统中，会在LM_LICENSE FILE环境变量附带的每个许可文件或许可服务器后插入一个分号（;）。 

​    注： 修改LM_LICENSE_FILE设置以纳入软件许可地址，请勿删除变量附带的现有许可位置。    

