# Blackjack-RL
Blackjack optimal strategy finder with reinforcement learning and other tools

## Possible DeepRL Methods (SARSA vs Q-Learning)

#### When to choose SARSA vs. Q-Learning (stack exchange question)
* https://stats.stackexchange.com/questions/326788/when-to-choose-sarsa-vs-q-learning
* https://stats.stackexchange.com/questions/361485/why-update-sarsa-with-sa-at-all-if-the-goal-is-a-less-aggressive-exploitation

Q: SARSA and Q Learning are both reinforcement learning algorithms that work in a similar way. The most striking difference is that SARSA is on policy while Q Learning is off policy. The update rules are as follows:

Q-Learning:
$$
    Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha [ R_{t+1}+\gamma \max_{a}Q(s_{t+1},a)-Q(s_t,a_t) ]
$$
SARSA (state-action-reward-state-action):
$$
    Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha [ R_{t+1}+\gamma Q(s_{t+1},a_{t+1})-Q(s_t,a_t) ]
$$
where $s_t,a_t$ and $R_t$ are state, action and reward at time step $t$ and $\gamma$ is a discount factor.

A: Actually in both you "take" the actual single generated action $a_{t+1}$ next. In Q learning, you update the estimate from the maximum estimate of possible next actions, regardless of which action you took. Whilst in SARSA, you update estimates based on and take the same action. 

This is probably what you meant by "take" in the question, but in the literature, taking an action means that it becomes the value of e.g. $a_{t}$, and influences $r_{t+1}, s_{t+1}$.

Q-learning has the following advantages and disadvantages compared to SARSA:

* Q-learning directly learns the optimal policy, whilst SARSA learns a near-optimal policy whilst exploring. If you want to learn an optimal policy using SARSA, then you will need to decide on a strategy to decay $\epsilon$ in $\epsilon$-greedy action choice, which may become a fiddly hyperparameter to tune.
* Q-learning (and off-policy learning in general) has higher per-sample variance than SARSA, and may suffer from problems converging as a result. This turns up as a problem when training neural networks via Q-learning.
* SARSA will approach convergence allowing for possible penalties from exploratory moves, whilst Q-learning will ignore them. That makes SARSA more conservative - if there is risk of a large negative reward close to the optimal path, Q-learning will tend to trigger that reward whilst exploring, whilst SARSA will tend to avoid a dangerous optimal path and only slowly learn to use it when the exploration parameters are reduced. The classic toy problem that demonstrates this effect is called [cliff walking](https://github.com/cvhu/CliffWalking). 

In practice the last point can make a big difference if mistakes are costly - e.g. you are training a robot not in simulation, but in the real world. You may prefer a more conservative learning algorithm that avoids high risk, if there was real time and money at stake if the robot was damaged.

If your goal is to train an optimal agent in simulation, or in a low-cost and fast-iterating environment, then Q-learning is a good choice, due to the first point (learning optimal policy directly). If your agent learns online, and you care about rewards gained whilst learning, then SARSA may be a better choice.