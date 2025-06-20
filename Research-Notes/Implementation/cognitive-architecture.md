# NPC Cognitive Architecture

## Overview

This document describes the verticle-based cognitive architecture for NPCs, extracted from the Cognition_Answer_List_Refactor_try_2.md table structure. The architecture consists of 97 processes distributed across 22 different verticles that work together to create autonomous NPC behavior.

## Architecture Summary

### Key Statistics
- **Total Verticles**: 22
- **Total Processes**: 97
- **Processing Phases**: 4 main phases
- **Message Types**: 2 (Message, Throttle)
- **Driver Types**: 2 (External, Self)

### Verticle Distribution by Process Count
1. **Prioritized Perceptions**: 19 processes (highest)
2. **Emotional Perception**: 7 processes
3. **Prioritized Goals**: 7 processes
4. **Body Perception**: 5 processes
5. **Behavior Tree Perception**: 5 processes
6. **World Translator**: 5 processes
7. **Other verticles**: 1-4 processes each

## Processing Phases

The cognitive architecture operates in four distinct phases:

### 1. Disperse Clean Input (5 processes)
**Purpose**: Receive and distribute raw input from Roblox
- **Primary Verticle**: World Translator
- **Function**: Receives updates (units, keys, behavior trees, environment) and distributes cleaned data to perception systems

### 2. Analyze Input (42 processes)
**Purpose**: Process raw inputs into meaningful perceptions
- **Key Verticles**: All perception types (Spatial, Unit, Body, Emotional, etc.)
- **Function**: Convert raw data into perception messages through analysis and storage

### 3. Generalize Perceptions (18 processes)
**Purpose**: Synthesize individual perceptions into prioritized understanding
- **Primary Verticle**: Prioritized Perceptions
- **Function**: Collects all perception types and creates unified prioritized perception messages

### 4. React to Perceptions (29 processes)
**Purpose**: Convert perceptions into goals and actions
- **Key Verticles**: Memory Weaver, Reflection Replay, Refinement Critic, Prioritized Goals
- **Function**: Use perceptions to update memory, reflect, refine, and prioritize goals

### 5. Planning Goal Actions (3 processes)
**Purpose**: Convert goals into executable behavior trees
- **Key Verticles**: Behavior Tree Builder, Small Goal Planner
- **Function**: Transform prioritized goals into behavior trees for execution

## Verticle Details

### Input Processing Verticles

#### World Translator
**Purpose**: External Input dispatcher
**Processes**: 5
**Message Flow**:
- **Listens for**: Unit Update, Key Update, Behavior Trees Update, Environment Update
- **Outputs**: Update.Unit, Update.Communication, Update.Key, Update.BehaviorTree, Update.Environment
- **Processing**: Cleans and transforms Roblox data, filters communications from key updates

### Perception Verticles

#### Spatial Perceptions
**Purpose**: Derive spatial understanding from world updates
**Processes**: 3
**Message Flow**:
- **Input**: Update.Unit → Inner.Spatial (analyzed and stored)
- **Throttle**: SendPerception.Spatial → Perception.Spatial
- **Processing**: 
  - Analyzes unit location changes
  - Tracks positions of key members, food, tools
  - Maintains spatial memory and pattern changes
  - Examples: Visual, auditory, tactile, olfactory, gustatory processing

#### Unit Perceptions
**Purpose**: Maintain world hierarchy and unit tracking
**Processes**: 3
**Message Flow**:
- **Input**: Update.Unit → Inner.Unit (stored)
- **Throttle**: SendPerception.Unit → Perception.Unit
- **Processing**: 
  - Maintains unit hierarchy tree
  - Archives all world objects with unique IDs
  - Tracks object locations and relationships

#### Fast Perception
**Purpose**: Rapid instinctual responses
**Processes**: 2
**Message Flow**:
- **Inputs**: Perception.Spatial, Update.Unit → Perception.Fast (immediate)
- **Processing**: 
  - No internal storage - responds immediately
  - Handles automatic/instinctual survival mechanisms
  - Examples: Ducking from loud noise, moving away from danger

#### Body Perception
**Purpose**: Track NPC's own body state
**Processes**: 5
**Message Flow**:
- **Inputs**: Perception.Spatial, Update.Unit, Perception.Fast → Inner.Body
- **Throttle**: SendPerception.Body → Perception.Body
- **Processing**: 
  - Monitors body part states and changes
  - Motor and action-based processing
  - Internal sensations (interoception)

#### Emotional Perception
**Purpose**: Derive emotional states from multiple inputs
**Processes**: 7
**Message Flow**:
- **Inputs**: Update.Unit, Update.Communication, Perception.Fast, Perception.Body, Perception.Spatial → Inner.Emotion
- **Throttle**: SendPerception.Emotion → Perception.Emotion
- **Processing**: 
  - Analyzes emotional undertones in communications
  - Detects emotional impacts of environmental changes
  - Processes feelings about situations

#### Communication Perception
**Purpose**: Process and understand communications
**Processes**: 3
**Message Flow**:
- **Input**: Update.Communication → Inner.Communication
- **Throttle**: SendPerception.Communication → Perception.Communication
- **Processing**: 
  - Maintains chat history
  - Categorizes messages
  - Draws perceptions from communications

#### Configurator
**Purpose**: Process configuration changes
**Processes**: 3
**Message Flow**:
- **Input**: Update.Key → Inner.Config
- **Throttle**: SendPerception.Config → Perception.Config

#### Behavior Tree Perception
**Purpose**: Monitor behavior execution status
**Processes**: 5
**Message Flow**:
- **Inputs**: Update.BehaviorTree, Update.Unit → Inner.BehaviorTree
- **Throttles**: 
  - SendPerception.BehaviorTree → Perception.BehaviorTree
  - SendGoal.BehaviorTree → Goal.BehaviorTree
- **Processing**: 
  - Tracks tree success/failure
  - Notes completed actions per unit
  - Informs goal system about behavior status

#### Goal Perception
**Purpose**: Derive goal understanding from behaviors
**Processes**: 3
**Message Flow**:
- **Input**: Update.BehaviorTree → Inner.Goal
- **Throttle**: SendPerception.Goal → Perception.Goal
- **Processing**: Analyzes behavior trees to understand active goals

#### Environment Perception
**Purpose**: Track environmental conditions
**Processes**: 3
**Message Flow**:
- **Input**: Update.Environment → Inner.Environment
- **Throttle**: SendPerception.Environment → Perception.Environment
- **Processing**: Weather, temperature, environment descriptions

#### Tool Perception
**Purpose**: Track tool availability and usage
**Processes**: 3
**Message Flow**:
- **Input**: Update.Unit → Inner.Tool
- **Throttle**: SendPerception.Tool → Perception.Tool
- **Processing**: 
  - Tracks tool appearances/disappearances
  - Monitors tool locations and ownership changes

#### Ownership Perception
**Purpose**: Track ownership relationships
**Processes**: 3
**Message Flow**:
- **Input**: Update.Unit → Inner.Ownership
- **Throttle**: SendPerception.Ownership → Perception.Ownership
- **Processing**: Maintains who owns what objects

#### Social Knowledge Perception
**Purpose**: Track social information
**Processes**: 4
**Message Flow**:
- **Inputs**: Update.Unit, Perception.Communication → Inner.SocialKnowledge
- **Throttle**: SendPerception.SocialKnowledge → Perception.SocialKnowledge
- **Processing**: 
  - Tracks locations of friends/family
  - Monitors what others are doing
  - Social cues and communication processing

#### Social Awareness Perception
**Purpose**: Social impact on behavior
**Processes**: 4
**Message Flow**:
- **Inputs**: Update.Unit, Perception.Communication → Inner.SocialAwareness
- **Throttle**: SendPerception.SocialAwareness → Perception.SocialAwareness
- **Processing**: Determines if social factors should impact current behavior

### Synthesis and Decision Verticles

#### Prioritized Perceptions
**Purpose**: Central perception synthesizer
**Processes**: 19 (most complex verticle)
**Message Flow**:
- **Inputs**: ALL perception types (Spatial, Unit, Fast, Emotion, etc.) → Inner.Perceptions
- **Throttles**:
  - SendPerception.Perceptions → Perception.Prioritized (main output)
  - SendPerception.Perceptions → Inner.Perceptions (self-reference)
- **Processing**: 
  - Collects and normalizes all perception data
  - Prioritizes perceptions
  - Removes outdated perceptions
  - Outputs prioritized perception messages

#### Memory Weaver
**Purpose**: Long-term memory management
**Processes**: 4
**Message Flow**:
- **Input**: Perception.Prioritized → Inner.Memory
- **Throttles**:
  - SendGoal.Memory → Goal.Memory
  - SendPerception.Memory → Perception.Memory
- **Processing**: 
  - Stores important perceptions in memory
  - Manages short-term vs long-term memory
  - Provides memory context for decisions

#### Reflection Replay
**Purpose**: Learn from past experiences
**Processes**: 4
**Message Flow**:
- **Input**: Perception.Prioritized → Inner.Reflection
- **Throttles**:
  - SendGoal.Reflection → Goal.Reflection
  - SendPerception.Reflection → Perception.Reflection
- **Processing**: Reflects on past events to inform future decisions

#### Refinement Critic
**Purpose**: Improve decision quality
**Processes**: 4
**Message Flow**:
- **Input**: Perception.Prioritized → Inner.Refinement
- **Throttles**:
  - SendGoal.Refinement → Goal.Refinement
  - SendPerception.Refinement → Perception.Refinement
- **Processing**: Critiques and refines perceptions and goals

#### Goals Lists
**Purpose**: Maintain goal inventory
**Processes**: 3
**Message Flow**:
- **Input**: Perception.Prioritized → Inner.GoalsLists
- **Throttles**:
  - SendGoal.GoalsLists → Goal.GoalsLists
  - SendPerception.GoalsLists → Perception.GoalsLists
- **Processing**: Add/delete/adjust goals based on perceptions

#### Prioritized Goals
**Purpose**: Central goal synthesizer and prioritizer
**Processes**: 7
**Message Flow**:
- **Inputs**: Goal.Memory, Goal.Reflection, Goal.Refinement, Goal.BehaviorTree, Goal.GoalsLists, Perception.Prioritized → Inner.Goals
- **Throttle**: SendGoal.Goals → Goal.Prioritized
- **Processing**: 
  - Maintains active goals and sub-goals
  - Handles goal interruption (e.g., attack while mowing grass)
  - Cognitive processing for reasoning and problem-solving
  - Goal-oriented processing for planning

### Action Generation Verticles

#### Behavior Tree Builder
**Purpose**: Convert goals to executable actions
**Processes**: 2
**Message Flow**:
- **Inputs**: 
  - Goal.Prioritized → BehaviorTree.Built
  - SmallGoalPlanner.Planned → BehaviorTree.Built
- **Output**: BehaviorTree.Built → Roblox JSON API
- **Processing**: 
  - Builds behavior trees from goals (30 seconds or less)
  - Motor and action-based processing
  - Sends executable trees to Roblox

#### Small Goal Planner
**Purpose**: Decompose larger goals
**Processes**: 1
**Message Flow**:
- **Input**: Goal.Prioritized → SmallGoalPlanner.Planned
- **Processing**: Breaks down complex goals into smaller executable steps

## Message Flow Patterns

### Perception Pipeline
1. External Input → World Translator → Update messages
2. Update messages → Perception Verticles → Inner storage
3. Throttle triggers → Perception messages sent
4. All Perception messages → Prioritized Perceptions

### Goal Pipeline
1. Perception.Prioritized → Reaction Verticles (Memory, Reflection, etc.)
2. Reaction Verticles → Goal messages
3. All Goal messages → Prioritized Goals
4. Goal.Prioritized → Behavior Tree Builder → Roblox

### Feedback Loops
- Prioritized Perceptions → Inner.Perceptions (self-improvement)
- Goals Lists → Perception.GoalsLists → Prioritized Perceptions (goal awareness)
- Multiple perception types cross-inform each other

## Timing and Throttling

### Throttle Patterns
- Most perception verticles: ~3 times per second
- Prioritized Perceptions: Every 0.5 seconds
- Goals: Synchronized with perception timing

### Processing Priorities
1. Fast Perception: Immediate response (no storage)
2. Regular Perceptions: Store then process on throttle
3. Synthesis: Collected and prioritized on longer intervals
4. Goals: Updated based on perception changes

## Ayoai Impact

This cognitive architecture provides:

1. **Parallel Processing**: 22 verticles operating simultaneously mirrors human cognitive parallelism
2. **Hierarchical Synthesis**: Multiple layers from raw input to high-level goals
3. **Reactive and Deliberative**: Fast perceptions for immediate response, slower processes for planning
4. **Memory Integration**: Past experiences influence current decisions
5. **Social Awareness**: NPCs understand and react to social context
6. **Goal Persistence**: Maintains goals while handling interruptions
7. **Continuous Refinement**: Reflection and refinement loops improve behavior

The architecture enables NPCs to:
- React instinctively to immediate threats
- Plan and execute complex multi-step behaviors  
- Learn from experience
- Maintain social relationships
- Adapt to changing environments
- Balance multiple competing goals

This design supports Ayoai's goal of creating truly autonomous NPCs that exhibit human-like decision-making and behavioral complexity in game environments.
