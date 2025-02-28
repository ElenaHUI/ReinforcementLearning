# Basic Concepts 基础概念

- State：状态信息

- State Space：状态空间，所有s的集合

- Action：每个状态下可采取的行动

- State transition：执行动作后，智能体可能从一个状态到另一个状态。定义了和环境的交互信息

- Forbidden area：无法进入或者进入之后会得到惩罚

- State transition probability：用条件概率来表示随机性的状态转移

- **Policy：**$$\pi$$ 策略，告诉智能体在哪个状态下该做出什么动作。用条件概率表示，总和为1 
- **Reward：**采取动作后取得的数字，正数代表奖励，负数代表惩罚。是一个人机交互的手段，可以引导智能体的行为。用条件概率表示，reward取决于目前的状态和当前的行动，和下一个状态无关
- **trajectory：**state-action-reward chain
- **return：**回报， 沿着trajectory所得到的reward总和

![image-20250228100446443](C:\Users\18035\AppData\Roaming\Typora\typora-user-images\image-20250228100446443.png)

- discounted return：为了解决奖励 无法终止的问题引入，$$\gamma$$越大越远视，越小越近视

![image-20250228100734484](C:\Users\18035\AppData\Roaming\Typora\typora-user-images\image-20250228100734484.png)

 

- episode：一次尝试，最终会到达 terminal state 。没有terminal state的程序叫做continuing tasks 。实际上把两种情况同等看待，即把episode转化为continuing tasks：1.把target state 设置为absorbing state 2.仍认为是普通状态

- MDP（markov decision process）框架

  Key elements of MDP:
  • Sets:
  • State: the set of states $S$
  • Action: the set of actions $A(s)$ is associated for state $s ∈ S$.
  • Reward: the set of rewards $R(s, a)$.
  • Probability distribution (or called system model):
  • State transition probability: at state $s$, taking action $a$, the probability to
  transit to state $s′$ is $p(s′|s, a)$
  • Reward probability: at state s, taking action a, the probability to get
  reward $$r$$ is $$p(r|s, a)$$
  • Policy: at state s, the probability to choose action a is $$π(a|s)$$
  • **Markov property: memoryless property**无记忆性的

  $$p(st+1|at, st, . . . , a0, s0) = p(st+1|at, st),\\
  p(rt+1|at, st, . . . , a0, s0) = p(rt+1|at, st).$$
  All the concepts introduced in this lecture can be put in the framework in MDP.

  当policy确定时就变成了Markov process

# Bellman Equation 贝尔曼公式

## State Value

The expectation 期望 (or called expected value or mean) of $G_t$(discounted return) is defined as the state-value function or simply state value:
$$ v_π (s) = E[G_t|S_t = s] $$

## Bellman Equation

### 1. Bellman方程的基本形式

Bellman公式的基本形式可以用以下方程表示：

推导过程暂略

![image-20250228114559630](C:\Users\18035\AppData\Roaming\Typora\typora-user-images\image-20250228114559630.png)

#### 对于状态值函数 $V(s)$：
$$
V(s) = \max_a \left( R(s, a) + \gamma \sum_{s'} P(s'|s, a) V(s') \right)
$$

- $V(s)$：表示从状态 $s$ 开始，智能体能够获得的期望回报。
- $a$：在状态 $s$ 下采取的动作。
- $R(s, a)$：从状态 $s$ 采取动作 $a$ 后获得的即时奖励。
- $\gamma$：折扣因子（$0 \leq \gamma < 1$），表示未来奖励的重要性。
- $P(s'|s, a)$：状态转移概率，表示从状态 $s$ 采取动作 $a$ 后转移到状态 $s'$ 的概率。
- $s'$：可能的下一状态。

#### 对于动作值函数 $Q(s, a)$：
$$
Q(s, a) = R(s, a) + \gamma \sum_{s'} P(s'|s, a) \max_{a'} Q(s', a')
$$
- $Q(s, a)$：表示从状态 $s$ 开始，采取动作 $a$ 后，能获得的最大期望回报。

### 2. Bellman方程的应用

Bellman公式在强化学习中的核心作用是帮助智能体估计一个状态或状态-动作对的“价值”。通过不断地更新和计算状态值函数或动作值函数，智能体可以逐步改进其决策策略，以实现最大化累积回报。

#### 动态规划中的应用：
在动态规划（例如价值迭代或策略迭代）中，Bellman公式用来迭代地更新每个状态的价值，直到收敛为止。

#### Q-learning中的应用：
在Q-learning等强化学习算法中，Bellman公式也用于更新动作值函数，从而在没有模型的情况下学习最优策略。

### 3. Bellman方程的直观理解

- **即时奖励** $R(s, a)$：智能体在当前状态下采取某个动作后所获得的奖励。
- **未来奖励的折扣**：由于未来奖励的不确定性，我们将未来的奖励乘以折扣因子 $\gamma$ 进行折算。
- **最优策略的选择**：Bellman方程中的最大值操作（$\max_a$）表示智能体总是选择一个能够最大化当前状态下期望回报的动作。

通过不断地执行这种递归计算，智能体能够逐步逼近最优策略，使得其在长期内获得最大化的总奖励。

来评估策略的优劣，帮助智能体在复杂的决策过程中做出最优选择。