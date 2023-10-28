An issue with [[Monte Carlo simulation]] that draw on a large number of random variables is sparsity and the large number of samples required to provide adequate coverage of the high dimensional sample sample space from which values can be drawn, with each additional variable exponentially increasing the size of the "search space".

This issue can be partially addressed by simply making the simulation faster, allowing for more samples to be taken, filling in the search space. This note covers one approach that I have found useful, which manipulates the order in which distributions are sampled to reduce simulation runtime.

The random variables in a Monte-Carlo simulation can be divided into 2 groups:
1. Random variables that affect the simulation flow - eg by determining a logical path or influencing the simulation state, which i will refer to as "node" variables
2. Random variables that affect the outcome but nothing else - "leaf" variables

In most simulations, the majority of the compute time is tied to the "node" variables and their interactions with simulation state, while the leaf variables require relatively little compute to have their affects on the final outcome determined. In this setup, we can reduce latency by pausing the resampling of node variables while we resample leaf variables, before moving to the next sample of node variables and repeating the process.

### A simple example: 

Consider a betting process by which the actor flips a coin to determine which of two horses to bet on. A single iteration of the simulation may involve:
1. Drawing a random variable to determine the coin flip outcome
2. Drawing many random variables to simulate the movement of the horses, decisions of the jockeys and the outcome of the race.

In this example, the race simulation is complex and affects both the outcome for the actor and the simulation itself, whereas the coin flip only affects the outcome. 

Due to this we can first simulate the race, then divide into two simulations, one for each of the equally likely coin flip outcomes which we evaluate. This has the effect of increasing our sample size without greatly increasing the computation required. 

If the actor rolled a dice or sampled from some other more complex distribution to determine which horse to back, we could accomplish the same result by sampling from this distribution a number of times, recoding the outcome in each case.

### Simulation termination use case

A more complex but useful use case is when a random variable has the effect of terminating the simulation. Consider a lifespan spending model, where each time period the agent has to determine how much to consume and how much to invest for their future, with some probability of death depending on age and other simulation states, with objective being maximisation of total lifetime consumption. 

In this simulation, a true Monte Carlo implementation would be to draw from the death probability distribution each period (adjusting parameters based on age and state), and terminate the iteration if the agent dies. 

However, the death of the agent can be viewed as a leaf variable in this simulation, with a simple optimisation being:
1. Simulate the agent to an improbably high age, recording at each timestep the probability of death.
2. Once the computationally intensive life situation process is complete, quickly iterate through the time steps and apply the death probability, computing the outcomes of each of these sub-simulations.

As long as an appropriately large number of "node" level samples are still taken, this should provide a close approximation for the true Monte Carlo simulation


A simple implementation of this example shows that this is the case:

True Monte Carlo implementation:
```
prob_death = [(1/120)*i for i in range(120)]


def life_simulation_simple(c, n):
	# c = Consumption rate
	outcomes = []
	for i in range(n):
		alive = True
		savings = 100
		spent = 0
		age = 0
		while alive:
			spent += c*savings
			savings = (1-c)*savings
			savings = savings * (1+random.random()/10)
			if random.random() < prob_death[age]:
				# actor dead
				outcomes.append(spent)
				alive = False
			age += 1
	return outcomes
```

Leaf variable optimisation implementation:
```
def life_simulation_opt(c, n, m):
	outcomes = []
	for i in range(n):
		savings = 100
		spent = 0
		age = 0
		death_probs_cache = []
		score_cache = []
		# Sample over node variables
		while age < 120:
			spent += c*savings
			savings = (1-c)*savings
			savings = savings * (1+random.random()/10)
			death_probs_cache.append(prob_death[age])
			score_cache.append(spent)
			age += 1
		# Sample deaths over ages - leaf variable
		for j in range(m):
			for death_prob, score in zip(death_probs_cache, score_cache):
				if random.random() < death_prob:
					outcomes.append(score)
					break
	return outcomes
```

For a test of 10,000 samples, which for the optimised version was divided into 100 node samples, each of which has 100 leaf samples, we get the following results (c=0.15):

**Mean of score:**
- True Monte Carlo: 104.31 
- Optimised: 104.65

**Variance in score:**
- True Monte Carlo: 602.77
- Optimised: 602.99

**Execution time:**
- True Monte Carlo: 0.0443s
- Optimised: 0.0180s

#Simulation #montecarlo