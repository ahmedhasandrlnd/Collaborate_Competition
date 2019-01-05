# Project report
### Learning algorithm

* There are many interesting papers out there on MARL. One particular paper is called [“Multi Agent Actor Critic for Mixed Cooperative Competitive environments “ ](https://papers.nips.cc/paper/7217-multi-agent-actor-critic-for-mixed-cooperative-competitive-environments.pdf) by OpenAI. This paper used a multi-agent decentralized actor, centralized crtic appraoch as shown below.
![MARL](images/marl.png)

We can also apply single agent techniques to multi-agent case in two approaches:
	* Non-stationarity approach: In this approach, all the agents are trained independently without considering the existence of other agents. In this approach, any agent considers all the other agents as part of the environment and learn its own policy. Since all agents are learning simultaneously, the environment as seen seen from the perspective of a single agent changes dynamically. Hence this non-stationarity condition does not guarante convergence.

	* Meta Agent approach: This approach takes into account the existence of multiple agents. Here a single policy is learned learned for all the agents. It takes as input the present state of the environment and returns the action of each agent in the form of a single joint action vector. The joint action vector increases exponentially with the number of agents. This approach works well when each agent knows everything about the environment.

	* In this project, we have applied this second approach with an adapted [DDPG algorithm](https://arxiv.org/abs/1509.02971):

		* In DDPG, we use two deep neural networks for each agent: actor and critic. The actor is used to approximate optimal policy deterministically. The critic learns to evaluate the optimal action value function by using the actor's best believed action. 

		* We use a regular (or local) and target networks for both the actor and ctitic networks. The regular networks are the most up to date networks that we are training. While target networks are used for prediction to stabilize training.

		* We also use replay buffer to save the state, action, reward, next state, done tuples and soft update to mix a small percentage of the regular or local networks parameters with the target networks parameters. Also a random noise is added with action value of the actor networks for facilitating the exploration.



### Parameters used in DDPG algorithm:

* BUFFER_SIZE = int(1e6)  # replay buffer size
* BATCH_SIZE = 128        # minibatch size
* LR_ACTOR = 1e-3         # learning rate of the actor
* LR_CRITIC = 1e-3        # learning rate of the critic
* WEIGHT_DECAY = 0        # L2 weight decay
* LEARN_EVERY = 1         # learning timestep interval
* LEARN_NUM = 1           # number of learning passes
* GAMMA = 0.99            # discount factor
* TAU = 7e-2              # for soft update of target parameters
* OU_SIGMA = 0.2          # Ornstein-Uhlenbeck noise parameter, volatility
* OU_THETA = 0.12         # Ornstein-Uhlenbeck noise parameter, speed of mean reversion
* EPS_START = 5.5         # initial value for epsilon in noise decay process in Agent.act()
* EPS_EP_END = 250        # episode to end the noise decay process
* EPS_FINAL = 0           # final value for epsilon after decay      
              
      

### Network Architecture

The actor networks used in our implementation have the following architecture:

Layer        | (in, out) | Batchnorm | Activation      
-------------|-----------|-----------|-----------
Layer 1 | (State_size*2,256)|no|ReLU
Layer 2 | (256,128)|no|ReLU
Layer 3 | (128,action_size)|no|tanh

The critic networks used in our implementation have the following architecture:

Layer        | (in, out) | Batchnorm | Activation      
-------------|-----------|-----------|-----------
Layer 1 | (State_size*2,256)|no|ReLU
Layer 2 | (256+action_size*2,128)|no|ReLU
Layer 3 | (128,1)|no|-



### Results

![plot](images/plot_tennis.png)
```

```


| Untrained Agents Playing Tennis | 
| ------------------------------- | 
| ![Untrained Tennis](images/tennis_untrained.gif)| 

|   Trained Agents Playing Tennis | 
| ------------------------------- | 
| ![Trained Tennis](images/tennis_trained.gif) |


### Ideas for future work
1. Solve a more difficult environment, where the goal is to train a small team of agents to play soccer.[envirorment](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Soccer/Soccer.app.zip)
1. Apply other algorithms to solve the multi-agent environment 
	1. Proximal Policy Optimization Algorithms ([PPO](https://arxiv.org/pdf/1707.06347.pdf))
	1. Asynchronous Methods for Deep Reinforcement Learning ([A3C](https://arxiv.org/pdf/1602.01783.pdf))
	1. Distributed Distributional Deterministic Policy Gradients ([D4PG](https://openreview.net/pdf?id=SyZipzbCb))
