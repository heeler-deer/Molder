参考内容：   
[知乎](https://zhuanlan.zhihu.com/deeplearningcat),[github](https://github.com/Exely/CSAPP-Labs)    
自己做的lab因为makefile的一些问题导致我以时冲动删除了。。。。    
实验环境是ubuntu20.04-64位。这个实验还是看github的链接吧，我的删了。。。。。。。   
值得一提的是，当你make clean了github里的lab后，重新生成的btest只能得到39分，github中的fitbits可能因为某些问题出现错误，报错为：  
```
ERROR: Test fitsBits(-2147483648[0x80000000],32[0x20]) failed...
...Gives 1[0x1]. Should be 0[0x0]
```
暂时不知道为什么会出现这样的错误。逻辑是对的，但就是报错。。。。。