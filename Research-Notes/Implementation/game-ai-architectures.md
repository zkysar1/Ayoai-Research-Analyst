# Game AI Architectures

Patterns and architectures from game development conferences and industry leaders.

## The Bitter Lesson

Rich Sutton's landmark essay on AI development provides crucial guidance for building intelligent agents. http://www.incompleteideas.net/IncIdeas/BitterLesson.html

```text
The bitter lesson is based on the historical observations that 1) AI researchers have often tried to build knowledge into their agents, 2) this always helps in the short term, and is personally satisfying to the researcher, but 3) in the long run it plateaus and even inhibits further progress, and 4) breakthrough progress eventually arrives by an opposing approach based on scaling computation by search and learning. The eventual success is tinged with bitterness, and often incompletely digested, because it is success over a favored, human-centric approach.

One thing that should be learned from the bitter lesson is the great power of general purpose methods, of methods that continue to scale with increased computation even as the available computation becomes very great. The two methods that seem to scale arbitrarily in this way are *search* and *learning*.

The second general point to be learned from the bitter lesson is that the actual contents of minds are tremendously, irredeemably complex; we should stop trying to find simple ways to think about the contents of minds, such as simple ways to think about space, objects, multiple agents, or symmetries. All these are part of the arbitrary, intrinsically-complex, outside world. They are not what should be built in, as their complexity is endless; instead we should build in only the meta-methods that can find and capture this arbitrary complexity. Essential to these methods is that they can find good approximations, but the search for them should be by our methods, not by us. We want AI agents that can discover like we can, not which contain what we have discovered. Building in our discoveries only makes it harder to see how the discovering process can be done.
```

**Ayoai Impact**: The Bitter Lesson fundamentally shapes Ayoai's architecture:
- Avoid hard-coding behaviors - enable agents to learn through experience
- Focus on scalable search and learning methods over hand-crafted rules
- Build meta-methods for discovery, not repositories of specific behaviors
- Let agents discover patterns through interaction, not pre-programmed knowledge
- Embrace computational scaling as the path to emergent intelligence

## Dave Mark's Utility AI Systems

Dave Mark's pioneering work in utility-based AI provides foundational concepts for game AI architecture.

### Multi-Tasking

- Multi-Tasking https://gameai.com/example-plan.php

  - What they see: An NPC is cold and needs to build a fire. But before that, the NPC needs to bring wood to the storage area near the campfire location. To do that, the NPC needs to wander and find wood to pick up into their personal inventory so they can head back to the storage location when their arms are full. But the NPC is also hungry---not just now, but wouldn't mind storing some food in a pouch for later. And the NPC is curious about new things that it sees nearby. And the NPC is wary of bad things that are nearby and acts friendly to the forest animals that it sees. So the NPC sets out from the campfire and is looking around the area. As it sees a stick of wood, it wanders over to pick it up, bends over, grabs it, and puts it into a sling for carrying wood. It wanders again, sees another piece of wood and heads in that direction. However, before getting to it, it sees a nearby piece of fruit on the ground. The NPC diverts slightly away from the piece of wood to get the fruit, bend over, pick it up, and pop it into it's mouth. It then resumes heading for the 2nd piece of wood, picks it up, and puts it into its pouch. The NPC continues on, seeing wood and food, diverting to pick them both up if they are close---even interrupting its path at times. In the meantime, it steers clear of a baddie it sees---ignoring a piece of fruit that was too close to risk. On the way to a stick on the ground, the character waves to a friendly woodland creature, cheerfully saying hello. At some point, their food pouch is full and they aren't hungry so they stop moving to and picking up more berries and fruits. And then, when their wood bag is full, they return to the camp, dump the contents into the storage area, use some of it to build a fire, light it, and then sit down to get warm. Mission accomplished---not just getting the wood and building the fire, but collecting the food, staying safe, and greeting little furry friends.

  - What is happening? The steps involved in building a fire seem like something out of an example of a planner such as GOAP. In fact, the collection of the food could also be a GOAP plan as well. However, planners will typically pick a single goal and execute a single plan (which may change) in order to accomplish that single goal. What is extrordinary here is that these plans are running at the same time---in parallel! Sometimes they would even be executing one step of one plan, discover something else that was more convenient at the time (from the other plan?), execute that, and resume what it was doing in the first plan (if it was still a good idea). Additionally, these two plans are being interupted by general "life" moments. At the very simple level, the agent is moving to, picking up, and storing or using the items because they are tagged like "Flammable" or "Edible". While the agent could have been left to simply pick up objects with the appropriate tags as it sees them, that takes away not only the reason for doing so, but what comes next once they are picked up. In our implementation, the IAUS has a series of behaviors that, at their core, have the same premise---in this case, "I am cold" (or need to cook food, etc.). However, thinking in terms of the series, each behavior has another consideration specifying a prerequisite that needs to be in place. So, while "I am cold" certainly is a valid reason to build a fire, the "build fire" behavior requires that there be a certain amount of wood in the storage pile. If the pile doesn't have enough, then "I am cold" plus "not enough wood" would lead to the "search for wood" behavior. This would continue on so that even "pick up wood" has the same "I am cold" consideration plus the others that go after. The result is that the behaviors get self-assembled in order of their need and, as they are satisfied, the progression towards the ultimate goal continues. But because, in the IAUS, we are evaluating all of our possible behaviors every think cycle, we can be thinking about "move to wood" and "move to food" at the same time. In the example case, if we are moving towards a piece of wood and see a convenient pice of fruit, we can execute that and, when it is no longer in play, the "move to wood" we were doing will likely be back to the highest scoring behavior to execute. The parallel plans all stay intact as long as they are appropriate!

### Infinite Axis Utility System

- Infinite Axis Utility System

  - The Infinite Axis Utility System (IAUS) provides the "brains" for game agents from NPCs, to squads, to armies, and even to otherwise inanimate objects. It is entirely data-driven through a database oriented design tool (included) that allows designers to create, manipulate, and tune the behaviors in subtle, yet very understandable ways. Because of the nature of utliity AI, much of the variation spills out of the nuanced nature of the behavior scoring and selection system. Additionally, because of this difference from the standard if/then approach to behavior selection, the behavior systems are less rigid and, therefore, less brittle. The AI rarely---if ever---get's "stuck" in a behavior that it shouldn't because it can't get out of what it is doing. Put another way, the AI is analyzing the complex situations in ways that make sense to us but are enormously difficult to script out. (For those familiar with computational complexity theory, the IAUS is helping you do the NP-Hard problems by making sure you don't have to think of everything!) The IAUS exists as a stand-alone black box that can be installed into a project codebase with minimal work. We have both C++ and C# versions based on the needs to the client. In theory, minus any specific custom language or library issues, this means that this core AI system can be installed on day 1 and work can begin on hooking it up to world information, existing behavior and animation code, and behavior logic can be authored in the tool we have developed for that purpose.

**Potential Enhancements for Ayoai**:
- Replace simple summation with neural network processing of utility inputs
- Enable hierarchical actions with embedded sub-actions
- Allow user-defined actions and evaluation axes for customization

**Ayoai Impact**: Dave Mark's work is foundational for game AI:
- Parallel goal execution (not sequential planning)
- Utility-based decision making
- Self-assembling behaviors based on needs
- Data-driven behavior tuning
- Avoiding brittle if/then logic
- Perfect model for Ayoai's behavior selection

## GDC Behavior Tree Notes

Behavior Trees: Three Ways of Cultivating Strong AI

https://www.gdcvault.com/play/1012416/Behavior-Trees-Three-Ways-of

Behavior trees have high-level decision nodes at the top layers with low-level action sequences at the bottom. A critical design consideration: treating trees like state machines (e.g., "in cover" state) can lead to agents getting locked into behaviors. Each behavior must continuously monitor whether its assumptions remain valid. Nodes should be forward-looking, anticipating changes rather than reacting after failures occur.  

![A white text on a black background Description automatically generated](../images/media/image198.png)

![A white background with black text Description automatically generated](../images/media/image199.png)

**Key Distinction**: Behavior trees are reactive systems, not planning systems. While planners look ahead to sequence future actions, behavior trees respond to current conditions. This fundamental difference shapes how we design agent decision-making architectures.

**Ayoai Impact**: Key insights from GDC:
- Trees are reactive, not planning
- Need forward-looking nodes
- Monitor assumption changes
- Avoid getting locked in states

## Key Concepts from Industry Research

### Affordances and Agent Capabilities

Google's robotics research highlights the importance of "affordances" - what an agent can actually do with reasonable success. Agents must understand their own limitations and capabilities. This ranges from simple tasks ("move forward") to complex, multi-step actions requiring understanding of both abilities and environment ("clean up spill and bring alternative drink"). https://techcrunch.com/2022/08/16/google-robotics/

### Autonomous Goal-Directed Agents

**BabyAGI** demonstrates autonomous task execution and goal-oriented behavior. https://twitter.com/yoheinakajima/status/1642881722495954945?s=19

**Decentraland NPCs** showcase AI integration in virtual worlds. https://decentraland.org/blog/announcements/ai-npcs-herald-the-beginning-of-ai-in-decentraland

### The Sims: Exemplar of Behavior Tree Architecture

The Sims franchise provides excellent examples of behavior tree implementation:

**Core Systems**:
- **Needs/Skills/Aspirations**: Drive selection of next behaviors
- **Central Tree Selector**: Manages active behavior trees
- **Skill Queue**: Maintains list of pending actions
- **Interruptible Sequences**: Player can cancel agent actions mid-execution

**Implementation Details**:
- Complex action sequences (e.g., cooking involves: walk to fridge → grab ingredients → walk to counter → chop → cook → serve → eat)
- Failure propagation through tree nodes when interrupted
- Autonomous behavior based on needs (hunger, hygiene, social, etc.)
- Wellbeing maintenance when not under direct player control

### Architectural Considerations

**NPC Portability**: Agents should be:
- Copyable and editable across different worlds
- Able to carry historical antecedents
- Capable of adapting when environmental assumptions change

**Social Structures**: Beyond individual agents:
- Company/organization AI
- Social faction systems
- Multi-level agent hierarchies

**Intelligence vs Education**: Observable behaviors (education) differ from underlying decision-making capabilities (intelligence)

### Emerging Technologies

**Fractal Neural Networks**: Novel architectures for hierarchical processing. http://sohl-dickstein.github.io/2024/02/12/fractal.html

**Edge Computing for AI**: Debate on shifting model workloads to end-user devices for cost-effective scaling. https://a16z.com/the-neverending-game-how-ai-will-create-a-new-category-of-games/

## Ayoai Impact

These industry patterns provide crucial implementation guidance:

1. **Affordance System**: Agents must understand their capabilities and limitations
2. **Need-Driven Architecture**: Skills, aspirations, and needs drive behavior selection
3. **Interruptible Sequences**: Support graceful interruption and task switching
4. **Skill Queuing**: Maintain pending action lists for complex behaviors
5. **Central Coordination**: Tree selector manages active behaviors
6. **Autonomous Maintenance**: Self-directed wellbeing and goal pursuit
7. **Portability**: Agents that can transfer between worlds with history intact
8. **Social Hierarchies**: Support for organizational and faction-based AI

## Integration with Ayoai Platform

Game AI architecture research suggests:

1. **Core Principles**
   - Follow the Bitter Lesson - scalable methods
   - Utility-based decision making
   - Parallel goal execution
   - Reactive behavior trees
   - Affordance awareness

2. **Architecture Patterns**
   - IAUS for behavior scoring
   - Forward-looking nodes
   - Self-assembling behaviors
   - Data-driven tuning
   - Interruptible sequences

3. **Implementation Approach**
   - Start with simple affordances
   - Build utility evaluation system
   - Support parallel goals
   - Enable behavior interruption
   - Track needs/skills/aspirations

This research provides the foundation for Ayoai's behavior architecture, combining utility theory with behavior trees for emergent, realistic NPC behaviors.
