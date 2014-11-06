---
layout: post
title: 漫谈随机数
category: algorithm
description: 说到生成任意范围内等概率随机数，如果止步于x = rand () % RANGE，恐怕有点不够意思
---
生成任意范围内等概率随机数是个在平常不过的需求。

##简单做法
C语言有个rand()函数，会返回一随机数值，范围在0至RAND_MAX间。于是很容易想到一个简单的做法

x = rand () % RANGE; /* Poor! return a random number in [0,RANGE) */ 

这种做法有三个问题：

1. rand()等概率返回[0, RAND_MAX]间这RAND_MAX+1个数中的任意一个，如果RAND_MAX+1不能被RANGE整除，那么这个[0, RANGE)分布就blatantly不均匀了。举个例子RAND_MAX=32767，目标区间为[0,10)，32767%10=7,则(7,10)间数的取值概率小于[0,7]间数。

2. rand()的伪随机性。大多数随机数发生器产生随机数的低位bits都distressingly不随机（有一个short cyclical pattern或者bits间有依赖关系）。

3. RAND_MAX在<stdlib.h>中定义,其值最小为32767(2^15-1),最大为2147483647(2^31-1),假如RAND_MAX+1 < RANGE呢？

##RANGE<=RAND_MAX+1
###方法1

这种方法相当于取高位bits

(int)(rand() / (RAND_MAX + 1.0) * RANGE)

但是这种方法解决了问题2，却并没有解决问题1，因为如果RAND_MAX+1不能被RANGE整除，不管按照什么规则，RAND_MAX+1个数始终是无法均分放到RANGE个格子里的，输入数据量较大时这个bias就会比较明显。
 
###方法2

去除x=rand()所产生最大的(RAND_MAX+1)%RANGE个数，再(int)x/(int)((RANG_MAX+1.0)/RANGE)

```cpp
int myrandom_range(int start,int end) {//[start,end)
    int N = end - start;
    int bound = (RAND_MAX + 1) / N * N;
    int r;
    do {
        r = rand();
    } while (r >= bound);
    return start + int(r / (RAND_MAX + 1.0) * N);
}
```

这种方法解决了问题1&2，但是这个做法在RAND_MAX%RANGE (< RANGE)比较大的时候，可能会很浪费时间（极端情况是接近1/2概率需要重试），一般情况下还是够用的。
Let p = (RAND_MAX % RANGE) / (RAND_MAX + 1.0). This is the probability that any given call to rand() will require a retry. Note that this value is maximized when RANGE*2 = RAND_MAX+3, and which will yield a value of p roughly equal to 1/2.
- The average number of times that rand() will be called is 1/(1-p) (with a worst case of about 2).
- The probability that at least n calls to rand() will be required beyond the first one is pn.
So for most values of p which will be much less than 1/32 say, performance should not be a concern at all.

##RANGE>RAND_MAX+1
###方法2++
现在我们来考虑一个相对简单的问题，给你一个能生成1到5随机数的函数，用它写一个函数生成1到7的随机数。
很简单,把rand的范围放大，在用方法2踢掉尾数，设有随机变量x=(rand5()-1)*5+rand5()，x的值可以一一对应到2位的5进制数，很显然00(1)-44(25)这25个数的概率都是均匀的(由于只有1个5进制bit就不考虑取高位bits这种优化了)。

```cpp
int Rand7(){
	int x;
    do {
        x = 5 * (Rand5() - 1) + Rand5() // Rand25
    } while(x > 21)
    return x%7 + 1;
}
```

###方法1++
考虑RANGE能被RAND_MAX+1整除的情况，例如数据范围[0,2^32)，前面提到，RAND_MAX其值最小为(2^15-1),最大为(2^31-1),按最坏的情况算至少需要调用三次rand()，才能获得32位随机数，我们采取方法1中取高位的做法，三次rand()分别取低15位中的前10,11,11位。

```cpp
inline unsigned __int32 rand32(){
    return ((rand()&0x00007FE0)>>5) + ((rand()&0x00007FF0)<<6) + ((rand()&0x00007FF0)<<17);
}
``` 

如果不能整除，就取一个刚好比RANGE大的可以整除的区间，然后再类似方法2++踢掉尾数。

###方法3
如果你懒得自己写，干脆用boost库，mt19937 produces integers in the range [0, 2^32-1].

>最后，使用之前rand()，别忘了设随机种子srand((unsigned int)time(0))哦。

##题外话
序列上的随机算法，往往可以用线性扫描+替换之前元素的思想
### 随机洗牌 (CC150v5-18.2)
基本思想是打乱子序列[0, i-1]的顺序，再将a[i]随机地跟a[0]到a[i]任一个元素交换，那么a[i]最终在位置k上的概率是

1/(i+1) * (i+1)/(i+2) * (i+2)/(i+3) *...* (N-2)/(N-1) * (N-1)/N = 1/N
###长度为N(未知)的序列中随机取出k个元素 (编程珠玑12.3)
基本思想是先选中前k个，从第k+1个元素开始， 以k/i (i=k+1, k+2,...,N) 的概率选中第i个元素，并且随机替换掉一个原先选中的元素，那么a[i]最终被选中的概率是

(k/ (i+1)) * (1 - 1/(i+2)) * (1 - 1/(i+3)*...*(1 - 1/(N+1))) = k/(N+1)

##参考

[C++生成随机数——生成任意范围内的等概率随机数](http://hi.baidu.com/silyt/item/6f9c19d352a4644dfb576891)

[Misconceptions about rand()](http://www.azillionmonkeys.com/qed/random.html)

<http://tech-wonderland.net/blog/reservoir-sampling-learning-notes.html>

<http://www.cnblogs.com/hellogiser/p/reservoir-sampling.html>

<http://www.geeksforgeeks.org/reservoir-sampling>

转载请注明出处：[{{ page.title }}]({{ page.url}})



