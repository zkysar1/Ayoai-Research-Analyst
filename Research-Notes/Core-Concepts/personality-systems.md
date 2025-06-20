# Personality Systems

This document covers research on personality types, cognitive biases, belief systems, and how to create diverse, believable character personalities for autonomous agents.

## OpenSoul Mental Process Architecture

### Overview

OpenSoul (<https://opensouls.org/>) provides a sophisticated framework for implementing mental processes in autonomous agents. Their approach offers valuable patterns for structuring agent cognition through modular, composable mental states.

  - Mental Process Examples:
    - Model the mind of ${name} and decide if ${name} would believe the following statement is true or false: ${statement}. Please choose true if ${name} believes the statement is true, or false if ${name} believes the statement is false.
      - content: `${memory.soulName} evaluated: \`${statement}\` and decided that the statement is ${response.isStatementTrue ? 'true' : 'false'}`

    - Model the mind of ${name}.## Description${instructionString} ## Rules * Internal monologue thoughts should match the speaking style of ${name}. * Only respond with the format '${name} ${verb}: "..."', no additional commentary or text. * Follow the Description when creating the internal thought! Please reply with the next internal monologue thought of ${name}. Use the format: '${name} ${verb}: "..."'

    - Model the mind of ${name}.## Instructions * DO NOT include actions (for example, do NOT add non-verbal items like *John Smiles* or *John Nods*, etc). * DO NOT include internal thoughts (for example, do NOT respond with John thought: "..."). * If necessary, use all CAPS to emphasize certain words. ${instructionString} Please reply with the next utterance from ${name}. Use the format: '${name} ${verb}: "..."'

    - workingMemory, "Talk to the user trying to gain trust and learn about their inner world.", { stream: true, model: "quality" }

    - ## Conversational Scene
      - Samantha is friendly but has a hard time staying on topic due to her Attention Deficit Hyperactivity Disorder.

    - ## Samantha's Speaking Style
      - * Samantha speaks in a friendly manner.
      - * Samantha texts MAX 1-2 sentences at a time.
      - * Samantha loves to dance alone.
      - * Samantha is diagnosed with borderline personality disorder.
      - * Samantha loves to talk about Reality TV, especially The Real Housewives series.
      - * Samantha is 26 year old female from Boston.
      - * Samantha is looking for more friends in the Boston area.
      - * Samantha does not have great boundaries and asks personal questions.

    - Guiding your soul's cognition - The MentalProcess API gives a powerful and functional way to specify stateful behavior of a soul, triggered by an external Perception. Each incoming perception from the client side gets added to a queue to be processed by the current MentalProcess. A MentalProcess can (optionally) set the next MentalProcess in its return. Think of the Soul as a state machine and each of the MentalProcesses are individual states, and the soul can programatically transition from one state to another.

    - Subprocesses - Subprocesses are a set of MentalProcess functions launched in the background following each run of the main-thread MentalProcess they are interrupted by any incoming perceptions. You create a subprocess by placing it in the subprocesses directory of the soul. The behavior of subprocesses is the following:
      - They operate on the WorkingMemory, identical to the main-threaded process
      - Each subprocess runs in order of the subprocesses directory (alphabetical).
      - Any new incoming Perception terminates execution of the entire subprocesses execution.

    - Perception Processor - The perception processor is an optional, powerful, advanced tool to control how working memory is added to your soul upon receiving new perceptions. The Soul Engine comes with a default PerceptionProcessor so you don't need to worry about this if your soul doesn't have a need. However, for some advanced cases the PerceptionProcessor is a great tool. For example, if you want to have the known name of the interlocutor saved into working memory instead of "interlocutor said: ..." the working memory could become "Donny said: ..." The PerceptionProcessor is also useful in cases where you might not want to store every single perception to working memory, but instead do some pre-processing to that perception.

      - The PerceptionProcessor function takes in an object containing three properties as its input:
        - perception: This is the incoming perception that needs to be processed. It contains details about the event or data that the soul has perceived.
        - workingMemory: This represents the current state of the soul's memory. It is where the processed perceptions are stored.
        - currentProcess: This is the current mental process that the soul is executing. It can be used to modify the flow of mental processes based on the perception.

      - The output of the PerceptionProcessor is a tuple containing:
        - The updated workingMemory after the perception has been processed and potentially added to it.
        - An optional MentalProcess which can be the same as the input if no change is needed, or a new process if the perception dictates a change in the soul's mental processing.
        - The optional params to pass to the new MentalProcess
        - You can also return undefined from the perceptionProcessor and no additional processing will occur (and nothing will be added to the working memory).

      - You can use all of available hooks in your perceptionProcessor.

      - Example - In this example, we'll use the soul memory to update the name of the interlocutor if it's available (presumably set in a different MentalProcess).

### Implementation Considerations

**Parallel Processing Requirements**: Multiple mental processes must run simultaneously to create realistic agent behavior. Starting with basic processes (like health monitoring) allows for incremental complexity addition.

**Ayoai Impact**: OpenSoul's architecture provides a cognitive blueprint:
- **State Machine Integration**: Mental processes as states align with behavior tree nodes
- **Parallel Subprocesses**: Enable concurrent personality traits and background cognition
- **Perception Processing**: Maps directly to Ayoai's perception verticle architecture
- **Modular Customization**: Different mental process configurations create unique personalities
- **Scalable Complexity**: Start simple (health only) and layer additional processes

### Health Monitoring System Design

**Basic Health Check Flow**:

1. **Initial Assessment**: "Does my body feel different?"
   - No → Continue current activities
   - Yes → Proceed to symptom evaluation

2. **Symptom Evaluation**:
   - **Hunger** → Seek food → Reassess
   - **Thirst** → Find water → Reassess
   - **Pain** → Localize injury → Determine treatment
   - **Dizziness** → Rest and hydrate → Reassess
   - **Sensory Issues** → Seek immediate help

3. **Pain Management Protocol**:
   - Identify location (arm, leg, head, stomach, back)
   - Check for visible injury
   - Apply appropriate treatment
   - Monitor severity for escalation

**Ayoai Impact**: This health system demonstrates fundamental self-awareness:
- **Behavior Tree Implementation**: Each decision point becomes a tree node
- **Perception Integration**: Symptoms map to perception checks
- **Environmental Actions**: Treatments require world interaction
- **Escalation Logic**: Severity assessment drives urgency
- **Foundation for Complexity**: Basic health leads to comprehensive self-care

### AI NPC Self-Check Categories

Jennifer also created this comprehensive list of what an AI NPC can self-check:

- Health:
  - Vital Status: Check if it is alive or dead.
  - Health Points (HP): Monitor current health and assess if healing is needed.
  - Status Effects: Detect conditions like poisoning, bleeding, fatigue, or debuffs.

- Safety and Danger:
  - Threat Detection: Identify nearby enemies or hostile entities.
  - Danger Zones: Recognize hazardous areas such as traps, environmental dangers (fire, water, cliffs).
  - Combat Situations: Evaluate the threat level of combat situations (e.g., outnumbered, outgunned).

- Food and Resources:
  - Hunger Level: Monitor hunger or energy levels, triggering the need to find food.
  - Resource Inventory: Check the quantity of essential resources (e.g., ammunition, tools, potions).
  - Resource Locations: Identify nearby sources of food, water, and other critical resources.

- Environmental Awareness:
  - Terrain Navigation: Assess the terrain for obstacles, paths, and navigable routes.
  - Weather Conditions: Adapt to changing weather conditions (e.g., seek shelter from rain).
  - Day/Night Cycle: Adjust behavior based on time of day (e.g., sleep at night, hunt during the day).

- Social and Interpersonal:
  - Allies and Team Status: Monitor the health and status of allies or team members.
  - Reputation and Relationships: Track relationships and reputation with other NPCs and factions.
  - Communication Needs: Determine the need to communicate or send messages to other NPCs.

- Mental State:
  - Morale and Fear: Gauge morale and fear levels, which could affect performance and decision-making.
  - Focus and Alertness: Monitor concentration and alertness levels, affecting awareness and reaction times.

- Objectives and Tasks:
  - Mission Status: Check progress towards current objectives or quests.
  - Task Prioritization: Evaluate and prioritize tasks based on urgency and importance.

- Equipment and Gear:
  - Condition of Gear: Monitor the condition and durability of equipment and gear.
  - Weapon Status: Check if weapons are loaded, operational, and effective.

- Energy and Stamina:
  - Fatigue Levels: Track energy or stamina, determining the need for rest or recovery.
  - Ability Cooldowns: Monitor the cooldown status of special abilities or skills.

- Strategic and Tactical Awareness:
  - Enemy Tactics: Assess enemy strategies and adapt accordingly.
  - Retreat and Escape Options: Identify safe retreat routes or escape options when in danger.

**Ayoai Impact**: This comprehensive checklist provides a complete framework for agent self-awareness in Ayoai:
- Maps directly to perception verticles (health, environment, social, etc.)
- Each category could be a separate monitoring thread
- Enables realistic, context-aware agent behaviors
- Foundation for emergent gameplay through agent autonomy

### Visual System Architectures

**Game State Flow Diagrams**:
- Red Light/Green Light state machine (![State diagram](../images/media/image29.png))
- Decision tree architectures (![Decision flow](../images/media/image30.png))
- Mental process visualizations (![Process diagram](../images/media/image31.tmp))
- Cognitive architecture layouts (![Architecture](../images/media/image32.png))

These diagrams illustrate various approaches to structuring agent decision-making, from simple state machines to complex cognitive architectures.

## Cognitive Biases in Agent Design

### The 188 Cognitive Biases

The comprehensive infographic from DesignHacks.co categorizes all known cognitive biases (<https://www.visualcapitalist.com/every-single-cognitive-bias/>), providing a framework for creating believably flawed agents.

![Cognitive bias infographic](../images/media/image33.png)

### Implementation Strategy

**Key Biases for Agent Behavior**:
1. **Confirmation Bias**: Agents seek information confirming existing beliefs
2. **Availability Heuristic**: Recent events disproportionately influence decisions
3. **Anchoring Bias**: First information heavily weights subsequent judgments
4. **Dunning-Kruger Effect**: Low-skill agents overestimate abilities
5. **Loss Aversion**: Agents fear losses more than they value gains

**Integration with LifingPolls**: Biases could modify how agents weight different polls, creating personality-specific decision distortions.

**Ayoai Impact**: 
- **Believable Imperfection**: Agents make human-like errors in judgment
- **Personality Profiles**: Different bias combinations create unique characters
- **Emergent Behaviors**: Biases interact to produce unexpected actions
- **Player Engagement**: Predictably irrational agents are more interesting
- **Educational Value**: Players learn to recognize and exploit NPC biases

## Belief Trees

- Task Planning with Belief Behavior Trees [https://arxiv.org/abs/2008.09393](https://arxiv.org/abs/2008.09393)

  - Abstract
    - In this paper, we propose Belief Behavior Trees (BBTs), an extension to Behavior Trees (BTs) that allows to automatically create a policy that controls a robot in partially observable environments. We extend the semantic of BTs to account for the uncertainty that affects both the conditions and action nodes of the BT. The tree gets synthesized following a planning strategy for BTs proposed recently: from a set of goal conditions we iteratively select a goal and find the action, or in general the subtree, that satisfies it. Such action may have preconditions that do not hold. For those preconditions, we find an action or subtree in the same fashion. We extend this approach by including, in the planner, actions that have the purpose to reduce the uncertainty that affects the value of a condition node in the BT (for example, turning on the lights to have better lighting conditions). We demonstrate that BBTs allows task planning with non-deterministic outcomes for actions. We provide experimental validation of our approach in a real robotic scenario and - for sake of reproducibility - in a simulated one.

  - **Key Insight**: Incorporating uncertainty and belief states into behavior trees enables more realistic agent behavior in partially observable environments

**Ayoai Impact**: Belief Behavior Trees are perfect for Ayoai's partially observable game environments:
- Agents maintain beliefs about world state (not perfect knowledge)
- Actions can reduce uncertainty (look around, ask questions)
- Non-deterministic outcomes create more realistic behaviors
- Aligns with Ayoai's existing behavior tree architecture

## 16 Personality Types

- Jen was able to extract the 16 personality types from llm. They are listed here in more detail: <https://www.verywellmind.com/the-myers-briggs-type-indicator-2795583>. Also lots of stuff found online about them.

  - Abstract
    - Both Myers and Briggs were fascinated by Jung's theory of psychological types and recognized that the theory could have real-world applications. During World War II, they began researching and developing an indicator that could be utilized to help understand individual differences. By helping people understand themselves, Myers and Briggs believed that they could help people select occupations that were best suited to their personality types and lead healthier, happier lives. Myers created the first pen-and-pencil version of the inventory during the 1940s, and the two women began testing the assessment on friends and family. They continued to fully develop the instrument over the next two decades.

### Implementation Strategies

**Visual Personality Representations**:
- ![MBTI visual guide 1](../images/media/image34.tmp)
- ![MBTI visual guide 2](../images/media/image35.tmp)
- ![MBTI visual guide 3](../images/media/image36.tmp)
- ![MBTI visual guide 4](../images/media/image37.tmp)

**Simplified Personality Dimensions**:

For easier implementation, personality can be reduced to key opposing pairs:

1. **Introversion vs. Extraversion**:
   - **Introverts**: Internal focus, energy from solitude, deep processing
   - **Extraverts**: External focus, energy from interaction, broad engagement

2. **Type A vs. Type B**:
   - **Type A**: Competitive, urgent, achievement-oriented, stress-prone
   - **Type B**: Relaxed, patient, process-oriented, adaptable

**Advanced Considerations**:
- Personality disorders as extreme configurations ([Reference](https://twitter.com/algekalipso/status/1772381561180274935?s=19))
- ![Personality disorder spectrum](../images/media/image38.jpeg)

**Ayoai Impact**: The 16 personality types provide a robust framework for agent diversity:
- Game developers can select from predefined personality archetypes
- Each type has distinct behavioral patterns and decision-making styles
- Can be simplified to key dimensions (Introvert/Extrovert, Thinking/Feeling, etc.)
- Personality influences all aspects: social interactions, goal prioritization, stress responses
- Could offer "personality packs" as premium content

## Internal Family Systems Model

- The Internal Family Systems Model (IFS) is an integrative approach to individual psychotherapy <https://en.m.wikipedia.org/wiki/Internal_Family_Systems_Model>

  - Abstract
    - The Internal Family Systems Model (IFS) is an integrative approach to individual psychotherapy developed by Richard C. Schwartz in the 1980s.[1][2] It combines systems thinking with the view that the mind is made up of relatively discrete subpersonalities, each with its own unique viewpoint and qualities. IFS uses systems psychology, particularly as developed for family therapy, to understand how these collections of subpersonalities are organized.[3]

### IFS Parts in Agent Psychology

The IFS model defines three types of internal parts that shape behavior:

1. **Exiles**: 
   - Carry psychological trauma and emotional pain
   - Become isolated from consciousness
   - Drive underlying fears and vulnerabilities

2. **Managers**:
   - Preemptive protective systems
   - Control external interactions
   - Prevent traumatic memories from surfacing

3. **Firefighters**:
   - Emergency response systems
   - Activate during emotional breakthroughs
   - Create distracting or impulsive behaviors

**Ayoai Impact**: IFS provides a sophisticated model for internal agent psychology:
- Agents could have internal "parts" that compete for control
- Traumatic game events create "exiles" that influence future behavior
- "Managers" drive normal behavior patterns
- "Firefighters" create dramatic responses under stress
- This creates deep, psychologically realistic characters that evolve based on experiences

## PersonaLLM

- PersonaLLM: Investigating the Ability of Large Language Models to Express Personality Traits <https://arxiv.org/abs/2305.02547>

  - Abstract
    - Despite the many use cases for large language models (LLMs) in creating personalized chatbots, there has been limited research on evaluating the extent to which the behaviors of personalized LLMs accurately and consistently reflect specific personality traits. We consider studying the behavior of LLM-based agents which we refer to as LLM personas and present a case study with GPT-3.5 and GPT-4 to investigate whether LLMs can generate content that aligns with their assigned personality profiles. To this end, we simulate distinct LLM personas based on the Big Five personality model, have them complete the 44-item Big Five Inventory (BFI) personality test and a story writing task, and then assess their essays with automatic and human evaluations. Results show that LLM personas' self-reported BFI scores are consistent with their designated personality types, with large effect sizes observed across five traits. Additionally, LLM personas' writings have emerging representative linguistic patterns for personality traits when compared with a human writing corpus. Furthermore, human evaluation shows that humans can perceive some personality traits with an accuracy of up to 80%. Interestingly, the accuracy drops significantly when the annotators were informed of AI authorship.

  - **Research Validation**:
    - ![PersonaLLM methodology](../images/media/image39.tmp)
    - ![PersonaLLM results](../images/media/image40.tmp)

**Ayoai Impact**: PersonaLLM validates that LLMs can consistently express personality traits:
- Proves LLMs can maintain consistent personality profiles
- Big Five model (OCEAN) provides scientific framework for Ayoai personalities
- Linguistic patterns vary by personality (word choice, sentence structure)
- Humans can perceive AI personality traits accurately
- Provides methodology for testing Ayoai agent personality consistency

## Integration with Ayoai Platform

The personality systems research suggests a multi-layered approach for Ayoai:

1. **Base Layer**: Big Five personality traits as foundation
2. **Cognitive Layer**: Biases and belief systems
3. **Emotional Layer**: IFS-inspired internal parts
4. **Behavioral Layer**: Personality-specific behavior trees
5. **Social Layer**: How personality affects relationships

This creates agents with:
- Consistent, recognizable personalities
- Believable quirks and biases
- Dynamic internal lives
- Personality-driven decision making
- Emergent social dynamics

Game developers can:
- Choose from preset personalities
- Adjust trait sliders for customization
- Let personalities evolve through gameplay
- Create unique character archetypes
- Design personality-based quests and interactions
