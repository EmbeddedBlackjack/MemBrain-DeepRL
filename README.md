# Blackjack-RL
Blackjack optimal strategy finder with reinforcement learning and other tools

## Possible DeepRL Methods (SARSA vs Q-Learning)

#### When to choose SARSA vs. Q-Learning (stack exchange question)
* https://stats.stackexchange.com/questions/326788/when-to-choose-sarsa-vs-q-learning

Q: SARSA and Q Learning are both reinforcement learning algorithms that work in a similar way. The most striking difference is that SARSA is on policy while Q Learning is off policy. The update rules are as follows:

Q-Learning:
$$
    Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha [ R_{t+1}+\gamma \max_{a^'}Q(s_{t+1},a^')-Q(s_t,a_t) ]
$$
SARSA (state-action-reward-state-action):
$$
    Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha [ R_{t+1}+\gamma Q(s_{t+1},a_{t+1})-Q(s_t,a_t) ]
$$
where $s_t,a_t$ and $R_t$ are state, action and reward at time step $t$ and $\gamma$ is a discount factor..