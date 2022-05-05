---
title: matlab_note_1
date: 2022-04-11 23:51:08
tags:matlab
---



# MATLAB 学习笔记-1

目录

## 参考资料

1. [B站Matlab入门到进阶系列课程](https://www.bilibili.com/video/BV1ND4y1o7h4?spm_id_from=333.999.0.0)



## 1.资源查找

### 1.matlab本身的资源

```matlab
%1、在matlab的命令行输入
demo

%2、
doc
%3、help 命令
help sum % help NAME
```

### 2.网站资源

1. [mathworks](https://ww2.mathworks.cn/)
   - 产品栏
   - 社区
   - 活动-研讨会的视频 有些视频教程
2. github
3. [matlab 中文论坛](https://www.ilovematlab.cn/)

## 2.变量定义



清空变量

clear

清除窗口

clc

### 注释与分节

```matlab
%matlab使用%进行注释
%分节

%% 我是分节符


```



向量：

```matlab
a = [1,2,3]
a = [1 2 3]

%二维的 分号换行
c = [1,2,3;4,5,6]

```



## 3. 矩阵操作

```matlab
%定义空矩阵
a = []
%判断矩阵是不是空矩阵
iswmpty(a)

%创建矩阵
	%%1
    a = [1,2,3;4,5,6]
    %%2
    b= ones(2,3)
    c = zeros(2,3)
%矩阵索引
    a(1,2)	%matlab的索引是从1开始的 第一行第二列
    a(1,:)  %：表示所有的列，表示取第一行所有的列
    a(1,[1,3]) %取第一行的第一三列
    %取最后一列
    a(1,end)
    %取多行多列
    a=[1,2,3,4,5,6;7,8,9,10,11,12]
    %取3-5列
    a(:,3:5) %[3,4,5]

%矩阵的合并
	a=[1,2,3;4,5,6]
    b= [7,8,9]
    %插入行
	a = [a;b]
	c= [11;12;13]
	插入列
	a=[a,c]
	sum(a) %求各个列的和
	sum(a,2)%求各个行的和
	sum(a(:))	%对a求和	
%matlab中矩阵的索引是按照列来的。

%左右翻转
a = 1:8;
fliplr(a)
%上下翻转操作
b=(1:9)';
flipud(b)

%reshape()函数为对矩阵的行和列进行重新排列的函数，需要注意的是，排列过程中不能改变矩阵中的元素个数，本例中元素个数为9，因此只能改变为9×1、1×9两种形式。
A = [1,2,3;4,5,6;7,8,9]
B=reshape(A,9,1) %将矩阵A由3×3改变为9×1即9行1列，

```

## 4. cell 变量

cell 变量使用{}定义，cell变量的成员可以是不同的类型

```matlab
%定义cell 变量
a = {[1,2,3],'abc'}
c = {[1,2,3],a}
c = cell(2)
c{2}=[1,2,3]
c(2)
%%%%%%%% 注意在cell中 {} 与()的区别

c={[1,2,3],'abc',@(x) x + 2}
%细胞数组，元胞数组，
c{2} = []
c(2)=[]
%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%元胞数组索引，小于号索引子cell,花括号索引具体内容

```



## 5.运算符



**不等于**

```matlab
~=	%不等于
```

**点乘**:对应的元素相乘

点除

```matlab
a = [1,2,3]
b = [4,5,6]
a.*b
a./b
a.^b
```

**矩阵乘法**：要求维度一致

```matlab
a=[1,2,3;4,5,6]
b=[1,2;3,4;5,6]
a*b %进行矩阵乘法

```

## 6.控制语句

for 循环

```matlab
for(i = (1:K))
    for(j=(1:P))
        fir_zu_coef(i,j*2-1)=lp_fir_coef(i+(j-1)*K);
    end
end
```



switch

```matlab
sig_mode = 2;
switch sig_mode
    case 0
        signal =signal_1;
    case 1
        signal =signal_3 +  signal_1;
    case 2
        signal = singal_lfm_1  +  signal_1  ;
    case 3
        signal = singal_lfm_1 + signal_3 +  signal_1  ;
    otherwise 
        signal =signal_1;
end
```





## 7.函数

```matlab
%函数定义
function y = f(x1,x2)
    y=x1+x2;
end

%函数调用
y = f(1,2)
```

子函数

```matlab
%函数定义
function y = f(x1,x2)
	a = 1;	%全局变量 global
    y=f1(x1)+x2;
    function y2 = f1(x)
    	y2 = x + a;
    end
end

%函数调用
y = f(1,2)
```

## 8.绘图

设置figure 窗口的属性

```matlab
%figure('Name','Measured Data','NumberTitle','off');
figure('numbertitle','off','name','原型信道化输出 IQ与包络信号')
```



```matlab
a = 0:0.01:10;
y = sin(a);
plot(a,y)
plot(a,y,'LineWidth',2) %这时把之前画的图清除掉了。
%可以使用hold on 保持之前的画图/hold off 关闭保持
plot(a,y,'r-.o')
%坐标轴
xlabel('x');
ylabel('y')

```



## 9.匿名函数

构造定义

```matlab

f= @(x)2*x

f(1)

f1 = @(x,y)x+y

f1(1,1)

f1([1,2],[3,4])

%嵌套
f = @(x,a) x^2+a^2
f1 = @(x) f(x,1)
f1(4)


```

## 10.聚类





## 11.符号变量



```matlab

syms x y z
f = (x + y )^2
%展开
expand(f)
%化简
simplify(ans)
%符号替换
subs(f,x,1)
subs(f,x,z)

```



## 12.梯度下降



## 13.集合

```matlab
%AUB union
%A∩B intersect
%AUB - A∩B setxor
%A-AB setdiff

```



## 14.调用python



```matlab
c = py.list([1,2,3])	%这个指令不对
c.append(4)
c
a = py.numpy.array([1,2,3,4])%这个指令不对
```



## 15.结构体



