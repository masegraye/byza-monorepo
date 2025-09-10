# Project Bootstrapping

## 1. Rough Thoughts

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

- **Additional Key Additions from LXD Analysis:**
  - **Learner-centric policies**: System needs to account for emotional states, learning styles, and accessibility from the ground up
  - **Outcome-based validation**: Policies should be validated against actual behavioral change/skill acquisition, not just content quality
  - **Assessment integration**: System needs both formative (feedback) and summative (evaluation) assessment policies built-in
  - **Social learning dimension**: Community/peer interaction policies needed beyond individual content consumption
  - **Multi-modal reinforcement**: LXD document reinforces our voice+visual approach
  - **Broader applicability**: Policy-based system for organizational knowledge extends well into educational contexts beyond just documentation

- **LXD Departures - Our Differentiation:**
  - **Professional-focused**: Primarily for professionals learning professional skills, not general education
  - **Limited social learning**: Only care about social aspects where professional performance is assessed by peers/managers/stakeholders
  - **"Non-woke" approach**: Acknowledge "emotional states" and "learning styles" exist but take "tough love" approach
  - **Personal accountability emphasis**: Foster self-responsibility rather than accommodation
  - **Customer self-selection strategy**: Attract "right kind" of customer who will self-drive to completion
  - **Success rate optimization**: By filtering for motivated learners, increase overall completion and success rates
  - **Results-oriented positioning**: Appeal to professionals who want direct, no-nonsense skill acquisition

- **Generation Strategy Considerations:**
  - **Timing of policy application**: Apply after every sentence? Paragraph? Page? Or at predefined checkpoints?
  - **Performance vs accuracy tradeoffs**: More frequent policy checks = better coherence but slower generation
  - **Streaming vs batch generation**: Does policy enforcement work with streaming LLM outputs or require complete chunks?
  - **Incremental vs holistic validation**: Can we validate incrementally as content grows, or need full context each time?
  - **Policy scope considerations**: Some policies only make sense at certain content levels (word-level vs document-level)
  - **Backtracking capabilities**: If policy violation detected mid-generation, do we regenerate from violation point or start over?
  - **Caching and optimization**: Can we cache policy validation results for unchanged content sections?
  - **Real-time vs asynchronous**: Should policy enforcement block content generation or run in background with alerts?
  - **Progressive refinement**: Generate rough draft first, then apply policies in multiple passes vs enforce during initial generation?
  - **Continuous evaluation triggers**: Policy checks at generation time vs after every meaningful change
  - **Change event types**: User edits, external document updates, policy modifications, principle updates
  - **Ripple effect handling**: When upstream document changes, how do we identify and re-evaluate all downstream dependents?
  - **Event-driven architecture**: System responds to change events rather than polling for changes
  - **Debouncing strategies**: Avoid thrashing when multiple rapid changes occur (e.g. typing)
  - **Dependency graph maintenance**: Track which content pieces depend on others for efficient re-evaluation
  - **Incremental vs full re-evaluation**: When change occurs, re-check everything or just affected portions?
  - **Background processing**: Long-running policy evaluations shouldn't block user interaction
  - **Change notification system**: How does system inform users when upstream changes affect their content?

- **Dependency Graph & External Artifact Management:**
  - **Cross-system dependency tracking**: Graph must understand which internal content feeds external artifacts (blogs, shared docs, printed materials)
  - **External feedback integration**: When corrections/changes come back from external systems, need reconciliation process within graph
  - **Bi-directional sync challenges**: Internal changes → external artifacts is manageable, but external changes → internal requires human judgment
  - **Out-of-date detection**: System must identify when downstream docs are affected by upstream changes but cannot auto-update them
  - **Human integration requirement**: People must understand and integrate changes into their own understanding - cannot be fully automated
  - **Notification vs auto-update distinction**: System should alert about dependencies but not automatically propagate changes
  - **Change impact analysis**: When upstream change occurs, system calculates which downstream artifacts are potentially affected
  - **Version reconciliation**: How to handle when external artifact diverges from internal source - which version is authoritative?
  - **Export/import boundaries**: Clear delineation between what system manages directly vs what it tracks as external dependencies
  - **Feedback loop integration**: External feedback becomes input to policy refinement and organizational knowledge updates

- **Policy Contradiction Detection - Complex Challenge:**
  - **Syntactic vs semantic contradictions**: Easy to detect "paragraphs must be <100 words" vs "paragraphs must be >150 words", harder to detect semantic conflicts
  - **Contextual contradictions**: Policies might only conflict under specific content scenarios or conditions
  - **Cross-level contradictions**: Sentence-level policy might conflict with paragraph-level policy when applied to same content
  - **Temporal contradictions**: Policies that conflict when applied in sequence but not in isolation
  - **Implicit contradictions**: Policies that don't explicitly contradict but make it impossible to satisfy both simultaneously
  - **Detection mechanisms**:
    - **Formal logical analysis**: Express policies as logical statements, use theorem proving to detect conflicts
    - **Semantic embedding analysis**: Vector representations of policies, detect when embeddings point in opposite directions
    - **Test case generation**: Generate sample content, see if applying policies produces contradictory feedback
    - **LLM-based reasoning**: Ask LLM to analyze policy sets for potential conflicts
    - **Runtime conflict detection**: Only discover contradictions when they occur during actual content evaluation
  - **Contradiction severity levels**: Some conflicts are absolute blockers, others are tensions that need human judgment
  - **Policy dependency mapping**: Understanding which policies interact with each other to focus contradiction detection

- **Constitutional AI - 3-Body Government Model:**
  - **Executive Branch (Content Generation)**: LLM creates content with broad discretion within constitutional constraints, like executive branch implements policy
  - **Legislative Branch (Policy Creation)**: System for creating, modifying, prioritizing policies - human-driven initially, potentially AI-assisted later
  - **Judicial Branch (Policy Interpretation)**: System that interprets policy conflicts, sets precedent, determines constitutional validity of policies against first principles
  - **Checks and balances mechanisms**: Each branch constrains the others, preventing any single component from dominating
  - **Precedent system**: Judicial decisions create binding interpretations for future similar conflicts
  - **Constitutional hierarchy**: First Principles > Policies > Implementation Rules (like Constitution > Laws > Regulations)
  - **Amendment process**: Structured methodology for changing fundamental principles when evidence warrants
  - **Judicial review capability**: Can invalidate policies that violate higher-order principles
  - **Separation of concerns**: Content generation, rule-making, and conflict resolution are distinct systems with different objectives

### 1.1 Open Questions

- **Technical Architecture:**
  - **Contradiction detection mechanisms**: How do we detect when policies contradict each other? Formal logical analysis, semantic embedding similarity, test case conflicts, human flagging?
  - **Conflict resolution mechanisms**: What happens when policies contradict each other? How do we prioritize or resolve conflicts?
  - **Scalability considerations**: How does the system perform with thousands of documents and complex dependency webs?
  - **Data persistence and backup**: How do we handle system failures, data corruption, version history?
  - **Security model**: Who can modify policies vs content? How do we handle sensitive organizational knowledge?

- **Business Model:**
  - **Pricing strategy**: Per-user, per-organization, per-policy-check, freemium model?
  - **Implementation methodology**: How do organizations onboard? Do we provide consulting services?
  - **Integration ecosystem**: APIs for connecting to existing tools (Slack, Notion, Google Docs, etc.)?

- **User Experience:**
  - **Learning curve**: How do non-technical users create effective policies?
  - **Policy debugging**: When content fails policy checks, how do users understand why and fix it?
  - **Collaboration workflows**: How do teams work together within the policy system?
  - **Mobile experience**: Does voice-first work on mobile? What's the mobile editing experience?

- **Competitive Landscape:**
  - **Existing solutions analysis**: What are companies currently using for this problem?
  - **Differentiation strategy**: Beyond policy enforcement, what makes us defensible?

## 2. Analysis

### 2.1 Elevator Pitch

We're building a policy-driven content creation system for professionals who need to transform their specialized knowledge into effective training materials for other professionals. Our platform combines instructional design expertise with AI to help subject matter experts create interactive, voice-first learning products that actually change behavior and build skills—not just transfer information. The system enforces proven learning principles through multi-level content policies, ensuring every piece of content serves the end goal of professional competency development. We're targeting self-motivated professionals who want results-oriented, no-nonsense training creation tools that filter for serious learners and drive high completion rates.

#### 2.1.1 Abstract Policy-Driven AI System

At its core, we're creating a policy enforcement engine that solves the fundamental problem of LLM content generation: models are excellent at creating content but terrible at ensuring it conforms to organizational policies and knowledge standards. Our system transforms high-level objectives into hierarchical policy sets, continuously validates content against these policies across multiple conceptual levels (from sentences to documents), and iteratively refines outputs until zero policy violations remain. The target customer is any software company building LLM-powered information systems who needs to ensure generated content aligns with their organizational knowledge, brand voice, compliance requirements, or domain-specific standards. This represents a much larger market than just learning content—spanning documentation systems, customer communications, technical writing, and any context where AI-generated content must meet specific quality and consistency criteria.

### 2.2 Meta-Analysis: Rules of Engagement for System Design

Given the complexity and scope of ideas in our rough thoughts, we need clear criteria for evaluating and prioritizing our approach. Our system spans multiple domains (policy enforcement, content generation, voice interfaces, organizational knowledge management) and serves different customer segments (learning professionals, software companies). To avoid building an unfocused system that does everything poorly, we establish these rules of engagement:

**Decision Criteria (in priority order):**
1. **Local-first viability**: Can this component work effectively without cloud dependencies? Does it support our "local-first, meshed second" architecture?
2. **Policy enforcement core**: Does this directly serve our core differentiator of multi-level policy compliance? Secondary features that don't strengthen policy enforcement get deprioritized.
3. **Simplest viable implementation**: When multiple approaches exist, choose the one that delivers core value with minimal complexity. We can iterate toward sophistication later.
4. **Customer self-selection alignment**: Does this feature attract our target "tough love" professional customers while filtering out users who won't drive to completion?
5. **Market validation potential**: Can we test this component's value proposition quickly and cheaply before committing to full development?

**Technical Architecture Rules:**
- Start with the abstract policy engine as the foundational system
- Build learning-focused features as a specialized application layer
- Voice-first interface comes after core policy system is proven
- System model must support cross-level content pulling from day one
- No feature gets built without a clear policy enforcement use case

**Go-to-Market Strategy:**
- Target the broader "policy-driven AI for software companies" market first (larger addressable market, faster validation)
- Use learning content creation as the initial proof-of-concept application
- Plan for the learning platform to become a showcase of the policy system's capabilities

## 3. Thesis

**The most valuable organizations don't just accumulate knowledge—they transform knowledge into coherent action.** Amazon does this through leadership principles and six-pagers, McKinsey through structured methodology, the military through doctrine and after-action reviews. These elite organizations invest enormous human capital to ensure their knowledge creates coherent action rather than information chaos. But this process doesn't scale: it requires intensive human intervention, breaks down across organizational boundaries, and fails when knowledge evolves faster than humans can process it.

**We're building a system that automates organizational coherence at the speed of knowledge creation.**

Our core insight: **The problem isn't that AI generates bad content—it's that AI generates content without organizational coherence.** LLMs can create excellent individual pieces but cannot ensure those pieces align with organizational principles, support consistent decision-making, or build toward coherent outcomes. This creates a massive opportunity: a policy-driven system that maintains organizational coherence automatically as new knowledge enters the system.

**Our thesis has four pillars:**

1. **Coherence-first architecture**: Rather than generating content and hoping for alignment, we enforce organizational coherence through hierarchical policies that ensure every piece of content serves the organization's strategic objectives.

2. **Continuous knowledge integration**: As new information becomes known to the system, it automatically validates against existing knowledge and updates policies to maintain coherence, replacing periodic human-intensive alignment processes.

3. **Local-first organizational privacy**: Organizations need to develop coherence privately before exposing their knowledge systems externally. Our local-first approach enables this natural progression from internal coherence to external collaboration.

4. **Outcome-focused customer alignment**: By targeting organizations and individuals who prioritize results over comfort, we create a feedback loop where customer success drives system improvement, building a moat through effectiveness rather than engagement.

**The learning platform serves as our proof-of-concept**: if we can help professionals create coherent learning experiences that actually change behavior, we demonstrate the system's ability to maintain coherence at the most challenging level—human behavioral change. Success here proves the system can handle any organizational coherence challenge.

The real business is selling organizational coherence as a service: giving any organization the ability to maintain Amazon-level knowledge-to-action alignment without Amazon-level process overhead.

### 2.3 Constitutional AI Architecture Analysis

The 3-body government model provides a compelling framework for operationalizing constitutional AI that solves many of our policy contradiction and organizational coherence challenges:

**System Architecture Parallels:**
- **Executive (Content Generation)**: LLMs generate content with broad creative discretion, but must operate within established constitutional constraints. Like a president implementing policy, they have interpretation flexibility but cannot violate fundamental principles.

- **Legislative (Policy Management)**: Human experts and potentially AI assistants create, modify, and prioritize policies. This branch responds to organizational needs and evidence about policy effectiveness, similar to how Congress adapts laws to changing circumstances.

- **Judicial (Conflict Resolution)**: Specialized AI system that interprets policy conflicts, establishes precedent, and validates new policies against first principles. Acts as constitutional court for the knowledge system.

**Key Benefits for Organizational Coherence:**
1. **Systematic conflict resolution**: Rather than ad-hoc policy debugging, conflicts get resolved through established judicial processes that create binding precedent
2. **Constitutional hierarchy**: Clear chain of authority from first principles down to implementation details
3. **Evolutionary stability**: System can adapt policies while maintaining constitutional coherence
4. **Separation of concerns**: Content creation, rule-making, and interpretation are distinct systems with different optimization targets

**Implementation Implications:**
This model suggests our system needs three distinct AI components with different training objectives, not just one policy-enforcement engine. The judicial component especially needs training on legal reasoning, precedent analysis, and principled decision-making rather than content generation.

## 4. Extrapolation

## 5. Draft Outline

## 6. Final Draft