# LeetCode
记录自己的刷题之路，贵在坚。

#### Table of contents
(toc generated by [ghtoc](https://github.com/sk1418/ghtoc))
- [LeetCode](#LeetCode)
    - [1 前缀和与差分](#1-前缀和与差分)
        - [1.1 简介](#1.1-简介)
        - [1.2 典型题目](#1.2-典型题目)
    - [2 双指针](#2-双指针)
        - [2.1 简介](#2.1-简介)
        - [2.2 典型题目](#2.2-典型题目)
    - [3 并查集](#3-并查集)
        - [3.1 简介](#3.1-简介)
        - [3.2 典型题目](#3.2-典型题目)
    - [4 单调栈与单调队列](#4-单调栈与单调队列)
        - [4.1 简介](#4.1-简介)
        - [4.2 典型题目](#4.2-典型题目)
    - [5 二叉树](#5-二叉树)
        - [5.1 简介](#5.1-简介)
        - [5.2 典型题目](#5.2-典型题目)
    - [6 DFS 与 BFS](#6-DFS-与-BFS)
        - [6.1 简介](#6.1-简介)
        - [6.2 典型题目](#6.2-典型题目)
    - [7 动态规划](#7-动态规划)
        - [7.1 简介](#7.1-简介)
        - [7.2 典型题目](#7.2-典型题目)
    - [8 贪心算法](#8-贪心算法)
        - [8.1 简介](#8.1-简介)
        - [8.2 典型题目](#8.2-典型题目)
    - [9 拓扑排序](#9-拓扑排序)
        - [9.1 简介](#9.1-简介)
        - [9.2 典型题目](#9.2-典型题目)
    - [10 字典树](#10-字典树)
        - [10.1 简介](#10.1-简介)
        - [10.2 典型题目](#10.2-典型题目)

## 1 前缀和与差分

### 1.1 简介



### 1.2 典型题目



## 2 双指针

### 2.1 简介



### 2.2 典型题目

 [15_三数之和](https://leetcode-cn.com/problems/3sum/)



## 3 并查集

### 3.1 简介
并查集听着比较高端，其实道理很简单。参考某高手的blog。通过读完这个故事，加上几个练习，你也能轻松了解这个名词，吹吹逼了，哈哈~

故事如下：

江湖上散落着各式各样的大侠，有上千个之多。他们没有什么正当职业，整天背着剑在外面走来走去，碰到和自己不是一路人的，就免不了要打一架。但大侠们有一个优点就是讲义气，绝对不打自己的朋友。而且他们信奉“朋友的朋友就是我的朋友”，只要是能通过朋友关系串联起来的，不管拐了多少个弯，都认为是自己人。这样一来，江湖上就形成了一个一个的帮派，通过两两之间的朋友关系串联起来。而不在同一个帮派的人，无论如何都无法通过朋友关系连起来，于是就可以放心往死了打。但是两个原本互不相识的人，如何判断是否属于一个朋友圈呢？

我们可以在每个朋友圈内推举出一个比较有名望的人，作为该圈子的代表人物。这样，每个圈子就可以这样命名“中国同胞队”美国同胞队”……两人只要互相对一下自己的队长是不是同一个人，就可以确定敌友关系了。

但是还有问题啊，大侠们只知道自己直接的朋友是谁，很多人压根就不认识队长要判断自己的队长是谁，只能漫无目的的通过朋友的朋友关系问下去：“你是不是队长？你是不是队长？”这样，想打一架得先问个几十年，饿都饿死了，受不了。这样一来，队长面子上也挂不住了，不仅效率太低，还有可能陷入无限循环中。于是队长下令，重新组队。队内所有人实行分等级制度，形成树状结构，我队长就是根节点，下面分别是二级队员、三级队员。每个人只要记住自己的上级是谁就行了。遇到判断敌友的时候，只要一层层向上问，直到最高层，就可以在短时间内确定队长是谁了。由于我们关心的只是两个人之间是否是一个帮派的，至于他们是如何通过朋友关系相关联的，以及每个圈子内部的结构是怎样的，甚至队长是谁，都不重要了。所以我们可以放任队长随意重新组队，只要不搞错敌友关系就好了。于是，门派产生了。

![image-20200618001347750](C:\Users\Fan\AppData\Roaming\Typora\typora-user-images\image-20200618001347750.png)

下面我们来看并查集的实现。 int pre[1000]; 这个数组，记录了每个大侠的上级是谁。大侠们从1或者0开始编号（依据题意而定），pre[15]=3就表示15号大侠的上级是3号大侠。如果一个人的上级就是他自己，那说明他就是掌门人了，查找到此为止。也有孤家寡人自成一派的，比如欧阳锋，那么他的上级就是他自己。每个人都只认自己的上级。比如胡青牛同学只知道自己的上级是杨左使。张无忌是谁？不认识！要想知道自己的掌门是谁，只能一级级查上去。 

find这个函数就是找掌门用的，意义再清楚不过了（路径压缩算法先不论，后面再说）。

```c
int unionsearch(int root) //查找根结点
{
	int son, tmp;
	son = root;
	while(root != pre[root]) //我的上级不是掌门
		root = pre[root];
	while(son != root) //我就找他的上级，直到掌门出现
	{
		tmp = pre[son];
		pre[son] = root;
		son = tmp;
	}
	return root; //掌门驾到~~
}
```

再来看看join函数，就是在两个点之间连一条线，这样一来，原先它们所在的两个板块的所有点就都可以互通了。这在图上很好办，画条线就行了。但我们现在是用并查集来描述武林中的状况的，一共只有一个pre[]数组，该如何实现呢？ 还是举江湖的例子，假设现在武林中的形势如图所示。虚竹帅锅与周芷若MM是我非常喜欢的两个人物，他们的终极boss分别是玄慈方丈和灭绝师太，那明显就是两个阵营了。我不希望他们互相打架，就对他俩说：“你们两位拉拉勾，做好朋友吧。”他们看在我的面子上，同意了。这一同意可非同小可，整个少林和峨眉派的人就不能打架了。这么重大的变化，可如何实现呀，要改动多少地方？其实非常简单，我对玄慈方丈说：“大师，麻烦你把你的上级改为灭绝师太吧。这样一来，两派原先的所有人员的终极boss都是师太，那还打个球啊！![大笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif)反正我们关心的只是连通性，门派内部的结构不要紧的。”玄慈一听肯定火大了：“我靠，凭什么是我变成她手下呀，怎么不反过来？我抗议！”于是，两人相约一战，杀的是天昏地暗，风云为之变色啊，但是啊，这场战争终究会有胜负，胜者为王。弱者就被吞并了。反正谁加入谁效果是一样的，门派就由两个变成一个了。这段函数的意思明白了吧？

```c
void join(int root1, int root2) //虚竹和周芷若做朋友
{
	int x, y;
	x = unionsearch(root1);//我老大是玄慈
	y = unionsearch(root2);//我老大是灭绝
	if(x != y) 
		pre[x] = y; //打一仗，谁赢就当对方老大
}
```

再来看看路径压缩算法。建立门派的过程是用join函数两个人两个人地连接起来的，谁当谁的手下完全随机。最后的树状结构会变成什么样，我也无法预知，一字长蛇阵也有可能。这样查找的效率就会比较低下。最理想的情况就是所有人的直接上级都是掌门，一共就两级结构，只要找一次就找到掌门了。哪怕不能完全做到，也最好尽量接近。这样就产生了路径压缩算法。

 设想这样一个场景：两个互不相识的大侠碰面了，想知道能不能干一场。 于是赶紧打电话问自己的上级：“你是不是掌门？” 上级说：“我不是呀，我的上级是谁谁谁，你问问他看看。” 一路问下去，原来两人的最终boss都是东厂曹公公。 “哎呀呀，原来是自己人，有礼有礼，在下三营六组白面葫芦娃!” “幸会幸会，在下九营十八组仙子狗尾巴花！” 两人高高兴兴地手拉手喝酒去了。 “等等等等，两位大侠请留步，还有事情没完成呢！”我叫住他俩。 “哦，对了，还要做路径压缩。”两人醒悟。 白面葫芦娃打电话给他的上级六组长：“组长啊，我查过了，其实偶们的掌门是曹公公。不如偶们一起结拜在曹公公手下吧，省得级别太低，以后查找掌门麻烦。” “唔，有道理。” 白面葫芦娃接着打电话给刚才拜访过的三营长……仙子狗尾巴花也做了同样的事情。 这样，查询中所有涉及到的人物都聚集在曹公公的直接领导下。每次查询都做了优化处理，所以整个门派树的层数都会维持在比较低的水平上。路径压缩的代码，看得懂很好，看不懂可以自己模拟一下，很简单的一个递归而已。总之它所实现的功能就是这么个意思。

![image-20200618001537138](C:\Users\Fan\AppData\Roaming\Typora\typora-user-images\image-20200618001537138.png)

于是，问题圆满解决。。。。。。。。。

代码如下：

```c
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
int pre[1010]; //里面全是掌门
 
int unionsearch(int root)
{
	int son, tmp;
	son = root;
	while(root != pre[root]) //寻找掌门ing……
		root = pre[root];
	while(son != root) //路径压缩
	{
		tmp = pre[son];
		pre[son] = root;
		son = tmp;
	}
	return root; //掌门驾到~
}
 
int main()
{
	int num, road, total, i, start, end, root1, root2;
	while(scanf("%d%d", &num, &road) && num)
	{
		total = num - 1; //共num-1个门派
		for(i = 1; i <= num; ++i) //每条路都是掌门
			pre[i] = i;
		while(road--)
		{
			scanf("%d%d", &start, &end); //他俩要结拜
			root1 = unionsearch(start);
			root2 = unionsearch(end);
			if(root1 != root2) //掌门不同？踢馆！~
			{
				pre[root1] = root2;
				total--; //门派少一个，敌人（要建的路）就少一个
			}
		}
		printf("%d\n", total);//天下局势：还剩几个门派
	}
	return 0;
}
```

参考链接：https://blog.csdn.net/qq_41593380/article/details/81146850

### 3.2 典型题目

 [200_岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)



## 4 单调栈与单调队列

### 4.1 简介



### 4.2 典型题目



## 5 二叉树

### 5.1 简介



### 5.2 典型题目

[98_验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

[101_对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

[102_二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

[104_ 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

[105_从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

[108_将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

[236_二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

[面试题17.12. BiNode](https://leetcode-cn.com/problems/binode-lcci/)



## 6 DFS 与 BFS

### 6.1 简介



### 6.2 典型题目

 [200_岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)



## 7 动态规划

### 7.1 简介



### 7.2 典型题目



## 8 贪心算法

### 8.1 简介



### 8.2 典型题目

[452_用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)



## 9 拓扑排序

### 9.1 简介



### 9.2 典型题目

[210_课程表Ⅱ](https://leetcode-cn.com/problems/course-schedule-ii/)



## 10 字典树

### 10.1 简介



### 10.2 典型题目




