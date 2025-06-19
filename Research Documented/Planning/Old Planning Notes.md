# Main Notes
• Started with trying to find HTN java libraries.  
	○ I tried PANDA and too big and too many errors: https://www.uni-ulm.de/en/in/institute-of-artificial-intelligence/research/software/panda/
	○ Explanation of HTNs: https://www.geeksforgeeks.org/hierarchical-task-network-htn-planning-in-ai/
	○ I did a lot on ChatGPT to learn as well.  
	○ Another good read/look: https://www.gameaipro.com/GameAIPro/GameAIPro_Chapter12_Exploring_HTN_Planners_through_Example.pdf
	○ PYSHOP: Then found an awesome HTN library in python I wanted to convert over. . .
		§ https://bitbucket.org/dananau/pyhop/src/master/pyhop.py./
		§ Or maybe this link: https://bitbucket.org/dananau/pyhop/src/master/
	○ GTPyhop: Then, I found GTN that was made by the same people
		§ https://www.cs.umd.edu/~nau/papers/nau2021gtpyhop.pdf
		§ Here is the code: https://github.com/dananau/GTPyhop
		§ I made this gtn and I really like its abstraction.  I think I want to use it, but I also want to check out rddl before I get too ahead. 
	○ Read about rddl language. The progression goes, PDDL, HDDL where a method can be hierarchical, then rddl where they add reward function.  I should start with pddl and hddl, then can expand further.  This rddl library is freaking huge and I want no part of it!
		§ Or maybe RDDL? RDDL incorporates rewards I think - look into how those are handled!!
			□ Cool explanation here: https://ssanner.github.io/slides/RDDL_Tutorial_ICAPS_2018.pdf
				® It said it was Mainly SPUDD / Symbolic Perseus with a different syntax  – A few enhancements • concurrency • constraints • integer / continuous variables
			□ Rddlism? https://github.com/ssanner/rddlsim
	○ Huh, cool difference here: 
		§ RDDLism:
			• In RDDL, planning is reward-based, aligning with the MDP framework. Agents aim to maximize a reward function over time, considering the probabilistic nature of actions and states.
			• The reward structure in RDDL can capture complex, multi-step goals, optimizing for long-term gains rather than immediate outcomes.
		§ GOAP:
			• GOAP uses goal-based planning, where actions are selected based on a goal’s requirements rather than maximizing a numerical reward.
			• Each action has a cost or priority that helps determine an optimal path toward the goal, often without considering long-term reward accumulation. This is more of a deterministic, direct path-finding approach towards the end goal.
	○ Old plan before going the BT route: First, get to know the code now. I really like it. It's the same abstraction that would have worked LifingPolls. However, next step would be too intake hddl language. Like, I cannot define tasks and goals and such in code, it should be in hddl. Also, and perhaps before switching to hddl language is, should I have parrell, sequence, selector nodes in my plan? Can't I have the same thing as in my lia code, with the listens to finish and black board.
		§ Oh but wait, what about rewards, how are those models? I think this zones last. If I have everything else, I can add reward
		
	
		
	
		
		
		
• Plan next up
	○ First, get to know the code now. 
		§ I really like it. It's the same abstraction that would have worked LifingPolls. 
	○ Make it able to intake hddl language. Like, I cannot define tasks and goals and such in code, it should be in hddl. I can migrate to RDDL later.  
	○ Also, and perhaps before switching to hddl language is, should I have parrell, sequence, selector nodes in my plan? Can't I have the same thing as in my lia code, with the listens to finish and black board.
	○ Oh but wait, what about rewards, how are those models? I think this zones last. If I have everything else, I can add reward
	
## Why GTN over HDDL
I am confused about the difference between task oriented planning (HDDL here) and goal oriented planning (GTN here). Can you give me an example of both? Is the difference that in HDDL the problem file specifies the task that needs to happen and it figures out if it is possible? And then for GTN, instead of picking a task, it picks a goal, and then the planner must pick the right set of tasks to meet the goal? Can you help me with the difference exactly? 

ChatGPT said:
ChatGPT
You’re exactly on the right track! The distinction between task-oriented planning (HDDL) and goal-oriented planning (GTN) lies in what is specified in the problem and how the planner approaches the problem. Here’s a breakdown to help clarify:

1. Task-Oriented Planning (HDDL / HTN) Example
In Hierarchical Task Network (HTN) or task-oriented planning, like in HDDL, the problem file specifies tasks that need to be achieved, and the planner’s job is to break down these tasks into smaller, executable steps.
Scenario:
Let’s say you want to plan a "trip to the grocery store."
	• Problem Specification: The problem specifies the task: "Complete grocery shopping."
	• Planner’s Job: The planner figures out a step-by-step breakdown of how to accomplish this task.
Decomposition (Hierarchical Breakdown):
In HTN/HDDL, the planner uses a predefined set of methods to break down the main task. For example:
	1. The task "Complete grocery shopping" could be decomposed into:
		○ "Go to store."
		○ "Get groceries."
		○ "Return home."
	2. Each subtask is further broken down. For instance:
		○ "Go to store" might involve either:
			§ Taking a car, or
			§ Walking, depending on other preconditions.
The planner follows the decomposition hierarchy until all steps are executable (e.g., drive, pick up item, pay).
Key Points:
	• Tasks are specified upfront.
	• The planner figures out how to accomplish the task by choosing appropriate methods and steps.
	• The process is hierarchical and follows a structured breakdown.

2. Goal-Oriented Planning (GTN) Example
In Goal Task Network (GTN) or goal-oriented planning, the problem specifies what goal needs to be achieved, rather than a specific task. The planner’s job is to choose and sequence tasks that will fulfill this goal.
Scenario:
You want a plan to achieve the goal of having groceries at home.
	• Problem Specification: The problem specifies the goal: "Have groceries at home."
	• Planner’s Job: The planner identifies what tasks are necessary to fulfill this goal.
Task Selection (Goal-Driven):
In GTN, the planner uses goal-oriented logic to decide the best way to achieve the specified goal. For example:
	• The planner might choose tasks based on current conditions:
		○ If you own a car, it might choose to “drive to store” and “buy groceries.”
		○ If stores are closed, it might choose a different goal-related task like “order groceries online.”
	• The planner selects tasks based on what best fulfills the goal rather than following a strict hierarchy of decompositions.
Key Points:
	• Goals are specified, not tasks.
	• The planner’s job is to identify and sequence the tasks needed to achieve the goal.
	• The process is dynamic and adaptable based on the current situation.

Summary of the Key Difference
	• HDDL/HTN (Task-Oriented): The problem file specifies a task to be completed. The planner decomposes the task into smaller subtasks, following predefined methods and hierarchies until all steps are actionable.
	• GTN (Goal-Oriented): The problem file specifies a goal to be achieved. The planner selects and sequences tasks dynamically based on what is required to achieve the goal, allowing it to adapt task choices based on changing conditions or constraints.

Example Comparison in Context
Imagine the following goal: Prepare dinner at home.
Task-Oriented Planning (HTN/HDDL):
	• The task might be defined as “Prepare dinner,” with a predefined method to break it down into “gather ingredients,” “cook meal,” and “serve meal.”
	• The planner follows this structured task breakdown without flexibility—if you’re missing ingredients, it may not complete the plan unless it has a task to buy them in the decomposition.
Goal-Oriented Planning (GTN):
	• The goal is defined as “Have dinner prepared at home.”
	• The planner identifies ways to fulfill this goal, such as “cook a meal” or “order delivery,” depending on available resources and constraints.
	• If you’re missing ingredients, the planner might choose to order food instead or select another recipe using what’s available.

Why Use One Over the Other?
	• HTN/Task-Oriented Planning is best for situations where the process must follow a clear hierarchy or set structure, such as manufacturing, routine tasks, or any process with strict requirements.
	• GTN/Goal-Oriented Planning is best for dynamic, adaptive environments where goals are clear, but the steps to achieve them can change based on the situation, like robotics, adaptive learning, or real-time strategy games.
I hope this helps clarify the distinctions! Let me know if you’d like more examples or further details on implementing one or the other.



4o
You said:
I am making humanoids smart in roblox, so I think i need to use more of a GTN approach. Would you agree?

ChatGPT said:
ChatGPT
Yes, for making humanoids smart in Roblox, a GTN (Goal Task Network) approach would indeed be a good fit! Here’s why GTN aligns well with what you’re aiming to do, and how it can enhance the AI behavior for your humanoids:
Why GTN is Suitable for Smart Humanoids in Roblox
	1. Dynamic Environment: In Roblox, the game world can be unpredictable and change based on player actions, game events, or other NPCs. GTNs allow humanoids to have goals that adapt to these changes, enabling more intelligent, reactive behavior. For instance, if a goal is to “find shelter,” a GTN approach allows the humanoid to adapt based on whether a shelter is available nearby or if it’s occupied.
	2. Flexible Goal Management: GTNs let you define high-level goals (like "survive," "explore," or "help the player") rather than forcing a specific sequence of tasks. This flexibility enables humanoids to dynamically choose and prioritize tasks based on the situation. For example, if a humanoid's goal is to "assist a player," the specific actions could range from finding health items to defending against enemies, depending on what’s currently needed.
	3. Adaptive Task Selection: GTNs allow tasks to be selected and sequenced in real-time to meet the desired goals, enabling smart NPC behaviors. For instance, a humanoid with a “patrol” goal could adaptively choose whether to walk, sprint, or hide based on threats or player proximity, making it appear more intelligent and responsive.
Example GTN Implementation for a Smart Humanoid in Roblox
Step 1: Define High-Level Goals
	• Goals could include things like:
		○ "Protect player"
		○ "Explore surroundings"
		○ "Defend area"
		○ "Gather resources"
		○ "Escape danger"
Step 2: Create Tasks for Each Goal
	• For each goal, design a set of tasks that can accomplish it. Some example goals and tasks:
		○ Goal: Protect Player
			§ Tasks:
				□ “Follow the player”
				□ “Identify nearby threats”
				□ “Engage threat if within range”
		○ Goal: Explore Surroundings
			§ Tasks:
				□ “Walk to random points in the environment”
				□ “Collect objects of interest”
				□ “Avoid hazards”
		○ Goal: Escape Danger
			§ Tasks:
				□ “Identify nearest exit”
				□ “Run to safe area”
				□ “Hide if pursued”
Step 3: Implement Dynamic Task Selection and Prioritization
	• Use a decision-making system that evaluates the humanoid's goals based on its environment, state, and the actions of players or other NPCs. For example:
		○ If the humanoid sees a threat and the goal is “Protect Player,” it may prioritize the task of “Engage threat” over “Follow the player.”
		○ If health is low, the goal “Escape Danger” might take precedence, dynamically selecting tasks like “Run to safe area” or “Hide.”
Step 4: Update Goals Based on State Changes
	• GTN allows humanoids to shift goals dynamically:
		○ If the humanoid’s health drops below a certain threshold, switch to the “Escape Danger” goal.
		○ If the player starts an important quest, the humanoid’s goal could switch to “Assist Player.”
How to Get Started with GTN in Roblox
	1. State Tracking: Set up variables to track states, such as health, isUnderAttack, playerNearby, and hasObjective, allowing the humanoid to evaluate when and how to change goals.
	2. Goal Selection Logic: Implement a decision tree or priority queue that enables the humanoid to select the most pressing goal based on current conditions. This could involve simple conditional statements or a more advanced rule-based system.
	3. Task Execution: Create task functions that correspond to the different possible actions the humanoid can perform. Each goal should have associated tasks that the humanoid can execute in varying sequences to meet the goal.
	4. Goal Management: Use Roblox’s scripting capabilities to continuously monitor the humanoid’s environment and update its goals as needed. Roblox’s Lua scripting language is well-suited for real-time condition checks and decision-making logic.
Example Code Snippet for a GTN System in Roblox (Lua)
Here's a basic outline in Lua to show how you might set up a GTN-inspired decision loop:

lua
Copy code
local humanoid = script.Parent
local currentGoal = "Explore"  -- Default goal
-- Define goals and tasks
local goals = {
    ProtectPlayer = function() ... end,
    Explore = function() ... end,
    EscapeDanger = function() ... end
}
-- Evaluate conditions to set goals
local function updateGoal()
    if humanoid.Health < 20 then
        currentGoal = "EscapeDanger"
    elseif playerNearby then
        currentGoal = "ProtectPlayer"
    else
        currentGoal = "Explore"
    end
end
-- Execute tasks based on current goal
local function executeTasks()
    if goals[currentGoal] then
        goals[currentGoal]()
    end
end
-- Main AI loop
while true do
    updateGoal()
    executeTasks()
    wait(1)  -- Run every second (adjust as needed)
end
This is a simplified version, but the core logic demonstrates how a humanoid can switch between goals and execute tasks based on environmental changes.
Final Thoughts
In a GTN approach, you’re empowering your Roblox humanoids to act adaptively and appear more intelligent by focusing on what goals they should achieve rather than prescribing an exact sequence of steps. This goal-oriented design will help them react to changing conditions and make gameplay more dynamic for players interacting with them.
Let me know if you need any more detail on specific aspects or implementation tips!

## The Plan (what I thought - the hypothisis)
I think I am leaning in this direction: "Dynamic Task Generation: Using HDDL as a Foundation." What I was thinking was using HDDL languages for actions, methods, and tasks, but adding a goal state above those three. So i will end up with actions, methods, tasks, and then goals. The actions, methods, and tasks can all be planned using HDDL, but only the HDDL that would result in the goal would win. Can you help me brainstorm this idea? Would this work? What am I not thinking about here on whether or not this will work for a roblox humanoid? 

ChatGPT said:
ChatGPT
Your idea of building a goal-oriented layer on top of HDDL’s actions, methods, and tasks is a strong approach and could absolutely work for making Roblox humanoids more intelligent and adaptable. Adding a goal state as a layer above tasks gives you a powerful way to organize behaviors and enables you to drive the humanoid's decisions in a more dynamic, flexible way. Let’s break down how this would work, and what to consider:

1. Framework Structure with Goals on Top
In this structure:
	• Actions: Your lowest-level primitives (like moveTo, attack, pickUp) that define basic behaviors and can be executed directly by the humanoid.
	• Methods: Ways to decompose tasks using actions. For example, a “navigateTo” method might decompose into a series of moveTo actions with different locations.
	• Tasks: Compound tasks that encapsulate a set of related methods (like “explore area” or “defend player”). Tasks use methods to achieve subtasks in sequence or parallel.
	• Goals: The highest-level states that the humanoid tries to achieve (like “survive,” “assist player,” or “navigate to cover”). Each goal can have one or more tasks associated with it, but only the task sequence leading to the current goal is actively pursued.
Using this layered approach, goals act as a filter: only the actions, methods, and tasks that align with achieving the current goal will be considered.
2. Goal-Driven Task Selection Process
	• Set the Goal: The current game state, player actions, or environment triggers determine which goal is active (e.g., if the humanoid’s health is low, the goal might switch to “find health”).
	• Activate Relevant Tasks: The planner selects tasks that contribute to the active goal. For instance, if the goal is “find health,” the tasks could include search area and moveTo healthPack.
	• Plan and Execute: Within the selected tasks, HDDL planning would determine the best methods and actions based on the available resources, location, and conditions, resulting in a plan specifically targeted to the goal.
3. Example Workflow with a Goal Layer
Let’s say the current goal is “assist player”:
	1. Goal: "Assist player" (top-level goal state)
	2. Relevant Tasks:
		○ protect player
		○ provide backup
	3. Methods:
		○ For protect player, methods might include moveTo cover positions and engage enemies.
	4. Actions:
		○ moveTo, attack, takeCover
If an enemy appears near the player, the humanoid’s planner selects tasks like protect player and activates methods to find cover and engage the enemy. All actions and tasks are generated dynamically, guided by the “assist player” goal.
4. Dynamic Goal Switching
An essential feature for Roblox gameplay is the ability to switch goals dynamically. Here’s how that could work in your setup:
	• Goal Condition Checks: At regular intervals or when specific events occur, evaluate conditions (like health status, proximity to enemies, or player location).
	• Goal Priority System: Assign priority levels to goals. For instance, "survive" might have the highest priority if health is below a threshold, overriding lower-priority goals like "assist player."
	• Dynamic Replanning: When a new goal becomes active, the system automatically recalculates which tasks, methods, and actions to pursue, allowing the humanoid to adapt to changing conditions.
5. Implementation Considerations for Roblox
To make this work smoothly with Roblox, here are some considerations:
	• Action and Method Granularity: In Roblox, actions should be kept simple and fast to execute (like moving to a point, attacking, or picking up an item). Methods should be designed to handle Roblox’s environment seamlessly, which might mean incorporating pathfinding or specific conditions (e.g., avoiding obstacles).
	• Goal Condition Evaluation: Since goals are higher-level states, you’ll need a consistent way to evaluate conditions in Roblox (like detecting nearby health packs or enemies). This might involve coding sensors or checks in Roblox scripts to trigger goal changes.
	• Real-Time Adaptation: Roblox games are real-time, so your system should be responsive enough to adapt as conditions change. Implementing a lightweight planner or even decision trees for smaller-scale games can help manage performance, or for complex games, you could limit the frequency of goal evaluation.
	• Modularity for Flexibility: Keeping goals, tasks, and methods modular allows you to easily add, modify, or remove elements without disrupting the entire AI system. For instance, if you add a new goal like "defend area," it should integrate smoothly with existing tasks and methods.
6. Pros and Cons of the HDDL Goal-Layered Approach
Pros:
	• Scalability: By organizing goals on top, you create a scalable framework where humanoids can handle complex scenarios by focusing on current goals without overwhelming task definitions.
	• Reusability: This approach allows reuse of tasks and methods across multiple goals, reducing redundancy and simplifying updates.
	• Realistic Behavior: Humanoids will appear more intelligent, switching goals in real time to adapt to the game environment, which enhances realism for players.
Cons:
	• Complexity in Goal Management: Managing goal priorities and ensuring smooth transitions between goals can be complex, especially as you add more goals.
	• Runtime Overhead: Depending on how frequently conditions are checked, there may be performance implications in high-action scenarios.
Putting It All Together
To implement this approach:
	1. Define Goals and Conditions in your system as high-level states with associated condition checks.
	2. Define Tasks in HDDL that can serve multiple goals (e.g., protect player can serve both “assist player” and “defend area”).
	3. Define Methods and Actions in HDDL for each task, specifying how they are broken down into executable actions.
	4. Implement a Runtime Goal Evaluator in Roblox that triggers goal changes based on environmental conditions.
	5. Integrate the Planner in Roblox: Run the HDDL planner in the background to continuously adapt to the active goal by selecting the most relevant tasks, methods, and actions.
Example Code Sketch for Goal Evaluation (in Lua)
Here’s a rough example of how you could structure a goal evaluator in Roblox:

lua
Copy code
local activeGoal = "explore"
local goals = {
    explore = {condition = function() return true end, priority = 1},
    assistPlayer = {condition = function() return playerNearby end, priority = 2},
    findHealth = {condition = function() return humanoid.Health < 20 end, priority = 3},
    survive = {condition = function() return humanoid.Health < 10 end, priority = 4}
}
-- Function to evaluate and set the active goal
local function evaluateGoals()
    for goal, data in pairs(goals) do
        if data.condition() and data.priority > goals[activeGoal].priority then
            activeGoal = goal
            break
        end
    end
end
-- Main loop for goal evaluation and task execution
while true do
    evaluateGoals()
    -- Execute tasks based on activeGoal using your HDDL planner
    -- planner.executeTasksForGoal(activeGoal)
    wait(1)  -- Adjust frequency as needed
end
This approach will allow the humanoid to dynamically adjust to changing circumstances, creating a compelling and adaptive AI that feels alive within Roblox. Let me know if you want to dive deeper into any specific part of the setup!


## Plan after ultra thinking (Nov 2024)
	1. Character State
		a. On Start up: Establish conflateStateFeatureList: A JsonArray feature data to monitor from each of the percepters. These can be from many different objects across the world that we want to monitor.  This should be keyed on the location within the world, so the full name path of each object should be the key.  
		b. Input: Trigger from Character Driver: State is a on a trigger that happens shortly after Roblox update (to give time for Roblox update to propagate).  This trigger time is feature.  
			i. Output in response to receiving this input: Sends the conflated State, a cell in go explore, as a json blob.
				1) do not save it back to the public node, because we do not want duplicate data.  Instead save this cell, as something to do later.  But what is the trajectory to get to that state?  It is likely a sequnce of a trees and then the final tree is only half done.  So we'd have to save the path we are at while saving that cell.   
				2) Only populate state data based on what is in the monitoring list.  To do this, Maintain a list of data points we are monitoring and from where.  
				3) Must only listen to our perceptions.  If we listen directly to roblox update things can get out of sync.  
				4) How am I able to save the trajectory to this?  How do I get access to the current trajectory? 
		c. Input: Add feature to conflateStateFeatureList  (like the blinking light property on object x)
		d. Input: Remove feature from conflateStateFeatureList (like the blinking light property on object x)
		
	2. Behavior Tree Manager
		a. Purpose: executes trees (and naturally while executing sends Roblox trees and waits for Roblox trees to complete)
		b. Input: State Messages: These are sent from the Roblox API
			i. While we wait here for each node to be successful we need to be accumulating state.  Like, if we did a moveTo action, and it takes 3 seconds for that to happen, I would want to have an accumulate state from all the state updates received.   That way, no matter what the sate stops or starts sending out, this behavior three executor will know it all for each action it takes. 
		c. Input: Behavior Tree Updates: These are sent from the Roblox API
			i. If node within tree (but not root) marked as Success
				1) Need to run the next node! 
					a) The next node may be a Roblox seed that needs to be sent, or it maybe a node that is only ran in java.  It does not matter, simply run the next one. 
					b) We could think about sending roblox seeds in advance, but for now I think we send them as the previous one completes.  This is the simplest implementation, and it may be fast enough - lets sett.  
						i) I am afraid of putting too many future trees into Roblox and then having to cancel them all later when the plan changes.  If we get a failure, we want to replan quickly.
			ii. If node within tree (but not root) marked as Fail
				1) Need to fail the current node and let the failure propagate up naturally.  The tree may keep going after this failure so let it.  No need to do a full replan until the entire tree fails.
			iii. Regardless of If node within tree marked as Fail or Success, but when node is marked as either, do these in order!! 
				1) Update the effects!
					a) For the things on the effects list that we predicted would be true for each node, compare those data points to what is in the current state.  
						i) If the effects match the state, then nothing to do.  
						ii) However, if the effects that we predicted do not match the current state, then we need to make that effects node equal that of the current state.  
							One. This is because for this node, which is about to be archived as what actually happened, needs to reflect what actually happened.  It no longer needs to reflect what we predicted.  We also do not need to save what we predicted, because that should already be there from copying it from the archived tasks.
					b) Need to look at what changed from previous accumulated state to the current state.  I need this list to detect things that may not be on the effects list yet.    
						i) This answers how we are going to know that health lowered by walking.  So it will be like, moveTo completed, let me check the state?  And then it is going to see all these things changed.  
							One. Yeah, and another algorithm will check that change data and be able to figure out what to add as a precondition in the future.  Then once it is added as a precondition in a future tree, that will be added to state, and etc.  The only one that can add predicate to a tree is the findCause maker?
				2) If tree (root) 
					1) Make new plan with the current intent (this is true with pass or fail).  Updates trees should be added to the archive, so each time we replan, things will get more and more interesting.  
						i) How do I do this?  Do I send a message to the intent engine?  Or is the intent engine listingin for trees to be done and sending its own new intent message?  I think this is the way.
					2) Once the tree is done, this is when we need to send it to be archived.  I think we need to added both failed trees and success trees, because we do not want to keep redoing our failures. . .   
						i) Sends message to save completed trees.  
	3. IntentEngine
		a. Input: State Messages: These are sent from the Roblox API
			i. Listen for the state message, and monitor the numbers. 
				1) For now, this is framed as my list of problems.  
				2) If there is an emergency, this intent engine needs to send out new intent and the urgency. 
					1) By priority, I think it is here that we need know our current intent, and we change that intent, we need to know if we are going to stop the current intent or waiting on that intent to finish.  
				3) If not an emergency, do nothing with this listener other than update numbers.  When the tree is done, which we will know listening to another message, that is when we will send new intent. 
					1) The intent should not be publicly available at all times because it comes with priority, and when it sends a message, things act.   Like, this has to be a message the verticles respond too.  This verticle should have the power to stop currently running trees. 
		b. Input: Behavior Tree Updates: These are sent from the Roblox API
			i. Listen for behavior tree completions.  When a tree completes, send the next intent to trigger the making of a new seed to they can start behaving again.  
		c. Input: Find Cause alert: These will come from Completed Trees Service
			i. I should maintain a list of problems here that helps this intent engine I think.  I think I should keep the problems queue, and only go elsewhere for desires when there is not problem. 
	4. CompletedTreesService
		a. On start up, need to open file from server for previously completed trees.  
			i. Could borrow code from the ayoKeyServer file.  I think we store them in a new file called CharacterBehaviorTrees, and store all behaviors in this list.  That way we only need a tree to be on memory once and used across all agents.  This is the only scalable way.  This is easy way to share the trees.  
		b. Input: Save Completed Trees: These are sent from the behavior tree manager
			i. First thing is we need to save this completed tree to the list of completed trees
				1) This will need to be keyed off outcome, effects, and preconditions.  That is the unique composite key we need.  Nah, needs to be keyed off of parameters too!
			ii. After we save though, this is important!!
				1) We check for if any nodes are similar with different outcomes, and investigate why.  Therefore, I feel it is this services responsibility to alert the intent verticle that there is an unknown discrepancy.  The intent engine can determine how important that discrepancy is, it is simply our job to let it know here.  
	5. SeedMaker
		a. The Loop
			i. New NPC starts up
			ii. First intent on default is null or something, but basically force it to MakeSeed_ExploreInterest(null)
			iii. Get the intent engine running from there
			iv. As soon as intent changes
				1) This could be for many reasons.  Say that it found an object that lowered its health.
			v. If intent not null, see if there are any archived actions that can move the needle on these intents. 
				1) Say the intent is to get health to 100.  What archieved actions will move me toward that intent?
				2) Probably actions with the most intent movement should be prioritized
				3) We  also need to be sure the preconditions are the same.  
		b. MakeSeed_Intent(JsonArray list of properties and their target values)
			i. The intent engine could call this when it wants to lower health, or what ever the intent was.  It should send all the intents, so the best seed will picked to meet the intents.  
			ii. This is when the main planner kicks in.  This is where we are not sure what the best seed to choose is, and we are not sure if we should ask llm to make us more trees here.  
		c. MakeSeed_ExploreInterest(String Interest)
			i. I need to build this algorithm and it will be called a lot.  I could feed an llm the world around them and ask llm what it should explore first. . .  I could do math to get things that are close around him.  
			ii. Based on an interst, which may be null, ask llm what we should explore.  
			iii. From here, we can deterministically make a behavior tree for the npc.  Like, we can simply say move to this place at this speed. Etc . . . 
		d. MakeSeed_FindCause(String predicate???)
			i. Say our health lowered and at first our intent was to increase health.  However, somehow here we need to be able to triggger an exploreCause behavior tree.  
			ii. How will this work though?  Like what if we are movingTo at one point, and then another point we moveTo and get new effects or new failures.  
				1) What if the parameters were the same?  If all of the parameters were the same, what preconditions were different?  Nah its normal to have different effects when the preconditions are different even if variables are the same.  Therefore, the better filter is, tell me cases where the parameters and the preconditions were the exact same, but the effect was different.  It is in these cases(!) where we want to find the cause - we need to start adding preconditions into a new tree and try it a few times over time to see if the precondition solves the problem?  This is what I need to find cause for.  
				2) But how do I know I found the cause?  I think what will happen when we make this new tree is that if successful, it will always be picked first, because intent is always 100 health, and why would we pick an action that made us lower health in the past?
					a) So eventually the very shity trees will end up at the bottom and will never be used!  We could clean them up later if we really wanted to. 
			iii. So I think this method is called over and over again until a new tree is made with a new precondition that gives us success, and if we hit another failure will try again.
				1) But what about my algo right above about parameters and preconditions matching?  Well if the new one fails the first time, and succeeds the second time, same algo as above, then we will find cause on those actions that have the same predicts and add more to them until we are successful.
				2) The way this would have to work is!!! Need to flag which Actions I tried to find cause for so that I do not try them again?
				3) Wait wait, shit, we should only add one predicate at a time
				4) If we only add one, then we can determinist recreate the entire progression.  We could, actions where these parameters are the same, and this predicate is the same, and there is one that is not the same.  That type of determinism is what this recursive function needs.   
				5) What I should do is evaluate the state before with the current state to cherry pick what was different, and have a precondition be to only do it when that data is the same for the one that was successful.  But still only add one at a time.  Force it to go one at a time so we know for sure what is causing and in what order and etc.  
			iv. No actually I do not think I like this idea.  Wait, another big problem here.  We only want to compare effects that were bad.  That is because sometimes there could be a bunch of trash in there that is not relevant.  So I need some where of weeding out the noise here.  
				1) !!!!!!!!!!!My perceptions need to clean out noise!!!!  Like, perception, or the state verticle, needs to only be publishing about state information we care about.  Health would be there, but a lot of other stuff would not.  
				2) But how to know when to pay attention to a blinking light!!
				3) Actually only do Ohhh, I could keep a list of predicates I am using, and that will be the list of things the Execution loop listens too.  These could leave over time too as we clear out older actions and predates we do not need, we can also clean this list out, so it will maintain itself.  
		e. MakeSeed_TryAgain(Tree tree)
			i. ?? Lol 
			ii. Like, if a seed was sent to grow, and it did not grow like we intended, we need to repair or make new seed?
		f. isNodeEqual(Node a, Node b)
			i. Need a special equals here that does not compare things like name, but does deep comparison of every node.  It is going need to be very unique for this behavior tree. 
		g. Wait, can nodes outside of these actons have preconditions and effects? Hell year??
			a. Ugh, I think tasks are going to need preconditions and effects as well as the goal nodes.  So I am going to have two node types both have preconditions and effects - I guess I can put that stuff at the node level?
		





**What about case where we do not know what is causing it?  Say health is lowing and our intent is to lower health, but how would our intent to be to find what causes health to lower?
