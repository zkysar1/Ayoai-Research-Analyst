# Thoughts1 - December 2023

Roblox Done, Next-Up

## Idea Generation -- March 2022

Playing Roblox with kids and not impressed. Always wanted to build a digital brain, and wanted a startup idea, so here we went. . .

## Task decomposition

- Not sure which ones I like yet - need to review them again at high level. I can borrow general things from all of them.

- These can be nodes in a behavior tree.

## Multi step planning 

- Self consistency seems easy and something that adds a lot of value!

- The tree/graph of thoughts is awesome. So it turns the llm strategy into a behavior tree basically. Each task node could be a different prompt. For composite nodes, we could do comparisons. We could have these auto generated too and add and mix in stuff as a way to sell upgrades and and extra polarity! There could be a blackboard.

  - In other words, self consistency is simply a task leaf node, and we should have other tools available to us as well.

  - Tree of thoughts can be inserted into graph of thoughts and parallelized, etc.

- LLM-MCTS - this paper was awesome and interesting about the idea of the l model and the l policy. Meaning, use the llm for the building a world model, then ask the llm for policies based on that world model. . . Hmmmm. This sounds awesome and I could do some of this within my behavior tree!!

## External planners

- PDDL: This one may be the one?!!?(LLM+PDDL [Guan et al., 2023])? Meaning, I have been looking for a planning framework to integrate, and this one looks freaking awesome and out of the box? Should I deep dive on this?

  - Yes!! I need something like this. Here is why:

  - In this paper, there is code that can translate human English into pddl models.

  - Furthermore, also in the Household domain, we observe that classical planners occasionally generate physically plausible but unconventional actions, such as placing a knife on a toaster when the knife is not being used. In contrast, the LLM planner rarely exhibits such actions, suggesting that LLMs possess knowledge of implicit human preferences. It would be meaningful to explore methods that more effectively combine the strengths of LLM planners and the correctness guarantee provided by symbolic domain models, particularly in determining which information from LLM plans should be preserved.

  - How are we going to generate a plan? What was the fast down. Does he give that code? Is that an external dependency or an algo?

    - I think this is the key!

- SwiftSage was really cool.

  - Ah man though, SwiftSage did this trick I need to integrate this and it can work theoretically with any underlying structure I choose: Inspired by the dual process theory [39, 16], we propose a novel framework that enables agents to closely emulate how humans solve complex, open-world tasks. The dual-process theory posits that human cognition is composed of two distinct systems: System 1, characterized by rapid, intuitive, and automatic thinking; and System 2, which entails methodical, analytical, and deliberate thought processes. System 1 is reminiscent of seq2seq methods, which learn through imitation of oracle agents and primarily operate utilizing shallow action patterns. Conversely, System 2 bears resemblance to LLMs that excel in applying commonsense knowledge, engaging in step-by-step reasoning, devising subgoal strategies, and exercising self-reflection. Thus, our proposed method, SWIFTSAGE, is designed to enable both fast and slow thinking in complex interactive reasoning tasks. It effectively integrates the strengths of behavior cloning (representing System 1) and prompting LLMs (emulating System 2), resulting in significant enhancements in task completion performance and efficiency.

  - There is a planning stage, and a grounding stage. During the planning stage, we want the llm to use English to step through the plan and self reflect etc, then I need to go to a grounding phase to be sure I am in the proper format. SwiftSage had some good ideas here.

- We must find a way around the fact that doing llm for reward is single turn. How to get reward for multi turn, and fast? How will that work?

## Reflection and refinement 

- Definitely need to do this. This can be integrated into a tree for awesome performance!

- There are some great prompts scattered all across these reflection papers - be sure to utilize. Voyager has some good ones, and so do the others.

- These can be nodes in a behavior tree.

- One odd takeaway was that shorter responses form the llm are better, so work with the critics and other llms we discuss with more step by step?

## Memory

- Generative Agents is awesome and has some great prompts. Careful its expensive though. There is some great code in there though. And the summarize and forget memory too. Need to research this.

- Definitely need to have some sort of memory.

- Isn't the art of saving and making memory also a task leaf node in the behavior tree I have in mind?

- Some memory needs to be updated - like health, or marriage status, state (like running or walking) hmm. Roblox is my world model here.

## What is PDDL? 

- Cool overview slides

- PDDL = Planning Domain Definition Language ❀ standard encoding language for "classical" planning tasks

- Components of a PDDL planning task: • Objects: Things in the world that interest us. • Predicates: Properties of objects that we are interested in; can be true or false. • Initial state: The state of the world that we start in. • Goal specification: Things that we want to be true. • Actions/Operators: Ways of changing the state of the world.

- Example: Gripper task with four balls: There is a robot that can move between two rooms and pick up or drop balls with either of his two arms. Initially, all balls and the robot are in the first room. We want the balls to be in the second room. • Objects: The two rooms, four balls and two robot arms. • Predicates: Is x a room? Is x a ball? Is ball x inside room y? Is robot arm x empty? [...] • Initial state: All balls and the robot are in the first room. All robot arms are empty. [...] • Goal specification: All balls must be in the second room. • Actions/Operators: The robot can move between rooms, pick up a ball or drop a ball.

## Zak old blue sky flow? 

- First tell the llm that its only job is to build behavior trees.

  - It gets paid based on the quality of the behavior trees only.

**Ayoai Impact**: December 2023 reflections show evolution of thinking:
- Task decomposition → behavior tree nodes
- Multi-step planning → tree/graph structures
- PDDL for formal planning
- SwiftSage dual-process theory
- Memory as behavior tree nodes
- Everything connects to behavior trees