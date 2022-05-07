## Chapter 1 Mechanism Design and Approximation



### 1.1 An Example: Congestion Control and Routing in Computer Networks 

首先考虑一个在计算机网络中的阻塞控制与路由的现实场景，网络中的用户使用电脑通过网络路由互联，这个网络中的任何一对路由器都可以通过网络链接连接，如果存在这样的网络链接，那么每个路由器都可以直接通过另一个路由器路由信息。假设网络是一个完全图同时网络连接传输的容量有限，则我们需要解决**阻塞控制**的问题：当需要传输的信息量超过容量时我们应该选择哪些，同时我们还是要处理一个**路由**的问题：每条信息选择哪一个路径的问题。本质上来说这就是一个资源分配的问题，我们先引入最简单的场景：单个物品的分配问题。

> **Definition 1.1.1.** The single item allocation problem is given by: (1). a single indivisible item available; (2). $n$ strategic agents competing for the item; (3). each agent $i$ has a value $v_i$ for receiving the item. 

目标时surplus最大化，获得该物品的agent的估值。当我们将物品分配给最高估值者时就达到了surplus的最大化。假设每个人的估值是公共知识，我们可以很容易给出一个分配规则。但是往往这是一个私人信息。一个想利用这种私人信息的机制必须征求agents的意见，我们考虑一个简单的机制如下：

1. 询问每个agent上报自己的value: $b_i$;
2. 选择出价最高的agent $i^\ast$，i.e. $i^\ast = \arg\max_i b_i$;
3. 将物品分配给$i^\ast$;

显然，如果想赢我们肯定尽可能高了报，所以这个结果就很不确定。有两个方式可以处理这种不可预测性，第一种，我们认为出价没有用，直接随机选winner；第二种我们可以对于出价高的agent进行惩罚，比如使用货币。

> **Definition 1.1.2.** The lottery mechanism is: (1). select a uniformly random agent; (2). allocate the item to this agent. 

我们很容易计算得到期望的surplus为$\frac{1}{n}\sum_{i} v_i$. 而最好的结果应该得到的是$v_{(1)}=\max_i v_i$. 考虑如果$v_1=1,v_i=\epsilon,(i\neq 1)$. 假设$\epsilon \to 0$. 我们发现lottery mechanism的结果是$\frac{v_{(1)}}{n}$. 因此最快情况这是一个$n$倍的近似OPT，同时我们也不难证明对于不带支付的机制而言，这个近似因子是所有机制中最好的结果。

**Theorem 1.1.1.** The surplus of the lottery mechanism is an $n-$approximation to the highest agent value. 

如果机制有可能收取费用且这种费用如果与代理人的出价成比例，可以阻止低价值的代理人出高价。这种动态分配和定价机制被称为拍卖。

> **Definition 1.1.3.** A single item auction is a solution to the single item allocation problem that solicits bids, picks a winner and determines payments. 

在政府采购拍卖中，往往采用的是一价拍卖：

1. 征询出价$b_i$;
2. 选择$i^\ast=\arg\max_{i} b_i$;
3. 将物品分配给$i^\ast$;
4. 要求winner支付他的报价$b_{i^\ast}$;

显然这种拍卖中会比较难分析，因为参与者会在报低价和害怕报的太低导致输掉之间纠结，这个结果预期很难说。

另一种拍卖的方式是我们平时常见的升价拍卖：

1. 从0开始提高一个offered price;
2. 如果agents认为这个offered price超过心理预期则允许中途退出;
3. 当只剩下一个agent还在情况下终止拍卖;
4. 将物品分配给唯一的agent同时收取终止拍卖时的offered price;

这种拍卖下，对于每个参与者来说玩法会比较简单，假设当前的agent对于这个item的估值为$v$，那么一个很好的策略就是当这个offered price超过$v$我们就退出。事实上，无论其他人怎么做，对于每个人自身而言这是一个好的策略，我们也将其称为**占优策略**。当所有的参与者都采取这种占优策略的情况下，我们会发现，当价格达到$v_{(2)}$的情况下，市场中只剩下$v_{(1)}$的agent，此时物品分配给该玩家，同时支付$v_{(2)}$的价格。

**Theorem 1.1.2.** The ascending-price auction maximizes the surplus in dominant strategy equilibrium. 

这种机制告诉我们一个很重要的道理，当agents的type是私人信息的情况下，带money的支付可以让资源配置的效率提高。

然而这种升价拍卖也存在一些弊端，首先这个运行的时间效率不高，其次他只能应对单物品的场景。

我们引入密封二价拍卖：

> **Definition 1.1.6.** The second price auction is: (1). accept sealed bids; (2). allocate the item to the agent with the highest bid; (3). charge this winning agent the second-highest bid.

**Theorem 1.1.3.** Truthful bidding is a dominant strategy in the second price auction. 

证明比较简单，略过。

**Corollary 1.1.4.** The second price auction maximizes the surplus in dominant strategy equilibrium. 

#### 1.1.1 Non-monetary payments 

我们考虑阻塞控制机制由于历史原因等不使用monetary transfers. 一种被考虑过且被用于一些相似的资源分配问题上的解决方案是**computational payments**. such as "proofs of work". 有了这样一个系统，agents可以通过让她的计算机执行一个大型的、可验证的、但在其他方面没有价值的计算任务来证明她的信息是高价值的。对于这种computational payment来说，这不是从winner到router的收益转化，而是一种社会收益的损失。

有计算性支付的机制的消费者剩余(**consumer surplus**)是产生的总价值减去任何支付。

对于单物品拍卖而言，消费者剩余指的是winner对于这个物品的价值减去其支付的值。对于二价拍卖，消费者剩余等于$v_{(1)}-v_{(2)}$, 对于lottery来说，消费者剩余就是$\frac{1}{n}\sum_i v_i$. 从消费者剩余的角度来考虑，无论是二价拍卖还是lottery的方式在考虑agent估值的情况下都无法做到最优。

- 第一种情况：$v_1=1,v_{i,i\neq 1}=\epsilon$, 如果我们令$\epsilon$趋近于0，则二价拍卖的消费者剩余为1，lottery的消费者剩余为$1/n$. 
- 第二种情况：$v_i=1,\forall i$. 二价拍卖的消费者剩余为0，lottery机制的消费者剩余为1.

对于上述的讨论而言，如果分别考虑surplus和consumer surplus，对于surplus最大化的问题，我们有二价拍卖可以达到最优，但是对于consumer surplus最优的问题而言，我们找不到这样的机制。由于不存在最优，我们考虑在这些指标之间存在一些trade-off. 

第一种方案：假设估值分布，然后计算期望consumer surplus；第二种方案：第二种方法从第一种方法的解决方案开始，要求有一个单一的机制，在最坏的情况下的分布中最好地接近最优机制。第二种方法可能对机制设计在计算机网络中的应用特别有用，因为它不可能改变路由协议以适应不断变化的流量工作负荷。

#### 1.1.2 Posted Pricing 

考虑最大化surplus的单物品分配问题，即使只有一轮，我们使用密封的二价拍卖都可能太过于复杂或者对于阻塞控制和routing都太慢了。下面我们考虑一个take-it-or-leave-it price. 

> **Definition 1.1.7** For a given price $\hat{v}$, the uniform-pricing mechanism serves the first agent willing to pay $\hat{v}$. (breaking ties in arrival order randomly)

假设我们考虑所有的agents在同一时刻到达同时我们设置$\hat{v}=0$, 则这个机制退化为lottery mechanism. 最坏的例子，当一个agent的估值为$1$同时剩余的$n-1$个agents的估值为$\epsilon$. 统一价格拍卖可以更加灵活，例如我们可以设置$\hat{v}=2\epsilon$, 这样的话只有最高估值的人会考虑购买商品。这样的一种固定价格机制还是很实用的。但是这个$\hat{v}$的选取需要注意。

在routing的场景下，我们大概$F(z)=P_{v\sim F}\{v < z\}$. For example the uniform-distribution on interval $[0,1]$ is denoted $U[0,1]$ its cumulative distribution function is $F(z)=z$. 

如何选择$\hat{v}$: 给出求解这个最优解的机制，我们可以尽可能多的仿真这个二价拍卖的流程。注意对于$n$个分布相同的参与者而言，我们认为事前的获胜概率均为$1/n$. 我们可以通过计算累积分布函数的反函数来求解$\hat{v}=F^{-1}(1-1/n)$. 对于均匀分布而言，我们可以计算得到$\hat{v} = 1- 1/n$. 

**Theorem 1.1.5** For values from independently and identically from any distribution $F$, the uniform pricing of $\hat{v}=F^{-1}(1-1/n)$ is an $\frac{e}{e-1}\approx 1.582$ approximation of the optimal surplus. 

**Proof**: 我们考虑对比三种不同的机制，定义$REF$表示为二价拍卖以及其社会福利，定义$APX$表示为统一价格拍卖以及其社会福利。对于二价拍卖而言$REF$在事后的供给约束条件下（我们最多分配出去一件item）可以达到社会福利的最大值，$REF$将会将物品分配给一个事前获胜概率为$1/n$的人。考虑对比第三个机制$UB$，它在每个agent被服务的事前概率最多为1/n的约束下实现surplus最大化，但没有供应约束，也就是说，$UB$可以选择多个获胜的agents.

首先我们可以考虑的是对于机制$UB$来说我们一定有surplus的比较$UB \geq REF$. 因为实际上UB可以选择像REF一样只分配一个，也可以分配多个，因为其不存在什么供给的约束，从而显然我们能够发现其社会福利的一定至少是REF的上界。

事实上对于UB而言，首先第一点：我们考虑UB的最优化是关于agents独立的；第二点：我们知道对于事前获胜概率为1/n的agent最优的方式就是使用统一价格$\hat{v}=F^{-1}(1-1/n)$. 我们希望计算UB的期望surplus. 定义$E[v\vert v\leq \hat{v}]$表示的是给一个agent估值超过统一价格的情况下的期望价值。对于UB而言这个agent的surplus的求解应该等于$E[v|v\geq \hat{v}]\cdot P\{v\geq \hat{v}\}$. 累计所有的参与者我们可以得到：
$$
UB = n\times E[v|v\geq \hat{v}]\times P\{v\geq \hat{v}\} = E[v|v\geq\hat{v}]
$$
至此我们可以通过将REF的上界与UB关联从而知道对于APX的surplus的下界。注意我们知道统一价格拍卖机制的价格是自己选择的，因此任何一个agent超过这个价格的概率一定为$1/n$. 我们考虑所有的参与者的估值都低于这个统一价格的概率等于所有人的估值都低于这个阈值的概率$(1-1/n)^n\leq 1/e$. 因此我们可以知道对于这个问题而言将物品卖出的概率至少是$1-1/e$. 而如果将这个item卖出，这个期望的估值至少是$\hat{v}$，因此统一价格的期望surplus:
$$
APX \geq (1-1/e)E[v|v\geq \hat{v}]
$$
我们结合前面的两个结论：$UB \geq REF$（UB是REF的上界）同时$UB = E[v\vert v\geq \hat{v}]$可以得到：$APX\geq (1-1/e)\cdot REF$. 

#### 1.1.3 General Routing Mechanism 

我们给出一个简单的用于阻塞控制和路由的机制，这个机制的主要想法如下：首先我们考虑关键估值这个概念，这个东西的构建帮助我们在二价拍卖中达到了IC性质，事实上，这个想法是可以扩展到任意的拍卖场景下的，因为我们知道对于任意的一个面对着关键报价的agent，这个agent支付的一定是这个关键的估值如果其估值超过这个值，否则的话他将成为一个loser. 

> **Definition 1.1.8** The second-price routing mechanism is:
>
> (1). Solicit sealed bids;
>
> (2). Find the set of messages that can be routed simultaneously with the largest cumulative bid;
>
> (3). charge the agents of each routed message their critical values. 

**Theorem 1.1.6** The second-price routing mechanism has truthful bidding as a dominant strategy. 

**Corollary 1.1.7** The second-price routing mechanism maximizes the surplus. 

观察定义1.1.8中的机制，其中第二步是在进行winner determination. 为了理解真实发生了什么，我们给出例子：例如，在互联网中信息在网络中的路线是由边界网关协议（BGP）预先确定的，它强制要求所有通过任何给定的路由器路由到同一目的地的信息遵循相同的路径。这里是不存在什么负载均衡问题的，因为传递到同一个终点的信息一定是通过同一路径过去的，而非是负载均衡要求的采取不同的路径同时使得网络的带宽压力最小。我们这里的考虑的是一种新型的网络，可以决定哪个信息去路由同时走哪一条路径。

这里有一个究极问题，我们应该如何找到价值最高的那些messages的子集来同时进行路由呢？我们的方案是遍历所有的子集在其中找到能够满足许多复杂约束条件的那些。这个问题很具有挑战性，这和"Disjoint Path Problem"是相关的：给定一个图中点对的集合，我们需要找到一个点对的子集来通过disjoint paths来连接。然而这个问题是一个NP-hard问题，找到最优解是不可能计算的。

### 1.2 Mechanism Design

A theory for mechanism design should satisfy the following four desiderata:

- **Informative**: It pinpoints salient features of the environment and characteristics of good mechanisms therein.
- **Prescriptive**: It gives concrete suggestions for how a good mechanism should be design.
- **Predictive**: The mechanisms that the theory predicts should be the same as the ones observed in practice.
- **Tractable**: The theory should not assume super-natural ability for the agents or designer to optimize. 

### 1.3 Approximation 

> **Definition 1.3.1** For an environment given implicitly, denote an approximation mechanism and its performance by APX, and a reference mechanism and its performance by REF. 

- 对于任意的环境，APX是一个关于REF的$\beta$近似如果APX $\geq 1/\beta$ REF. 
- 对于任意一类的环境而言，我们认为一类机制是$\beta$近似REF的如果对于任意的这类环境中的一个场景，存在一个机制APX是关于REF$\beta$近似的。
- 对于任意一类的环境而言，**一个**机制APX是$\beta$近似REF的如果对于这个机制而言APX在任意的环境中都是$\beta$近似REF的。

我们之前的论述中已经给出了两个例子：对于i.i.d的U[0,1]的n个agents的单物品环境，给出一个固定价格$\hat{v}=1-1/n$的统一价格机制是可以关于二价拍卖的$\frac{e}{e-1}$的近似解。更一般地情况下，对于任意的i.i.d的单物品拍卖场景，这个统一价格拍卖是一个$\frac{e}{e-1}$的关于二价拍卖的近似。另外，对于任意的单物品拍卖的环境下，lottery机制给出了一个$1/n$倍的二价拍卖社会福利的近似解。

#### 1.3.1 Philosophy of Approximation 

对于近似解，首先我们可以考虑在理想环境下有一些机制确实可以达到最优，但是通过近似我们可以放宽这种机制的有效性，在更广的范围下达到近似最优的结果。在本书的chapter4和chapter8中，作者将会描述关于保留价机制以及固定出价的近似最优性质。

近似解提供了很好的地方是我们可以针对某个环境或者机制探索其突出的特点：假定我们希望决定某些机制的性质是否重要：通过近似，如果存在一些机制不带有那些特征同时获得了良好的近似，那么大概率这些特征不是很重要。例如我们之前提到的如果机制不含钱，我们的机制无法获得一个超过线性近似的最优解（单物品场景下）这恰恰说明了钱是在这类机制设计中十分重要的东西。换一个方向，我们也看到了对于统一价格拍卖，我们可以得到$\frac{e}{e-1}$的近似解。这说明固定价格没有充分利用agents之间的竞争关系，因此我们可以总结在这种场景下agents之间的竞争是不太重要的。

假设一个物品的卖家对于合谋，风险态度，市场后效应等等经济现象有很大的担忧，而这些现象往往不在我们机制的考虑范围之内，一个选择是我们对这些现象进行建模同时找到对应的最优的机制。另一方面我们也可以找一些近似的算法机制。事实上在一些现实场景的约束下，最优的机制往往在一些方面不如近似机制。比如说考虑统一价格拍卖的问题，我们可以得到近似的最优解，这种方式拍卖来的更加方便，计算复杂度也越低，在实际的网络拍卖中是比较实用的。

#### 1.3.2 Approximation Factors 

根据问题的不同以及机制的不同，近似的因子可以从$(1+\epsilon)$任意形式的close approximation到线性因子的近似甚至更差。注意到一个线性因子的近似，随着参数的数量的增加，也就是说更多的参与者，更多的资源的场景下，这个近似的结果往往会变得更差。就像我们之前说过的，统一价格拍卖的近似是常数近似，而lottery机制的近似是线性近似，随着人越来越多，结果会越来越差。

在这里我们使用常数近似和非常数近似来区分好的近似算法和不好的近似算法。对于机制设计而言，非常数的近似往往会带来不好的结果: 

- a bad mechanism.
- failure of approximately characterize optimal mechanisms.
- an imposition of incompatible modeling assumptions or constraints. 

对于常数的近似我们强调几点

- 一个2倍近似的结果可以是informative的，这其实可以告诉我们的是我们往往可以达到更好的近似比。
- 一个近似因子为2的近似在很多环境下都是成立的，在一些具体的场景下其会更加优秀。
- 在许多情况下，最优机制的可解释性不是那么的好。
- 那些在理论上达到2倍近似的机制往往在实际中的效果更好一些。

更好的像是$(1+\epsilon)$的近似比往往是通过下面的一些方式找到的：

- identifying a restricted class of mechanisms wherein a near-opt mechanism can be found. 
- conducting a BF search over the restricted class. 
