##Dynamic Programming Introduction

> **定义:** 利用小问题来解决大问题，记忆化搜索  
> 
> [1]**与Divide & Conquer区别：**
	DP的子问题可以有交集，分治的子问题完全独立
	与递归的区别
	
        动归                    分治  
        node                   node
        /   \                  /   \
     node   node             node  node
     /   \   /  \           /  \    /  \
    node  node  node    node node node node
   
> [2] **与DFS的区别：**
	DFS是一种实现方法approach
	DP是一种解题思想
	DP可以用DFS来实现也可以用loop来实现

**DP 实质上时搜索加上记忆化的功能, 因为每个(x,y)只计算一次，所以总的时间复杂度为O(n^2)**
-	大状态
	-	起点 -> 终点
-	小状态
	-	(x, y)  -> (x + 1, y + 1) 
-	大状态依赖小状态的计算结果

**实现方式有两种：DFS vs Loop**
-	Loop: f(x, y)从哪个点来？ f(x - 1, y) || f(x, y - 1)
-	DFS: f(x, y)到哪个点去？f(x + 1, y + 1)


**实现方式有两种：DFS vs Loop 既 两种状态转移思想**
-	Loop: f(x, y)从哪个点来？ f(x - 1, y) || f(x, y - 1)
-	DFS: f(x, y)到哪个点去？f(x + 1, y + 1)


**DP 的四点要素**
-	State: (x, y) 状态代表什么？**需要灵感**
-	Function: f(x, y)的状态转移方程
-	Initialization: 初始值 (a.k.a 起点)
-	Answer: 结果 (a.k.a 重点)

**什么问题要用DP解决？**
-	**When you see string problem that is about subsequence or matching, DP should come to your mind naturally!**
-	[1] Find Max/Min 找最大值/最小值
-	[2] Decide if there is a way ? Yes/No 是否存在一种可能
-	[3] Count all possible ways 求方案总数
-	[4] 待解 数组/字符串/Matrix **不能sort/swap**

**什么问题不能用DP解决？**
-	[1] 找出所有方案 All Solution 一定是DFS，一定不是DP
-	[2] 如果 待解 数组 交换Swap两个数对结果没影响，一定不是DP
	-	e.g. [100, 3, 1, 2, 7] longest consecutive subset?
	-	answer: [1, 2, 3] 如果交换［3， 2］对结果无影响
-	[3] Matrix **DP只能向左向下走**, 若上下左右都可以行 一定不是DP
-	因为DP利用重复计算的结果，[2][3]会造成闭合回路(a.k.a 环)