# Ⅰ
作用：提高cin速度，但副作用是可能干扰文件输入，程序中不能继续使用scanf，优化后没有scanf快
```
ios::sync_with_stdio(false);
```


算法有模板的以背为主，理解并会默写，重复三到五次默写模板；
# 快排的思想是分治
```
#include <iostream>
using namespace std;
int n,a[1000001];
void qsort(int l,int r){
    int mid=a[(l+r)/2];
    int i=l,j=r;
    do{
        while(a[i]<mid)i++;
        while(a[j]>mid)j--;
        if(i<=j){
            swap(a[i],a[j]);
            i++;j--;
        }
    }while(i<=j);
    if(l<j)qsort(l,j);
    if(r>i)qsort(i,r);
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    qsort(1,n);
    for(int i=1;i<=n;i++) cout<<a[i]<<" ";
    return 0;
}
```
# 高精度
```
#include<stdio.h>
#include<string>
#include<string.h>
#include<iostream>
using namespace std;
//compare比较函数：相等返回0，大于返回1，小于返回-1
int compare(string str1,string str2){
    if(str1.length()>str2.length()) return 1;
    else if(str1.length()<str2.length())  return -1;
    else return str1.compare(str2);
}
//高精度加法//只能是两个正数相加
string add(string str1,string str2)//高精度加法{
    string str;
    int len1=str1.length();
    int len2=str2.length();
    //前面补0，弄成长度相同
    if(len1<len2)
    {
        for(int i=1;i<=len2-len1;i++)
           str1="0"+str1;
    }
    else
    {
        for(int i=1;i<=len1-len2;i++)
           str2="0"+str2;
    }
    len1=str1.length();
    int cf=0;
    int temp;
    for(int i=len1-1;i>=0;i--)
    {
        temp=str1[i]-'0'+str2[i]-'0'+cf;
        cf=temp/10;
        temp%=10;
        str=char(temp+'0')+str;
    }
    if(cf!=0)  str=char(cf+'0')+str;
    return str;
}
//高精度减法//只能是两个正数相减，而且要大减小/*
string sub(string str1,string str2)//高精度减法
{
    string str;
    int tmp=str1.length()-str2.length();
    int cf=0;
    for(int i=str2.length()-1;i>=0;i--)
    {
        if(str1[tmp+i]<str2[i]+cf)
        {
            str=char(str1[tmp+i]-str2[i]-cf+'0'+10)+str;
            cf=1;
        }
        else
        {
            str=char(str1[tmp+i]-str2[i]-cf+'0')+str;
            cf=0;
        }
    }
    for(int i=tmp-1;i>=0;i--)
    {
        if(str1[i]-cf>='0')
        {
            str=char(str1[i]-cf)+str;
            cf=0;
        }
        else
        {
            str=char(str1[i]-cf+10)+str;
            cf=1;
        }
    }
    str.erase(0,str.find_first_not_of('0'));//去除结果中多余的前导0
    return str;
}
//高精度乘法
//只能是两个正数相乘
string mul(string str1,string str2)
{
    string str;
    int len1=str1.length();
    int len2=str2.length();
    string tempstr;
    for(int i=len2-1;i>=0;i--)
    {
        tempstr="";
        int temp=str2[i]-'0';
        int t=0;
        int cf=0;
        if(temp!=0)
        {
            for(int j=1;j<=len2-1-i;j++)
              tempstr+="0";
            for(int j=len1-1;j>=0;j--)
            {
                t=(temp*(str1[j]-'0')+cf)%10;
                cf=(temp*(str1[j]-'0')+cf)/10;
                tempstr=char(t+'0')+tempstr;
            }
            if(cf!=0) tempstr=char(cf+'0')+tempstr;
        }
        str=add(str,tempstr);
    }
    str.erase(0,str.find_first_not_of('0'));
    return str;
}
//高精度除法
//两个正数相除，商为quotient,余数为residue
//需要高精度减法和乘法
void div(string str1,string str2,string &quotient,string &residue)
{
    quotient=residue="";//清空
    if(str2=="0")//判断除数是否为0
    {
        quotient=residue="ERROR";
        return;
    }
    if(str1=="0")//判断被除数是否为0
    {
        quotient=residue="0";
        return;
    }
    int res=compare(str1,str2);
    if(res<0)
    {
        quotient="0";
        residue=str1;
        return;
    }
    else if(res==0)
    {
        quotient="1";
        residue="0";
        return;
    }
    else
    {
        int len1=str1.length();
        int len2=str2.length();
        string tempstr;
        tempstr.append(str1,0,len2-1);
        for(int i=len2-1;i<len1;i++)
        {
            tempstr=tempstr+str1[i];
            tempstr.erase(0,tempstr.find_first_not_of('0'));
            if(tempstr.empty())
              tempstr="0";
            for(char ch='9';ch>='0';ch--)//试商
            {
                string str,tmp;
                str=str+ch;
                tmp=mul(str2,str);
                if(compare(tmp,tempstr)<=0)//试商成功
                {
                    quotient=quotient+ch;
                    tempstr=sub(tempstr,tmp);
                    break;
                }
            }
        }
        residue=tempstr;
    }
    quotient.erase(0,quotient.find_first_not_of('0'));
    if(quotient.empty()) quotient="0";
}
int main(){
return 0;
}
```
# 前缀和与差分
前缀和与差分互为逆运算
前缀和就是之前所有数的和
差分就是相邻两个数的差
一维差分可以用来解决一个数组内一个连续部分多次加上一段数字的问题
/树状数组/
二维差分可以用来解决一个矩阵内一个矩阵块的元素的多次加上一段数字的问题
一维前缀和就是预处理，目前来看没多大用
二维前缀和可以用来计算矩阵内一个矩阵块的元素和
一、一维前缀和
```
   si=a1+a2+...+ai
   sum(L+R)=sR-sI

      二、二维前缀和

s[i][j]怎么算
s[i,j]=s[i-1,j]+s[i,j-1]-s[i-1,j-1]+a[i,j];
(x1,y1)与（x2,y2)内所有的数怎么算
s[x2,y2]-s[x1-1,y2]-s[x2,y1-1]+s[x1-1,y1-1]
```
小数据下python等语言快，大数据下c++等语言快


给定原矩阵，构造差分矩阵
假定原矩阵全为0，则差分矩阵全为0，
差分核心操作：
给以（x1,y1）为左上角，(x2,y2)为右下角的子矩阵中所有数
a[i][j],加上C
对于差分数组的影响：
```
s[x1,y1]+=c;
s[x1,y2+1]-=c;
s[x2+1,y1]-=c;
s[x2+1,y2+1]+=c;
```
即把原数组先看做全是0，之后依次插入每一个数
# 位运算
lowbit(x)=x&(-x)；
其中lowbit(X)得到一个数m，m是x能整除的最大的2的幂次，
如x=8,m=8;
   x=6,m=2;
   x=14,m=2;
建议再程序头文件之后直接加
```
#define lowbit(x)  (x&(-x))
```
## 一、求n的二进制表示中第K位是几
步骤：
    Ⅰ先把第K位移到最后一位，n>>k;
    Ⅱ看个位是几，x&1
即n>>k&1
## 二、lowbit
补码   1.....10110    即-x+1
原码   假设x=10,则  0....01010    即x
反码   1.....10101    即-x


人类做题的过程其实是一个dfs的过程


# 离散化
对于一些比较大的数值为保证其顺序不变，
出于某些特殊的需要而把值域映射到从0开
始的自然数。
```
vector<int> alls;
sort(alls.begin(),alls.end());
alls.erase(unique(alls.begin(),alls.end()),alls.end());
//单独的unique把不同的数放在前面，重复的数放在后面，返回第一个重复数的位置


unique
vector<int>::iterator uinque(vector<int> &a){
int j=0;
for(int i=0;i<a.size();i++){
      if(!i||a[i]!=a[i-1])
          a[j++]=a[i];
       }
return a.begin()+j;
}
```









