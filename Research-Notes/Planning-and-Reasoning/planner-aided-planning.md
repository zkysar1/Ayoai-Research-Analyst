# Planner-Aided Planning

Employ an external planner to elevate the planning procedure, while the LLM mainly plays the role in formalizing the tasks. Challenges arise when confronted with environments featuring intricate constraints, such as mathematical problem-solving or generating admissible actions.

## External Planner Integration

### Planner Categories

**Symbolic Planners**:
- Foundation of automated planning for decades
- Based on formalized models (PDDL, ASP)
- Use symbolic reasoning for optimal path finding
- Guarantee constraint satisfaction

**Neural Planners**:
- Learning-based approaches
- Handle uncertainty and partial observability
- Less common in current implementations

### LLM Role in Planner-Aided Systems

**Primary Functions**:
1. **Task Formalization**: Convert natural language to formal representations
2. **Feedback Parsing**: Interpret planner outputs for users
3. **Reasoning Support**: Provide additional context and explanations
4. **Model Construction**: Accelerate symbolic model creation

### Key Advantages

**Constraint Satisfaction**:
- Symbolic planners guarantee feasible plans
- Handle complex preconditions and effects
- Ensure logical consistency

**Reduced Expert Dependency**:
- LLMs democratize symbolic model construction
- Faster prototyping and iteration
- Natural language interface to formal systems

### Future Directions

**Hybrid Approaches**:
- Statistical AI (LLMs) + Symbolic AI (planners)
- Leverage strengths of both paradigms
- Maintain LLM flexibility with planner rigor

## Implementation Considerations

### Antecedent Engine Design

**Environmental Events**:
- Randomness engine for weather, disasters, etc.
- State transitions (outdoor→indoor)
- Impact on intrinsic motivation systems
- Dynamic world simulation

### PDDL to Behavior Tree Pipeline

**Translation Requirements**:
- PDDL plans as high-level goals
- Behavior trees for execution details
- Generative sub-actions (e.g., "dance together" includes approach, interact, synchronize)
- Memory and perception actions in domain

### Action Design Principles

**Goal Justification**:
- LLM explains "why" for each goal
- Impact assessment ("what happens if not done")
- Preference modeling

**Action Visibility**:
- Public actions (observable by others)
- Private actions (internal state changes)
- Action reasons (motivation tracking)

### PDDL vs Behavior Trees Comparison

**Behavior Trees (BTs)**:
- **Domain**: Game development, robotics
- **Strengths**: 
  - Real-time decision making
  - Modular and debuggable
  - Dynamic environment adaptation
  - Developer-friendly
- **Best For**: Reactive behaviors, immediate responses

**Planning Domain Definition Language (PDDL)**:
- **Domain**: Automated planning, scheduling
- **Strengths**:
  - Formal problem representation
  - Guaranteed optimal/valid plans
  - Complex constraint handling
  - Domain/problem separation
- **Best For**: Offline planning, complex goal achievement

**Hybrid Approach for Ayoai**:
1. Use PDDL for high-level goal planning
2. Translate plans to behavior trees for execution
3. Maintain reactivity while ensuring goal achievement

### PDDL to Behavior Tree Translation

**Translation Framework**:
- Input: PDDL domain and problem files
- Process: Map planning constructs to tree nodes
- Output: Hierarchical behavior tree with condition checks and action sequences

**Translation Pattern**:
1. Goal decomposition → Root selector node
2. Preconditions → Condition nodes
3. Actions → Action leaf nodes
4. Effects → State updates

### Planning Paradigm Selection

**Reference**: [Planning Paradigms Comparison](https://arxiv.org/abs/1804.08229)

**ASP vs PDDL**:
- **ASP (Answer Set Programming)**: Logic programming, handles defaults and exceptions
- **PDDL**: Action-based planning, explicit preconditions/effects
- **Selection Criteria**: Problem structure, constraint types, reasoning requirements

### PDDL Fundamentals

**Resource**: [PDDL Tutorial](http://pddl4j.imag.fr/pddl_tutorial.html#defining-the-problem)

**Core Components**:
- **Objects**: Entities in the world
- **Predicates**: Boolean properties of objects
- **Initial State**: Starting world configuration
- **Goal Specification**: Desired end states
- **Actions/Operators**: State transformation rules

**Key Insight**: PDDL provides a common format for any planner, enabling tool-agnostic planning.

    - Example domain file
      - (define (domain logistics)
      - (:requirements :strips :typing)
      - (:types city place physobj - object
        package vehicle - physobj
        truck airplane - vehicle
        airport location - place
        )
      - (:predicates (in-city ?loc - place ?city - city)
                     (at ?obj - physobj ?loc - place)
                     (in ?pkg - package ?veh - vehicle))
      - (:action load-truck
        :parameters (?pkg - package ?truck - truck ?loc - place)
        :precondition (and (at ?truck ?loc) (at ?pkg ?loc))
        :effect (and (not (at ?pkg ?loc)) (in ?pkg ?truck))
        )
      - (:action load-airplane
        :parameters (?pkg - package ?airplane - airplane ?loc - place)
        :precondition (and (at ?pkg ?loc) (at ?airplane ?loc))
        :effect (and (not (at ?pkg ?loc)) (in ?pkg ?airplane))
        )
      - (:action unload-truck
        :parameters (?pkg - package ?truck - truck ?loc - place)
        :precondition (and (at ?truck ?loc) (in ?pkg ?truck))
        :effect (and (not (in ?pkg ?truck)) (at ?pkg ?loc))
        )
      - (:action unload-airplane
        :parameters (?pkg - package ?airplane - airplane ?loc - place)
        :precondition (and (in ?pkg ?airplane) (at ?airplane ?loc))
        :effect (and (not (in ?pkg ?airplane)) (at ?pkg ?loc))
        )
      - (:action fly-airplane
        :parameters (?airplane - airplane ?loc-from - airport ?loc-to - airport)
        :precondition (at ?airplane ?loc-from)
        :effect (and (not (at ?airplane ?loc-from)) (at ?airplane ?loc-to))
        )
      - (:action drive-truck
        :parameters (?truck - truck ?loc-from - place ?loc-to - place ?city - city)
        :precondition (and (at ?truck ?loc-from) (in-city ?loc-from ?city) (in-city ?loc-to ?city))
        :effect (and (not (at ?truck ?loc-from)) (at ?truck ?loc-to))
        )
        )

    - Example problem file:
      - (define (problem pb_logistics)
        (:domain logistics)
      - (:objects
        plane - airplane
        truck - truck
        cdg lhr - airport
        south north - location
        paris london - city
        p1 p2 - package)
      - (:init (in-city cdg paris)
        (in-city lhr london)
        (in-city north paris)
        (in-city south paris)
        (at plane lhr)
        (at truck cdg)
        (at p1 lhr)
        (at p2 lhr)
        )
      - (:goal (and (at p1 north) (at p2 south)))
        )

### GOAP (Goal-Oriented Action Planning)

**Resources**:
- [Tutorial: GOAP for Smarter AI](https://gamedevelopment.tutsplus.com/tutorials/goal-oriented-action-planning-for-a-smarter-ai--cms-20793)
- [Medium: Understanding GOAP](https://medium.com/@vedantchaudhari/goal-oriented-action-planning-34035ed40d0b)

**Implementation Libraries**:
- [PyGOAP](https://github.com/bitcraft/pygoap/tree/master/pygoap) - Python implementation
- [GOAP Library](https://github.com/agoose77/GOAP) - Alternative Python approach

**Key Features**: Uses A* search for optimal action sequences

### Hierarchical Planning Approaches

**HTN (Hierarchical Task Network)**:
- Task decomposition focus
- Similar structure to behavior trees
- Methods decompose tasks into subtasks
- [GPT-HTN-Planner](https://github.com/DaemonIB/GPT-HTN-Planner): LLM-based task decomposition

**HTN vs GOAP Comparison**:
- **HTN**: Top-down decomposition, predefined methods
- **GOAP**: Bottom-up search, dynamic action selection
- **Hybrid Potential**: HTN for structure, GOAP for flexibility

**Example Task Decomposition**:
```
Task: "Make sandwich"
├── Get ingredients
│   ├── Get bread
│   └── Get ham
├── Assemble sandwich
│   └── Place ham between bread
└── Serve
    └── Place on plate
```

**Key Design Decision**: The "replan required" trigger determines recursive depth - if the initial plan is sufficient, no further decomposition occurs. This creates an efficient, context-aware planning system.

### PDDL Implementation Resources

**Domain File Construction**:
- [LLMs as World Models for Planning](https://arxiv.org/pdf/2305.14909.pdf)
- [GitHub: LLMs-World-Models-for-Planning](https://github.com/GuanSuns/LLMs-World-Models-for-Planning)
- Limitation: Manual problem file creation required

**Problem File Generation**:
- **LLM+P** ([Paper](https://arxiv.org/abs/2304.11477), [Code](https://github.com/Cranial-XIX/llm-pddl/blob/main/main.py)): Complete PDDL generation
- **PROC2PDDL** ([Code](https://github.com/zharry29/proc2pddl)): Open-domain planning
- **PDDL Parser** ([GitHub](https://github.com/pucrs-automated-planning/pddl-parser/tree/master/pddl_parser)): Validation tool

**Multi-Agent Framework**:
- [AgentLite](https://github.com/SalesforceAIResearch/AgentLite): Hierarchical agent management

**Implementation Pipeline**:
1. Determine domain requirements
2. Generate PDDL domain file
3. Create PDDL problem file
4. Plan and generate behavior trees
5. Optimize and deploy trees

### Planning Hierarchy Design

**Three-Layer Architecture**:
1. **Strategic Layer (PDDL)**: High-level goals ("go to park", "cook dinner")
2. **Tactical Layer (GOAP)**: Mid-level action selection
3. **Execution Layer (Behavior Trees)**: Low-level actions ("move left", "pick up item")

**Key Insights**:
- PDDL defines the planning problem, not the solution format
- Plans can be output as behavior trees
- PDDL must encompass all possible behavior tree actions
- The PDDL generator effectively defines available behavior tree nodes

**Ayoai Impact**: PDDL provides a formal foundation for agent planning:
- Domain files define available actions and preconditions
- Problem files specify initial state and goals
- Can translate PDDL plans to behavior trees
- Supports hierarchical planning (HTN)
- External planners guarantee correct plans
- LLMs can generate PDDL from natural language

## LLM+P

- LLM+P [Liu et al., 2023a] [https://arxiv.org/abs/2304.11477](https://arxiv.org/abs/2304.11477) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Large language models (LLMs) have demonstrated remarkable zero-shot generalization abilities: state-of-the-art chatbots can provide plausible answers to many common questions that arise in daily life. However, so far, LLMs cannot reliably solve long-horizon planning problems. By contrast, classical planners, once a problem is given in a formatted way, can use efficient search algorithms to quickly identify correct, or even optimal, plans. In an effort to get the best of both worlds, this paper introduces LLM+P, the first framework that incorporates the strengths of classical planners into LLMs. LLM+P takes in a natural language description of a planning problem, then returns a correct (or optimal) plan for solving that problem in natural language. LLM+P does so by first converting the language description into a file written in the planning domain definition language (PDDL), then leveraging classical planners to quickly find a solution, and then translating the found solution back into natural language. Along with LLM+P, we define a diverse set of different benchmark problems taken from common planning scenarios. Via a comprehensive set of experiments on these benchmark problems, we find that LLM+P is able to provide optimal solutions for most problems, while LLMs fail to provide even feasible plans for most problems. The code and results are publicly available at [this https URL](https://github.com/Cranial-XIX/llm-pddl.git).

  - Descriptions from this planning survey:
    - Leveraging the semantic understanding and coding capabilities of LLM, the authors organize problems into textual language prompts inputted to LLM. This prompts LLM to organize the actions within the environment and specified tasks into the format of the PDDL language. Subsequently, after obtaining a formalized description, the authors employ the Fast-Downward 1 solver for the planning process.
    - symbolic planner

  - Zak thoughts
    - Has code!!! Looks like an install. [https://github.com/Cranial-XIX/llm-pddl/tree/main](https://github.com/Cranial-XIX/llm-pddl/tree/main)
    - Good but, meh, nothing to really add.
    - No wait - this one creates problem files!! This is the one I need. If I an can create these problem files, I can start planning. Hmm.

**Ayoai Impact**: LLM+P demonstrates the power of hybrid approaches:
- Natural language to PDDL translation
- Optimal planning guarantees
- Creates both domain AND problem files
- Code available for implementation
- Bridges gap between LLM flexibility and planner correctness

## LLM-DP

- LLM+P, LLM-DP [Dagan et al., 2023] [https://arxiv.org/abs/2308.06391](https://arxiv.org/abs/2308.06391) (found from: Planning-of-LLM-Agents)

  - Abstract
    - While Large Language Models (LLMs) can solve many NLP tasks in zero-shot settings, applications involving embodied agents remain problematic. In particular, complex plans that require multi-step reasoning become difficult and too costly as the context window grows. Planning requires understanding the likely effects of one's actions and identifying whether the current environment satisfies the goal state. While symbolic planners find optimal solutions quickly, they require a complete and accurate representation of the planning problem, severely limiting their use in practical scenarios. In contrast, modern LLMs cope with noisy observations and high levels of uncertainty when reasoning about a task. Our work presents LLM Dynamic Planner (LLM-DP): a neuro-symbolic framework where an LLM works hand-in-hand with a traditional planner to solve an embodied task. Given action-descriptions, LLM-DP solves Alfworld faster and more efficiently than a naive LLM ReAct baseline.

  - Descriptions from this planning survey:
    - Specifically designed for dynamic interactive environments. Upon receiving feedback from the environment, LLM processes the information, formalizes it into PDDL language, and then employs a BFS [Lipovetzky et al., 2014] solver to generate a plan.
    - symbolic planner

### Action Selection Architecture

**Key Innovation**: Action Selector (AS) module that:
- Processes multiple plans from the planner
- Selects optimal action (typically shortest plan)
- Handles failure cases with re-sampling strategy

![System architecture](../images/media/image149.png)
![Sample prompt template](../images/media/image150.png)

**Ayoai Impact**: LLM-DP shows dynamic replanning capabilities:
- Handles dynamic environments (critical for games)
- Action selector module for plan execution
- Fallback strategies when plans fail
- BFS solver for efficient search
- Neuro-symbolic approach combines best of both worlds

## LLM+PDDL

- LLM+PDDL [Guan et al., 2023] [https://arxiv.org/abs/2305.14909](https://arxiv.org/abs/2305.14909) (found from: Planning-of-LLM-Agents)

  - Abstract
    - There is a growing interest in applying pre-trained large language models (LLMs) to planning problems. However, methods that use LLMs directly as planners are currently impractical due to several factors, including limited correctness of plans, strong reliance on feedback from interactions with simulators or even the actual environment, and the inefficiency in utilizing human feedback. In this work, we introduce a novel alternative paradigm that constructs an explicit world (domain) model in planning domain definition language (PDDL) and then uses it to plan with sound domain-independent planners. To address the fact that LLMs may not generate a fully functional PDDL model initially, we employ LLMs as an interface between PDDL and sources of corrective feedback, such as PDDL validators and humans. For users who lack a background in PDDL, we show that LLMs can translate PDDL into natural language and effectively encode corrective feedback back to the underlying domain model. Our framework not only enjoys the correctness guarantee offered by the external planners but also reduces human involvement by allowing users to correct domain models at the beginning, rather than inspecting and correcting (through interactive prompting) every generated plan as in previous work. On two IPC domains and a Household domain that is more complicated than commonly used benchmarks such as ALFWorld, we demonstrate that GPT-4 can be leveraged to produce high-quality PDDL models for over 40 actions, and the corrected PDDL models are then used to successfully solve 48 challenging planning tasks. Resources, including the source code, are released at: [this https URL](https://guansuns.github.io/pages/llm-dm).

  - Descriptions from this planning survey:
    - Also utilizes the PDDL language to formalize the task, incorporating an additional step for manual verification to check for potential issues in the PDDL model generated by LLM. During the planning process, the authors propose using the plan generated by LLM as an initial heuristic solution to accelerate the search process of local search planners, such as LPG [Gerevini and Serina, 2002].
    - symbolic planner

### Implementation Framework

**Code Repository**: [https://github.com/GuanSuns/LLMs-World-Models-for-Planning](https://github.com/GuanSuns/LLMs-World-Models-for-Planning)

**Advanced Features**:

1. **Validation Loop Architecture**:
   - PDDL model validates LLM-generated plans
   - Iterative refinement through re-prompting
   - VAL (validator) provides corrective feedback

2. **Human Preference Integration**:
   - Classical planners generate unconventional but valid actions
   - LLMs naturally encode implicit human preferences
   - Hybrid approach preserves both correctness and naturalness

3. **ReAct Integration**:
   - Modified prompt design with detailed action descriptions
   - Natural language translation of PDDL models
   - Parameters, preconditions, and effects in readable format

![PDDL validation system](../images/media/image151.png)
![System overview diagram](../images/media/image152.png)

**Ayoai Impact**: LLM+PDDL provides comprehensive planning framework:
- Human feedback integration for model correction
- PDDL validation ensures correctness
- Handles complex domains (40+ actions)
- LLMs understand implicit human preferences
- Combines planner correctness with LLM common sense
- Perfect for building Ayoai's planning architecture

## Generalized Planning in PDDL

- Generalized Planning in PDDL Domains with Pretrained Large Language Models [https://arxiv.org/abs/2305.11014v2](https://arxiv.org/abs/2305.11014v2)

  - Abstract:
    - Recent work has considered whether large language models (LLMs) can function as planners: given a task, generate a plan. We investigate whether LLMs can serve as generalized planners: given a domain and training tasks, generate a program that efficiently produces plans for other tasks in the domain. In particular, we consider PDDL domains and use GPT-4 to synthesize Python programs. We also consider (1) Chain-of-Thought (CoT) summarization, where the LLM is prompted to summarize the domain and propose a strategy in words before synthesizing the program; and (2) automated debugging, where the program is validated with respect to the training tasks, and in case of errors, the LLM is re-prompted with four types of feedback. We evaluate this approach in seven PDDL domains and compare it to four ablations and four baselines. Overall, we find that GPT-4 is a surprisingly powerful generalized planner. We also conclude that automated debugging is very important, that CoT summarization has non-uniform impact, that GPT-4 is far superior to GPT-3.5, and that just two training tasks are often sufficient for strong generalization.

### Implementation and Output

**Code Repository**: [https://github.com/tomsilver/llm-genplan/tree/master](https://github.com/tomsilver/llm-genplan/tree/master)

**Architecture Features**:
- Generates Python planning programs (adaptable to other languages)
- Uses PDDL format for problem representation
- Focuses on planning execution rather than domain/problem generation

![Planning strategy diagram](../images/media/image153.png)

**Ayoai Impact**: Shows LLMs as generalized planners:
- GPT-4 can generate planning programs
- Automated debugging improves reliability
- Only 2 training tasks needed for generalization
- Python output could be adapted to Lua for Roblox
- Chain-of-Thought improves planning strategy

## Human-Level Forecasting

- Approaching Human-Level Forecasting with Language Models [https://arxiv.org/abs/2402.18563](https://arxiv.org/abs/2402.18563)

  - Abstract
    - Forecasting future events is important for policy and decision making. In this work, we study whether language models (LMs) can forecast at the level of competitive human forecasters. Towards this goal, we develop a retrieval-augmented LM system designed to automatically search for relevant information, generate forecasts, and aggregate predictions. To facilitate our study, we collect a large dataset of questions from competitive forecasting platforms. Under a test set published after the knowledge cut-offs of our LMs, we evaluate the end-to-end performance of our system against the aggregates of human forecasts. On average, the system nears the crowd aggregate of competitive forecasters, and in some settings surpasses it. Our work suggests that using LMs to forecast the future could provide accurate predictions at scale and help to inform institutional decision making.

### Prompt Engineering Insights

**Key Value**: Comprehensive prompt templates for each planning stage

![Prompt structure example](../images/media/image154.png)
![System architecture](../images/media/image155.png)
![Detailed prompt template](../images/media/image156.png)

**Ayoai Impact**: Forecasting capabilities for predictive agents:
- Agents can predict player behavior
- Retrieval-augmented approach for context
- Matches human-level forecasting
- Useful for strategic NPC planning
- Great prompt examples for implementation

## LLM-Planner

- LLM-Planner: Few-Shot Grounded Planning for Embodied Agents with Large Language Models [https://arxiv.org/abs/2212.04088](https://arxiv.org/abs/2212.04088)

  - Abstract
    - This study focuses on embodied agents that can follow natural language instructions to complete complex tasks in a visually-perceived environment. Existing methods rely on a large amount of (instruction, gold trajectory) pairs to learn a good policy. The high data cost and poor sample efficiency prevents the development of versatile agents that are capable of many tasks and can learn new tasks quickly. In this work, we propose a novel method, LLM-Planner, that harnesses the power of large language models (LLMs) such as GPT-3 to do few-shot planning for embodied agents. We further propose a simple but effective way to enhance LLMs with physical grounding to generate plans that are grounded in the current environment. Experiments on the ALFRED dataset show that our method can achieve very competitive few-shot performance, even outperforming several recent baselines that are trained using the full training data despite using less than 0.5% of paired training data. Existing methods can barely complete any task successfully under the same few-shot setting. Our work opens the door for developing versatile and sample-efficient embodied agents that can quickly learn many tasks.

### Grounding Architecture

![Physical grounding system](../images/media/image157.png)

**Ayoai Impact**: LLM-Planner demonstrates few-shot learning capabilities:
- Less than 0.5% training data needed
- Physical grounding for environment awareness
- Opens door for versatile agents
- Perfect for rapidly adding new behaviors to Ayoai

## Goal-Directed Dialogue via RL

- Zero-Shot Goal-Directed Dialogue via RL on Imagined Conversations [https://arxiv.org/abs/2311.05584](https://arxiv.org/abs/2311.05584)

  - Abstract
    - Large language models (LLMs) have emerged as powerful and general solutions to many natural language tasks. However, many of the most important applications of language generation are interactive, where an agent has to talk to a person to reach a desired outcome. For example, a teacher might try to understand their student's current comprehension level to tailor their instruction accordingly, and a travel agent might ask questions of their customer to understand their preferences in order to recommend activities they might enjoy. LLMs trained with supervised fine-tuning or "single-step" RL, as with standard RLHF, might struggle which tasks that require such goal-directed behavior, since they are not trained to optimize for overall conversational outcomes after multiple turns of interaction. In this work, we explore a new method for adapting LLMs with RL for such goal-directed dialogue. Our key insight is that, though LLMs might not effectively solve goal-directed dialogue tasks out of the box, they can provide useful data for solving such tasks by simulating suboptimal but human-like behaviors. Given a textual description of a goal-directed dialogue task, we leverage LLMs to sample diverse synthetic rollouts of hypothetical in-domain human-human interactions. Our algorithm then utilizes this dataset with offline reinforcement learning to train an interactive conversational agent that can optimize goal-directed objectives over multiple turns. In effect, the LLM produces examples of possible interactions, and RL then processes these examples to learn to perform more optimal interactions. Empirically, we show that our proposed approach achieves state-of-the-art performance in various goal-directed dialogue tasks that include teaching and preference elicitation.

### Imagination-Based Training

**Key Innovation**: Pre-generate synthetic dialogues for training

![Imagination step process](../images/media/image158.png)

**Ayoai Impact**: Goal-directed dialogue is essential for NPC interactions:
- Multi-turn conversation optimization
- Imagined conversations for training data
- Perfect for NPCs with specific goals (shopkeeper, quest-giver)
- RL on synthetic data reduces real player testing needs

## LLM+ASP

- LLM+ASP [Yang et al., 2023b] [https://arxiv.org/abs/2307.07696](https://arxiv.org/abs/2307.07696) (found from: Planning-of-LLM-Agents)

  - Abstract
    - While large language models (LLMs), such as GPT-3, appear to be robust and general, their reasoning ability is not at a level to compete with the best models trained for specific natural language reasoning problems. In this study, we observe that a large language model can serve as a highly effective few-shot semantic parser. It can convert natural language sentences into a logical form that serves as input for answer set programs, a logic-based declarative knowledge representation formalism. The combination results in a robust and general system that can handle multiple question-answering tasks without requiring retraining for each new task. It only needs a few examples to guide the LLM's adaptation to a specific task, along with reusable ASP knowledge modules that can be applied to multiple tasks. We demonstrate that this method achieves state-of-the-art performance on several NLP benchmarks, including bAbI, StepGame, CLUTRR, and gSCAN. Additionally, it successfully tackles robot planning tasks that an LLM alone fails to solve.

  - Descriptions from this planning survey:
    - Transforms problems described in natural language by LLM into atomic facts, converting tasks into ASP problems. Subsequently, the ASP solver CLINGO is utilized to generate plans.
    - symbolic planner

### ASP Implementation

**Code Repository**: [https://github.com/azreasoners/LLM-ASP](https://github.com/azreasoners/LLM-ASP)

**Framework Overview**: 
- LLM serves as semantic parser
- Converts natural language to atomic facts
- ASP solver (CLINGO) processes facts with background rules
- Output interpreted as predictions

![ASP system architecture](../images/media/image159.png)
![Example ASP encoding](../images/media/image160.png)
![ASP rule structure](../images/media/image161.png)

**Ayoai Impact**: ASP provides another formal reasoning approach:
- Answer Set Programming for logical reasoning
- Reusable knowledge modules
- Few-shot adaptation to new tasks
- Alternative to PDDL for certain problem types

## Option Critic Architecture

- Option Critic Architecture (found from: Lyfe Agents [Kaiya et al., 2023]) : [https://ojs.aaai.org/index.php/AAAI/article/view/10916](https://ojs.aaai.org/index.php/AAAI/article/view/10916)

  - Abstract
    - To tackle this challenge, we take ideas from hierarchical reinforcement learning (HRL) in machine learning (Bacon et al., 2017; Sutton et al., 1999) and the brain (Graybiel, 1998). In HRL, a "manager" chooses an option or high-level action that lasts for an extended amount of time while subsequent low-level actions are selected by a "worker". This design can allow the manager to focus on long-horizon decision making, see Pateria et al. (2021) for a review. [https://dl.acm.org/doi/10.1145/3453160]. This leads to many, but perhapes this most interesting: Deep Hierarchical Approach to Lifelong Learning in Minecraft. [https://arxiv.org/abs/1604.07255](https://arxiv.org/abs/1604.07255). We propose a lifelong learning system that has the ability to reuse and transfer knowledge from one task to another while efficiently retaining the previously learned knowledge-base. Knowledge is transferred by learning reusable skills to solve tasks in Minecraft, a popular video game which is an unsolved and high-dimensional lifelong learning problem. These reusable skills, which we refer to as Deep Skill Networks, are then incorporated into our novel Hierarchical Deep Reinforcement Learning Network (H-DRLN) architecture using two techniques: (1) a deep skill array and (2) skill distillation, our novel variation of policy distillation (Rusu et. al. 2015) for learning skills.

**Historical Context**: Early hierarchical RL approach that influenced modern implementations.

**Ayoai Impact**: Hierarchical RL aligns with Ayoai's architecture:
- Manager/worker separation (like perception verticles)
- Reusable skills (behavior tree nodes)
- Lifelong learning capabilities
- Proven in Minecraft (similar to Roblox)

## PROC2PDDL

- PROC2PDDL: Open-Domain Planning Representations from Texts [https://arxiv.org/abs/2403.00092v1](https://arxiv.org/abs/2403.00092v1)

  - Abstract
    - Planning in a text-based environment continues to be a major challenge for AI systems. Recent approaches have used language models to predict a planning domain definition (e.g., PDDL) but have only been evaluated in closed-domain simulated environments. To address this, we present Proc2PDDL , the first dataset containing open-domain procedural texts paired with expert-annotated PDDL representations. Using this dataset, we evaluate state-of-the-art models on defining the preconditions and effects of actions. We show that Proc2PDDL is highly challenging, with GPT-3.5's success rate close to 0% and GPT-4's around 35%. Our analysis shows both syntactic and semantic errors, indicating LMs' deficiency in both generating domain-specific prgorams and reasoning about events. We hope this analysis and dataset helps future progress towards integrating the best of LMs and formal planning.

### Implementation Challenges

**Code Repository**: [https://github.com/zharry29/proc2pddl](https://github.com/zharry29/proc2pddl)

**Domain Types**:
- **Closed-domain**: Predefined, limited action/object sets
- **Open-domain**: Arbitrary actions and objects from natural language

**Performance Reality**: GPT-3.5 near 0%, GPT-4 ~35% success rate

![Problem file generation](../images/media/image162.png)
![PDDL conversion example](../images/media/image163.png)

**Ayoai Impact**: PROC2PDDL reveals current limitations:
- Open-domain planning is still challenging
- Low success rates indicate need for hybrid approaches
- Dataset available for training/testing
- Ayoai's controlled domain may perform better

## CALM

- CALM [Yao et al., 2020a] (1 of 2 mentions) [https://arxiv.org/abs/2010.02903](https://arxiv.org/abs/2010.02903) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Text-based games present a unique challenge for autonomous agents to operate in natural language and handle enormous action spaces. In this paper, we propose the Contextual Action Language Model (CALM) to generate a compact set of action candidates at each game state. Our key insight is to train language models on human gameplay, where people demonstrate linguistic priors and a general game sense for promising actions conditioned on game history. We combine CALM with a reinforcement learning agent which re-ranks the generated action candidates to maximize in-game rewards. We evaluate our approach using the Jericho benchmark, on games unseen by CALM during training. Our method obtains a 69% relative improvement in average game score over the previous state-of-the-art model. Surprisingly, on half of these games, CALM is competitive with or better than other models that have access to ground truth admissible actions. Code and data are available at [this https URL](https://github.com/princeton-nlp/calm-textgame).

  - Descriptions from this planning survey:
    - Proposed an early approach that combines a language model with an RL-based neural planner. One language model processes textual environmental information, generating a set of candidate actions as priors based on the environmental information. A DRRN policy network is then employed to re-rank these candidate actions, ultimately selecting the optimal action.
    - Neural Planner - Neural planners are deep models trained on collected planning data with reinforcement learning or imitation learning techniques, showing effective planning abilities within the specific domain. For instance, DRRN [He et al., 2015] models the planning process as a Markov Decision Process through reinforcement learning, training a policy network to obtain a deep decision model. Decision Transformer (DT) [Chen et al., 2021a] empowers a transformer model to clone human decision-making behavior with planning data. However, when faced with complex and less frequently encountered problems, where training data is scarce, these small models tend to perform poorly due to insufficient generalization ability. Therefore, several works explore combining an LLM with a light-weight neural planner, to further enhance the planning capabilities.
    - Utilizes ground-truth action trajectories collected from the text-world environment to finetune GPT-2 using next token prediction task, enabling it to memorize planning-related information and generalize well on planning tasks
    - Also was labeled as this section which is below: Embodied memory involves finetuning the LLM with the agent's historical experiential samples, embedding memories into the model parameters. Usually the experiential samples are collected from the agents's interactions with environment, which may consist of commonsense knowledge about the environment, task-related priors, and successful or failed experiences. While the cost of training a language model with more than billions of parameters is huge, parameter-efficient fine-tuning (PEFT) techniques are leveraged to reduce cost and speed up by training a small part of parameters only, such as LoRA, QLoRA, P-tuning, et al.

### Legacy Implementation

**Code Repository**: [https://github.com/princeton-nlp/calm-textgame](https://github.com/princeton-nlp/calm-textgame)

**Note**: 2020 implementation, superseded by newer approaches

**Ayoai Impact**: CALM shows early neural planning approach:
- Combines LM with RL for action selection
- 69% improvement over baselines
- Shows value of human gameplay data
- Could use player data to improve NPCs

## SwiftSage

- SwiftSage [Lin et al., 2023] [https://arxiv.org/abs/2305.17390](https://arxiv.org/abs/2305.17390) (found from: Planning-of-LLM-Agents)

  - Abstract
    - We introduce SwiftSage, a novel agent framework inspired by the dual-process theory of human cognition, designed to excel in action planning for complex interactive reasoning tasks. SwiftSage integrates the strengths of behavior cloning and prompting large language models (LLMs) to enhance task completion performance. The framework comprises two primary modules: the Swift module, representing fast and intuitive thinking, and the Sage module, emulating deliberate thought processes. The Swift module is a small encoder-decoder LM fine-tuned on the oracle agent's action trajectories, while the Sage module employs LLMs such as GPT-4 for subgoal planning and grounding. We develop a heuristic method to harmoniously integrate the two modules, resulting in a more efficient and robust problem-solving process. In 30 tasks from the ScienceWorld benchmark, SwiftSage significantly outperforms other methods such as SayCan, ReAct, and Reflexion, demonstrating its effectiveness in solving complex interactive tasks.

  - Descriptions from this planning survey:
    - Leverages the dual-process theory from cognitive psychology, dividing the planning process into slow thinking and fast thinking. The slow-thinking process involves complex reasoning and rational deliberation while fast-thinking resembles an instinctive response developed through long-term training. The authors utilize a DT model, trained through imitation learning, as the fast-thinking model for rapid plan generation. When errors occur during plan execution, indicating a more complex problem, the agent switches to the slow-thinking process, where LLM engages in reasoning and planning based on the current state. This combination of fast and slow thinking has proven to be highly effective in terms of efficiency.
    - Neural Planner - Neural planners are deep models trained on collected planning data with reinforcement learning or imitation learning techniques, showing effective planning abilities within the specific domain. For instance, DRRN [He et al., 2015] models the planning process as a Markov Decision Process through reinforcement learning, training a policy network to obtain a deep decision model. Decision Transformer (DT) [Chen et al., 2021a] empowers a transformer model to clone human decision-making behavior with planning data. However, when faced with complex and less frequently encountered problems, where training data is scarce, these small models tend to perform poorly due to insufficient generalization ability. Therefore, several works explore combining an LLM with a light-weight neural planner, to further enhance the planning capabilities.

### Dual-Process Architecture

**Code and Documentation**: [https://swiftsage.github.io/](https://swiftsage.github.io/)

**System Design**:

1. **SWIFT Module (System 1)**:
   - T5-large (770M) encoder-decoder
   - Trained on oracle trajectories
   - Handles fast, intuitive responses
   - Encodes: actions, observations, locations, state

2. **SAGE Module (System 2)**:
   - GPT-4 for deliberate reasoning
   - Two-stage process: planning → grounding
   - Planning: item location, subgoal tracking, exception handling
   - Grounding: converts subgoals to action sequences

3. **Integration Mechanism**:
   - Heuristic algorithm for module switching
   - Action buffer for multi-step execution
   - Fallback to SWIFT on parsing errors

![System architecture](../images/media/image164.png)
![Module interaction](../images/media/image165.png)

**Grounding Stage Innovation**:
- Converts abstract plans to executable actions
- Uses 10-step action history for context
- Generates action sequences, not single actions
- Q1-Q3 inclusion improves performance by ~2 points

![Grounding process](../images/media/image166.png)
![Action template](../images/media/image167.png)

**Memory Augmentation**:
- Tracks objects from previously visited locations
- Location-tagged action history: "pick up metal pot [location: kitchen]"
- Enables spatial reasoning across environments

**Error Handling**:
- Invalid actions discarded from buffer
- Automatic reversion to SWIFT mode
- Graceful degradation on parsing failures

**Ayoai Impact**: SwiftSage's dual-process theory is revolutionary for Ayoai:
- System 1 (Swift): Reactive behaviors, combat reflexes
- System 2 (Sage): Strategic planning, puzzle solving
- Action buffer for multi-step execution
- Memory augmentation for spatial reasoning
- Perfect model for Ayoai's behavior tree architecture
- Grounding stage converts plans to executable actions

## Self Rewarding

## Self-Rewarding Language Models

- Self-Rewarding Language Models [https://arxiv.org/abs/2401.10020](https://arxiv.org/abs/2401.10020)

  - Abstract
    - We posit that to achieve superhuman agents, future models require superhuman feedback in order to provide an adequate training signal. Current approaches commonly train reward models from human preferences, which may then be bottlenecked by human performance level, and secondly these separate frozen reward models cannot then learn to improve during LLM training. In this work, we study Self-Rewarding Language Models, where the language model itself is used via LLM-as-a-Judge prompting to provide its own rewards during training. We show that during Iterative DPO training that not only does instruction following ability improve, but also the ability to provide high-quality rewards to itself. Fine-tuning Llama 2 70B on three iterations of our approach yields a model that outperforms many existing systems on the AlpacaEval 2.0 leaderboard, including Claude 2, Gemini Pro, and GPT-4 0613. While there is much left still to explore, this work opens the door to the possibility of models that can continually improve in both axes.

### Self-Rewarding Architecture

**System Overview**:
![Self-rewarding language model](../images/media/image168.png)

**Key Insights**:
- Self-evaluation reward system
- No external reward model needed
- Continuous improvement capability

**Implementation Consideration**: High computational cost but eliminates human bottleneck

**Ayoai Impact**: Self-rewarding opens continuous improvement:
- Agents can evaluate their own performance
- No human bottleneck for feedback
- Continuous improvement without retraining
- Could enable agents to learn from player interactions
- Self-improving NPCs become more engaging over time

## Summary for Ayoai Implementation

The research on planner-aided approaches suggests a hybrid architecture:

1. **Planning Languages**
   - PDDL for formal problem specification
   - Domain files define available actions
   - Problem files specify states and goals
   - LLMs translate natural language to PDDL

2. **Planning Algorithms**
   - Classical planners (Fast-Downward, LPG)
   - GOAP for real-time replanning
   - HTN for hierarchical decomposition
   - A* for pathfinding

3. **Integration Strategy**
   - LLMs generate PDDL from game context
   - External planners ensure correctness
   - Plans translated to behavior trees
   - Dynamic replanning for changing environments

4. **Key Benefits**
   - Guaranteed correct plans
   - Handles complex constraints
   - Scales to many actions
   - Combines with LLM common sense

This enables Ayoai to:
- Generate provably correct agent behaviors
- Handle complex multi-step goals
- Adapt to dynamic game environments
- Scale to sophisticated planning problems
- Maintain both correctness and naturalness