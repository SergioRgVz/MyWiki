
State, Action, Reward(State), State'

## The return
El retorno lo que refleja es que la recompensa que puedas conseguir más rápido puede ser más atractiva en un momento determinado. Ejemplo: te encuentras un billete de 5€ o andas media hora y te encuentras un billete de 10€

The goal of reinforcement learning:

Find a policy $\pi$  that tellls you what action ($a=\pi(s)$) to take in every state ($s$) so as to maximize the return.

![[1. Projects/Learning Unsupervised Learning, Recommenders, Reinforcement Learning/resources/Pasted image 20241013190214.png]]

## State-action value function

$Q(s,a)$ = Return if you 
- start in state s
- take action a(once)
- then behave optimally after that
The best possible return from state $s$ is $max\ Q(s,a)$. The best possible action in state $s$ is the action $a$ that gives $max\ Q(s,a)$.

![[1. Projects/Learning Unsupervised Learning, Recommenders, Reinforcement Learning/resources/Pasted image 20241013193431.png]]

DQN (Deep Q-Network)