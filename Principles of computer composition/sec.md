# 计算机的存储


0代表正，1代表负
## BCD码
### ８４２１码：　　　
０　００００   
１　０００１   
４　０１００   
每一个位对应其值，５＋８＝０１０１＋１０００＝１１０１   
而１０１０～１１１１没有意义，故加上０１１０修正，变为：　　
０００１　００１１
### 余3码
直接就是8421码的映射表上加上0011，但是是无权码。  即每一位没有固定的取值  
### 2421码
改变权值，采用类似二进制的方法计数   
![13](https://github.com/heeler-deer/Molder/blob/main/png/13.png)
## 字符与字符串
32~126为可印刷字符，其余为控制、通信字符     
每一个字符的ASCII码都是八位　　　　
GB 2312-80:分为94个区，对每个区分位置号，区有区号   
为防止冲突，加上`20H`成为国标码，为了保证高位为1即与
ASCII区分，再加上`80H`得到汉字内码    
这是机内码的存储方式，对于输入编码和字形编码，建立映射即可。
## 奇偶校验码
冗余     
两个码字的距离为其不同的位的个数，而各个合法码字的距离称为码距，码距大于等于3是具有校验、纠错的能力
### 奇校验码
1的个数为奇数
### 偶校验码
1的个数为偶数   
## 海明校验码
设信息位有n个，校验位有k个，总共2^k个状态，总共n+k位，若要检验，则需要满足     
2^k>=n+k+1   
校验位应放在2^i-1的位置上   
海明码纠错能力：1，检错能力：2位，最前方加上一个全校验位   
![14](https://github.com/heeler-deer/Molder/blob/main/png/14.png)
## 循环冗余校验码CRC
k个信息位加r个校验位确保被约定的除数相除后余数为0           
