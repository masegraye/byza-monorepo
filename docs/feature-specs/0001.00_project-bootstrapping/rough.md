# Project Bootstrapping

## Rough Thoughts

- New repo/project/product combining instructional design expertise with customer knowledge
- Brother is a teacher specializing in instructional design - his knowledge is the core asset
- Goal: Pack instructional design knowledge into a system
- System combines with customer's specialized knowledge to produce information products
- Flow: System + Specialized Knowledge + LLM -> Info Product
- Alternative flow: Info Product + System + LLM -> Interactive Learning Product (ILP)
- End outcomes: ILP + End User -> Educated User OR Info Product + End User -> Educated User

- Need to develop multiple software systems for orchestration
- Key insight: LLMs are bad at producing content that conforms to policy
- Major system needed: Policy enforcement engine
  - High-level objectives -> set of policies
  - Set of policies + input -> policy violations + suggested fixes
  - Set of policies + input + suggested fixes -> policy violations + suggested fixes (iterative until zero violations)

- Policy enforcement needs to work at multiple conceptual content levels:
  - Words within sentences
  - Sentences within paragraphs  
  - Paragraphs within segments
  - Segments within chapters
  - Chapters within sections
  - Sections within works

- System must be able to "uplevel" content units to analyze at appropriate policy level
- Example: Chapter-level policy "each chapter should focus primarily on one topic, with no more than 2 related sub-topics" requires whole-chapter analysis capability
- Challenge: LLMs are bad at counting/analyzing large chunks of text - may need LLM-augmented "map reduce" approach
- Performance consideration: On-demand analysis may not be effective
- Proposed solution: "Continuous upleveling" system
  - Higher-level policies generate lower-level policies 
  - Lower-level policies continuously evaluated as content changes
  - Changes trigger higher-level policy checks (push system vs pull system)
  - Potentially hybrid push/pull: pulling triggers bottom-up pushing

- **Claude's Analysis of System Design:**
  - Makes sense: Hierarchical policy enforcement mirrors how instructional design actually works (learning objectives cascade down to activities)
  - Prior art: Similar to linting systems (ESLint) but for content semantics rather than syntax
  - Academic precedent: Bloom's Taxonomy operates at multiple cognitive levels, educational scaffolding theory
  - Technical parallels: Document analysis pipelines (e.g., academic paper structure validation), content management systems with approval workflows
  - Alternative approaches to consider:
    - Rule-based systems (more deterministic but less flexible)
    - Template-driven generation with constraints (like Madlibs but sophisticated)
    - Human-in-the-loop validation at key checkpoints rather than continuous
    - Compositional approaches: small validated building blocks that combine safely
  - Potential challenges: Policy conflict resolution, performance at scale, maintaining coherence across levels
  - Strength: Addresses real LLM weakness (policy adherence) while leveraging strength (content generation)

- **RL/Fine-tuning Parallels and Insights:**
  - System resembles Constitutional AI / RLHF where policy violations = negative rewards
  - Reward shaping opportunity: Policy compliance scores could guide generation rather than just validate
  - Synthetic data generation: Policy-compliant examples become training data for domain-specific fine-tuning
  - Active learning: System identifies edge cases where policies are unclear/conflicting for human refinement
  - Multi-objective optimization: Balance policy compliance vs content quality vs engagement metrics
  - Curriculum learning: Start with simple policies, gradually add complexity as system improves
  - Few-shot learning: Use policy-compliant examples as in-context learning demonstrations
  - Techniques to borrow:
    - Proximal Policy Optimization for gradual policy updates
    - Experience replay: Store successful policy-compliant generations for retraining
    - Exploration vs exploitation: Sometimes generate "risky" content to discover policy boundaries
  - Data flywheel: More usage -> more policy violations -> better policy refinement -> better content generation

- **Voice-First, Multi-Modal Interface Design:**
  - Heavy emphasis on voice for input and output
  - Input: Voice primary, text as backup/correction mechanism for transcription errors
  - Output: Voice + visuals (graphs, graphics) - NOT primarily text-based
  - Goal: Non-text-based LLM experience that generates multimedia content
  - Text still present but secondary: subtitles, under-the-covers processing, expandable detail sections
  - Primary consumption: Interactive with summary text instead of long paragraphs
  - Longer content hidden/collapsible - users can drill down if needed
  - Voice input advantages: High-bandwidth communication
  - Key insight: LLM can clean up "cluttered voice input" that normally makes voice interfaces ineffective
  - Apply same content policies to transcribed voice as any other LLM input
  - Interactive learning products naturally suited for this multimedia approach
  - Multi-modal interface applies at multiple levels:
    - Level 1: System ↔ Customer (info product author) - voice-first authoring experience
    - Level 2: Info Product ↔ End-user/End-customer - voice-first learning experience

- **Need to define naming conventions:**
  - Actors: System, Customer/Info Product Author, End-user/End-customer, (Brother's instructional design knowledge?)
  - Time phases: Authorship time, Learning time, (Policy creation time? System development time?)
  - Products: Info Product, Interactive Learning Product (ILP), Specialized Knowledge, Policies
  - Processes: Policy enforcement, Content generation, Upleveling, Continuous evaluation

- **Experience & Architecture Considerations:**
  - Platform: Desktop app (Electron Forge) or web app - need to decide
  - User model questions: Two separate apps or one? Support "remixing"? "Everyone is learner and publisher" model?
  - Principle: Start with simplest thing that works well
  - Architecture priority: "Local-first, meshed second"
  - Ulterior motive: Make policy-based system available in other contexts beyond education
  - Use case: Organizational documentation that uses other docs as "organizational policy/knowledge"
  - Example: Write roadmap that uplevels from three detailed roadmaps, all using same style/policies
  - Requirements for broader use: Users must work with local content initially
  - Evolution path: Local content → comfortable with system → connect to shared knowledge-base
  - End goal: Automatic linkage and policy application across shared knowledge-base

- **Content Policy vs Organizational Knowledge - Claude's Thoughts:**
  - Blurry boundary - potentially one is just a "lively" form of the other
  - Content policy: Rules about HOW to write/structure content
  - Organizational knowledge: WHAT the organization knows/believes/has learned
  - But organizational knowledge often implies content policies:
    - "We learned X approach works better" → policy to prefer X approach
    - "Our customers respond well to Y format" → policy to use Y format
  - Maybe organizational knowledge is the "why" behind content policies?
  - Living documents: Policies could evolve as organizational knowledge grows
  - Feedback loop: Content created under policies → generates new organizational knowledge → updates policies
  - Knowledge graphs: Policies and knowledge could be nodes in same graph, just different types
  - Both are constraints on content generation - one structural, one semantic
  - Consider: Are we building a policy engine or a knowledge management system? Maybe both?

- **First Principles → Policies Hierarchy:**
  - Need foundational first principles with policies derived from/rolling up to them
  - Key question: How do we determine what qualifies as a "first principle" or "tenet"?
  - Definition: Principle = something held as true until evidence suggests otherwise
  - Tenets = colloquially weaker form but technically same definition as principles
  - First principles = potentially stronger form (physics context - empirically testable)
  - Hierarchy structure: First Principles → Tenets → Policies → Implementation Rules
  - Challenge: In instructional design/content creation, what's "empirically testable"?
  - Possible criteria for first principles: Learning science research, cognitive psychology, proven pedagogical methods
  - Evidence-based updating: System should track when policies conflict with principles, suggest principle revision

- **Principle vs Policy Modification Dynamics:**
  - Principles = slow-changing, Policies = faster-changing
  - When many policies conflict with principles, possible causes:
    - Bad actors proposing poor policies
    - Bad content driving demand for inappropriate policies  
    - Working policies revealing principle is actually wrong
  - Default: Prefer modifying policies over principles
  - Principle change mechanism needed: Accumulated feedback indicating policy-compliant content fails larger fitness function
  - Ultimate "proof" test: Does the content actually achieve desired outcomes?
  - Utility-based validation: If policy-compliant content consistently underperforms on effectiveness metrics, principles may be flawed
  - Feedback loop: Content outcomes → Policy effectiveness → Principle validity
  - System should track: Policy compliance vs actual learning outcomes/user success

- **System Model Brainstorm - Claude's Thoughts:**
  - **Hierarchical Tree Model**: Content as root node, components (paragraphs, sections) as children, policies applied at each level
  - **DAG (Directed Acyclic Graph)**: Content units can be shared across multiple parent documents, enables reuse and consistency
  - **Actor Model**: Each content unit is an actor that responds to policy messages, enables distributed processing
  - **Event Sourcing**: All content changes as events, policies as event handlers, enables replay and audit trails
  - **Compositional Model**: Small validated content blocks combine into larger structures, like functional composition
  - **Multi-layer Network**: 
    - Layer 1: Raw content nodes
    - Layer 2: Policy nodes that evaluate content
    - Layer 3: Principle nodes that validate policies
    - Layer 4: Outcome/feedback nodes
  - **Knowledge Graph**: Entities (content, policies, principles) with relationships, enables semantic queries
  - **Pipeline Model**: Content flows through policy stages, each stage can modify or reject
  - **Reactive System**: Changes propagate up/down the hierarchy automatically, like spreadsheet cell dependencies
  - Question: Should a paragraph participate in multiple documents? Should policies be inherited or composed?

- **System Model Evaluation Framework - Claude's Brainstorm:**
  - **Evaluation Criteria:**
    - Flexibility: Can any node pull from any other node regardless of conceptual level?
    - Consistency: Do structural connections align with conceptual hierarchy intentions?
    - Transformation capability: Can nodes apply policy-driven transformations to their inputs?
    - Cycle detection: How does system handle circular dependencies?
    - Performance: How does cross-level pulling scale with content size?
  - **Key insight from your description:** 
    - Conceptual level = node attribute (intent) + structural position (connections)
    - Example: Paragraph node can pull from sentences AND other paragraphs
    - Content transformation: Raw inputs → policy-driven function → final node text
  - **Graph-based model implications:**
    - Nodes have type/level metadata (word, sentence, paragraph, etc.)
    - Edges represent "pulls from" relationships
    - Each node has transformation function based on its policies
    - System validates that connections make semantic sense
  - **Testing framework:** Create sample content graphs, verify policy enforcement works across all connection types

- **LXD Document Insights - Additional Requirements:**
  - **Human-Centered Design Integration**: Our system needs empathy mapping and user personas capabilities - policies should account for learner needs/motivations/challenges
  - **Real-World Application Alignment**: Policies must ensure content leads to behavioral changes/skill acquisition, not just knowledge transfer
  - **Accessibility as Core Requirement**: WCAG compliance should be built-in policy, not optional - affects content generation at all levels
  - **Emotional Consideration**: System needs to track and optimize for learner emotions (anxiety, confidence, engagement) - new dimension beyond content structure
  - **Iterative/Evidence-Based Design**: Our policy system should incorporate feedback loops from actual learning outcomes, not just content quality
  - **Multi-Modal Learning Styles**: Policies need to account for visual/auditory/kinesthetic preferences - impacts our voice+visual approach
  - **Microlearning Support**: System should support short, focused content bursts - affects content structuring policies
  - **Assessment Integration**: Formative vs summative assessment policies needed - both content structure AND pedagogical approach
  - **Transferable Skills Focus**: Policies should emphasize connecting new knowledge to existing learner expertise
  - **Community/Social Learning**: Need policies for peer interaction, forums, collaborative elements

## Analysis

## Thesis

## Extrapolation

## Draft Outline

## Final Draft