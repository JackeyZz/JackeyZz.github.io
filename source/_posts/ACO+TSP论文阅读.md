---
title: ACO+TSP论文阅读
date: 2018-04-11 19:47:40
tags: [ACO+TSP论文]
---

## 论文+1
[A New Ant Colony Optimization with Global Exploring Capability and Rapid Convergence](论文+1.pdf)
- 题目：一种具有全局搜索能力和快速收敛的蚁群算法
- 改进点：
  - 基于子路径排序的信息素更新策略(子路径指城市之间的连线)  
  信息素轨迹更新公式

  $$\tau_{ij}=(1-\rho) * \tau_{ij}+\rho * \triangle\tau_{ij}$$

  $$\triangle\tau_{ij}=\dfrac{1}{f_{best}}+\zeta$$

  - 在每次迭代之后，采用迭代最优和排序较优子路径相结合的方式更新信息素。在对最优路径的子路径释放信息素的同时，也对经过蚂蚁数量较多的子路径片段释放信息素。
  $$\zeta=Q\quad if\quad rank<=l$$
  - 随着解的不断进化，蚂蚁路径逐渐收敛，此时排序子路径信息素叠加机制会限制蚂蚁的随机搜索，因此提出自适应信息素更新机制不断减少叠加的信息素量，但需要保留叠加信息素提高算法收敛速度。$a$是参与计算的子路径数的比例因子。
  $$l=a * n-iteration$$

## 论文+2
[Adaptive Ant Colony Optimization Algorithm](论文+2.pdf)
- 题目：自适应性的蚁群优化算法
- 改进点：
  - 蚁群分为三类：正常蚁群(路径选择正比于信息素信息和启发信息)、异常蚁群(路径选择反比于信息素信息和启发信息)、随机蚁群(路径选择与信息素信息和启发信息无关)  
  状态转移公式:$r_k = 0.3$, $q$是[0,1]的随机数，$n_k$是禁忌表之外的城市总数。  

  异常蚁群的状态转移公式
  $$p_{ij}^k(t)=[1-\dfrac{(\tau_{ij}^\alpha(t)
  \eta_{ij}^{\beta})}{\sum_{r\notin{tabu_k}}(\tau_{ij}^\alpha(t)
  \eta_{ij}^{\beta})}]/(n_k-1)\quad if\quad q<r_k$$
  随机蚁群的状态转移公式
  $$p_{ij}^k(t)=\dfrac{1}{n_k}\quad if\quad q>1-r_k$$
  正常蚁群的状态转移公式
  $$p_{ij}^k(t)=\dfrac{(\tau_{ij}^\alpha(t)
  \eta_{ij}^{\beta})}{\sum_{r\notin{tabu_k}}(\tau_{ij}^\alpha(t)
  \eta_{ij}^{\beta})}\quad if\quad r_k<q<1-r_k$$  
  $$r_k(t+1)=(1-\delta)r_k$$
