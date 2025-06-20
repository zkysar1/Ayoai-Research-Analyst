# RAG Based Memory

Retrieval Augmented Generation (RAG) [Lewis et al., 2020; Mao et al., 2020; Cai et al., 2022] techniques are proposed to aid text generation with retrieved information. It is capable of enhancing the LLM with the latest knowledge. For LLM agents, past experiences could be stored in the memory and retrieved when needed. The core idea of such methods is to retrieve task-relevant experiences from the memory during task planning. Among those methods, memories are typically stored in additional storage, and the forms are diverse, such as texts [Park et al., 2023; Liu et al., 2023b; Packer et al., 2023; Wang et al., 2023c; Zhong et al., 2023], tabular forms [Zhang et al., 2023a], knowledge graph [Pan et al., 2024], etc.

## RAG vs Fine-tuning Trade-offs

**Key Distinctions**:
- **RAG-based Methods**: Real-time, low-cost memory updates with natural language storage, dependent on retrieval accuracy
- **Fine-tuning Methods**: Larger memorization capacity through parameter modification, but high update costs and limited fine-grained detail retention
- **Hybrid Potential**: Memory-enhanced agents show improved planning and fault tolerance, though performance depends on base LLM capabilities

## Implementation Resources

### Vector Database Solutions

**Weaviate**:
- [Verba](https://verba.weaviate.io/) - Open-source RAG chat interface
- [GitHub Repository](https://github.com/weaviate/Verba)
- [Video: Retrieval Augmented Generation with Weaviate](https://www.youtube.com/watch?v=OSt3sFT1i18)
- [Video: Open-Source RAG with Weaviate](https://www.youtube.com/watch?v=IiNDCPwmqF8)
- [Multi-Tenancy in Vector Search](https://www.youtube.com/watch?v=KT2RFMTJKGs)
- [Python Quickstart](https://replit.com/@Weaviate/Python-Quickstart-1-Semantic-Search-with-Weaviate-Embedded#main.py)
- [JavaScript Quickstart](https://replit.com/@Weaviate/Quickstart-1-Create-Object-and-Import-Data-and-Create-Vectors#index.js)

### RAG Implementation Guides

**LangChain Integration**:
- [Cohere Integration Guide](https://python.langchain.com/docs/integrations/chat/cohere)

**Custom RAG Development**:
- [Build Your Own RAG with Mistral 7B and LangChain](https://medium.com/@thakermadhav/build-your-own-rag-with-mistral-7b-and-langchain-97d0c92fa146)

**Advanced RAG Techniques**:
- [RAG with Rerankers](https://www.youtube.com/watch?v=Uh9bYiVrW_s)
- [Code Example](https://github.com/pinecone-io/examples/blob/master/learn/generation/better-rag/00-rerankers.ipynb)
- [Article Series](https://www.pinecone.io/learn/series/rag/rerankers/)

**Ayoai Impact**: RAG is essential for Ayoai's memory system:
- Real-time memory updates without retraining
- Cost-effective scalability for thousands of agents
- Enables persistent memory across game sessions
- Critical for maintaining character continuity

## Generative Agents

- Generative Agents: Interactive Simulacra of Human Behavior [Park et al., 2023] [https://arxiv.org/abs/2304.03442](https://arxiv.org/abs/2304.03442) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Believable proxies of human behavior can empower interactive applications ranging from immersive environments to rehearsal spaces for interpersonal communication to prototyping tools. In this paper, we introduce generative agents--computational software agents that simulate believable human behavior. Generative agents wake up, cook breakfast, and head to work; artists paint, while authors write; they form opinions, notice each other, and initiate conversations; they remember and reflect on days past as they plan the next day. To enable generative agents, we describe an architecture that extends a large language model to store a complete record of the agent's experiences using natural language, synthesize those memories over time into higher-level reflections, and retrieve them dynamically to plan behavior. We instantiate generative agents to populate an interactive sandbox environment inspired by The Sims, where end users can interact with a small town of twenty five agents using natural language. In an evaluation, these generative agents produce believable individual and emergent social behaviors: for example, starting with only a single user-specified notion that one agent wants to throw a Valentine's Day party, the agents autonomously spread invitations to the party over the next two days, make new acquaintances, ask each other out on dates to the party, and coordinate to show up for the party together at the right time. We demonstrate through ablation that the components of our agent architecture--observation, planning, and reflection--each contribute critically to the believability of agent behavior. By fusing large language models with computational, interactive agents, this work introduces architectural and interaction patterns for enabling believable simulations of human behavior.

  - Descriptions from this planning survey:
    - Store the daily experiences of human-like agents in text form and retrieve memories based on a composite score of recency and relevance to the current situation.

### Key Implementation Details

**Code Repository**: [https://github.com/joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents)

**Architecture Components**:

1. **Environmental Tree Structure**:
   - Hierarchical representation of the game world
   - Real-time updates as environment changes
   - Efficient querying for agent perception
   - ![Simulation environment](../images/media/image49.png)

2. **Memory Stream Architecture**:
   - Chronological storage of all experiences
   - Composite retrieval scoring (recency + relevance)
   - ![Memory stream visualization](../images/media/image50.png)
   - ![Memory architecture](../images/media/image51.png)

3. **Poignancy Rating System**:
   - Scale: 1 (mundane) to 10 (extremely significant)
   - Example prompt: "On the scale of 1 to 10, where 1 is purely mundane (e.g., brushing teeth, making bed) and 10 is extremely poignant (e.g., a break up, college acceptance), rate the likely poignancy of the following piece of memory."
   - Prioritizes important memories for retention and retrieval

4. **Reflection Generation**:
   - Periodic synthesis of experiences into higher-level insights
   - ![Reflection process](../images/media/image52.png)

### Behavioral Evaluation Prompts

**Planning Consistency**:
- Short-term planning ("What will you be doing at 6am today?")
- Long-term planning ("What will you be doing at 10pm today?")
- Task completion tracking ("What will you have just finished doing at 1pm?")

**Reactive Capabilities**:
- Emergency responses ("Your breakfast is burning!")
- Resource management ("You need to cook dinner but your refrigerator is empty")
- Social interactions ("You see your friend walking by")
- Crisis handling ("You see fire on the street")

**Reflective Synthesis**:
- Personal inspiration and motivation
- Theory of mind (predicting others' preferences)
- Social reasoning (gift selection, compliments)
- Relationship prioritization

**Ayoai Impact**: This landmark paper provides the blueprint for Ayoai's memory architecture:
- Memory stream with recency + relevance scoring
- Reflection mechanism for higher-level insights
- Poignancy ratings to prioritize important memories
- Natural language storage for flexibility
- Proven to create emergent social behaviors

## MemoryBank

- MemoryBank [Zhong et al., 2023], TiM [Liu et al., 2023b], and RecMind [Wang et al., 2023c] [https://arxiv.org/abs/2305.10250](https://arxiv.org/abs/2305.10250),, (found from: Planning-of-LLM-Agents)

  - Abstract
    - Revolutionary advancements in Large Language Models have drastically reshaped our interactions with artificial intelligence systems. Despite this, a notable hindrance remains-the deficiency of a long-term memory mechanism within these models. This shortfall becomes increasingly evident in situations demanding sustained interaction, such as personal companion systems and psychological counseling. Therefore, we propose MemoryBank, a novel memory mechanism tailored for LLMs. MemoryBank enables the models to summon relevant memories, continually evolve through continuous memory updates, comprehend, and adapt to a user personality by synthesizing information from past interactions. To mimic anthropomorphic behaviors and selectively preserve memory, MemoryBank incorporates a memory updating mechanism, inspired by the Ebbinghaus Forgetting Curve theory, which permits the AI to forget and reinforce memory based on time elapsed and the relative significance of the memory, thereby offering a human-like memory mechanism. MemoryBank is versatile in accommodating both closed-source models like ChatGPT and open-source models like ChatGLM. We exemplify application of MemoryBank through the creation of an LLM-based chatbot named SiliconFriend in a long-term AI Companion scenario. Further tuned with psychological dialogs, SiliconFriend displays heightened empathy in its interactions. Experiment involves both qualitative analysis with real-world user dialogs and quantitative analysis with simulated dialogs. In the latter, ChatGPT acts as users with diverse characteristics and generates long-term dialog contexts covering a wide array of topics. The results of our analysis reveal that SiliconFriend, equipped with MemoryBank, exhibits a strong capability for long-term companionship as it can provide emphatic response, recall relevant memories and understand user personality.

  - Descriptions from this planning survey:
    - Encode each memory using a text encoding model into a vector and establish an indexing structure, such as FAISS library [Johnson et al., 2019]. During retrieval, the description of the current status is used as a query to retrieve memories from the memory pool. The difference between the three lies in the way memories are updated.

### Implementation Insights

**Code Repository**: [https://github.com/zhongwanjun/MemoryBank-SiliconFriend](https://github.com/zhongwanjun/MemoryBank-SiliconFriend)

**Key Innovations**:

1. **Multi-Type Memory Architecture**:
   - Different memory types with distinct retention policies
   - Aligns with behavior tree task nodes for memory operations
   - ![Memory bank architecture](../images/media/image53.png)

2. **Ebbinghaus Forgetting Curve Implementation**:
   - Natural memory decay over time
   - Reinforcement through repetition
   - Significance-based retention weighting
   - ![Chat interface demonstration](../images/media/image54.png)

3. **Integration Potential**:
   - Memory operations as behavior tree leaf nodes
   - Type-specific retention policies for different experiences
   - Anthropomorphic memory behaviors

**Ayoai Impact**: MemoryBank's Ebbinghaus Forgetting Curve implementation is crucial:
- Natural memory decay over time (human-like forgetting)
- Reinforcement of important memories through repetition
- Different retention policies for different memory types
- Creates more believable long-term character development

## MemGPT

- MemGPT [Packer et al., 2023] [https://arxiv.org/abs/2310.08560](https://arxiv.org/abs/2310.08560) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Large language models (LLMs) have revolutionized AI, but are constrained by limited context windows, hindering their utility in tasks like extended conversations and document analysis. To enable using context beyond limited context windows, we propose virtual context management, a technique drawing inspiration from hierarchical memory systems in traditional operating systems that provide the appearance of large memory resources through data movement between fast and slow memory. Using this technique, we introduce MemGPT (Memory-GPT), a system that intelligently manages different memory tiers in order to effectively provide extended context within the LLM's limited context window, and utilizes interrupts to manage control flow between itself and the user. We evaluate our OS-inspired design in two domains where the limited context windows of modern LLMs severely handicaps their performance: document analysis, where MemGPT is able to analyze large documents that far exceed the underlying LLM's context window, and multi-session chat, where MemGPT can create conversational agents that remember, reflect, and evolve dynamically through long-term interactions with their users. We release MemGPT code and data for our experiments at [this https URL](https://memgpt.ai/).

  - Descriptions from this planning survey:
    - Leverages the concept of multiple levels of storage in computer architecture, abstracting the context of LLM into RAM and treating the additional storage structure as a disk. LLM can spontaneously decide whether to retrieve historical memories or save the current context to storage.

### Implementation Details

**Code Repository**: [https://github.com/cpacker/MemGPT](https://github.com/cpacker/MemGPT)

**Key Resources**:
- [Towards AGI Part 2](https://superagi.com/towards-agi-part-2/) - Recommended starting point for long-term memory implementation

**Context-Aware Memory Management**:
- Location-based memory prioritization (home vs. bar example)
- Dynamic RAM allocation based on current context
- Intelligent memory tier management for extended conversations

**Architecture Highlights**:
- OS-inspired memory hierarchy
- Automatic context window management
- Critical information persistence to vector database
- Actor-specific prompts for different agent roles
- ![MemGPT workflow](../images/media/image55.png)
- ![Chat interface example](../images/media/image56.png)

**Ayoai Impact**: MemGPT's OS-inspired memory hierarchy is perfect for Ayoai:
- RAM = Working memory for current scene/interaction
- Disk = Long-term storage for persistent memories
- Context-aware memory management (home vs. bar example)
- Enables infinite conversation length within fixed context windows
- Could implement location-based memory loading

## REMEMBER

- REMEMBER [Zhang et al., 2023a] [https://arxiv.org/abs/2306.07929](https://arxiv.org/abs/2306.07929) (found from: Planning-of-LLM-Agents)

  - Abstract
    - Inspired by the insights in cognitive science with respect to human memory and reasoning mechanism, a novel evolvable LLM-based (Large Language Model) agent framework is proposed as REMEMBERER. By equipping the LLM with a long-term experience memory, REMEMBERER is capable of exploiting the experiences from the past episodes even for different task goals, which excels an LLM-based agent with fixed exemplars or equipped with a transient working memory. We further introduce Reinforcement Learning with Experience Memory (RLEM) to update the memory. Thus, the whole system can learn from the experiences of both success and failure, and evolve its capability without fine-tuning the parameters of the LLM. In this way, the proposed REMEMBERER constitutes a semi-parametric RL agent. Extensive experiments are conducted on two RL task sets to evaluate the proposed framework. The average results with different initialization and training sets exceed the prior SOTA by 4% and 2% for the success rate on two task sets and demonstrate the superiority and robustness of REMEMBERER.

  - Descriptions from this planning survey:
    - Stores historical memories in the form of a Q-value table, where each record is (environment, task, action, Q-value)-tuple. During retrieval, positive and negative memories are both retrieved for LLM to generate plan based on the similarity of the environment and task.

### Implementation and Theory

**Code Repository**: [https://github.com/OpenDFM/Rememberer](https://github.com/OpenDFM/Rememberer)

**Core Philosophy**: "Reasoning is remembering" (Seifert et al., 1997)
- Episodic memory crucial for complex decision-making
- Learn from successes to repeat them
- Learn from failures to avoid them
- Equip LLM agents with interaction experiences

**Architecture Components**:
- Q-value table storage format
- Positive and negative memory retrieval
- Experience transfer across tasks
- ![REMEMBER architecture](../images/media/image57.png)

**Key Takeaway**: While the reinforcement learning implementation has limitations, the dual memory system (short-term and long-term) concept remains valuable for agent architectures.

**Ayoai Impact**: REMEMBER's Q-value approach offers unique benefits:
- Learn from both successes AND failures
- No fine-tuning required (cost-effective)
- Experience transfer across different tasks
- Could track action outcomes for behavior improvement

## Knowledge Graph

- knowledge graph [Pan et al., 2024]

  - Abstract
    - Large language models (LLMs), such as ChatGPT and GPT4, are making new waves in the field of natural language processing and artificial intelligence, due to their emergent ability and generalizability. However, LLMs are black-box models, which often fall short of capturing and accessing factual knowledge. In contrast, Knowledge Graphs (KGs), Wikipedia and Huapu for example, are structured knowledge models that explicitly store rich factual knowledge. KGs can enhance LLMs by providing external knowledge for inference and interpretability. Meanwhile, KGs are difficult to construct and evolving by nature, which challenges the existing methods in KGs to generate new facts and represent unseen knowledge. Therefore, it is complementary to unify LLMs and KGs together and simultaneously leverage their advantages. In this article, we present a forward-looking roadmap for the unification of LLMs and KGs. Our roadmap consists of three general frameworks, namely, 1) KG-enhanced LLMs, which incorporate KGs during the pre-training and inference phases of LLMs, or for the purpose of enhancing understanding of the knowledge learned by LLMs; 2) LLM-augmented KGs, that leverage LLMs for different KG tasks such as embedding, completion, construction, graph-to-text generation, and question answering; and 3) Synergized LLMs + KGs, in which LLMs and KGs play equal roles and work in a mutually beneficial way to enhance both LLMs and KGs for bidirectional reasoning driven by both data and knowledge. We review and summarize existing efforts within these three frameworks in our roadmap and pinpoint their future research directions.

### Knowledge Graph Integration

**Conceptual Framework**:
- Structured representation of factual knowledge
- Complementary to LLM's implicit knowledge
- Three integration patterns:
  1. KG-enhanced LLMs
  2. LLM-augmented KGs
  3. Synergized LLMs + KGs

**Visual Architecture**:
- ![Knowledge graph structure](../images/media/image58.png)
- ![Integration process](../images/media/image59.png)

**Implementation Considerations**:
- Complex but powerful for relationship tracking
- May be overengineered for initial implementations
- Consider simplified graph structures for game-specific knowledge

**Ayoai Impact**: Knowledge graphs could enhance Ayoai's relationship tracking:
- Explicit representation of agent relationships
- Track facts about the game world
- Enable complex reasoning about social networks
- Could be overkill for initial implementation

## Agents OSFFAA

- Agents: An Open-source Framework for Autonomous Language Agents ([https://arxiv.org/abs/2309.07870](https://arxiv.org/abs/2309.07870)): this is a loop of agents?

  - Abstract
    - Recent advances on large language models (LLMs) enable researchers and developers to build autonomous language agents that can automatically solve various tasks and interact with environments, humans, and other agents using natural language interfaces. We consider language agents as a promising direction towards artificial general intelligence and release Agents, an open-source library with the goal of opening up these advances to a wider non-specialist audience. Agents is carefully engineered to support important features including planning, memory, tool usage, multi-agent communication, and fine-grained symbolic control. Agents is user-friendly as it enables non-specialists to build, customize, test, tune, and deploy state-of-the-art autonomous language agents without much coding. The library is also research-friendly as its modularized design makes it easily extensible for researchers. Agents is available at [this https URL](https://github.com/aiwaves-cn/agents).

### Framework Architecture

**Code Repository**: [https://github.com/aiwaves-cn/agents](https://github.com/aiwaves-cn/agents)

**Modular Design**:
- Planning module
- Memory module
- Tool usage module
- Multi-agent communication module
- Fine-grained symbolic control

![Agents framework architecture](../images/media/image60.png)

**Key Benefits**:
- Non-specialist friendly
- Research-oriented extensibility
- State-of-the-art autonomous capabilities

**Ayoai Impact**: This framework validates Ayoai's modular approach:
- Planning, memory, tools, and communication as separate modules
- Enables game developers to customize without deep coding
- Multi-agent communication patterns
- Could adopt some architectural patterns

## RAT: Retrieval Augmented Thoughts

- RAT: Retrieval Augmented Thoughts Elicit Context-Aware Reasoning in Long-Horizon Generation <https://arxiv.org/abs/2403.05313>

  - Abstract
    - We explore how iterative revising a chain of thoughts with the help of information retrieval significantly improves large language models' reasoning and generation ability in long-horizon generation tasks, while hugely mitigating hallucination. In particular, the proposed method -- *retrieval-augmented thoughts* (RAT) -- revises each thought step one by one with retrieved information relevant to the task query, the current and the past thought steps, after the initial zero-shot CoT is generated. Applying RAT to GPT-3.5, GPT-4, and CodeLLaMA-7b substantially improves their performances on various long-horizon generation tasks; on average of relatively increasing rating scores by 13.63% on code generation, 16.96% on mathematical reasoning, 19.2% on creative writing, and 42.78% on embodied task planning. The demo page can be found at [this https URL](https://craftjarvis.github.io/RAT)

### Implementation Resources

**Demo and Code**: [https://craftjarvis.github.io/RAT/](https://craftjarvis.github.io/RAT/)

**Performance Improvements**:
- 13.63% improvement in code generation
- 16.96% improvement in mathematical reasoning
- 19.2% improvement in creative writing
- 42.78% improvement in embodied task planning

**Visual Documentation**:
- ![RAT performance comparison](../images/media/image61.png)
- ![RAT implementation example](../images/media/image62.png)

**Ayoai Impact**: RAT's iterative refinement is valuable for agent planning:
- Retrieve relevant memories during each planning step
- Reduces hallucination in long-term planning
- Particularly useful for complex multi-step behaviors
- 42.78% improvement in embodied task planning!

## T-RAG

- T-RAG: Lessons from the LLM Trenches <https://arxiv.org/abs/2402.07483>

  - Abstract
    - Large Language Models (LLM) have shown remarkable language capabilities fueling attempts to integrate them into applications across a wide range of domains. An important application area is question answering over private enterprise documents where the main considerations are data security, which necessitates applications that can be deployed on-prem, limited computational resources and the need for a robust application that correctly responds to queries. Retrieval-Augmented Generation (RAG) has emerged as the most prominent framework for building LLM-based applications. While building a RAG is relatively straightforward, making it robust and a reliable application requires extensive customization and relatively deep knowledge of the application domain. We share our experiences building and deploying an LLM application for question answering over private organizational documents. Our application combines the use of RAG with a finetuned open-source LLM. Additionally, our system, which we call Tree-RAG (T-RAG), uses a tree structure to represent entity hierarchies within the organization. This is used to generate a textual description to augment the context when responding to user queries pertaining to entities within the organization's hierarchy. Our evaluations, including a Needle in a Haystack test, show that this combination performs better than a simple RAG or finetuning implementation. Finally, we share some lessons learned based on our experiences building an LLM application for real-world use.

### Architecture Visualizations

- ![T-RAG system architecture](../images/media/image63.tmp)
- ![Hierarchical organization structure](../images/media/image64.tmp)

**Key Innovation**: Tree-based entity hierarchy representation particularly suitable for game factions, guilds, and organizational structures.

**Ayoai Impact**: T-RAG's hierarchical approach maps well to game structures:
- Tree structure for faction/guild hierarchies
- Entity relationships in game worlds
- Organizational knowledge (who reports to whom)
- Practical lessons for production deployment

## HippoRAG

- HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models <https://arxiv.org/abs/2405.14831>

  - Abstract
    - In order to thrive in hostile and ever-changing natural environments, mammalian brains evolved to store large amounts of knowledge about the world and continually integrate new information while avoiding catastrophic forgetting. Despite the impressive accomplishments, large language models (LLMs), even with retrieval-augmented generation (RAG), still struggle to efficiently and effectively integrate a large amount of new experiences after pre-training. In this work, we introduce HippoRAG, a novel retrieval framework inspired by the hippocampal indexing theory of human long-term memory to enable deeper and more efficient knowledge integration over new experiences. HippoRAG synergistically orchestrates LLMs, knowledge graphs, and the Personalized PageRank algorithm to mimic the different roles of neocortex and hippocampus in human memory. We compare HippoRAG with existing RAG methods on multi-hop question answering and show that our method outperforms the state-of-the-art methods remarkably, by up to 20%. Single-step retrieval with HippoRAG achieves comparable or better performance than iterative retrieval like IRCoT while being 10-30 times cheaper and 6-13 times faster, and integrating HippoRAG into IRCoT brings further substantial gains. Finally, we show that our method can tackle new types of scenarios that are out of reach of existing methods. Code and data are available at [this https URL](https://github.com/OSU-NLP-Group/HippoRAG).

### Implementation and Architecture

**Code Repository**: [https://github.com/OSU-NLP-Group/HippoRAG](https://github.com/OSU-NLP-Group/HippoRAG)

**Brain-Inspired Design**:
- Mimics hippocampal indexing theory
- Orchestrates LLMs, knowledge graphs, and PageRank
- Models neocortex and hippocampus roles

**Visual Architecture**:
- ![Hippocampus-inspired memory system](../images/media/image65.tmp)
- ![HippoRAG architecture diagram](../images/media/image66.tmp)

**Performance Benefits**:
- 20% improvement over state-of-the-art RAG methods
- 10-30x cost reduction
- 6-13x speed improvement
- Handles previously impossible scenarios

**Ayoai Impact**: HippoRAG's brain-inspired architecture is revolutionary:
- Mimics hippocampus/neocortex memory systems
- 20% performance improvement over standard RAG
- 10-30x cost reduction with 6-13x speed improvement
- Perfect for Ayoai's real-time performance requirements
- Biologically plausible memory architecture creates more believable agents

## Summary for Ayoai Implementation

The research suggests a hybrid memory architecture:

1. **Working Memory** (MemGPT-style RAM)
   - Current scene context
   - Active relationships
   - Immediate goals

2. **Long-term Memory** (HippoRAG + MemoryBank)
   - Persistent experiences with forgetting curve
   - Hippocampus-inspired indexing
   - Reflection generation (Generative Agents)

3. **Retrieval Strategy**
   - Recency + Relevance + Poignancy scoring
   - Context-aware loading (location, social situation)
   - Iterative refinement (RAT)

This architecture enables:
- Persistent character development
- Emergent social behaviors
- Efficient real-time performance
- Scalability to thousands of agents