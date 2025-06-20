# Research Notes Organization

This directory contains Zak's research notes reorganized from the original `ZakResearchSurvey.md` file for better navigation and ongoing research documentation.

## Directory Structure

### Core-Concepts/
Fundamental concepts for autonomous agent behavior:
- `perception.md` - World understanding, spatial perception, player tracking, ownership
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
- `testing.md` - Testing strategies and frameworks
- `prompt-engineering.md` - Prompt building techniques and archives
- `cognitive-architecture.md` - NPC cognitive architecture with 22 verticles and 97 processes
- `roblox-json-examples.md` - Roblox JSON data structures and message formats

### Market-Research/
Competitive analysis and industry research:
- `competitors.md` - Analysis of Altera, Inworld, Soul Machines, etc.
- `surveys-reviewed.md` - Academic surveys and papers reviewed
- `general-agents.md` - General agent frameworks and architectures

### Project-Notes/
Personal reflections and evolving thoughts:
- `thoughts-december-2023.md` - December 2023 reflections on planning and implementation

## Migration Map from ZakResearchSurvey.md

| Original Section | New Location | Lines |
|-----------------|--------------|-------|
| World Understanding & Perception | Core-Concepts/perception.md | 540-599 |
| Player Tracking | Core-Concepts/perception.md | 542-571 |
| Ownership | Core-Concepts/perception.md | 572-599 |
| Core Loops of Every Soul | Core-Concepts/consciousness-and-cognition.md | 750-835 |
| Cognition is All You Need | Core-Concepts/consciousness-and-cognition.md | 890-918 |
| Specific Loops per personality | Core-Concepts/personality-systems.md | 1834-1876 |
| Cognitive Biases | Core-Concepts/personality-systems.md | 2244-2264 |
| Memory Systems (all subsections) | Memory-Systems/* | 2656-3726 |
| Task Decomposition | Planning-and-Reasoning/task-decomposition.md | 4100-4596 |
| Multi-Plan Selection | Planning-and-Reasoning/multi-plan-selection.md | 5303-6500 |
| Planner-Aided Planning | Planning-and-Reasoning/planner-aided-planning.md | 6501-7350 |
| Reflection and Refinement | Learning-and-Adaptation/reflection-and-refinement.md | 7816-8471 |
| Behavior Generation | Learning-and-Adaptation/behavior-generation.md | 8472-9400 |
| The Bitter Lesson & Dave Mark | Implementation/game-ai-architectures.md | 10025-10255 |
| ABC Method | Implementation/abc-method.md | 10406-10863 |
| General Agents | Market-Research/general-agents.md | 10864-11219 |
| Competitors section | Market-Research/competitors.md | 11220-11703 |
| Testing | Implementation/testing.md | 11704-11761 |
| Prompt Building | Implementation/prompt-engineering.md | 11763-12609 |
| Surveys Reviewed | Market-Research/surveys-reviewed.md | 10256-10379 |
| Project Notes - December 2023 | Project-Notes/thoughts-december-2023.md | 9570-9650 |

## How to Add New Research

When adding new research:

1. **Determine the category** - Which directory best fits your research topic?
2. **Check existing files** - Does it belong in an existing file or need a new one?
3. **Follow the format**:
   ```markdown
   ## Paper/Tool/Concept Name
   
   ### Overall Notes
   - Key insights
   - How it relates to Ayoai's autonomous behavior goals
   
   ### Details
   [Research content, quotes, analysis]
   
   ### Screenshots/Figures
   ![Description](/images/media/imageXXX.png)
   ```

4. **Update this README** if you create new files
5. **Keep images** in the original `ZakResearchSurveyImages/media/` directory

## Key Patterns to Maintain

1. **"Overall Notes" sections** - Start major topics with high-level context
2. **Ayoai relevance** - Always connect research back to autonomous agent behavior
3. **Visual documentation** - Include screenshots and diagrams where helpful
4. **Dated reflections** - Add dates to evolving thoughts and insights

## Quick Reference

- **Total research items**: ~168 subsections from original document
- **Images**: 276 screenshots/diagrams (kept in original location)
- **Time span**: December 2023 - present
- **Focus**: Autonomous agent behavior for Roblox and similar environments