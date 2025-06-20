# Intelligence modules
These are plug and play modules that drive agent behavior.  These modules provide the ability for builders to iterate on different types of agents and costs points.

Here is start list example (officilly documented at https://www.ayoai.com/Interface_Specifications.md):
- Zombie AI : Pursue and attack nearest character until death.
- Guardian AI: Follow and protect one character until death.
- Explore AI: Touch least known touched unit while staying alive.
- Command-Only AI: Do nothing until given a command through chat, then do the command regardless of outcome.  

This document is to keep notes on things i am thinking about in this area.  

## Thinking about Explore AI
- Some random notes I had: 
```
[1901.10995] Go-Explore: a New Approach for Hard-Exploration Problems

This is new.  

Zak notes
	• Think about difference between hypothesized state and current state.
	• It's like go explore algorithm, but instead of picking random actions to explore, it asks llm. Beaty here is that these trajectories could be made before simulation, and these trajectories can be shared across agents.
	• While the goal may remain the same, we can pick different trajectories to accomplish it given the state we are in.  
		○ Need to work on the ability to alter tasks under one goal dynamically. Like during move too. Of they stay moving and get stuck. Am action may fail, but that does not mean a new action can be made and we continue with our goal. Like. We could try moving to the left or right. Or try jumping. How is this going to work?
	

The archive updates in two conditions
	1. When the agent visits a cell that was not yet in the archive (which can happen multiple times while exploring from a given cell). In this case, that cell is added to the archive with four associated pieces of metadata: 
		a. How the agent got to that cell (here, a full trajectory from the starting state to that cell)
		b. The state of the environment at the time of discovering the cell (if the environment supports such an operation, which is true for the two Atari-game domains in this paper)
		c. The cumulative score of that trajectory
		d. The length of that trajectory
	2. The second condition is when a newly-encountered trajectory is “better” than that belonging to a cell already in the archive.  
		a. For the experiments below, we define a new trajectory as better than an existing trajectory when the new trajectory either has a higher cumulative score or when it is a shorter trajectory with the same score. In either case, the existing cell in the archive is updated with the new trajectory, the new trajectory length, the new environment state, and the new score. 
		b. In addition, information affecting the likelihood of this cell being chosen (see Appendix A.5) is reset, including the total number of times the cell has been chosen and the number of times the cell has been chosen since leading to the discovery of another cell. Resetting these values is beneficial when cells conflate many different states because a new way of reaching a cell may actually be a more promising stepping stone to explore from (so we want to encourage its selection). We do not reset the counter that records the number of times the cell has been visited because that would make recently discovered cells indistinguishable from recently updated cells, and recently discovered cells (i.e. those with low visit counts) are more promising to explore because they are likely near the surface of our expanding sphere of knowledge. Because cells conflate many states, we cannot assume that a trajectory from start state A through cell B to cell C will still reach C if we substitute a different, better way to get from A to B; therefore, the better way of reaching a cell is not integrated into the trajectories of other cells that built upon the original trajectory. However, performing such substitutions might work with goal-conditioned or otherwise robust policies, and investigating that possibility is an interesting avenue for future work.
```


