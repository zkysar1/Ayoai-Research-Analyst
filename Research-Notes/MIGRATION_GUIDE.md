# Research Notes Migration Guide

This guide explains how to organize your research notes using the new structure and how to add new research going forward.

## Quick Start for Adding New Research

### 1. Determine the Category

Ask yourself: "What is the primary focus of this research?"

- **Perception/Sensing**: How agents understand their environment → `Core-Concepts/perception.md`
- **Consciousness/Thinking**: Cognitive architectures, consciousness models → `Core-Concepts/consciousness-and-cognition.md`
- **Personality/Psychology**: Personality types, biases, emotions → `Core-Concepts/personality-systems.md`
- **Memory**: Any memory architecture or system → `Memory-Systems/`
- **Planning**: Task decomposition, search algorithms, decision making → `Planning-and-Reasoning/`
- **Learning**: Self-improvement, reflection, adaptation → `Learning-and-Adaptation/`
- **Implementation**: Practical coding, tools, communication → `Implementation/`
- **Competition**: Analysis of other companies/products → `Market-Research/competitors.md`
- **Personal Notes**: Your evolving thoughts and reflections → `Project-Notes/`

### 2. Format Your Research Entry

Use this template:

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
![Description](../../ZakResearchSurveyImages/media/imageXXX.png)

### Code/Examples
```language
// Any code snippets
```
```

### 3. When to Create New Files

Create a new file when:
- A topic becomes too large (>500 lines)
- You have a distinct subtopic with 5+ research items
- It would improve navigation and clarity

Name files descriptively: `specific-topic-name.md` (lowercase, hyphens)

## Migration Process from ZakResearchSurvey.md

### Step 1: Identify Content Block
1. Find your section in the original file
2. Note the line numbers (use the Migration Map in README.md)
3. Include all subsections and "Overall Notes"

### Step 2: Extract Content
1. Copy the entire section including headers
2. Preserve all formatting (lists, code blocks, etc.)
3. Keep image references exactly as they are

### Step 3: Update Image Paths
Change image paths from:
```
![Description](ZakResearchSurveyImages/media/image1.png)
```
To:
```
![Description](../../ZakResearchSurveyImages/media/image1.png)
```

### Step 4: Add Context Header
At the top of each new file, add:
```markdown
# [Topic Name]

[One paragraph description of what this research covers and why it matters for Ayoai]

---
[Original content follows]
```

## Best Practices

### 1. Maintain Connectivity
- Cross-reference related research with links
- Example: "See also: [Memory Systems](../Memory-Systems/rag-based-memory.md#specific-section)"

### 2. Date Your Entries
- Add dates to new research entries
- Include "Last Updated" dates when revisiting topics

### 3. Tag Actionable Items
Use consistent tags:
- `[TODO]` - Needs investigation
- `[IMPLEMENT]` - Ready to prototype
- `[QUESTION]` - Open questions
- `[IDEA]` - Potential features

### 4. Keep Images Organized
- Continue using the central `ZakResearchSurveyImages/media/` directory
- Name new images sequentially: `image###.png`
- Add descriptive alt text

### 5. Regular Reviews
- Monthly: Review and consolidate related entries
- Quarterly: Reorganize if categories become too large
- Annually: Archive outdated research to `Archive/` directory

## Example: Adding New Research

Let's say you find a paper on "Hierarchical Memory Networks for Agents":

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
![Memory hierarchy diagram](../../ZakResearchSurveyImages/media/image363.png)
```

4. **Update the README** if you created a new file

## Maintenance Schedule

- **Daily**: Add new research to appropriate files
- **Weekly**: Review and organize recent additions
- **Monthly**: Consolidate related entries, update cross-references
- **Quarterly**: Major reorganization if needed

## Questions?

If unsure where something belongs:
1. Check similar existing research
2. Default to broader categories
3. Add a note in your entry about potential reorganization
4. Review during monthly consolidation