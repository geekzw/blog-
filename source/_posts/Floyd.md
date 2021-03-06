---
title: Floyd
date: 2016-12-09 12:36:29
categories: algorithm
tags: algorithm
description: "Floyd-Warshall算法（Floyd-Warshall algorithm）是解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权的最短路径问题"
---
## Floyd概述
Floyd-Warshall算法（Floyd-Warshall algorithm）是解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权的最短路径问题
## 分析
floyd算法里面实际用了动态规划的思想。我们先想想动态规划思想。
* 目标问题可以划分成众多相同的子问题
* 子问题具有重叠性，子问题的解对进一步求目标问题有帮助
那么现在再来思考下floyd是怎么做的。假如求u到v的最短路径。第一种方法是求u到v的直接路径。第二种是间接路径，就是说可以通过其他路径例如i到v，这样最终路径就是（u,i）+(i,v)。这情况就多了，因为i可以是其他任意一条路径，也可以多条路径。这个时候就需要划分问题了。我们可以先算出经过1个点的路径，再算出经过2个点的路径。。。。。。下面就通过图，具体分析一个问题
![floyd][1]
如图，我们第一步要做的，就是上面的第一种情况，先无脑的保存任意两点的距离，不直接相连的，就是无穷大。比如1->2 = 2;1->3 = 6;1->5 = 无穷大。这是直接相连情况。如果可以经过中间点，那么问题立马就复杂了（因为中间节点可以是任意一个，也可以是多个）。这个时候就拆分子问题。例如，我开始只让经过2号节点，那么现在就可以计算出，任意两点相连，是直接连短，还是经过2转折一下短。比如1->3。
原来最短距离就是直接相连为6，那么如果可以经过2，就变成了(1,2)+(2,3)和(1,3)这两条路了，显然经过2节点这条路更近一些，那么就更新(1,3)的距离为5。这里就是dp思想中的划分子问题，求出子问题的解。当我们再开放3节点的时候。也就是可以经过3这个节点。原来1->5距离是无穷大，因为没有直接相连，通过2也不能到达。现在放开3节点了。那么我们就可以通过(1,3,5)和(1,2,3,5)两条路到达5了，那么哪条更近啊，显然是(1,2,3,5)啊，因为里面公共部分是(3,5)，前面有分析过了(1,2,3)比(1,3)这条路近，所以(1,5)的距离就可以更新为(1,2,3,5)这条路的距离了。这里发现没，我们用到的还是dp思想，前面是说划分子问题，然后扩大规模。现在是子问题的求解对目标问题有帮助，前一个子问题计算出了(1,3)的最短距离，现在就可以拿来用了。否则，还是要比较一遍(1,2,3)和(1,3)谁短，这就是dp说的子问题的重叠。后面的就是老套路了，不停的开放新节点，计算加入新节点后的两点间的最短路。其实说是解释floyd，倒是把dp解释了一遍，dp说完，floyd算法自然也就有了。下面给出核心算法，简单到吓你一跳
```java
for(k=1;k<=n;k++)//新开允许路过的节点 
    for(i=1;i<=n;i++)//新开节点后任意节点A 
        for(j=1;j<=n;j++)//新开节点后任意节点B
            if(e[i][j]>e[i][k]+e[k][j])//判断通过新开节点(A,B)距离会不会变短  
            e[i][j]=e[i][k]+e[k][j]; 
```


  [1]: http://ofy9dm2ii.bkt.clouddn.com/image/article/floyd.png