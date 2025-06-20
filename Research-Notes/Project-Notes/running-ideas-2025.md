# Ongoing Research Ideas 2025

This document captures a running list of ideas for expanding Ayoai's autonomous agent research. Each entry includes initial thoughts and possible directions for future exploration.

## Collective Agent Organizations
**Date Added**: 2025-06-04

### Overall Notes
- Extend the "cognitive light cone" concept from [Consciousness and Cognition](../Core-Concepts/consciousness-and-cognition.md#the-computational-boundary-of-a-self) to groups of agents.
- Treat guilds or factions as higher-order entities with their own goals and memory.
- **Ayoai Relevance**: Enables emergent faction behavior and large-scale coordinated actions.

### Implementation Ideas
- Shared memory pools for group-level knowledge.
- Distributed decision making via message passing between member NPCs.
- Group reputation scores influencing individual motivations.

## Adaptive Player Modeling
**Date Added**: 2025-06-04

### Overall Notes
- NPCs continuously analyze player interactions and adjust behavior trees accordingly.
- Builds on the preference learning notes in [Personality-Specific Processing](../Core-Concepts/consciousness-and-cognition.md#personality-specific-processing).
- **Ayoai Relevance**: Creates personalized experiences that evolve with each player's style.

### Implementation Ideas
- Maintain a lightweight player profile (preferred tactics, interaction frequency).
- Behavior tree parameters adapt based on profile metrics.
- Optional privacy-friendly analytics for developers.

## Emotionally Weighted Memory
**Date Added**: 2025-06-04

### Overall Notes
- Memories associated with strong emotions have greater retrieval priority.
- Connects to the emotional perception system in [Perception and World Understanding](../Core-Concepts/perception.md#non-verbal-communication).
- **Ayoai Relevance**: More believable recall patterns and evolving personalities.

### Implementation Ideas
- Tag memory entries with emotion intensity scores.
- Decay rates modified by emotional weight.
- Influence goal selection when recalling similar past situations.

## Procedural World Scripting
**Date Added**: 2025-06-04

### Overall Notes
- Automate Roblox world creation using text prompts.
- Related to the World2World concept noted in [Perception](../Core-Concepts/perception.md#world2world).
- **Ayoai Relevance**: Rapidly prototype environments for agent training and demos.

### Implementation Ideas
- LLM generates a high-level scene description.
- A converter script translates description into Roblox API calls.
- Attach basic behavior trees to created objects for immediate testing.

## Self-Evolving Behavior Trees
**Date Added**: 2025-06-04

### Overall Notes
- Combine evolutionary algorithms with the existing ABC framework.
- Inspired by the "Darwin Godel Machine" mention in [Research Inbox](../../Research%20Inbox.md).
- **Ayoai Relevance**: Allows agents to optimize strategies over time without manual tuning.

### Implementation Ideas
- Mutation operators for behavior tree nodes.
- Fitness evaluation based on goal completion efficiency.
- Archive successful trees for reuse across similar NPC types.
