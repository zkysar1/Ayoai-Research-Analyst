# Historical Planning Notes

## Initial HTN Library Research

### Libraries Evaluated

**PANDA**
- Link: https://www.uni-ulm.de/en/in/institute-of-artificial-intelligence/research/software/panda/
- Assessment: Too large and error-prone for practical implementation

**PyHop**
- Python HTN library: https://bitbucket.org/dananau/pyhop/src/master/
- Considered for conversion to Java

**GTPyhop**
- Paper: https://www.cs.umd.edu/~nau/papers/nau2021gtpyhop.pdf
- Code: https://github.com/dananau/GTPyhop
- Promising abstraction model compatible with LifingPolls concept

### Planning Language Progression

The research revealed a clear progression in planning languages:
1. **PDDL**: Basic planning domain definition
2. **HDDL**: Hierarchical extensions for methods
3. **RDDL**: Reward-based planning with MDP framework

**Key Resources**:
- HTN explanation: https://www.geeksforgeeks.org/hierarchical-task-network-htn-planning-in-ai/
- HTN in games: https://www.gameaipro.com/GameAIPro/GameAIPro_Chapter12_Exploring_HTN_Planners_through_Example.pdf
- RDDL tutorial: https://ssanner.github.io/slides/RDDL_Tutorial_ICAPS_2018.pdf
- RDDL simulator: https://github.com/ssanner/rddlsim

### RDDL vs GOAP Comparison

**RDDL (Relational Dynamic Influence Diagram Language)**:
- Reward-based planning aligned with MDP framework
- Agents maximize reward functions over time
- Handles probabilistic actions and states
- Optimizes for long-term gains
- Extensions: concurrency, constraints, integer/continuous variables

**GOAP (Goal-Oriented Action Planning)**:
- Goal-based planning with action requirements
- Actions selected based on goal satisfaction
- Cost/priority-based path optimization
- Deterministic path-finding approach
- More immediate goal focus

### Development Strategy

Initial plan before adopting behavior trees:
1. Understand code abstractions (compatible with LifingPolls)
2. Enable HDDL language intake (not hard-coded tasks/goals)
3. Consider parallel, sequence, selector nodes integration
4. Add reward modeling as final layer

## GTN vs HDDL Analysis

### Key Distinction

**Task-Oriented Planning (HDDL/HTN)**:
- Problem specifies tasks to complete
- Planner decomposes tasks hierarchically
- Follows predefined methods and structures
- Example: "Complete grocery shopping" → decompose into subtasks

**Goal-Oriented Planning (GTN)**:
- Problem specifies goals to achieve
- Planner selects and sequences tasks dynamically
- Adapts task choices based on conditions
- Example: "Have groceries at home" → choose best approach

### Roblox Application

For smart humanoids in Roblox, GTN approach is preferred because:
1. **Dynamic Environment**: Adapts to player actions and game events
2. **Flexible Goal Management**: High-level goals over specific sequences
3. **Adaptive Task Selection**: Real-time task choosing based on context
4. **Intelligent Behavior**: More reactive and responsive NPCs

### GTN Implementation Example

**Goal Structure**:
```
Goals:
- Protect Player
- Explore Surroundings  
- Defend Area
- Gather Resources
- Escape Danger

Tasks per Goal:
- Protect Player:
  - Follow player
  - Identify threats
  - Engage if in range
- Explore:
  - Walk to random points
  - Collect objects
  - Avoid hazards
```

**Basic Lua Implementation**:
```lua
local humanoid = script.Parent
local currentGoal = "Explore"

local goals = {
    ProtectPlayer = function() ... end,
    Explore = function() ... end,
    EscapeDanger = function() ... end
}

local function updateGoal()
    if humanoid.Health < 20 then
        currentGoal = "EscapeDanger"
    elseif playerNearby then
        currentGoal = "ProtectPlayer"
    else
        currentGoal = "Explore"
    end
end
```

## Hybrid Approach: Goals + HDDL

### Proposed Architecture

Combining goal-oriented layer with HDDL foundation:
- **Actions**: Low-level primitives (moveTo, attack, pickUp)
- **Methods**: Decomposition strategies using actions
- **Tasks**: Compound behaviors using methods
- **Goals**: High-level states driving task selection

### Benefits
1. **Scalability**: Organized framework for complex scenarios
2. **Reusability**: Tasks/methods shared across goals
3. **Realistic Behavior**: Dynamic goal switching
4. **Modularity**: Easy addition of new elements

### Implementation Considerations
- Simple, fast Roblox actions
- Lightweight real-time planning
- Modular goal/task definitions
- Performance-conscious design

## Final Architecture Plan (November 2024)

### 1. Character State Service
**Purpose**: Maintain conflated world state for decision-making

**Key Features**:
- conflateStateFeatureList: Monitored feature data from perceivers
- Triggered after Roblox updates
- Outputs conflated state as JSON
- Dynamic feature list management
- Perception-based filtering

### 2. Behavior Tree Manager
**Purpose**: Execute behavior trees and coordinate with Roblox

**Key Features**:
- State accumulation during action execution
- Success/failure propagation
- Effects validation and updating
- Archive completed trees
- Trigger replanning on completion

### 3. Intent Engine
**Purpose**: Manage character goals and priorities

**Key Features**:
- Monitor state for emergencies
- Priority-based intent switching
- Trigger new plans on tree completion
- Maintain problem queue
- Coordinate with find-cause alerts

### 4. Completed Trees Service
**Purpose**: Archive and analyze behavior history

**Key Features**:
- Persistent storage of completed trees
- Composite key: outcome + effects + preconditions + parameters
- Discrepancy detection for similar actions
- Alert intent engine of anomalies

### 5. Seed Maker
**Purpose**: Generate new behavior trees based on intent

**Methods**:
- **MakeSeed_Intent**: Plan based on property targets
- **MakeSeed_ExploreInterest**: Generate exploration behaviors
- **MakeSeed_FindCause**: Investigate effect discrepancies
- **MakeSeed_TryAgain**: Repair failed trees

**Key Algorithms**:
- Match archived actions to current intent
- Prioritize by intent movement potential
- Validate precondition matching
- Single predicate addition for cause finding

### Critical Design Decisions

1. **Noise Filtering**: Perceptions must clean irrelevant state data
2. **Predicate Tracking**: Maintain list of active predicates for monitoring
3. **Effect Comparison**: Only compare negative effects to reduce noise
4. **Incremental Learning**: Add one predicate at a time for clear causation
5. **Shared Tree Storage**: One copy per unique tree across all agents

### Open Questions
- How to handle unknown causation (e.g., health lowering without clear cause)
- Balance between exploration and exploitation in seed generation
- Optimal frequency for goal reevaluation
- Managing computational overhead in real-time environment