# ABC-Based NPC Design System

## Overview

This document outlines a comprehensive NPC behavior system based on the Antecedent-Behavior-Consequence (ABC) framework. The system enables NPCs to learn from observation, make decisions based on past experiences, and exhibit autonomous behaviors in game environments.

## Core Concepts

### The ABC Framework
- **Antecedent**: An event or situation that triggers a behavior
- **Behavior**: The action taken in response to the antecedent
- **Consequence**: The outcome or result of the behavior

### Key Principle: Observational Learning
"When someone observes a behaviour, they can learn it. When someone observes a consequence to a behaviour, they can learn both the behaviour and its consequence."

## System Architecture

### 1. PAST_ABCs Table
The main historical record of all antecedents, behaviors, and consequences for each NPC.

**Key Fields**:
- `objectName`: The NPC experiencing the antecedent
- `antecedentName`: The triggering event
- `antecedentType`: Category (system/act of god, behavior, context)
- `behaviorName`: The action taken
- `initiatedBy_objectName`: Who initiated the antecedent
- `initiatedBy_purposeBar`: Which purpose bar motivated it
- `affects_objectName`: Who is affected by the consequence
- `affects_purposeBar`: Which purpose bar is impacted
- `affects_consequence`: The specific consequence value
- `fastTriggerPercent`: How quickly the trigger fires
- `impactValue`: Magnitude of the consequence
- `probabilityOfImpact`: Likelihood of the consequence

**Example Entries**:
- Birth: System-initiated, affects all core purpose bars (Health=100, Energy=100, etc.)
- Attack: Behavior-initiated, affects Health bar negatively
- Observation: Context-initiated, affects awareness/learning

### 2. Player_Knowables_Mapping
Tracks what each NPC knows about other objects in the world.

**Structure**:
```
key: knownBy_Player + objectName
knownBy_Player: The NPC who knows
objectName: The object they know about
accessToObjectConfig: Boolean - can they see the object's configurations?
```

**Purpose**: Controls information access and enables shared/hidden knowledge between NPCs.

### 3. Configurations
Stores the current state and attributes of each object/NPC.

**Key Attributes**:
- `objectType`: character, shape, tool, etc.
- `areConfigurationsAccessibleIfSeen`: Privacy setting
- `characterOwner_playerID`: Ownership
- Functions of Behavior parameters

**Note**: Must be bi-temporal to track historical states for analysis and prediction.

### 4. Consequence_Bars (Purpose Bars)
The motivational system that drives NPC behavior, inspired by personal life tracking methodologies.

**Core Concept**: Purpose bars represent an NPC's life goals, habits, and motivational drivers - essentially "polls" that the NPC continuously responds to based on their experiences and environment.

**Types**:
- **Core/Instinctual**: Health, Energy, Safety (required for survival)
- **Social**: Family bonds, Friendship, Social status
- **Personal Growth**: Learning, Skill development, Achievement
- **Lifestyle**: Exercise, Diet, Sleep quality, Entertainment
- **Professional**: Work satisfaction, Career advancement, Income
- **Optional/Custom**: Seasonal activities, hobbies, specific project goals

**Properties**:
- `consequenceCategoryImportance`: Priority level (how much this matters to the NPC)
- `consequenceCategoryGauge`: Current value/satisfaction level
- `purposeGaugeType`: max (capped) or cumulative
- `purposeMax`: Maximum possible value
- `frequency`: How often this goal needs attention (daily, weekly, monthly, yearly)
- `streakCount`: Consecutive successful responses
- `averageDays`: Historical performance metrics
- `dueDate`: When this goal next needs attention
- `urgencyFlag`: Visual/priority indicator when overdue

**Advanced Features**:
- **Goal Linking**: Purpose bars can be interconnected (e.g., "exercise" helps "health" and "mood")
- **Seasonal Variation**: Some bars fluctuate with time/environment (e.g., "outdoor activities" peaks in summer)
- **Automatic Triggers**: Some bars can auto-populate based on NPC actions or observations
- **Streak Tracking**: NPCs maintain streaks and feel consequences when broken
- **Reason Tracking**: NPCs can have "reasons why" for each goal, driving decision weight

**Machine Learning Integration**:
- **Ideal Frequency Prediction**: AI learns optimal frequency for each goal based on NPC personality
- **Pattern Recognition**: Automatic categorization and labeling of related behaviors
- **Predictive Analytics**: Anticipate when NPCs will likely fail at goals and suggest interventions
- **Behavioral Clustering**: Group similar NPCs by their purpose bar patterns

**Monetization Opportunity**: Players can create and sell custom purpose bars in a marketplace, complete with pre-built reasoning and behavioral patterns.

### 5. Antecedents_Available
The library of possible triggers that NPCs can recognize and respond to.

**Categories**:
- Life-threatening events (fast trigger)
- Goal-oriented events (slow trigger)
- Context changes
- Recursive antecedents (antecedents that trigger other antecedents)

**Key Concept**: Antecedents can be hierarchical and recursive, allowing complex behavioral chains.

### 6. Behaviors_Available
The library of actions NPCs can perform.

**Structure**:
```
behaviorName: The action identifier
methodNameInCode: Implementation method
variablePurpose: What the variable controls
variableName: Parameter name
variableValueType: Data type
```

**Examples**:
- Movement: walk, run, move (with destination, reason, speed)
- Combat: attack (with target, weapon, reason)
- Social: observe, share_config, request_permission
- Tool use: take_tool, use_tool

### 7. Methods_Per_Behavior
Physics engine adapter that translates behaviors into game engine actions.

**Purpose**: Maps abstract behaviors to concrete physics/game engine calls with required parameters.

## Heartbeat Algorithm

The decision-making process that runs each game tick:

1. **Gather Inputs**:
   - World events
   - World knowledge
   - Known contexts
   - Other NPCs' previous behaviors

2. **Priority Check**:
   - Is anything urgent? (life threats)
   - Check fastTriggerPercent for immediate actions
   - Evaluate consequence impacts

3. **Goal Evaluation**:
   - Check if current goals are complete
   - Identify new goals based on purpose bars
   - Consider recursive antecedents

4. **Behavior Selection**:
   - Sum all antecedents
   - Weight by function of behavior preferences
   - Select highest priority action

5. **Execute and Record**:
   - Perform selected behavior
   - Record in PAST_ABCs
   - Update consequence bars

## Learning Mechanisms

### Observational Learning
NPCs can learn by observing others:
- See behavior → Learn behavior
- See behavior + consequence → Learn both
- Share configurations → Share knowledge

### Sequence Recognition
- Detect patterns in antecedent sequences
- Create new compound antecedents
- Propose patterns to users for naming/selling

### Experience Sharing
- NPCs can share past experiences
- Family/team relationships affect sharing
- Permanent vs temporary sharing

## Example Scenario: Red Light Green Light

**Setup**:
- NPC_With_RLGL_History: Knows the game rules
- NPC_No_RLGL_History: Doesn't know the game
- Both NPCs like money (high Money purpose bar)

**Process**:
1. Game starts, money reward announced
2. Experienced NPC starts playing
3. Inexperienced NPC observes:
   - Sees behavior (stop on red, go on green)
   - Sees consequence (earn money)
   - Learns the pattern
4. Inexperienced NPC can now play

## Implementation Considerations

### API Design
- One API call per NPC per heartbeat
- Input: Past antecedents, current state
- Output: Next behavior to execute

### Scalability
- NPCs continue running on server in "autopilot"
- Players can observe and adjust parameters
- Behaviors persist across sessions

### Monetization
- Antecedent packs (pre-built behavioral patterns)
- Purpose bar marketplace
- Custom behavior libraries

## Future Extensions

### Planned Features
1. **Predictive Modeling**: Use past ABCs to predict future behaviors
2. **AI-Generated Purpose Bars**: System creates new motivations
3. **Cross-Environment Learning**: NPCs carry knowledge between games
4. **Emotion Modeling**: Add emotional states as purpose bars

### Integration Points
- Roblox adapter (current)
- Unity adapter (planned)
- Other game engines

## Ayoai Impact

This ABC-based system directly supports Ayoai's autonomous NPC goals by:

1. **True Autonomy**: NPCs make decisions based on internal motivations and past experiences
2. **Emergent Behavior**: Complex behaviors arise from simple ABC chains
3. **Social Learning**: NPCs learn from each other, creating dynamic environments
4. **Player Engagement**: Players can teach, influence, and observe NPC development
5. **Scalability**: Server-side processing enables persistent NPC lives
6. **Monetization**: Multiple revenue streams through behavioral content

The system provides a foundation for NPCs that genuinely learn, adapt, and surprise players with emergent behaviors, moving beyond scripted responses to create living game worlds.