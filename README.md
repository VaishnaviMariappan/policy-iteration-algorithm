# POLICY ITERATION ALGORITHM

## AIM
To implement a policy iteration algorithm for the given MDP.

## PROBLEM STATEMENT
The problem statement is a Five stage slippery walk where there are five stages excluding goal and hole.The problem is stochastic thus doesnt allow transition probability of 1 for each action it takes.It changes according to the state and policy.

### State Space:
The states include two terminal states: 0-Hole[H] and 6-Goal[G].
It has five non terminal states includin starting state.

### Action Space:
* Left:0
* Right:1

### Transition Probability:
The transition probabilities for the problem statement is:

* 50% - The agent moves in intended direction.
* 33.33% - The agent stays in the same state.
* 16.66% - The agent moves in orthogonal direction.

### Reward:
To reach state 7 (Goal) : +1 otherwise : 0

### Graphical Representation:
![image](https://github.com/Aashima02/policy-iteration-algorithm/assets/93427086/23c50d3f-ad11-49b5-8dc1-3f3338f54ec6)


## POLICY ITERATION ALGORITHM:

1. Initialize the policy pi with a random action on each state. The initial policy is represented by the lambda function pi=lambda s:{s:a for s,a in enumerate(random_actions)}[s], where random_actions is a list of randomly chosen actions for each state.

2. Enter a loop that continues until the policy pi is no longer changing. This is determined by comparing the previous policy (old_pi) with the current policy computed in the loop.

3. Store the previous policy as old_pi for comparison later.

4. Perform policy evaluation and calculate the state-values (V). It represent the expected cumulative rewards starting from state s following policy pi and discounting future rewards by a factor of gamma. The function policy_evaluation is called with the arguments pi, P, gamma, and theta.

5. Perform policy improvement. This step updates the policy pi based on the current state-values V. The function policy_improvement is called with the arguments V, P, and gamma.

6. Check if the policy has converged by comparing the previous policy old_pi with the current policy. If they are the same for all states s, the loop is exited.

7. Return the final state-values V and the optimal policy pi.

## POLICY IMPROVEMENT FUNCTION
```python
def policy_improvement(V, P, gamma=1.0):
    Q = np.zeros((len(P), len(P[0])), dtype=np.float64)
    # Write your code here to implement policy improvement algorithm
    for s in range(len(P)):
      for a in range(len(P[s])):
        for prob, next_state,reward, done in P[s][a]:
          Q[s][a]+= prob*(reward+gamma*V[next_state]*(not done))
          new_pi = lambda s: {s:a for s, a in enumerate(np.argmax(Q, axis=1))}[s]

    return new_pi
```
## POLICY ITERATION FUNCTION
```python
def policy_iteration(P, gamma=1.0,theta=1e-10):
  random_actions=np.random.choice(tuple(P[0].keys()),len(P))
  pi = lambda s: {s:a for s, a in enumerate(random_actions)}[s]
  while True:
    old_pi = {s:pi(s) for s in range(len(P))}
    V = policy_evaluation(pi, P,gamma,theta)
    pi = policy_improvement(V,P,gamma)
    if old_pi == {s:pi(s) for s in range(len(P))}:
      break
  return V,pi
```
## OUTPUT:
### Optimal Policy:
![image](https://github.com/VaishnaviMariappan/policy-iteration-algorithm/assets/94169913/00cb919e-6581-44a1-a994-1b3fd6cceacd)

### Optimal Value Function
![image](https://github.com/VaishnaviMariappan/policy-iteration-algorithm/assets/94169913/54ac9220-46a0-4fa5-8103-b706aa9e58ef)

### Success Rate:
![image](https://github.com/VaishnaviMariappan/policy-iteration-algorithm/assets/94169913/d2458594-f3d1-437c-a951-c6da9f4c8b87)

## RESULT:

Thus, a program is developed to perform policy iteration for the given MDP.
