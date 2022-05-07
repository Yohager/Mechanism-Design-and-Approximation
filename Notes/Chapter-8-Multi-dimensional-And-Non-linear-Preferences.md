## Chapter 8 Multi-dimensional and Non-linear Preferences

---

chapter 2-7 都是在单维度上讨论agent的线性偏好，也就是说每个获胜的玩家的收益等于他的估值减去其付出的价格。很多时候agents的偏好往往是多维度的且非线性的，有很多这样的例子：

1. 多物品拍卖场景，对于每个物品的估值不相同；
2. agents收到经济约束，其收益等于其估值减去价格，这个价格最高就是其预算的值（考虑这个预算是私人信息的情况下）；
3. agents不是风险中性的情况，这种情况下agent的收益建模为一个关于估值的concave function再减去其需要支付的价格；

就像是对于单维线性的agents的而言，对于多维度的线性agents来说，我们知道其中存在一个DSIC的机制能最大化社会福利。这个机制不依赖于关于agents的types的先验分布。对于非线性收益的agents或者对于收益最大化的目标而言，基于先验分布的最优机制的求解会变得相当复杂。

由多维度非线性偏好带来的挑战分为三个方面：

- 多维types的空间相当庞大，即使是面对单个agent的场景都可能非常复杂(example in section 3.4 on page 79)。
- 非线性的场景下的agents的收益不再具有revenue-linearity条件（事实上这个性质在线性多维条件下也往往不满足）。
- 存在多维agents的环境下往往有多个items，因此当一个agent获得其中一个item的时候，他对于其他的agents施加的外部性往往也是多维的。

### 8.1 Social Surplus 

在section 3.2中给出了单维线性的agents的社会福利最大化的机制可以扩展到多维线性的场景。

考虑设计一个机制从outcomes的集合$\mathcal{X}$中选择一个$x$同时每个agent的非负收益$p\in \mathbb{R}^n_{+}$. 定义收益函数如下：

> **Definition 8.1.1** A utility function $u_i:\mathcal{X}\times \mathbb{R}_{+}\to \mathbb{R}$ is quasi-linear if it is linear in payments, i.e. $u_i(x,p_i)=u_i(x)+p_i$ where $u_i(x)=u_i(x,0)$ is short hand for the utility of the agent with no payment.

拟线性不代表收益函数一定是线性的，考虑当我们分配一个可划分的物品时，拟线性允许agents的收益是一个关于资源分配划分比例的非线性函数，在之前的场景中我们往往认为对于一个人要么是分配到一个物品要么是啥都没有，因此在那种情况下我们知道拟线性和风险中性导出的是一个线性的收益函数。

**Example 8.1.1**: An important example environment for multi-dimensional mechanism design is that of combinatorial auctions. In a combinatorial auction there are $m$ items $\{1,2,\cdots,m\}$ and the feasible $\mathcal{X}$ correspond to allocations that partitioning the items into $n+1$ sets, one set for each agent and a set of unallocated items. The utility of an agent is a function only of the set of items she received and her payment. 

我们把单物品下的externality pricing mechanism迁移过来：

> **Definition 8.1.2.** The externality pricing mechanism for surplus maximization with quasi-linear agent is:
>
> (1). Solicit and accpet sealed bdis $b$ with $b_i:\mathcal{X}\to \mathbb{R}$;
>
> (2). Find the optimal outcome $x\leftarrow OPT(b)$;
>
> (3). Set prices $p$ as $p_i\leftarrow OPT(0,b_{-i})-OPT_{-i}(b)$ where a bid of $b_i=0$ denotes the constant function $b_i(x)=0$ for all outcomes $x$. 

我们这里理解一下$p_i\leftarrow OPT(0,b_{-i})=OPT_{-i}(b)$，大概理解为VCG机制的形式，前项表示$i$不在这个市场中的结果，后项表示$i$在市场中的OPT结果减去$i$的surplus的结果。

### 8.2 Optimal Single-agent Mechanisms

考虑一个场景：一个agent $t\in \mathcal{T}$ according to a distribution $F$. 一个机制导出了一个结果$w\in\mathcal{W}$. 

最简单的机制，考虑使用taxtation principle，对于这个agent，我们给出一个outcomes的menu让其从中选取一个认为最喜欢的结果，The menu representation of a mechanism with outcome rule $w(\cdot)$ is:
$$
\mathcal{M}=\{w(t):t\in \mathcal{T}\}
$$
为了满足IC性质和IR性质显然我们的机制应该满足：
$$
\begin{split}
&u(t,w(t)) \geq u(t,w(s)),&\forall t,s\in \mathcal{T}\\
&u(t,w(t))\geq 0, &\forall t\in \mathcal{T}
\end{split}
$$
接下来我们会讨论两种不同场景下的例子：

- a single-dimensional agent with a public budget(a non-linear preference);
- a multi-dimensional unit demand agent with linear utility given by her value for the alternative obtained minus her payment.

> **Defintion 8.2.1.** A public budget agent has a single-dimensional value $t$ for service and a public budget $B$. Her utility is linear for outcomes with required payment that is within her budget, and infinitely negative for outcomes with payments that exceed her budget. For outcome $w=(x,p)$, where $x$ denotes the probability she is allocated and $p$ is her required payment, her utility is $u(t,w)=tx-p$ where $p\leq B$(and $-\infty$, otherwise).

第一种场景，一个参与者其type为$t$, 有一个公共预算$B$, 如果支付的值没有超过预算，则收益为$tx-p$, 否则其收益值认为是负无穷。

>**Definition 8.2.2.** A unit-demand agent desires one of  alternatives. Her type $t$ is $t=(\{t\}_1,\cdots,\{t\}_m)$ $m$-dimensional where $\{t\}_j$ is her value for alternative $j$. Her utility is linear, an outcome $w$ is given by a payment and a probability measure over the  alternatives and nothing. For outcome $w=(\{x\}_1,\cdots,\{x\}_m,p)$ where $\{x\}_j$ denotes the probability she obtains alternative $j$ and $p$ is her required payment, her utility is $u(t,w)=\sum_j\{t\}_j\{x\}_j-p$. The probability she receives any allocation is $x=\sum_j\{x\}_j$. 

第二种场景，一个参与者，type是一个关于商品多维的type，每一维都是关于单个商品的估值，在这个估值之上对应的outcome有一个对于每个物品获胜的概率。

对于single-agent的问题我们考虑三种不同的场景：(1). 没有限制的single-agent问题；(2). 事前约束的single-agent问题；(3). 事中约束的single-agent问题。

这三种场景对应的收益的分别为：$R(1),R(\hat{q}),Rev[\hat{y}]$. 

- Unconstrained: 任意的一个机制都是可行的，optimal unconstrained mechanism's revenue is denote $R(1)$. The monopoly quantile $\hat{q}^\ast$ is defined to be its ex-ante sale probability.

- (Weak) ex-ante constrained: for ex-ante constraint $\hat{q}$, a mechanism is feasible if its ex-ante allocation probability is at most $\hat{q}$. i.e. $\mathbb{E}_{t\sim F}[x(t)]\leq \hat{q}$. The optimal $\hat{q}$ ex-ante mechanism's revenue is denote $R(\hat{q})$.

- (Weak) ex-interim constrained: for interim constraint $\hat{y}$ (单调非递减的函数从[0,1]映射到[0,1]), a mechanism $\mathcal{M}$ is feasible if for any subspace of types $\mathcal{S}\subset \mathcal{T}$ with $\hat{q}=P[t\in S]$ under $\mathcal{M}$ is at most that of a quantile $q\in [0,\hat{q}]$ under constraint $\hat{y}$. In other words, the allocation constraint $\hat{y}$ is satisfied if for type distribution $F$, quantile $q\sim U[0,1]$, and subspace of types $S$ with $P_{t\sim F}[t\in S]=\hat{q}$. 
  $$
  \mathbb{E}[x(t)\vert t\in S] \leq \mathbb{E}[\hat{y}(q)\vert q\leq \hat{q}]
  $$

#### 8.2.1 Public Budget Preferences 

**Example 8.2.1.** 假设单个agent的private type $t$ 服从的分布为$U[0,1]$ 同时其公共预算为$\frac{1}{4}$. 

我们考虑可以带入allocation的约束条件，比如说下面的条件事中概率约束$\hat{q}=\frac{1}{2}$，另外保证预算约束条件。

- A $\frac{3}{4}$ lottery at price $\frac{1}{4}$ is $\mathcal{M}=\{(\frac{3}{4},\frac{1}{4})\}$. The utility of an agent with type $t$ for this lottery pricing is $\frac{3}{4}t-\frac{1}{4}$ and thus type $t\in [\frac{1}{3},1]$ buy. The ex-ante sale probability is $\frac{3}{4}\times \frac{2}{3}=\hat{q}=\frac{1}{2}$. 这里满足我们定下的事中的概率约束，从而我们可以给出这个分配规则：
  $$
  x(t) = 
  \begin{cases}
  0 & t\leq \frac{1}{3}\\
  \frac{3}{4} & \text{otherwise}
  \end{cases}
  $$

- In a two-agent all-pay auction, types $t\in [\frac{1}{2},1]$ bid the budget $B=\frac{1}{4}$. 

  