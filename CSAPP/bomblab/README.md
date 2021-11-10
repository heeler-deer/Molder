该题目要求你通过输入6个正确的phase，来完成bomb。输入正确即可解除炸弹，否则炸弹爆炸。    
lab给出了无法编译运行的bomb.c和一个`ELF 64-bit LSB executable`的bomb，此题的解法是反编译bomb,根据bomb和bomb.c来推断出正确的phase。最后把正确的phase保存到一个文件result.txt中，输入`./bomb result.txt`即可知晓结果         
参考[CSDN](https://blog.csdn.net/astrotycoon/article/details/73248964),[知乎](https://zhuanlan.zhihu.com/deeplearningcat),[github](https://github.com/Exely/CSAPP-Labs) ,[chuanleiguo](https://chuanleiguo.com/2017/04/17/csapp-bomblab/)         
值得一提的是，chuanleiguo博客中关于phase_5的答案是错误的，至少目前看来是这样。     
常用调试命令：      
[gdb 指令](http://csapp.cs.cmu.edu/2e/docs/gdbnotes-x86-64.pdf)         
先反编译：
```
objdump bomb -d > obj.txt
```
把结果保存到了obj.txt中。    
## phase_1
看bomb.c我们可以知道，程序通过读入相应数据，作出判断，看是否会爆炸。那么我们可以在obj.txt中搜寻对应的phase（ctrl+f），看看他们是怎么实现的。    
寻找`phase_1`,phase_1第一次出现是和其他编号的phase出现在同一个片段，应该是程序的main函数调用各个phase的操作，而不是phase_1实现其功能的片段。那么我们继续向下找，后两次phase_1出现在同一片段。     
前两行汇编语言，寄存器%esi存了值0x402400，然后调用了一个函数strings_not_equal，然后对返回值%eax用了test指令，判断是否是0，如果等于0，跳过explode_bomb，返回。     
那么我们的任务显然是让%eax中的值返回0.那么继续搜索strings_not_equal函数，看看他干了什么。    
共13次出现，但我们可以轻而易举的找到strings_not_equal的实现部分。      
我们看看下面的代码：
```
  40133c:       48 89 fb                mov    %rdi,%rbx
  40133f:       48 89 f5                mov    %rsi,%rbp
```
他把%rdi的值赋给%rbx,把%rsi的值赋给%rbp。显然这意味着%rdi,%rsi存着该函数的参数。而参数显然是刚刚的phase_1传的。那么我们看看phase_1传了什么：
```
0000000000400ee0 <phase_1>:
  400ee0:	48 83 ec 08          	sub    $0x8,%rsp
  400ee4:	be 00 24 40 00       	mov    $0x402400,%esi
  400ee9:	e8 4a 04 00 00       	callq  401338 <strings_not_equal>
```
我们通过phase_1的汇编，分析出phase_1就是比较两个字符串看是否相等，那么%rdi,%rsi必然一个存着我们要求解的phase_1的input，另一个就是原先bomb.c中设定的字符串。他的地址，显然是phase_1中那个`mov    $0x402400,%esi`中的`$0x402400`.那么我们打印出这个地址即可。输入`gdb bomb`,之后输入`print (char *) 0x402400`即可得到phase_1正确的input:
```
gdb bomb
GNU gdb (Ubuntu 10.1-2ubuntu2) 10.1.90.20210411-git
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from bomb...
(gdb) print (char *) 0x402400
$1 = 0x402400 "Border relations with Canada have never been better."
```
其中 **Border relations with Canada have never been better.** 即为正确结果。     
## phase_2
老套路找phase_2，
```
  400efc:	55                   	push   %rbp
  400efd:	53                   	push   %rbx
  400efe:	48 83 ec 28          	sub    $0x28,%rsp
  400f02:	48 89 e6             	mov    %rsp,%rsi
  400f05:	e8 52 05 00 00       	call   40145c <read_six_numbers>
```
接下来找read_six_numbers。     
注意到这些语句：
```
  400f17:       8b 43 fc                mov    -0x4(%rbx),%eax
  400f1a:       01 c0                   add    %eax,%eax
  400f1c:       39 03                   cmp    %eax,(%rbx)
  400f1e:       74 05                   je     400f25 <phase_2+0x29>
  400f20:       e8 15 05 00 00          callq  40143a <explode_bomb>
  400f25:       48 83 c3 04             add    $0x4,%rbx
  400f29:       48 39 eb                cmp    %rbp,%rbx
  400f2c:       75 e9                   jne    400f17 <phase_2+0x1b>
```
最后一句是jne 400f17,即返回上面，那么这就是一个循环。在看add %eax,%eax,即把输入值变为原先的2倍，如果phase_2输入的六个值中对应的值不是前一个值的2倍，就会bomb.那么我们需要回到phase_2找到最开始那个不会爆炸的值：
```
  400f0a:	83 3c 24 01          	cmpl   $0x1,(%rsp)
  400f0e:	74 20                	je     400f30 <phase_2+0x34>
  400f10:	e8 25 05 00 00       	call   40143a <explode_bomb>
```
这里把1和(%rsp)作了比较，那么显然，第一个值应该是1,那么输入的六个数分别是：
```
1 2 4 8 16 32
```
这就是phase_2
## phase_3
```
  400f43:	48 83 ec 18          	sub    $0x18,%rsp
  400f47:	48 8d 4c 24 0c       	lea    0xc(%rsp),%rcx
  400f4c:	48 8d 54 24 08       	lea    0x8(%rsp),%rdx
  400f51:	be cf 25 40 00       	mov    $0x4025cf,%esi
  400f56:	b8 00 00 00 00       	mov    $0x0,%eax
  400f5b:	e8 90 fc ff ff       	call   400bf0 <__isoc99_sscanf@plt>
```
先按以前的方法找到phase_3函数。      
我们不妨先用gdb看看400f51处是什么意思：    
```
(gdb) x/s 0x4025cf
0x4025cf:	"%d %d"

```
400f5b处的代码call了一个类似于scanf的输入函数,输入两个值，之后比较$0x1,%eax,如果%eax的值大于1就会执行jg下的`call   40143a <explode_bomb>`,而之前赋值给%eax的值为0,因此直接跳转给400f6a,        
接下来继续看，比较7和%rsp的值，如果%rsp大于7就会跳转到bomb,因此我们的第一个输入可以是0～7的任意一个数。       
之后是把%rsp的第二个值赋给%eax,然后是一个神奇的跳转语句，猜测是根据输入的第一个数进行运算后跳转到一个地址，我们用gdb调试：     
>第二条指令jmpq   *0x402470(,%rax,8)，这条指令什么意思呢？ 可能AT&T的汇编指令不太容易看懂，那我们通过set disassembly-flavor intel来查看intel形式的这条指令为jmp    QWORD PTR [rax*8+0x402470]，这下就容易多了 -- 取出地址rax*8+0x402470处的值，并调转到这个值指示的内存地址处继续执行。如果读者有一点点的经验，就可以很容易看出此处是switch语句的跳转表，跳转表的首地址为0x402470，我们可以通过x命令看看这个表中都存储了哪些地址。







这是跳转表：
```
(gdb) x/8g 0x402470
0x402470:	0x0000000000400f7c	0x0000000000400fb9
0x402480:	0x0000000000400f83	0x0000000000400f8a
0x402490:	0x0000000000400f91	0x0000000000400f98
0x4024a0:	0x0000000000400f9f	0x0000000000400fa6
```
那么我们依次查看这些地址，发现他们都是比较两个数的值，%eax即输入的第二个数，如果两者不相等就爆炸，那么我们可以根据输入的第一个数推断出第二个数，总共有8种可能：    
| 第一个数      | 第二个数 |
| :---        |    :----:   |
| 0      | 207      | 
|1 |	311|
|2 |	707|
|3 |	256|
|4 |	389|
|5 |	206|
|6 |	682|
|7 |	327|
这就是答案了。     
## phase_4
找到phase_4,和phase_3一样，先输入两个数，第一个数要小于14（40102e处）,第二个数必须是等于0（401051处）。在扫一遍，发现401048处调用了func4，紧接着是`test %eax,%eax`,即要求%eax为0,否则爆炸，接下来我们找到func4        
观察其结构，不难发现func4中有递归结构的存在，（400fe9处）下面引用别人的分析：       
>`func4` 函数在 `%ecx` 中利用了一个递归分别存入 7 , 3 , 1 ，并使用跳转使得，第一个数小于7时不断递归且 `%ecx`小于等于第一个数，而跳转后即在下面的代码中会要求 `%ecx` 大于等于第一个数，否则递归，递归过程会设置eax使得其不为 0 ，所以只有当第一数等于 `%ecx` 时即 1 ， 3 , 7 时才能使最后返回值为 0 ；









最终答案为`1 0` 或 `3 0` 或 `7 0` 。
## phase_5
找到phase_5，在40107f处比较了6和%eax的长度，显然是要我们输入一个长度为6的字符串。接下来则是通过一个循环对输入的字符串进行变换。即40108b到4010ac处的代码：
```
40108b:	0f b6 0c 03          	movzbl (%rbx,%rax,1),%ecx
40108f:	88 0c 24             	mov    %cl,(%rsp)
401092:	48 8b 14 24          	mov    (%rsp),%rdx
401096:	83 e2 0f             	and    $0xf,%edx
401099:	0f b6 92 b0 24 40 00 	movzbl 0x4024b0(%rdx),%edx
4010a0:	88 54 04 10          	mov    %dl,0x10(%rsp,%rax,1)
4010a4:	48 83 c0 01          	add    $0x1,%rax
4010a8:	48 83 f8 06          	cmp    $0x6,%rax
4010ac:	75 dd                	jne    40108b <phase_5+0x29>
4010ae:	c6 44 24 16 00       	movb   $0x0,0x16(%rsp)
```
我们尝试理解这段变换，%rax是循环中的计数器，到6后退出循环；在movzbl、两个mov、一个and之后，代码中出现了一个操作`movzbl 0x4024b0(%rdx),%edx`，我们不妨打印一下这个0x4024b0,得到：
```
(gdb) x/sb 0x4024b0
0x4024b0 <array.3449>:	"maduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"
```
`maduiersnfotvbyl`显然不是一个有意义的英文，我们继续看phase_5,
```
4010b3:	be 5e 24 40 00       	mov    $0x40245e,%esi
```
在这段代码之后紧接着即是调用strings_not_equal，那么我们的目标字符串必然是0x40245e的字符串，打印出来：
```
(gdb) x/sb 0x40245e
0x40245e:	"flyers"
```
那么答案显而易见，~~留给读者证明~~      
在上面的循环中，代码把我们输入的每一个字符都先存到了%rdx，然后用%rdx与$0xf作&运算(相当于提取16进制表示的第四位)，之后movzbl（映射）到0x4024b0这个地址上。要想最终得到目标字符串，就要找到这个映射关系---由于是与0xf作&，那么我们的映射关系就是：
```
0123456789abcdef
maduiersnfotvbyl
```
所以最后的答案就是9 f e 5 6 7对应的ASCII字符：       
9?>567
## phase_6
找到phase_6，代码要求我们输入6个数，之后判断6个数中是否有重复的，同时以7为值，保存原先的数与7的差值，在然后是401183处的又一个地址，我们按照老方法打印他：
```
(gdb) x/sb 0x6032d0
0x6032d0 <node1>:	"L\001"
```
看起来挺奇怪的，先继续看phase_6,仔细分析代码：
```
40116f:       be 00 00 00 00          mov    $0x0,%esi
  401174:       eb 21                   jmp    401197 <phase_6+0xa3>
  401176:       48 8b 52 08             mov    0x8(%rdx),%rdx
  40117a:       83 c0 01                add    $0x1,%eax
  40117d:       39 c8                   cmp    %ecx,%eax
  40117f:       75 f5                   jne    401176 <phase_6+0x82>
  401181:       eb 05                   jmp    401188 <phase_6+0x94>
  401183:       ba d0 32 60 00          mov    $0x6032d0,%edx
  401188:       48 89 54 74 20          mov    %rdx,0x20(%rsp,%rsi,2)
  40118d:       48 83 c6 04             add    $0x4,%rsi
  401191:       48 83 fe 18             cmp    $0x18,%rsi
  401195:       74 14                   je     4011ab <phase_6+0xb7>
  401197:       8b 0c 34                mov    (%rsp,%rsi,1),%ecx
  40119a:       83 f9 01                cmp    $0x1,%ecx
  40119d:       7e e4                   jle    401183 <phase_6+0x8f>
  40119f:       b8 01 00 00 00          mov    $0x1,%eax
  4011a4:       ba d0 32 60 00          mov    $0x6032d0,%edx
  4011a9:       eb cb                   jmp    401176 <phase_6+0x82>
```
发现这个奇怪的数字附近有循环，即4011a9处的jmp语句。再仔细看代码，会发现这段代码从地址0x6032d0开始，将输入的6个数的依次存入了其后对应的地址。gdb调试一下即可：
```
(gdb) x/18a 0x6032d0
0x6032d0 <node1>:	0x10000014c	0x6032e0 <node2>
0x6032e0 <node2>:	0x2000000a8	0x6032f0 <node3>
0x6032f0 <node3>:	0x30000039c	0x603300 <node4>
0x603300 <node4>:	0x4000002b3	0x603310 <node5>
0x603310 <node5>:	0x5000001dd	0x603320 <node6>
0x603320 <node6>:	0x6000001bb	0x0
0x603330:	0x0	0x0
0x603340 <host_table>:	0x402629	0x402643
0x603350 <host_table+16>:	0x40265d	0x0
```     
可以多试一下x/后的数字；看上面的调试结果，显然这是一个链表。       
之后又把链表从大到小排序：
```
  011da:       bd 05 00 00 00          mov    $0x5,%ebp
  4011df:       48 8b 43 08             mov    0x8(%rbx),%rax
  4011e3:       8b 00                   mov    (%rax),%eax
  4011e5:       39 03                   cmp    %eax,(%rbx)
  4011e7:       7d 05                   jge    4011ee <phase_6+0xfa>
  4011e9:       e8 4c 02 00 00          callq  40143a <explode_bomb>
  4011ee:       48 8b 5b 08             mov    0x8(%rbx),%rbx
  4011f2:       83 ed 01                sub    $0x1,%ebp
  4011f5:       75 e8                   jne    4011df <phase_6+0xeb>
```
那么我们一次打印以0x6032d0开始的连续6个地址的值，分别是：
332、168、924、691、477、443,     
对应的序号分别是：3、4、5、6、1、2，那么我们实际的输入索引应该是：     
4、3、2、1、6、5