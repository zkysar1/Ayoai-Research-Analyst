# Research Notes Organization

This directory contains comprehensive research notes on autonomous agent behavior, reorganized from the original survey document for better navigation and ongoing documentation.

## Directory Structure

### Core-Concepts/
Fundamental concepts for autonomous agent behavior:
- `perception.md` - World understanding, spatial perception, ownership systems
- `consciousness-and-cognition.md` - Core loops, cognitive mechanisms, consciousness models
- `personality-systems.md` - Personality types, cognitive biases, belief systems

### Memory-Systems/
Various memory architectures and implementations:
- `memory-overview.md` - General memory architecture considerations
- `rag-based-memory.md` - RAG implementations (MemoryBank, MemGPT, HippoRAG, etc.)

### Planning-and-Reasoning/
Planning algorithms and decision-making:
- `task-decomposition.md` - Breaking down complex tasks into manageable steps
- `multi-plan-selection.md` - Tree/Graph search methods (MCTS, A*, etc.)
- `planner-aided-planning.md` - PDDL and external planner integration

### Learning-and-Adaptation/
Self-improvement and behavior generation:
- `reflection-and-refinement.md` - Self-improvement mechanisms (Reflexion, CRITIC, Voyager)
- `behavior-generation.md` - Behavior trees and intrinsic motivation

### Implementation/
Practical implementation details:
- `game-ai-architectures.md` - Dave Mark's utility AI, behavior trees, The Bitter Lesson
- `abc-method.md` - Antecedent-Behavior-Consequence framework
- `abc-npc-design.md` - Comprehensive ABC framework with table structures and decision-making
- `testing.md` - Testing strategies and frameworks
- `prompt-engineering.md` - Prompt building techniques and archives
- `cognitive-architecture.md` - NPC cognitive architecture with 22 verticles and 97 processes
- `agent-capabilities.md` - Communication systems and tool use capabilities
- `heartbeat-algorithm.md` - Core decision-making process for NPCs per game tick
- `decision-augmentation.md` - Evidence-based decision making systems
- `behavioral-analytics.md` - Pattern recognition and predictive modeling for NPC behavior
- `npc-experience-tracking.md` - Journaling and experience systems for character depth
- `Player Tracking.md` - Player detection, tracking systems, and agent-player interactions
- `Parameterizing Behavior Trees.md` - Technical implementation of behavior tree parameterization
- `Public Datasets.md` - Available datasets for training and testing

### Market-Research/
Competitive analysis and industry research:
- `competitors.md` - Analysis of Altera, Inworld, Soul Machines, and other key players
- `surveys-reviewed.md` - Academic surveys and foundational papers
- `general-agents.md` - General agent frameworks and architectures
- `roblox-worlds.md` - Roblox-specific opportunities and game concepts

## Overview

- **Research Items**: ~168 subsections covering all aspects of autonomous agents
- **Visual Documentation**: 276 screenshots and diagrams
- **Time Span**: December 2023 - present
- **Primary Focus**: Autonomous agent behavior for gaming environments

## Contributing New Research

### 1. Determine the Category

Choose the appropriate category based on primary focus:

- **Perception/Sensing**: Environmental understanding → `Core-Concepts/perception.md`
- **Consciousness/Cognition**: Cognitive architectures and models → `Core-Concepts/consciousness-and-cognition.md`
- **Personality/Psychology**: Character traits and biases → `Core-Concepts/personality-systems.md`
- **Memory**: Architecture and retrieval systems → `Memory-Systems/`
- **Planning**: Decision-making and task decomposition → `Planning-and-Reasoning/`
- **Learning**: Adaptation and self-improvement → `Learning-and-Adaptation/`
- **Implementation**: Technical details and tools → `Implementation/`
- **Market Analysis**: Competitive landscape → `Market-Research/`
- **Project Evolution**: Design decisions and rationale → `Project-Notes/`

### 2. Research Entry Format

```markdown
## [Paper/Tool/Concept Name]

**Source**: [URL or citation]
**Date Added**: [YYYY-MM-DD]

### Overall Notes
- Key insight 1
- Key insight 2
- **Ayoai Relevance**: How this helps with autonomous agent behavior

### Abstract/Summary
[Original abstract or your summary]

### Details
[Your detailed notes, analysis, quotes]

### Implementation Ideas
- How could we use this in Ayoai?
- What would need to be adapted?

### Screenshots/Figures
![Description](../images/media/imageXXX.png)

### Code/Examples
```language
// Any code snippets
```
```

### 3. File Creation Guidelines

Create new files when:
- Topics exceed 500 lines
- Distinct subtopics contain 5+ research items
- Navigation would be improved

File naming: `descriptive-topic-name.md` (lowercase with hyphens)

## Best Practices

1. **Cross-Reference Related Research**
   - Link between related topics
   - Format: `[Topic Name](../Category/file.md#section)`

2. **Date All Entries**
   - Include creation date
   - Update modification dates

3. **Use Consistent Tags**
   - `[TODO]` - Requires investigation
   - `[IMPLEMENT]` - Ready for prototyping
   - `[QUESTION]` - Open questions
   - `[IDEA]` - Feature concepts

4. **Image Management**
   - Store in `images/media/` directory
   - Sequential naming: `image###.png`
   - Include descriptive alt text

5. **Maintenance Schedule**
   - Monthly: Consolidate related entries
   - Quarterly: Reorganize large categories
   - Annually: Archive outdated research

## Example Research Entry

Adding a paper on "Hierarchical Memory Networks for Agents":

1. **Categorize**: This goes in `Memory-Systems/`
2. **Check existing files**: Does it fit in `rag-based-memory.md`? If not, create `hierarchical-memory.md`
3. **Format your entry**:
```markdown
## Hierarchical Memory Networks for Multi-Agent Systems

**Source**: https://arxiv.org/abs/2024.example
**Date Added**: 2024-11-08

### Overall Notes
- Introduces 3-tier memory hierarchy
- Reduces memory search time by 70%
- **Ayoai Relevance**: Could dramatically improve our agent response times in complex Roblox environments

### Abstract
[Paper abstract here]

### Implementation Ideas
- Tier 1: Immediate sensory buffer (last 30 seconds)
- Tier 2: Working memory (current task context)
- Tier 3: Long-term storage (compressed experiences)

### Screenshots
![Memory hierarchy diagram](../images/media/image363.png)
```

4. **Update this README** when creating new files

## Navigation Tips

When uncertain about placement:
1. Review similar existing research
2. Choose broader categories when unsure
3. Note potential reorganization needs
4. Address during monthly reviews
