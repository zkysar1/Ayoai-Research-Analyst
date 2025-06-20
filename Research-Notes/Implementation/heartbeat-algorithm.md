# NPC Heartbeat Algorithm

## Overview

The heartbeat algorithm is the core decision-making process that runs for each NPC on every game tick. It determines what behavior an NPC should execute next based on their current state, past experiences, and environmental context.

## Algorithm Flow

### Main Function: getNextBehavior()
A recursive function that determines the next action for an NPC.

### Step 1: Gather World State
```
1. Gather Worldly Events
   - Environmental changes (weather, time, disasters)
   - Object state changes
   - Sound levels, visibility conditions
   
2. Gather Worldly Knowledge
   - Known objects and their locations
   - Access permissions
   - Relationship statuses
   
3. Gather Known Contexts
   - Current situation awareness
   - Active threats or opportunities
   - Social dynamics
   
4. Gather Previous Heartbeat Behaviors
   - What other NPCs just did
   - Success/failure of recent actions
   - Observable consequences
```

### Step 2: Sum Antecedents
Function: `sumAntecedents()`

**Process**:
1. Collect all active antecedents
2. Check recursive antecedent patterns
3. Evaluate antecedent criteria:
   - Part of ongoing sequence?
   - Did not result in death?
   - Recent completion of prerequisite?
   - Success/failure flags

**Key Considerations**:
- Antecedents can be hierarchical (parent-child relationships)
- Sequences matter (three events in a row = new antecedent)
- Machine learning to detect patterns

### Step 3: Priority Evaluation

**Urgent Check**:
```
IF any antecedent has:
   - Life threat consequence AND
   - Fast trigger (high fastTriggerPercent)
THEN prioritize immediate response
```

**Non-Urgent Processing**:
1. Look at consequence list
2. Map to affected purpose bars
3. Check function of behavior importance
4. Select based on highest impact

### Step 4: Goal Management

**Goal Completion Check**:
- When sum of certain antecedents occur
- When absence of antecedents detected
- Unlock new goals and possibilities

**Active Goal Tracking**:
- Maintain in-flight goals
- Handle interruptions (e.g., attack while mowing grass)
- Remember progress for resumption

### Step 5: Behavior Selection

**Decision Factors**:
1. **Purpose Bar States**:
   - Current values vs. importance
   - If bar very full, avoid actions that increase it
   - If bar very low, prioritize actions that fill it

2. **Probability Calculation**:
   - Each antecedent affects functions of behavior
   - Important enough = do it despite poor odds
   - Weight by NPC personality configuration

3. **Context Awareness**:
   - Must know consequences to make connections
   - E.g., expensive suit = tangible wealth indicator

### Step 6: Execution and Learning

**Execute Behavior**:
1. Call appropriate method from Behaviors_Available
2. Pass required parameters
3. Update game state

**Record Results**:
1. Add to PAST_ABCs table
2. Update consequence bars
3. Mark old antecedents as irrelevant if needed
4. Check for carryover consequences

## Special Processes

### Sequence Detection
```
FOR each completed behavior sequence:
   IF outcomes are consistent:
      - Propose as new compound antecedent
      - Tag as "potential antecedent"
      - Offer to user for naming/monetization
```

### Risk Assessment
- Calculate: `riskLevel = impactValue × (1 - probabilityOfImpact)`
- Consider counter behaviors and their strength
- Evaluate escape routes

### Learning from Others
```
IF observed behavior has positive outcome:
   - Add to available behaviors
   - Copy antecedent-consequence mapping
   - Adjust for own purpose bars
```

## Performance Optimizations

### Caching Strategies
- Keep recent antecedents in fast-access queue
- Pre-calculate common sequences
- Use binary flags for speed (1/0 for active/inactive)

### Pruning Old Data
- Mark irrelevant antecedents (e.g., threat from dead NPC)
- Use bi-temporal model for "what we thought was true"
- Limit date searches with indexed lookups

### Parallel Processing
- NPCs process independently
- Batch API calls for efficiency
- Async behavior execution

## Configuration Integration

### NPC-Specific Weights
Each NPC has unique:
- Function of behavior preferences
- Purpose bar importance values
- Risk tolerance levels
- Learning rates

### Dynamic Adjustments
- Purpose bars change during gameplay
- Importance shifts based on context
- New bars can be added/removed

## Edge Cases

### Conflicting Goals
When multiple high-priority goals conflict:
1. Check life-essential first
2. Use function of behavior tiebreaker
3. Random selection if still tied

### No Valid Behaviors
If no behaviors meet criteria:
1. Default to "wait/stand by"
2. Enter "think state" to gather information
3. Request help from other NPCs

### Recursive Loops
Prevent infinite recursion:
- Max depth limit
- Cycle detection
- Fallback behaviors

## Example Execution

**Scenario**: NPC sees another NPC with treasure

**Heartbeat Process**:
1. **Antecedent Detected**: "sees valuable object"
2. **Sum Antecedents**: 
   - Treasure visible
   - Owner present
   - Low money bar
3. **Priority Check**: Not life-threatening, medium priority
4. **Goal Evaluation**: "Acquire money" goal active
5. **Behavior Options**:
   - Steal (if evil function high)
   - Trade (if good function high)
   - Ignore (if risk too high)
6. **Selection**: Based on personality, choose action
7. **Execute**: Perform chosen behavior
8. **Record**: Update PAST_ABCs with results

## Future Enhancements

### Predictive Lookahead
- Simulate next 5 heartbeats
- Choose path with best outcomes
- Build in design for future implementation

### Multi-Agent Coordination
- Share heartbeat results between allied NPCs
- Coordinate complex behaviors
- Distributed decision making

### Emotional Modeling
- Add emotional state to decision weight
- Temporary priority shifts
- Stress/calm affects processing

## Ayoai Impact

The heartbeat algorithm enables:
- **Real-time Decision Making**: NPCs respond naturally to dynamic environments
- **Emergent Behavior**: Complex patterns from simple rules
- **Scalable Architecture**: Efficient processing for many NPCs
- **Player Predictability**: Understandable cause-effect relationships
- **Continuous Learning**: NPCs improve over time

This creates NPCs that feel alive and responsive, supporting Ayoai's vision of truly autonomous game characters.
