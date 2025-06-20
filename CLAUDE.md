# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a research documentation repository for Ayoai, focusing on autonomous agent behavior research for game environments (particularly Roblox). The repository contains organized research notes, academic papers analysis, competitive research, and technical implementation details for building intelligent NPCs.

## Repository Architecture

### Research Organization Structure

The repository is organized around a multi-disciplinary research approach:

**Core-Concepts/**: Fundamental cognitive and behavioral frameworks
- Perception systems and world understanding
- Consciousness models and cognitive architectures  
- Personality systems and psychological models

**Implementation/**: Technical implementation details and frameworks
- ABC (Antecedent-Behavior-Consequence) behavioral modeling
- 22-verticle cognitive architecture with 97 processes
- Decision augmentation and behavioral analytics systems
- NPC experience tracking and journaling systems

**Memory-Systems/**: Various memory architectures for agents
- RAG-based memory implementations
- Hierarchical memory networks
- Long-term and working memory systems

**Planning-and-Reasoning/**: Decision-making and planning algorithms
- Task decomposition strategies
- Multi-plan selection (MCTS, A*, etc.)
- Planner-aided planning (PDDL integration)

**Learning-and-Adaptation/**: Self-improvement mechanisms
- Reflection and refinement systems
- Behavior generation and intrinsic motivation

**Market-Research/**: Competitive analysis and industry research
- Competitor analysis (Altera, Inworld, Soul Machines)
- General agent frameworks
- Monetization models and business strategies

### Key Technical Concepts

**ABC Framework**: The central behavioral model where NPCs learn through Antecedent-Behavior-Consequence chains, enabling observational learning and autonomous decision-making.

**Cognitive Architecture**: A 22-verticle system with 97 processes operating in 5 phases:
1. Disperse Clean Input (5 processes)
2. Analyze Input (42 processes) 
3. Generalize Perceptions (18 processes)
4. React to Perceptions (29 processes)
5. Planning Goal Actions (3 processes)

**Purpose Bars System**: NPC motivational framework inspired by personal life tracking, where NPCs maintain goals across categories like health, social, learning, lifestyle, and professional development.

**Decision Augmentation**: Evidence-based decision making where NPCs accumulate reasons and data before taking action, creating thoughtful and believable character behaviors.

## Common Research Tasks

### Adding New Research
1. **Categorize**: Determine which directory fits the research topic
2. **Check existing files**: See if it belongs in an existing file or needs a new one
3. **Follow the format**:
   ```markdown
   ## Paper/Tool/Concept Name
   
   **Source**: [URL or citation]
   **Date Added**: [YYYY-MM-DD]
   
   ### Overall Notes
   - Key insights
   - **Ayoai Relevance**: How this helps autonomous agent behavior
   
   ### Details
   [Research content, analysis]
   
   ### Screenshots/Figures
   ![Description](../images/media/imageXXX.png)
   ```

### Image Management
- Images are stored in `Research-Notes/images/media/` directory
- Use sequential naming: `image###.png`
- Reference from markdown files using relative paths: `../images/media/imageXXX.png`

### Research Migration from Large Files
When extracting content from large research files:
1. Identify content blocks by topic
2. Preserve all formatting (lists, code blocks, etc.)
3. Update image paths from old to new structure
4. Add contextual headers explaining relevance to Ayoai

## Research Quality Standards

### Required Elements for Research Entries
- **Ayoai Relevance**: Every research item must explain connection to autonomous agent behavior
- **Implementation Ideas**: How concepts could be used in Ayoai
- **Visual Documentation**: Include screenshots and diagrams where helpful
- **Cross-References**: Link to related research in other files

### Tagging System
Use consistent tags for actionable items:
- `[TODO]` - Needs investigation
- `[IMPLEMENT]` - Ready to prototype  
- `[QUESTION]` - Open questions
- `[IDEA]` - Potential features

## File Organization Principles

### When to Create New Files
- A topic becomes too large (>500 lines)
- Distinct subtopic with 5+ research items
- Would improve navigation and clarity

### Naming Conventions
- Files: `specific-topic-name.md` (lowercase, hyphens)
- Directories: `Title-Case-With-Hyphens/`
- Images: `image###.png` (sequential numbering)

## Integration with Ayoai Ecosystem

This research repository feeds into several implementation repositories:
- **Ayoai-Environment-Server**: Java-based AI engine using these cognitive architectures
- **Ayoai-Public-Web-App**: Dashboard for monitoring NPC behavior and analytics
- **Ayoai-Roblox-Integration**: Game integration using ABC frameworks and purpose bars
- **Lambda Functions**: Microservices implementing specific behavioral components

The research directly informs technical implementation of autonomous NPCs that can learn, adapt, and exhibit believable behaviors in game environments.

## Research Maintenance

### Review Schedule
- **Daily**: Add new research to appropriate files
- **Weekly**: Review and organize recent additions  
- **Monthly**: Consolidate related entries, update cross-references
- **Quarterly**: Major reorganization if categories become too large

### Quality Maintenance
- Ensure all research connects back to autonomous agent behavior goals
- Maintain visual documentation with screenshots and diagrams
- Keep cross-references updated as content evolves
- Archive outdated research to maintain relevance