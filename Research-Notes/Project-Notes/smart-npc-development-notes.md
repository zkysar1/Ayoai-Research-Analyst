# Smart NPC Development Notes

## Overview

This document captures various development thoughts, implementation challenges, and design decisions for the Smart NPC system. These are raw notes and evolving ideas about building truly autonomous NPCs.

## Key Insights and Decisions

### On Learning and Observation
"When someone observes a behaviour, they can learn it. When someone observes a consequence, to a behaviour, they can learn both the behaviour and its consequence."

This fundamental principle drives the entire system - NPCs must be able to learn from watching others, not just from their own experiences.

### The Input Problem
The core challenge: "What are the inputs required to create the 'saving' of new antecedents to the past antecedents tab?"

The system needs to capture:
- The behavior observed
- The context in which it occurred
- The consequences that followed
- The observer's interpretation

### Recursive Antecedents
Major architectural decision: Antecedents can trigger other antecedents, creating behavioral chains.

Example: 
- "Go to dojo" → "Enter building" → "Bow" → "Begin training"
- Each step can be its own antecedent with criteria

This recursive nature allows complex behaviors from simple building blocks.

### Model vs Predictor
"This is not a predictor. This is a model. It is only as good as the inputs."

Important distinction - we're modeling behavior based on inputs, not trying to predict the future. Quality of NPC behavior directly correlates with quality of input data.

## Technical Challenges

### Sequence Recognition
Problem: How to detect meaningful patterns in behavior sequences?

Proposed Solution:
1. Brute force correlation analysis (>0.9 correlation)
2. Manual data building initially
3. Side algorithm to recognize and propose sequences
4. User naming and monetization of discovered patterns

### Bi-Temporal Data Model
Need to track both:
- What actually happened (objective history)
- What NPCs believed happened (subjective history)

"We need to change what we thought was true."

### Fast vs Slow Triggers
Design consideration: Some antecedents need immediate response (life threats), others can be deliberated.

Implementation: `fastTriggerPercent` field determines urgency.

### Context Understanding
Challenge: NPCs need to make connections that aren't explicit.

Example: "Expensive suit" → "Has money" → "Potential theft target"

This requires building association layers into the model.

## Architecture Evolution

### From Simple to Complex
Initial: Direct antecedent → behavior mapping
Evolved: Antecedent → context evaluation → goal consideration → behavior selection

### API Design Philosophy
"One API call per NPC" - Each NPC is independent, making the system:
- Scalable
- Chargeable per unit
- Easy to parallelize

### World Persistence
"NPCs should continue playing behind the scenes on the server"

This enables:
- Autopilot mode for observation
- Persistent world simulation
- Emergent behaviors when players return

## Monetization Insights

### The App Store Model
"Like a market in a market, holy cow"

Realization that users creating content for other users creates exponential value:
- Purpose bars marketplace
- Antecedent packs
- Experience sharing

### Accessibility vs Monetization
- Free: Build for yourself, single use
- Paid: Multiple NPCs, sharing, advanced features

This freemium model encourages experimentation while monetizing scale.

## Future Considerations

### Cross-Platform Vision
"Build Roblox adapter... then Unity adapter, etc."

System designed to be platform-agnostic from the start, with adapters for each game engine.

### Learning Optimization
"Give NPCs a 'want to learn' purpose bar"

NPCs with high learning motivation could:
- Watch TV to learn behaviors
- Observe other NPCs more carefully
- Experiment with new actions

### Failure Handling
"Humans do not repeat behaviors that lead to failure"

But: "Not sure I agree with removing everything... That does not change that the behavior did actually happen"

Decision: Keep failed behaviors in history but mark them to affect future probability calculations.

## Implementation Priorities

### Phase 1: Core System
1. Basic ABC recording
2. Simple decision algorithm
3. Purpose bar system
4. Basic behaviors (move, interact)

### Phase 2: Learning Systems
1. Observation mechanics
2. Sequence detection
3. Experience sharing
4. Recursive antecedents

### Phase 3: Advanced Features
1. Predictive lookahead
2. Multi-agent coordination
3. Emotional modeling
4. Cross-game transfer

## Quotes and Thoughts

### On Complexity
"This got out of date. I should update the scenario."
- Reminder that documentation needs constant updating as system evolves

### On Scope
"Nah, won't make it that much quicker? But maybe in the end it would."
- Constant evaluation of optimization vs development time

### On Vision
"Players can change their own bars and play with up down switches. They can switch to autopilot and let it run, or start moving around and new behaviors should be introduced."
- The dream of truly interactive, learning NPCs

## Lessons Learned

1. **Start Simple**: Basic behaviors first, complex emergence later
2. **Design for Recursion**: Everything should be able to reference everything
3. **Think Monetization Early**: Built into the architecture, not bolted on
4. **Observation is Key**: Learning from others is more important than individual experience
5. **Context Matters**: Behaviors without context are meaningless

## Open Questions

1. How to handle NPC "death" and respawning with memories?
2. Should NPCs be able to "forget" to save processing?
3. How to balance realism with fun gameplay?
4. What's the right level of behavioral granularity?
5. How to prevent exploitation of the learning system?

## Ayoai Impact

These development notes reveal the deep thinking behind creating truly autonomous NPCs:

1. **Beyond Scripting**: Moving from predetermined behaviors to emergent ones
2. **Social Learning**: NPCs that genuinely learn from each other
3. **Player Agency**: Giving players tools to shape NPC development
4. **Sustainable Business**: Monetization through value creation, not just consumption
5. **Technical Innovation**: Novel approaches to behavior modeling and decision making

The system represents a fundamental shift in how game AI works - from scripted responses to genuine autonomy based on experience and observation.