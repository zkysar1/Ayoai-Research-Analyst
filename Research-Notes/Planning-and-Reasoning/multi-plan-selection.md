# Multi-Plan Selection

Multi-Plan Generation involves generating a dozen paths of plans to comprise the candidate plan set. Lead the LLM to "think" more, generating various alternative plans for a task. Then a task-related search algorithm is employed to select one plan to execute.

## Core Principles of Multi-Plan Selection

### Fundamental Approach

Multi-plan selection addresses the inherent limitations of single-plan generation through a two-step process:
1. **Multi-plan Generation**: Create diverse candidate plans
2. **Optimal Plan Selection**: Evaluate and choose the best plan

### Advantages and Trade-offs

**Advantages**:
- Broader exploration of solution space
- Increased robustness against suboptimal plans
- Better handling of uncertainty and edge cases

**Trade-offs**:
- Increased computational cost (especially for large models)
- Resource constraints in online services
- Stochastic variation affecting consistency
- Need for efficiency evaluation modules

### Implementation Considerations

**Rule Compliance**:
- Ensure plans follow context-specific rules
- Validate against environmental constraints
- Provide feedback through annotation systems

**State-Dependent Policies**:
- Actions availability depends on current state
- Example: "change lightbulb" only when bulb is broken
- Requires robust state representation

**Temporal Organization**:
- Partition history by location (spatial indexing)
- Implement bitemporal tables for time-based queries
- Enable "given current circumstances, what should I do?" queries

**Action Equivalence**:
- Identify interchangeable actions
- Enable flexible plan adaptation
- Reduce plan rigidity through equivalence classes

**Ayoai Impact**: 
- **Multiple Behavior Trees**: Generate diverse trees per goal
- **Evaluation Criteria**: Efficiency, feasibility, personality alignment
- **Plan Persistence**: Save successful plans for reuse
- **Dynamic Adaptation**: State-dependent action availability
- **Flexible Execution**: Action equivalence for robustness

## Self-consistency

- Self-consistency [Wang et al., 2022b] (1 of 2 mentions) [https://arxiv.org/abs/2203.11171](https://arxiv.org/abs/2203.11171) (found from: Planning-of-LLM-Agents)

  - Abstract:
    - Chain-of-thought prompting combined with pre-trained large language models has achieved encouraging results on complex reasoning tasks. In this paper, we propose a new decoding strategy, self-consistency, to replace the naive greedy decoding used in chain-of-thought prompting. It first samples a diverse set of reasoning paths instead of only taking the greedy one, and then selects the most consistent answer by marginalizing out the sampled reasoning paths. Self-consistency leverages the intuition that a complex reasoning problem typically admits multiple different ways of thinking leading to its unique correct answer. Our extensive empirical evaluation shows that self-consistency boosts the performance of chain-of-thought prompting with a striking margin on a range of popular arithmetic and commonsense reasoning benchmarks, including GSM8K (+17.9%), SVAMP (+11.0%), AQuA (+12.2%), StrategyQA (+6.4%) and ARC-challenge (+3.9%).

    - ![A diagram of a model Description automatically generated](../images/media/image117.png)

  - Descriptions from this planning survey:
    - Employs a simple intuition: the solutions for complex problems are rarely unique. In contrast to CoT, which generates a single path, Self-consistency obtains multiple distinct reasoning paths via sampling strategies embodied in the decoding process, such as temperature sampling, top-k sampling.
    - Applies the naive majority vote strategy, regarding the plan with the most votes as the optimal choice.

### Implementation Strategy

**Key Challenge**: Extending self-consistency to open-ended generation

**Paper Quote**: "Self-consistency can be applied only to problems where the final answer is from a fixed answer set, but in principle this approach can be extended to open-text generation problems if a good metric of consistency can be defined between multiple generations."

**Proposed Solution**:
1. Generate 3+ plans with varied temperature settings
2. Optionally use slightly different prompts
3. Use LLM to evaluate consistency between plans
4. Weight evaluations by NPC personality traits
5. Select plan with highest weighted consistency score

**Ayoai Impact**: Self-consistency is crucial for reliable agent decisions:
- Generate multiple plans with different temperature settings
- Use LLM to evaluate consistency between plans
- Weight evaluations based on character personality
- Reduces erratic behavior from single-sample planning

## Tree-of-Thought

- Tree-of-Thought (ToT) [Yao et al., 2023] (1 of 2 mentions) [https://arxiv.org/abs/2305.10601](https://arxiv.org/abs/2305.10601) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Language models are increasingly being deployed for general problem solving across a wide range of tasks, but are still confined to token-level, left-to-right decision-making processes during inference. This means they can fall short in tasks that require exploration, strategic lookahead, or where initial decisions play a pivotal role. To surmount these challenges, we introduce a new framework for language model inference, Tree of Thoughts (ToT), which generalizes over the popular Chain of Thought approach to prompting language models, and enables exploration over coherent units of text (thoughts) that serve as intermediate steps toward problem solving. ToT allows LMs to perform deliberate decision making by considering multiple different reasoning paths and self-evaluating choices to decide the next course of action, as well as looking ahead or backtracking when necessary to make global choices. Our experiments show that ToT significantly enhances language models' problem-solving abilities on three novel tasks requiring non-trivial planning or search: Game of 24, Creative Writing, and Mini Crosswords. For instance, in Game of 24, while GPT-4 with chain-of-thought prompting only solved 4% of tasks, our method achieved a success rate of 74%. Code repo with all prompts: [this https URL](https://github.com/princeton-nlp/tree-of-thought-llm).

  - Descriptions from this planning survey:
    - Proposes two strategies to generate plans (i.e. thoughts): sample and propose. The sample strategy is consistent with Self-consistency, where LLM would sample multiple plans in decoding process. The propose strategy explicitly instructs the LLM to generate various plans via few-shot examples in prompts.
    - Supports tree search algorithms, such as conventional BFS and DFS. When selecting a node for expansion, it uses LLM to evaluate multiple actions and chooses the optimal one.

### Implementation Resources

**Code Repository**: [https://github.com/princeton-nlp/tree-of-thought-llm](https://github.com/princeton-nlp/tree-of-thought-llm)

![Tree of Thought architecture](../images/media/image118.png)

### Theoretical Foundation

**Human Problem-Solving Analogy**:
"Research on human problem-solving suggests that people search through a combinatorial problem space -- a tree where nodes represent partial solutions, and branches correspond to operators that modify them. This perspective highlights two key shortcomings of existing LM approaches:
1. Locally, they do not explore different continuations within a thought process
2. Globally, they do not incorporate planning, lookahead, or backtracking"

### ToT Framework Components

**1. Thought Decomposition**:
- Thoughts sized for coherent generation yet meaningful evaluation
- Examples: words (Crosswords), equations (Game of 24), paragraphs (Creative Writing)

**2. Thought Generation Strategies**:
- **Sample**: I.I.D. thoughts from CoT prompt
- **Propose**: Sequential generation with "propose prompt" for constrained spaces

**3. State Evaluation Methods**:
- **Independent**: Value each state separately
- **Voting**: Compare states and select most promising
- LLM-based heuristics offer flexibility over programmed/learned approaches

**4. Search Algorithms**:
- **BFS**: Maintains b most promising states per step (shallow trees)
- **DFS**: Explores most promising first with backtracking (deep trees)
- Future work: A*, MCTS integration

### Visual Examples
![ToT examples 1](../images/media/image119.png)
![ToT examples 2](../images/media/image120.png)

**Ayoai Impact**: ToT provides a complete framework for agent planning:
- Thought decomposition maps to behavior tree node design
- State evaluation using LLMs for flexibility
- BFS/DFS search strategies for different planning scenarios
- Backtracking capability for failed plans
- The 74% vs 4% improvement shows the power of structured search

## Graph-of-Thought

- Graph-of-Thought (GoT) [Besta et al., 2023] [https://arxiv.org/abs/2308.09687](https://arxiv.org/abs/2308.09687) (found from: Planning-of-LLM-Agents)

  - Abstract
    - We introduce Graph of Thoughts (GoT): a framework that advances prompting capabilities in large language models (LLMs) beyond those offered by paradigms such as Chain-of-Thought or Tree of Thoughts (ToT). The key idea and primary advantage of GoT is the ability to model the information generated by an LLM as an arbitrary graph, where units of information ("LLM thoughts") are vertices, and edges correspond to dependencies between these vertices. This approach enables combining arbitrary LLM thoughts into synergistic outcomes, distilling the essence of whole networks of thoughts, or enhancing thoughts using feedback loops. We illustrate that GoT offers advantages over state of the art on different tasks, for example increasing the quality of sorting by 62% over ToT, while simultaneously reducing costs by >31%. We ensure that GoT is extensible with new thought transformations and thus can be used to spearhead new prompting schemes. This work brings the LLM reasoning closer to human thinking or brain mechanisms such as recurrence, both of which form complex networks.

  - Descriptions from this planning survey:
    - Extends ToT by adding transformations of thoughts, which supports arbitrary thoughts aggregation.

### Visual Architecture

**Comparison of Approaches**:
![Planning approach comparison](../images/media/image121.png)

**Graph Structure Evolution**:
![Evolution from chain to graph](../images/media/image122.png)

**Task Node Architecture**:
The parser API demonstrates excellent task decomposition into leaf nodes:
![Task node structure](../images/media/image123.png)

**Prompting Strategy Templates**:
![GoT prompting strategies](../images/media/image124.png)

**Ayoai Impact**: GoT's graph structure is revolutionary for complex agent reasoning:
- Allows merging multiple reasoning paths (not just trees)
- Feedback loops enable iterative improvement
- 62% quality improvement with 31% cost reduction
- Maps well to multi-agent coordination scenarios
- Thought transformations could enable personality-specific reasoning

## LLM-MCTS

- LLM-MCTS [Zhao et al., 2023b] (1 of 2 mentions) [https://arxiv.org/abs/2305.14078](https://arxiv.org/abs/2305.14078) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Large-scale task planning is a major challenge. Recent work exploits large language models (LLMs) directly as a policy and shows surprisingly interesting results. This paper shows that LLMs provide a commonsense model of the world in addition to a policy that acts on it. The world model and the policy can be combined in a search algorithm, such as Monte Carlo Tree Search (MCTS), to scale up task planning. In our new LLM-MCTS algorithm, the LLM-induced world model provides a commonsense prior belief for MCTS to achieve effective reasoning; the LLM-induced policy acts as a heuristic to guide the search, vastly improving search efficiency. Experiments show that LLM-MCTS outperforms both MCTS alone and policies induced by LLMs (GPT2 and GPT3.5) by a wide margin, for complex, novel tasks. Further experiments and analyses on multiple tasks -- multiplication, multi-hop travel planning, object rearrangement -- suggest minimum description length (MDL) as a general guiding principle: if the description length of the world model is substantially smaller than that of the policy, using LLM as a world model for model-based planning is likely better than using LLM solely as a policy.

  - Descriptions from this planning survey:
    - Leverages LLM as the heuristic policy function for the Monte Carlo Tree Search (MCTS), where multiple potential actions are obtained by multiple calls.
    - Specifically, MCTS builds a reasoning tree iteratively, where each node represents a state, and each edge represents an action and the transition from the current state to the next state after applying the action (Figure 1).
    - Also employ a tree structure to assist in multi-plan search. Unlike ToT, they employ the Monte Carlo Tree Search (MCTS) algorithm for search.

### Core Strategies

**L-Policy Strategy**:
"Treat the LLM as a policy and query it directly for next actions, given history of past actions and observations. Exploits LLMs' vast commonsense knowledge to circumvent searching large spaces."

**L-Model Strategy**:
"Use LLMs' knowledge to build a world model and apply planning algorithms. Performance depends on model accuracy and planning efficiency."

**LLM-MCTS Hybrid**:
![LLM-MCTS architecture](../images/media/image125.png)

### Empirical Findings

1. **L-Model**: Poor performance due to search space size, not model inaccuracy
2. **L-Policy**: Reasonable performance but degrades on novel, complex tasks
3. **LLM-MCTS > L-Model**: LLM as heuristic policy effectively guides search
4. **LLM-MCTS > L-Policy**: Combined approach excels on complex tasks

**Key Insight**: "The LLM-induced world model is sufficiently accurate, and tree search with this world model, when limited to the neighbourhood of the LLM-induced policy, provides improved performance."

### Implementation Resources

**Code Repository**: [https://github.com/1989Ryan/llm-mcts](https://github.com/1989Ryan/llm-mcts)

**Prompt Templates**:
![MCTS prompt 1](../images/media/image126.png)
![MCTS prompt 2](../images/media/image127.png)
![MCTS prompt 3](../images/media/image128.png)

**Ayoai Impact**: LLM-MCTS is perfect for Ayoai's game agent planning:
- Combines world model (game state) with policy (agent behavior)
- MCTS provides principled exploration/exploitation tradeoff
- Outperforms both pure model and pure policy approaches
- Scales well to complex, novel tasks (like emergent gameplay)
- The MDL principle helps decide when to use planning vs reactive behavior

## Integration with Ayoai Platform

Multi-plan selection research suggests a sophisticated planning architecture:

1. **Plan Generation**
   - Self-consistency: Multiple samples with different temperatures
   - ToT: Structured decomposition with evaluation
   - GoT: Graph-based reasoning with feedback loops

2. **Search Algorithms**
   - BFS for shallow, broad exploration
   - DFS for deep, focused search
   - MCTS for balanced exploration/exploitation

3. **Evaluation Methods**
   - LLM-based state evaluation
   - Voting across candidate plans
   - Personality-weighted preferences

4. **Implementation Strategy**
   - Start with self-consistency for robustness
   - Add ToT for complex multi-step planning
   - Integrate MCTS for real-time decision making
   - Use GoT for multi-agent coordination

This enables Ayoai agents to:
- Generate diverse, creative solutions
- Adapt to novel situations
- Learn from successful plans
- Coordinate complex multi-agent behaviors
- Maintain character consistency while exploring options

## RAP: Reasoning via Planning

- RAP: Reasoning via Planning [Hao et al., 2023b] [https://arxiv.org/abs/2305.14992](https://arxiv.org/abs/2305.14992) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Despite the many recent achievements in developing general large language models (LLMs), LLMs often fall short when it comes to tasks that require deliberate planning and multi-step reasoning. The deficiency stems from the key fact that LLMs lack an internal world model to predict the world state (e.g., environment status, intermediate variable values) and simulate long-term outcomes of actions. This prevents LLMs from performing deliberate planning akin to human brains, which involves exploring alternative reasoning paths, anticipating future states and rewards, and iteratively refining existing reasoning steps. To overcome the limitations, we propose a new LLM reasoning framework, Reasoning via Planning (RAP). RAP repurposes the LLM as both a world model and a reasoning agent, and incorporates a principled planning algorithm (based on Monto Carlo Tree Search) for strategic exploration in the vast reasoning space. During reasoning, the LLM (as agent) incrementally builds a reasoning tree under the guidance of the LLM (as world model) and task-specific rewards, and obtains a high-reward reasoning path efficiently with a proper balance between exploration vs. exploitation. We apply RAP to a variety of challenging reasoning problems including plan generation, math reasoning, and logical inference. Empirical results on these tasks demonstrate the superiority of RAP over various strong baselines, including CoT and least-to-most prompting with self-consistency. RAP on LLAMA-33B surpasses CoT on GPT-4 with 33% relative improvement in a plan generation setting.

  - Descriptions from this planning survey:
    - Leverages LLM as the heuristic policy function for the Monte Carlo Tree Search (MCTS), where multiple potential actions are obtained by multiple calls.
    - Also employ a tree structure to assist in multi-plan search. Unlike ToT, they employ the Monte Carlo Tree Search (MCTS) algorithm for search.

### RAP Architecture

![RAP overview](../images/media/image129.png)
![RAP details](../images/media/image130.png)

### Key Strategies

**1. Self-Evaluation**:
- LLM critiques its own reasoning steps
- "Is this reasoning step correct?" with probability of "Yes" as reward
- Easier to recognize errors than avoid them initially
- Task-specific evaluation criteria

**2. Task-Specific Heuristics**:
- Flexible reward function integration
- Example: Blocksworld compares current state to goal
- Rewards encourage progress toward target

**3. Tree Expansion**:
- Sample d possible actions from current state
- Use LLM as world model to predict next states
- Create d child nodes for exploration
- Skip expansion for terminal nodes

### Back-Propagation Mechanism

![Back-propagation process](../images/media/image131.png)

**Process**:
1. Reach terminal state
2. Back-propagate rewards along path
3. Update Q values by aggregating future rewards
4. Build value estimates for state-action pairs

### Selection Strategies

**Three approaches for final path selection**:
1. **Greedy Q-value**: Choose highest Q at each step
2. **Highest reward**: Select path with maximum reward (often best)
3. **Most visited**: Choose most explored path

### Confidence Estimation

![Confidence calculation](../images/media/image132.png)

**Method**: Sample multiple answers and use frequency as confidence metric. Higher consistency with LLM world knowledge indicates more reliable reasoning.

**Ayoai Impact**: RAP's dual-role LLM approach is groundbreaking:
- LLM as both world model AND reasoning agent
- Task-specific reward functions for goal-oriented planning
- Self-evaluation capability for quality control
- 33% improvement over GPT-4 CoT shows massive potential
- Back-propagation enables learning from exploration

## LLM A*

- LLM A* [Xiao and Wang, 2023] [https://arxiv.org/abs/2312.01797](https://arxiv.org/abs/2312.01797) (found from: Planning-of-LLM-Agents)

  - Abstract
    - This research focuses on how Large Language Models (LLMs) can help with path planning for mobile embodied agents such as robots, in a human-in-the-loop and interactive manner. A novel framework named LLM A*, aims to leverage the commonsense of LLMs, and the utility-optimal A* is proposed to facilitate few-shot near-optimal path planning. Prompts are used to 1) provide LLMs with essential information like environment, cost, heuristics, etc.; 2) communicate human feedback to LLMs on intermediate planning results. This makes the whole path planning process a 'white box' and human feedback guides LLM A* to converge quickly compared to other data-driven methods such as reinforcement learning-based (RL) path planning. In addition, it makes code-free path planning practical, henceforth promoting the inclusiveness of artificial intelligence techniques. Comparative analysis against A* and RL shows that LLM A* is more efficient in terms of search space and achieves an on-a-par path with A* and a better path than RL. The interactive nature of LLM A* also makes it a promising tool for deployment in collaborative human-robot tasks.

  - Descriptions from this planning survey:
    - Optimal Plan Selection = To select the optimal plan among the candidate plans, diverse strategies are adopted as heuristic search algorithms.
    - Utilizes the classic A* algorithm from artificial intelligence to assist LLM in search. The Chebyshev distance from the current position to the target position serves as the heuristic cost function for selecting the optimal path.

### Implementation Notes

**Prompt Template**:
![A* prompt structure](../images/media/image132.png)

While the prompting approach is interesting, the key value lies in integrating classical A* pathfinding with LLM-based planning for navigation tasks.

**Ayoai Impact**: LLM A* demonstrates pathfinding integration:
- A* provides optimal path guarantees
- Human-in-the-loop for interactive refinement
- Could be used for Roblox navigation planning
- White-box approach enables debugging

## RankPrompt

- RankPrompt: Step-by-Step Comparisons Make Language Models Better Reasoners https://arxiv.org/abs/2403.12373

  - Abstract
    - Large Language Models (LLMs) have achieved impressive performance across various reasoning tasks. However, even state-of-the-art LLMs such as ChatGPT are prone to logical errors during their reasoning processes. Existing solutions, such as deploying task-specific verifiers or voting over multiple reasoning paths, either require extensive human annotations or fail in scenarios with inconsistent responses. To address these challenges, we introduce RankPrompt, a new prompting method that enables LLMs to self-rank their responses without additional resources. RankPrompt breaks down the ranking problem into a series of comparisons among diverse responses, leveraging the inherent capabilities of LLMs to generate chains of comparison as contextual exemplars. Our experiments across 11 arithmetic and commonsense reasoning tasks show that RankPrompt significantly enhances the reasoning performance of ChatGPT and GPT-4, with improvements of up to 13%. Moreover, RankPrompt excels in LLM-based automatic evaluations for open-ended tasks, aligning with human judgments 74% of the time in the AlpacaEval dataset. It also exhibits robustness to variations in response order and consistency. Collectively, our results validate RankPrompt as an effective method for eliciting high-quality feedback from language models.

### RankPrompt Methodology

**Prompt Strategy**:
![RankPrompt example 1](../images/media/image133.tmp)
![RankPrompt example 2](../images/media/image134.tmp)

**Key Innovation**: Step-by-step pairwise comparisons leverage LLMs' comparative abilities better than absolute scoring.

**Ayoai Impact**: RankPrompt offers self-evaluation without extra resources:
- Agents can rank their own plan quality
- 13% performance improvement
- No additional training or annotations needed
- Perfect for selecting between multiple behavior tree options

## MacGyver

- MacGyver: Are Large Language Models Creative Problem Solvers? <https://arxiv.org/abs/2311.09682>

  - Abstract
    - We explore the creative problem-solving capabilities of modern LLMs in a novel constrained setting. To this end, we create MACGYVER, an automatically generated dataset consisting of over 1,600 real-world problems deliberately designed to trigger innovative usage of objects and necessitate out-of-the-box thinking. We then present our collection to both LLMs and humans to compare and contrast their problem-solving abilities. MACGYVER is challenging for both groups, but in unique and complementary ways. For instance, humans excel in tasks they are familiar with but struggle with domain-specific knowledge, leading to a higher variance. In contrast, LLMs, exposed to a variety of specialized knowledge, attempt broader problems but fail by proposing physically-infeasible actions. Finally, we provide a detailed error analysis of LLMs, and demonstrate the potential of enhancing their problem-solving ability with novel prompting techniques such as iterative step-wise reflection and divergent-convergent thinking. This work (1) introduces a fresh arena for intelligent agents focusing on intricate aspects of physical reasoning, planning, and unconventional thinking, which supplements the existing spectrum of machine intelligence; and (2) provides insight into the constrained problem-solving capabilities of both humans and AI.

### Creative Problem-Solving Framework

![MacGyver examples](../images/media/image135.tmp)
![Problem-solving architecture](../images/media/image136.png)
![Results comparison](../images/media/image137.png)

**Relevance**: While not immediately critical, this research provides insights into handling edge cases where conventional solutions fail, important for sandbox game environments.

**Ayoai Impact**: MacGyver reveals creative problem-solving challenges:
- Tests unconventional object usage (important for sandbox games)
- Shows LLM limitations with physical feasibility
- Divergent-convergent thinking could enhance agent creativity
- Helps identify when agents propose impossible actions

## Violation of Expectation

- Violation of Expectation via Metacognitive Prompting Reduces Theory of Mind Prediction Error in Large Language Models <https://arxiv.org/abs/2310.06983>

  - Abstract
    - Recent research shows that Large Language Models (LLMs) exhibit a compelling level of proficiency in Theory of Mind (ToM) tasks. This ability to impute unobservable mental states to others is vital to human social cognition and may prove equally important in principal-agent relations between individual humans and Artificial Intelligences (AIs). In this paper, we explore how a mechanism studied in developmental psychology known as Violation of Expectation (VoE) can be implemented to reduce errors in LLM prediction about users by leveraging emergent ToM affordances. And we introduce a \textit{metacognitive prompting} framework to apply VoE in the context of an AI tutor. By storing and retrieving facts derived in cases where LLM expectation about the user was violated, we find that LLMs are able to learn about users in ways that echo theories of human learning. Finally, we discuss latent hazards and augmentative opportunities associated with modeling user psychology and propose ways to mitigate risk along with possible directions for future inquiry.

### Metacognitive Learning Framework

![VoE architecture](../images/media/image138.png)

**Key Implementation Ideas**:
- Track violations between expected and actual behavior tree outcomes
- Use LifingPolls as baseline for agent expectations
- Store violations as learning experiences
- Enables personalized adaptation to individual players

**Ayoai Impact**: VoE is crucial for adaptive agent behavior:
- Agents can learn when their predictions about players fail
- Store violations as learning experiences
- Improves Theory of Mind capabilities
- Could use LifingPolls for baseline expectations
- Enables agents to adapt to individual player behaviors

## Pairwise Better Than Score

- Instead of using a score, asking the llm to choose is better. <https://x.com/hwchase17/status/1796269356625875049?s=19>
  - ![](../images/media/image139.png)

**Ayoai Impact**: Simple but effective insight:
- Use pairwise comparisons instead of absolute scoring
- More reliable plan selection
- Aligns with RankPrompt findings

## Goal Misgeneralization

- Goal Misgeneralization: Why Correct Specifications Aren't Enough For Correct Goals <https://arxiv.org/abs/2210.01790>

  - Abstract
    - The field of AI alignment is concerned with AI systems that pursue unintended goals. One commonly studied mechanism by which an unintended goal might arise is specification gaming, in which the designer-provided specification is flawed in a way that the designers did not foresee. However, an AI system may pursue an undesired goal even when the specification is correct, in the case of goal misgeneralization. Goal misgeneralization is a specific form of robustness failure for learning algorithms in which the learned program competently pursues an undesired goal that leads to good performance in training situations but bad performance in novel test situations. We demonstrate that goal misgeneralization can occur in practical systems by providing several examples in deep learning systems across a variety of domains. Extrapolating forward to more capable systems, we provide hypotheticals that illustrate how goal misgeneralization could lead to catastrophic risk. We suggest several research directions that could reduce the risk of goal misgeneralization for future systems.

### Visual Examples

![Goal misgeneralization examples](../images/media/image140.tmp)

**Critical Safety Consideration**: These examples demonstrate how agents can pursue seemingly correct but ultimately harmful goals, emphasizing the need for robust goal specification and monitoring.

**Ayoai Impact**: Critical safety consideration for game agents:
- Agents might pursue unintended goals that seem correct
- Important for preventing exploitative behavior
- Need robust goal specification in behavior trees
- Regular monitoring for emergent undesired behaviors

## Devil's Advocate

- Devil's Advocate: Anticipatory Reflection for LLM Agents <https://arxiv.org/abs/2405.16334>

  - Abstract
    - In this work, we introduce a novel approach that equips LLM agents with introspection, enhancing consistency and adaptability in solving complex tasks. Our approach prompts LLM agents to decompose a given task into manageable subtasks (i.e., to make a plan), and to continuously introspect upon the suitability and results of their actions. We implement a three-fold introspective intervention: 1) anticipatory reflection on potential failures and alternative remedy before action execution, 2) post-action alignment with subtask objectives and backtracking with remedy to ensure utmost effort in plan execution, and 3) comprehensive review upon plan completion for future strategy refinement. By deploying and experimenting with this methodology - a zero-shot approach - within WebArena for practical tasks in web environments, our agent demonstrates superior performance over existing zero-shot methods. The experimental results suggest that our introspection-driven approach not only enhances the agent's ability to navigate unanticipated challenges through a robust mechanism of plan execution, but also improves efficiency by reducing the number of trials and plan revisions needed to achieve a task.

### Introspection Framework

![Devil's Advocate methodology](../images/media/image141.tmp)

**Implementation Value**: This three-stage introspection process provides a concrete framework for enhancing agent decision-making reliability.

**Ayoai Impact**: Three-fold introspection is perfect for robust agents:
- Anticipatory reflection before actions (prevent failures)
- Post-action alignment checks (ensure goals met)
- Comprehensive review for learning
- Reduces trial-and-error iterations
- Could integrate with behavior tree execution monitoring

## Additional Considerations

### Concise Chain of Thought
- The Benefits of a Concise Chain of Thought on Problem-Solving in Large Language Models <https://arxiv.org/abs/2401.05618>
  - 48.70% response length reduction with negligible performance impact
  - Important for real-time game performance

### OMNI-EPIC
- OMNI-EPIC: Open-endedness via Models of human Notions of Interestingness with Environments Programmed in Code <https://arxiv.org/abs/2405.15568>
  - Has code!!! <https://omni-epic.vercel.app/>
  - Uses previous behaviors as stepping stones for elaboration
  - Generates learnable and interesting environments
  - Perfect for creating varied NPC behaviors

### Intelligent Go-Explore
- Intelligent Go-Explore: Standing on the Shoulders of Giant Foundation Models <https://arxiv.org/abs/2405.15143>
  - Has website with code: <https://www.conglu.co.uk/intelligentgoexplore/>
  - Leverages FM ability to judge interestingness
  - Captures serendipity on the fly
  - 100% success rate 70.8% faster than baselines