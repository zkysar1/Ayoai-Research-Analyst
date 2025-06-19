# 📋 Public Datasets

### Public PDDL Datasets
PDDL (Planning Domain Definition Language) files are crucial for Ayoai's work in AI planning. Access to a diverse range of PDDL problems and domains allows for:
  - **Benchmarking planning algorithms:** Testing the performance and scalability of planners.
  - **Training AI agents:** Providing environments and tasks for reinforcement learning or imitation learning agents.
  - **Developing new planning techniques:** Offering a corpus of existing problems to analyze and inspire new approaches.

**Interesting Aspects of Public PDDL Datasets:**
  - **Variety of Domains:** Datasets often span numerous domains, from logistics and robotics to game playing and software engineering. This variety is essential for developing robust and generalizable AI planners.
  - **Complexity Levels:** Problems within these datasets typically range from simple to extremely complex, allowing for incremental testing and development.
  - **Standardized Format:** PDDL provides a common language for representing planning problems, facilitating the comparison of different planners and techniques.

**Key Resources:**
  - **International Planning Competition (IPC):** The IPC is a major source of PDDL benchmarks. Competitions are held regularly, and the problems and domains are usually made public.
      - **IPC 2023 Classical Track Dataset:** Contains PDDL files for 7 domains (Folding, Labyrinth, Quantum Circuit Layout Synthesis, Recharging Robots, Ricochet Robots, Rubik's Cube, Slitherlink) used in the optimal, satisficing, and agile tracks. Some domains include generators and normalized versions.
          - Link: [https://github.com/ipc2023-classical/ipc2023-dataset](https://github.com/ipc2023-classical/ipc2023-dataset)
          - Associated IPC 2023 Classical Track website: [https://ipc2023-classical.github.io/](https://ipc2023-classical.github.io/)
  - **Planning.Domains Collection:** While not directly browsed in this research, [https://planning.domains/](https://planning.domains/) is a well-known community resource that hosts a large collection of PDDL domains and problem instances, including those from past IPCs. This would be a good next place to look for an even wider variety of datasets.

**Further Considerations & Ideas for Ayoai (PDDL Datasets):**
  - *Potential Challenge:* Dataset Size and Complexity: Some datasets (especially from IPCs) can be very large, requiring significant computational resources.
  - *Potential Challenge:* PDDL Feature Subsets: Planners vary in PDDL feature support; compatibility can be an issue. Normalized versions (as in IPC 2023) are helpful.
  - *Potential Challenge:* Domain-Specific Knowledge: Effective use may require understanding the nuances of each PDDL domain.
  - *Potential Challenge:* Understanding Problem Generators: While some domains come with generators for creating more instances (e.g., in IPC 2023), understanding their specific usage and parameters can be a hurdle.
  - *Key Consideration:* License and Usage: Always verify licensing terms, especially for commercial applications or redistribution.
  - *Idea for Ayoai:* Develop a strategy for local dataset management, including versioning.
  - *Idea for Ayoai:* Consider creating or utilizing PDDL feature compatibility checkers or preprocessing tools (e.g., `ipc23-normalize.def` from IPC 2023).
  - *Idea for Ayoai:* For benchmarking, strategically select a diverse subset of domains and complexity levels relevant to Ayoai’s applications.

### Character Description Database
A database of character descriptions would be a valuable asset for Ayoai, supporting various applications:
  - **Procedural Content Generation (PCG) in Games:** Generating diverse and interesting non-player characters (NPCs) with unique backstories, personalities, and appearances.
  - **Story Writing Assistance:** Providing writers with inspiration or pre-built character archetypes to populate their narratives.
  - **Training Models for Character Creation:** Fine-tuning large language models (LLMs) or other generative models to produce high-quality character descriptions based on specific prompts or constraints.
  - **Diverse AI Agent Personas:** Enabling the creation of AI agents with more relatable and varied personalities for interactive simulations, virtual assistants, or educational tools.

**Interesting Aspects of Character Description Databases:**
  - **Structured vs. Unstructured Data:** Data could range from highly structured entries (e.g., key-value pairs for traits like `strength: 10`, `hair_color: "blonde"`) to unstructured natural language descriptions (e.g., a paragraph describing a character's personality and history). A combination might be most useful.
  - **Level of Detail:** Descriptions can vary significantly in depth, from simple tags (e.g., "brave", "mysterious") to multi-page biographies.
  - **Sourcing:** Potential sources are vast, including:
      - **Public Domain Fiction:** Characters from classic literature (e.g., Project Gutenberg).
      - **Community Contributions:** Platforms allowing users to submit and tag character descriptions (e.g., wikis, forums for writers or role-players).
      - **Crowdsourcing:** Dedicated efforts to create and annotate character profiles.
  - **Ontologies/Taxonomies:** A well-defined structure for traits, personality models (e.g., Big Five, Myers-Briggs), and relationships could make the database more powerful.

**Potential Sources (General Ideas):**
  - **Writing Communities & Forums:** Websites like NaNoWriMo forums, Reddit (r/writing, r/worldbuilding), or specific genre fiction groups often have discussions or resources related to character creation.
  - **Role-Playing Game (RPG) Resources:** Websites for tabletop RPGs (like D&D Beyond, Pathfinder SRDs) or community-created NPC repositories.
  - **Public Domain Literature Archives:** Project Gutenberg, Archive.org for characters from pre-copyright texts.
  - **Existing Game Asset Packs:** Some commercially available or free game asset packs might include character profiles or lore.
  - **Wikis for Fictional Universes:** Fan-created wikis for books, TV shows, and games (e.g., Fandom wikis) contain vast amounts of character information, though copyright is a major consideration here.

**Further Considerations & Ideas for Ayoai (Character Descriptions):**
  - *Potential Challenge:* Complex Data Formatting: Extracting structured data from varied text is challenging; a consistent schema/ontology is highly beneficial for usability.
  - *Key Consideration:* Copyright and Licensing: Information from existing fiction or fan wikis is often copyrighted. Public domain sources are safer. Community-contributed databases need clear open licenses (e.g., Creative Commons).
  - *Potential Challenge:* Subjectivity and Bias: Character traits can be subjective and reflect biases from original authors or contributors.
  - *Potential Challenge:* Quality and Consistency: Level of detail, writing quality, and consistency will vary across sources.
  - *Potential Challenge:* Maintenance and Updates: Large databases require ongoing curation efforts.
  - *Idea for Ayoai:* Leverage NLP tools (e.g., NER, relation extraction) when processing public domain literature for character information.
  - *Idea for Ayoai:* For community-driven approaches, emphasize clear open licensing from the outset.
  - *Key Consideration:* Ethical AI: Be mindful of data biases and ensure responsible development and use of generated characters/personas.
  - *Idea for Ayoai:* Integrate the database with LLMs for fine-tuning or as a knowledge base for Retrieval Augmented Generation (RAG) systems.

### Exploring Goal Databases
The concept of a "goals database" can be interpreted in several ways beneficial to Ayoai, primarily relating to AI planning and character/narrative development.

**For AI Planning:**
A "goals database" in this context refers to a structured collection of goal specifications, typically within PDDL problems. This is less about a standalone database *of goals* and more about how goals are represented and varied within existing PDDL problem repositories.
  - **Importance to Ayoai:**
      - **Systematic Planner Evaluation:** Testing planners against a wide variety of goal types and complexities within a given domain.
      - **Procedural Problem Generation:** Using a diverse set of goal templates to automatically generate new PDDL problems.
      - **Learning Goal Patterns:** Analyzing common goal structures to inform heuristic development or automated planning strategies.
  - **Findings & Resources:**
      - **PDDL Problem Repositories:** Existing collections of PDDL problems are de facto "goal databases." Each problem file defines a specific goal.
          - **IPC Datasets (e.g., [IPC 2023 Classical Track Dataset](https://github.com/ipc2023-classical/ipc2023-dataset)):** These contain numerous problems per domain, each with distinct goals, offering a rich source for goal variety.
          - **[Planning.Domains](https://planning.domains/):** A comprehensive collection of PDDL domains and associated problems, providing a vast implicit database of goals.
      - **Goal Generation Research:** Some research in automated planning focuses on generating diverse and interesting goals, but these are often techniques rather than static datasets.
  - **Interesting Aspects:**
      - **Goal Representation:** Goals in PDDL are logical statements about desired world states. Their complexity can range from simple atomic propositions to complex quantified expressions.
      - **Relationship between Goals and Domain Complexity:** The difficulty of achieving a goal is heavily tied to the actions available in the domain and the initial state.
      - **Goal-Driven Behavior:** In AI planning, the goal is the primary driver of the planning process.

**For Character Development & Narrative:**
A "goals database" here would be a collection of character motivations, objectives, desires, and ambitions.
  - **Importance to Ayoai:**
      - **Creating Believable NPCs:** Giving game characters meaningful objectives that drive their behavior.
      - **Story Idea Generation:** Providing a palette of
potential motivations for protagonists or antagonists.
      - **Training Generative Models:** Fine-tuning models to create characters with compelling and coherent goals.
  - **Findings & Resources:**
      - **No Single "Goals Database" Found:** Unlike structured PDDL problems, a unified, publicly available database specifically for character *goals* is not immediately apparent. Such data is more diffuse.
      - **Potential Sources for Curation:**
          - **Psychology & Motivation Theories:** Taxonomies of human needs and motivations (e.g., Maslow's hierarchy, Reiss Motivation Profile) could provide a structural basis.
          - **Storytelling Resources:** Books, websites, and courses on narrative construction often list common character goals, desires, and flaws (e.g., "save the world," "find true love," "achieve power").
          - **Literary Analysis:** Extracting and categorizing character goals from public domain literature or plot summaries.
          - **Crowdsourcing/Community Efforts:** Similar to character description databases, a community could collaboratively build a database of motivations.
  - **Interesting Aspects:**
      - **Hierarchy of Goals:** Characters often have short-term and long-term goals, which can conflict or align.
      - **Intrinsic vs. Extrinsic Motivation:** Goals can arise from internal desires or external pressures.
      - **Goal Evolution:** Character goals can change over time in response to events or personal development.

**Conclusion:**
While dedicated "goal databases" as standalone entities are not common, existing PDDL problem repositories serve this function for AI planning. For character development, such a database would likely need to be curated from various sources, potentially leveraging psychological frameworks and narrative theory.

**Further Considerations & Ideas for Ayoai (Goal Databases):**
  - *Idea for Ayoai (AI Planning):* Extract and categorize goals from existing PDDL problems; consider developing a taxonomy for PDDL goals (e.g., based on type like achievement/maintenance, or complexity).
  - *Idea for Ayoai (AI Planning):* Explore tools or techniques for *generating* varied goals for PDDL domains, as an alternative/complement to static goal databases.
  - *Idea for Ayoai (Character/Narrative):* When curating a character goals database, structure it hierarchically (e.g., using Maslow's hierarchy or other psychological models) and define relevant fields (e.g., Goal Name, Description, Common Obstacles, Related Traits, Example Scenarios).
  - *Key Insight (Character/Narrative):* Emphasize the complementary nature of a character goals database with a character description database, as goals often stem from traits and backstories.
  - *Potential Challenge (Character/Narrative):* Address the subjectivity in defining "goal" and the need to handle both abstract (e.g., "find happiness") and concrete (e.g., "win the lottery") goals.
  - *Idea for Ayoai (Character/Narrative):* Detail specific use cases such as driving NPC behavior in simulations, generating plot hooks, and ensuring consistency in procedurally generated characters.