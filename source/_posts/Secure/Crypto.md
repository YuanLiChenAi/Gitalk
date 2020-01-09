---
title: 密码学基础与应用
src: //dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/title.jpg
permalink: Crypto
date: 2019-05-08 15:03:34
updated: 2019-05-08 15:03:34
categories:
    - Crypto
tags:
    - AES
    - DES
    - SM4
    - RC4
    - SHA
    - RSA
---
{% blockquote %}
梳理下最近学的密码学基础知识
{% endblockquote %}
<!--more-->
# 密码学概论
## 基本概念
- 密码学是一门古老的学科，研究密码学编制的科学称为**密码编制学**(Cryptography),研究破译密码学的科学称为**密码分析学**(Cryptoanalysis),密码编制学和密码分析学共同组成**密码学**。
- 密码学的三个安全目标
    - 保密性      
    - 完整性
    - 可用性

>- 保密性是确保信息仅被合法用户访问，而不被泄露给非授权用户、实体或过程，或供其利用的特性。
- 完整性是指所有资源只能由授权方或以授权的方式进行修改，即信息未经授权不能进行改变的特性。
- 可用性是指所有资源在适当的时候可以由授权方访问，即信息可被授权实体访问并按需求使用的特性。

## 密码体制
- 明文空间M
- 密文空间C
- 密钥空间K
- 加密算法E
- 解密算法D

![密码体制](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-11-11-03-49.png)
<br>
## 密码分析学
**攻击方法**
- 穷举法
- 数学分析攻击
- 基于物理的攻击

**可利用的数学资源**
- 仅知密文攻击(最不利的情况)
- 已知明文攻击(某些明文-密文对)
- 选择明文攻击(计算机系统和数据库系统特别容易受到这种攻击)
- 选择密文攻击(主要用于攻击公开密钥密码体制，特别是攻击其数字签名)

<br>
## 古典密码
- 置换密码
将明文中的字母重新排列，字母本身不变，但其位置改变了，这样编成的密码称为**置换密码**。
- 代替密码
用密文字母表中的字母或字母组来代替明文字母或字母组，各字母或字母组的相对位置不变，但其本身改变了，这样编成的密码称为**代替密码**。
    1. 加法密码：凯撒密码(Caesar)，右移3位
    ![加法密码](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-11-11-23-35.png)
    2. 乘法密码
    ![乘法密码](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-11-11-24-00.png)
    3. 仿射密码：乘法密码与加法密码相结合
    ![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-11-11-28-05.png)
    简单代替密码
    多表代替密码：维吉尼亚密码(Vigenre)，密行、明列。
    4. 代数密码
    相同为0，不同为1
    ![代数密码](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-09-40-56.png)

## 古典密码破解方法
**穷举分析**
- 加法密码：对k的可能取值逐一穷举
- 乘法密码：密钥k要满足(n,k)=1
- 放射密码：密钥k有$26*12-1=311$种

**统计分析**
- 统计密文的各种统计特性
- 分析双字母、三字母密文组，以区分元音和辅音字母
- 最后分析字母较多的密文

<br>
<br>
# 分组密码
## 概念
- 设M为明文，分组密码将M划分为一系列的明文块M<sub>i</sub>,通常每块包含若干个位或字符，并且对每一块M<sub>i</sub>都用同一个密文K<sub>e</sub>进行加密。常用于商用领域。
![分组密码](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-10-06-02.png)

## 分组密码算法结构
### Feistel结构
- Feistel结构把任何函数(一般称为f函数，又称轮函数)转化为一个置换
![Feistel结构](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/Feistel.png)

### SP结构
- SP结构是Feistel结构的一种推广，其结构清晰
![SP结构](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-10-35-09.png)
>替换S：混淆的作用，即密文和密钥尽可能的复杂
>线性变换P：扩散的作用，即明文当中的每一位能够影响密文当中的每一位。

<br>
<br>

## DES算法
__DES算法流程__
- 初始置换IP
- 生成16个48位的子密钥
- 16轮Feistel结构迭代
    - 扩展置换E
    - S盒代换
    - 置换P
- 逆初始置换IP<sup>-1</sup>

![DES算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-10-41-40.png)

### 初始置换IP
![初始置换IP](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-11-17.png)

### 生成16个48位的子密钥
![生成16个48位的子密钥](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-16-11.png)
![置换选择1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-24-44.png)
![循环左移](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-49-03.png)
![置换选择2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-51-43.png)
<br>
### 16轮Feistel结构迭代
![16轮Feistel结构迭代](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-11-58-51.png)
![扩展置换E](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-14-12-16.png)
![S盒代换](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-14-18-18.png)
![置换P](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-14-21-27.png)
<br>
### 逆初始置换IP<sup>-1</sup>
![逆初始置换IP<sup>-1</sup>](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-14-29-25.png)
![IP<sup>-1</sup>](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-14-48-49.png)
<br>
### DES算法的安全性
- 密钥较短
面对计算能力高速发展的形势，DES采用56位密钥，密钥量仅为2<sup>56</sup>，约为10<sup>17</sup>
- 存在弱密钥
    - 弱密钥K
    K<sub>1</sub>=K<sub>2</sub>=......K<sub>16</sub>,弱密钥不受任何循环移位影响。
    全为0或全为1，被分割的两部分分别为全0或全1，共__4__种弱密钥。
    - 半弱密钥K
    有些种子密钥只能生成两个不同的子密钥，这样的种子密钥称为半弱密钥。DES至少存在__12__个半弱密钥，半弱密钥将导致把明文加密成相同的密文。

### 三重DES
__优点__
- 密钥长度为168位，能够抵挡有效攻击(第一次和第三次加密都是同一个密钥则为112位)
- 相当安全，而且经过实践检验
- 加密算法与DES相同，现有的DES软硬件产品都能方便实现3DES

__缺点__
- 用软件实现该算法的速度比较慢

### 考试知识点
在DES算法中
- 密钥的长度(有效)是56位
- 加密分组的长度是64位
- 子密钥的长度是48位
- 函数F输入时32位

<br>
<br>
## AES算法
- 一个初始轮密钥加
- Nr-1圈的标准轮函数
- 最后一圈的非标准轮函数
- 最后一轮不进行列混淆变换
![AES算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-15-29-15.png)

- RIJNDAEL算法
数据块长度和密钥长度都可变的分组加密算法，其数据块长度和密钥长度都可独立地选定为大于等于128位、且小于等于256位的32位的任意倍数。
- AES(先进加密标准)算法
数据块长度为128位，密钥的长度可分别选择为128位、192位或256位。

>__状态__：在RIJNDAEL算法中，加解密要经过多次数据变换操作，每一次变换操作产生一个中间结果，称这个中间结果叫做**状态**。状态表示为**二维字节数组**(每个元素为一个字节)，它有**四行**，**Nb列**，Nb列等于数据块长度除以32。

![数据块长度为128位的状态](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-16-11-56.png)

>密钥也可以表示为**二维字节数组**(每个元素为一个字节)，它有四行，Nk列，Nk列等于密钥块长度除以32。

![密钥长度为128位的密钥二维字节数组](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-16-13-54.png)

>迭代圈数Nr由Nb和Nk共同决定。

![算法迭代圈数Nr](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-16-33-03.png)

### 初始轮密钥加
- 将明文矩阵与子密钥矩阵的对应字节进行**逐比特异或**。
$AddRoundKey(a_0,k_0)=a_0 \bigoplus k_0$

![逐比特异或](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-16-43-38.png)
<br>
<br>

### 轮函数
- 采用替换/置换网络(SP)结构，组成：
    - 非线性层：S盒变换ByteSub，**混淆作用**。
    - 线性混合层：行位移变换ShiftRow和列混合变换MixColumn，**扩散作用**。
    - 密钥加层：圈密钥加变换AddRoundkey组成，最后一圈的轮函数**没有**列混合变换。

![轮函数](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-09-16-49-55.png)
<br>
#### 字节代换
**字节代换：S盒变换ByteSub**
- 一个关于字节的非线性变换，可以使用**S盒查表**得出输出。
- AES定义了一个S盒，由16*16个字节组成的方形表，包含了8位值所能表示的256种可能的变换。

![字节代换](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-10-16-13.png)
![AES加密算法 S盒表(十六进制)](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-10-40-53.png)
- 高位为行，低位为列。即51对应5行1列的D1

>AES算法与DES算法的S盒比较：
>- AES，由16个S盒组成，运算是可逆的；输入输出位数相同。
>- DES，由8个S盒组成，运算不可逆，每个S盒输入是6bit,输出是4bit。

#### 行移位
**行移位：ShiftRow**
- 将状态矩阵的每行进行循环左移
- 第0行不移；第i行左移i个字节(i=1,2,3)

![行移位1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-11-05-20.png)
![行移位2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-11-07-53.png)

#### 列混合
**列混合：MixColumn**
- 列混合变换将**State乘以一个固定的矩阵A**，对State逐列进行变换，每一列中的每个字节将变换成一个新值，直到4列均变换完毕。
- 相乘后得到的乘积矩阵，其中每个元素均是一行和一列中所对元素的乘积之和。

![列混合](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-11-09-43.png)
>将固定矩阵中的第一行和State的第一列的每个字节分别相乘，再进行异或运算得到第一行第一列的值，即：
$(02\*D1) \bigoplus (03\*93) \bigoplus (01\*CA) \bigoplus (01\*40)=9D$
第二行第一列则通过固定矩阵中的第二行和State的第一列进行计算，即：
$(01\*D1) \bigoplus (02\*93) \bigoplus (03\*CA) \bigoplus (01\*40)=E9$
以此类推...

#### 轮密钥加
**轮密钥加：AddRoundkey(State,RoundKey)**
- 将状态矩阵与子密钥矩阵的对应字节进行**逐比特异或**：
$AddRoundKey(a_{ij},k_{ij})=a_{ij} \bigoplus k_{ij}$

![轮密钥加](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-11-31-48.png)

> **轮函数：**第一轮结束后，回到轮函数迭代10轮(主要针对数据块长度和密钥长度为128位)，最后一轮不需要进行列混合变换，结束迭代后输出密文。 

### 解密算法
> AES算法的解密过程中依次进行逆字节代换，逆行移位，逆列混合和轮密钥加。
- InvByteSub()是ByteSub()的逆变换，通过查逆S盒来实现。
- InvShiftRow()是ShiftRow()的逆变换，对State的各行进行一定量的循环移位(Nb=4)。第0行不移位，第1行循环右移1个字节，第2行循环右移2个字节，第三行右移3个字节。
- InvMixColumn()是MixColumn()的逆变换。
- AddRoundKey的逆就是他本身。

### AES算法的安全性
>- **不存在弱密钥**
该算法对密钥的选择没有任何限制，还没有发现弱密钥和半弱密钥的存在。
- **抗攻击能力强**
可抵抗穷举法密钥的攻击，密钥量为2<sup>128</sup>/2<sup>192</sup>/2<sup>256</sup>,足以抵抗穷举攻击。
可抵抗线性攻击(已知明文攻击)，经4轮变换后，线性分析就无能为力了。
可抵抗差分攻击(选择明文攻击)，经8轮变换后，差分攻击就无从着手了。
- **适应性强**
Rijindael的数据库长度和密钥长度都可变，因此能够适应不同的安全应用环境。

<br>
<br>
## SM4算法
- 简介
    - 我国国家密码管理局公布的**无线局域网**产品使用的密码算法。
    - 数据分组为128位，密钥长度为128位，迭代32次。
    - 数据处理单位：字节(8位)，字(32位)。
- 特点
    - 对合运算：解密算法和加密算法结构相同
    - 子密钥生成算法与加密算法结构类似
- 结构
    - 采用非对称Feistel结构。

### 基本运算
- 模2加：$\bigoplus$，32位异或运算
- 循环移位：<<<i，把32位字循环左移i位

### 基本密码部件
1. **S盒：**
    - 非线性**字节**变换，起混淆作用
    - 8位输入，8位输出。
    - 本质上是8位的非线性置换。
    - 设输入字节为a，输出字节为b，则S盒的运算可表示为：b=S_Box(a)
    - S盒置换规则
        以输入的**高半字节为行号，低半字节为列号**，行列交叉点的数据即为输出。
        例：设S盒的输入为EF，则S盒的输出为E行F列交叉点的值，即
        Sbox('ef')='84'
        说明：在主要密码学指标上达到最佳，与AES的S盒相当

![SM4密码算法S盒数据表](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-14-53-14.png)
<br>
2. **非线性变换$τ$:起混淆作用**
4个S和并行置换
设输入字$A=(a_0,a_1,a_2,a_3)$,输出字$B=(b_0,b_1,b_2,b_3)$
B=τ(A)=(S_box(a<sub>0</sub>),S_box(a<sub>1</sub>), S_box(a<sub>2</sub>),S_box(a<sub>3</sub>))

![非线性变换$τ$](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-15-29-37.png)

<br>
3. **字线性部件L变换：起扩散作用**
- 32位输入，32位输出。
- 设输入为字B，输出为字C，则
$C=L(B)=B \bigoplus (B<<<2) \bigoplus (B<<<10) \bigoplus (B<<<18) \bigoplus (B<<<24)$

4. **字合成变换$τ$：**
- 由非线性变换$τ$和线性变换L复合而成。
$T(x)=L(τ(A))$
**先S变换，再L变换**

<br>

### 轮函数
- 输入数据：$(x_0,x_1,x_2,x_3)$，128位，四个32位字。
- 输入轮密钥：rk，32位字。
- 输出数据32位字。
- 轮函数：$F(x_0,x_1,x_2,x_3)=x_0 \bigoplus T((x_1 \bigoplus x_2 \bigoplus x_3 \bigoplus rk))$

![轮函数](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-15-37-34.png)
<br>

### 加密算法
- 输入明文：$(M_0,M_1,M_2,M_3)=(X_0,X_1,X_2,X_3)$四个字，128位。
- 输入轮密钥：$rk_i,i=0,1,......,31$，共32个轮密钥。
- 输出密文：$(Y_0,Y_1,Y_2,Y_3)$，四个字，128位。
- 算法结构：轮函数32轮迭代，每轮使用一个轮密钥。
- 加密算法：
    - 加密变换 $X_{i+4}=F(x_i,x_{i+1},x_{x+2},x_{i+3},rk_i)=x_i \bigoplus T(x_{i+1} \bigoplus x_{i+2} \bigoplus x_{i+3} \bigoplus rk_i,i=0,1...31)$
    - 反序变换：$(Y_0,Y_1,Y_2,Y_3)=(X_{35},X_{34},X_{33},X_{32})$

![加密算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-16-19-20.png)
<br>

### 解密算法
- SM4密码算法是对合的，因此解密与加密算法相同，只是轮密钥的使用顺序相反。
- 输入密文：$(Y_0,Y_1,Y_2,Y_3)=(X_35,X_34,X_33，X_32)$
- 输入轮密钥：$rk_i,i=31,30,......,0$
- 输出明文：(X_0,X_1,X_2,X_3)
- 算法：轮函数的32轮迭代，每轮使用一个轮密钥
- 解密算法：用符号X描述

![解密算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-16-55-38.png)
<br>

### 密钥扩展算法
1. 常数FK
在密钥扩展中使用一些常数(32位字)；
$FK_0=(A3B1BAC6)$
$FK_1=(56AA3350)$
$FK_2=(677D9197)$
$FK_3=(B27022DC)$

2. 固定参数CK，32位：32个固定参数CK<sub>i</sub>,i=0,1,2,...,31

![固定参数CK](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-17-01-15.png)
- 输入加密密钥：$MK=(MK_0,MK_1,MK_2,MK_3)$
- 输出轮密钥：$rk_i,i=0,1,...,30,31$
- 中间数据：$K_i,i=0,1,...,34,35$
- 密钥扩展算法：

![密钥扩展算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-10-17-04-34.png)

### SM4密码结构
- SM4密码与DES密码有相似性
- DES密码采用的是Feistel结构，SM4也采用Feistel结构。
- DES中参与交换的两个数据块的长度也是相等的，而在SM4中参与交换的两个数据块的长度是不相等的。
- 因此，密码届称DES密码采用的是对称Feistel结构，SM4密码采用的是非对称Feistel结构。

<br>
<br>
## 分组密码工作模式
- 分组密码可以按不同的模式工作，实际应用的环境不同应采用不同的工作模式。只有这样才能既确保安全，又方便高效。

### 电子密码本(ECB)
![ECB](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-09-58-00.png)
![ECB加密](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-10-58-37.png)
**特点**
- 数据的长度是密码分组长度的整数倍
- 容易暴露明文的数据模式
- 由于所有数据块分组的加密方式一致，难以抵抗统计分析攻击
- 操作简单，容易实现
- 由于每个块的加密和解密都是独立的，可并行进行加密

<br>
### 密码块链接(CBC)
#### 明密文链接模式
- 解决了ECB的安全缺陷，可以让重复的明文分组产生不同的密文分组，可以抵御重放攻击。
- 适合传输数据长度大的报文
- 加解密传播无界，加密时，当明文块或密文块中发生一位错误时，自此以后的密文都发生错误
- 数据的长度是密码分组的整数倍
- 不能进行并行加密

![明密文链接模式](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-11-00-33.png)

#### 密文链接模式
- 使用了不同的初始向量组，完全相同的明文在加密之后可以得到不同的密文，可以抵御重放攻击
- 适合传输数据长度大的报文
- 加密传播无界，但解密传播有界，解密时，当密文块中发生一位错误时，只影响其中相应解密的明文，其余不会发生错误，适合磁盘文件加解密
- 数据的长度是密码分组的整数倍
- 不能进行并行加密

![密文链接模式](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-11-13-55.png)

#### CBC特点
- 解决了ECB的安全缺陷，可以让重复的明文分组产生不同的密文分组，可以抵御重放攻击。
- 适合传输数据长度大的报文
- 明密文链接模式加解密传播无界；密文链接模式加密错误传播无界，解密错误传播有界
- 数据的长度是密码分组的整数倍
- 不能进行并行加密

<br>
### 填充密码块链接(PCBC)

<br>
### 输出反馈(OFB)
![OFB](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-14-38-47.png)
<br>

### 密文反馈(CFB)
![CFB](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-14-59-34.png)
<br>

### 计数器模式(CTR)
![CTR](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-03-03.png)
![CTR特点](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-05-01.png)
<br>

### 例题
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-09-41.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-11-07.png)
![例题3](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-11-32-33.png)
1. 明密文链接模式
2. 加解密错误传播无界
数据的长度是密码分组的整数倍

<br>
<br>
# 序列密码
![概念](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-15-55.png)
![与分组密码的区别](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-20-49.png)
<br>

## RC4序列密码
![rc4](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-21-40.png)
![KSA算法1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-32-32.png)
![KSA算法2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-41-43.png)
![PRGA算法1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-44-26.png)
![PRGA算法2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-46-25.png)
![RC4的安全性](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-50-53.png)
<br>

## ZUC算法
ZUC算法，又称为祖冲之算法，是移动通信3GPP机密性算法EEA3和完整性算法EIA3的核心，是中国自主设计的加密算法。
<br>

## 例题
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-54-32.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-55-20.png)
<br>
<br>

# Hash函数
![概念](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-15-57-46.png)
![Hash函数基本设计结构](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-04-46.png)
<br>

## SHA算法
![SHA算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-06-22.png)
**算法步骤**
![消息填充](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-12-56.png)
![初始化缓冲区](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-16-32.png)
![算法主循环](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-17-26.png)
![压缩函数处理1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-18-41.png)
![压缩函数处理2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-21-50.png)
![压缩函数处理3](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-27-19.png)
![压缩函数处理4](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-29-03.png)
![压缩函数处理5及输出](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-30-01.png)

## 例题
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-30-43.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-31-17.png)
![例题3](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-31-32.png)
![例题4](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-32-32.png)
![例题5](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-16-33-45.png)

# 公钥密码体制
**对称密码存在哪些局限性**
- 密钥分发的问题
- 密钥数量的问题
- 数字签名问题

## 公钥密码体制模型
![加密模型](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-19-20.png)
![认证模型](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-23-58.png)
![加密认证混合模型](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-24-48.png)
![数字信封技术](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-35-47.png)
**公钥加密的安全性**
- 确保数据的秘密性
- 确保数据的真实性
- 同时确保数据的秘密性和真实性

**例题**
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-38-56.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-44-12.png)
![例题3](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-45-16.png)
![例题4](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-49-35.png)
![例题5](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-51-10.png)
![例题6](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-52-51.png)
![例题7](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-54-50.png)
![例题8](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-20-58-55.png)
![例题9](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-01-28.png)

## RSA密码
![相关数学基础](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-08-22.png)
![RSA的加密和解密](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-11-37.png)
- n,e公开

![求解乘法逆元](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-22-07.png)
![RSA实例](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-24-36.png)
![RSA实例体制的特点及安全性](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-24-58.png)
<br>

## ELGamal密码
**了解**
![相关数学基础](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-27-18.png)
![参数定义和密钥生成](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-29-27.png)
![加密算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-30-58.png)
![解密算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-31-16.png)
![实例](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-32-17.png)
![特点和安全性](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-33-09.png)
<br>

**例题**
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-34-57.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-35-36.png)
![例题3](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-40-22.png)
![例题4](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-41-54.png)
<br>

## 数字签名
![数字签名的特征](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-46-25.png)
### RSA
![RSA签名的生成和验证算法1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-47-54.png)
![RSA签名的生成和验证算法2 ](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-49-04.png)
![RSA签名验证算法实例](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-52-04.png)
<br>

### ELGamal密码
![ELGamal参数定义和密钥生成](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-53-59.png)
![ELGamal签名的生成和验证算法](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-55-41.png)
![ELGamal签名验证算法实例](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-56-02.png)
<br>

### RSA和ELGamal签名安全性
![安全性](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-21-56-57.png)

### 例题
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-12-22-00-56.png)
<br>
<br>

# 认证
## 概念
- 认证，又称鉴别或确认，是证实某事是否名副其实或是否真实有效的一个过程。
- 认证和加密的区别
    - 加密用以确保数据的保密性，阻止对手的被动攻击，如截取、窃听等。
    - 认证用以确保报文发送者和接受者的真实性以及报文的完整性，阻止对手的主动攻击，如冒充、篡改、重播等。

![认证原理](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-13-20-50-55.png)

- 认证和数字签名技术的区别
    - 认证总是基于某种收发双方共享的保密数据来认证被鉴别对象的真实性，数字签名中用于验证签名的数据是公开的。
    - 认证允许收发双方互相验证其真实性，不准许第三者验证，数字签名允许双方和第三者都能认证。
    - 数字签名具有发送方不能抵赖，接收方不能伪造和具有在公证人前解决纠纷的能力，而认证不一定具备。

## 身份认证
### 口令认证
- 利用单向函数加密口令
    用户口令在系统中以密文的形式存储。只能加密，不能解密。
- 利用数字签名方法验证口令
    用户的公钥存储在系统中。系统为用户设置了时间标志T<sub>i</sub>,可以抵抗重播攻击。
- 口令的双向验证
    用户和系统相互平等地验证对方的身份
    选择一个单向函数f，假设A要求与B，则A和B可如下相互认证对方的身份：
    1. A->B:R<sub>A</sub>
    2. B->A:f(P<sub>B</sub>||R<sub>A</sub>)||R<sub>B</sub>
    3. A->B:f(P<sub>A</sub>||R<sub>B</sub>)
    由于f是单向函数，即使知道f(P<sub>B</sub>||R<sub>A</sub>)也不能计算出P<sub>A</sub>，即使知道f(P<sub>A</sub>||R<sub>B</sub>)，也不能计算出P<sub>B</sub>。可在f函数中加入时间参数，阻止重播攻击。
- 一次性口令
    一个口令只用一次，可以抵御重放攻击。

### 生物特征识别
- 通过识别用户的生理特征来认证用户的身份，具有唯一性和稳定性
- 如指纹、掌纹、面孔、发音、虹膜、视网膜、骨架等

## 报文认证
报文认证，必须是通信方能够验证每份报文的发送方、接收方、内容和时间性的真实性和完整性

### 报文源的认证
- 设A为报文发送方，简称为源，B是报文的接收方，简称为宿。
- A与B共享保密的密钥$K_S$。A的标识为$ID_A$，要发送的报文为$M$，那么B认证A的过程如下
    $A->B:E(ID_A||M,K_S)$
- A在发给B的每份报文中都增加标识$ID_A$，然后用$K_S$加密并发送给B。
- 公开密钥密码实现报文源的认证：$A->B:E(ID_A||M,K_{dA})$

### 报文宿的认证
- 在以密钥为基础的认证方案的每份报文中加入接受方标识符$ID_B$
    $A->B:E(ID_B||M,K_S)$
- 公开密钥密码实现报文宿的认证：$A->B:E(ID_B||M,K_{eB})$

### 报文内容的认证
报文内容认证使接收方确认报文内容的真实性，通过验证认证码的正确性来实现。
#### 报文加密
将整个报文的密文作为认证码：发送方A要发送报文给接收方B，则A用他们的共享密钥K对发送的报文M加密后发给B；
    $A->B:E(M,K)$

- **报文的密码性**：如只有A和B知道密钥K，其他任何人均不能恢复出报文明文。
- **报文源的认证**：除B外只有A拥有K，也就只有A可产生出B能解密的密文。
- **报文认证**：如果B可以恢复出明文，则可认为M中的每一位都未被改变。

#### 消息认证码(MAC)
消息认证的实质是：发送者通过双方协商的某种函数(如Hash函数)，以待发消息作为输入经过函数变换产生消息认证码(简称MAC)。
    $MAC=C(M,K)$
    $A->B:E(M||C(M,K_1),K_2)$
    $A->B:E(M,K_2)||C(E(M,K_2),K_1)$

#### 基于hash函数的消息认证码(HMAC)
带密钥的Hash函数构造方案
![HMAC](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-13-21-37-09.png)

## 例题
![例题1](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-13-21-41-52.png)
![例题2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-13-21-43-06.png)
<br>
<br>

# 密钥管理
## 概念
- 密钥管理包括密钥的产生、存储、分配、组织、使用、更换、销毁等一系列技术问题。

## 对称密码的密钥管理
![对称密码的密钥管理](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Crypto/2019-05-13-21-44-34.png)

## 非对称密码的密钥管理
### 公约基础设施PKI
- **签证机构CA**：负责签发证书，管理和撤销证书
- **注册机构RA**：专门负责受理用户申请证书的机构
- **证书的签发**：CA签发
    **CA签发过程**：
    1. 用户向CA提交RA的注册批准信息及自己的身份等信息(或由RA向CA提交)；
    2. CA验证所提交信息的正确性和真实性；
    3. CA为用户产生密钥(或由用户自己产生并提供密钥)，并进行备份；
    4. CA生成证书，并施加签名；
    5. 将证书的一个副本交给用户，并存档入库。
- 证书目录
- 证书的认证
- 证书的撤销
- 信任模型

<br>
<br>