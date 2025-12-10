
  # 📘 Multi-Agent Content Generation System — Project Documentation  
  ### Kasparro Applied AI Engineer Challenge  
  ### Author: Mohit Anand  

  ---

  # 1. Problem Statement  

  Build a **modular agentic automation system** that consumes a small product dataset (GlowBoost Vitamin C Serum) and autonomously generates **structured, machine-readable JSON pages**:

  - Product Description Page  
  - FAQ Page (minimum 5 Q&As)  
  - Comparison Page (GlowBoost vs fictional Product B)  

  The system must:

  - Use **multiple agents**, not a single monolith  
  - Contain **reusable content logic blocks**  
  - Implement your **own template engine**  
  - Output **clean JSON**  
  - Execute via an **orchestrated automation flow**  
  - Use **ONLY the provided dataset** — no external knowledge, no web research  

  This challenge evaluates system design, agent orchestration, abstraction, and engineering maturity — **not writing skills**.  

  ---

  # 2. Solution Overview  

  The solution is implemented as a **4-agent pipeline** supported by modular content blocks and templates.  
  The core idea:

  - Convert raw product → internal models  
  - Generate categorized questions  
  - Transform questions → FAQ items using logic blocks  
  - Assemble structured JSON pages using templates  
  - Orchestrate end-to-end execution using a controller  

  The system demonstrates:  
  ✔ clear agent boundaries  
  ✔ reusable logic functions  
  ✔ template-defined page composition  
  ✔ full JSON output for all pages  
  ✔ deterministic, non-LLM, rule-based generation  
  ✔ extensibility for future AI/LLM integration  

  ---

  # 3. Scopes & Assumptions  

  **In Scope**  
  - Using only the fixed GlowBoost dataset provided in the assignment  
  - Multi-agent deterministic processing  
  - Autonomous question generation  
  - FAQ creation using logic blocks  
  - Template-driven page assembly  
  - JSON output in `/outputs`  

  **Out of Scope**  
  - No external API calls  
  - No UI or website  
  - No GPT or external LLM calls  
  - No new facts beyond the dataset  
  - No real Product B — must be fictional but structured  

  **Assumptions**  
  - Input dataset will always follow the predefined shape  
  - The system must run entirely offline  
  - Future extensions (LLMs, APIs, UI) should be easily integrable  

  ---

  # 4. System Design (Most Important Section)    

  The system is engineered as a **four-stage agent pipeline**, each stage producing structured intermediate artifacts stored in a shared `PageContext` object.

  ## 4.1 Architecture Diagram  

  ```mermaid
  flowchart TD

      A[Raw Product Data] --> B[ParserAgent<br/>Create Models + PageContext]
      B --> C[QuestionGeneratorAgent<br/>Generate 15+ Categorized Questions]
      C --> D[ContentPlannerAgent<br/>Map Questions → FAQ Items using Logic Blocks]
      D --> E[PageAssemblerAgent<br/>Templates Build Product & Comparison Pages]
      E --> F[FAQ Template<br/>Build FAQ Page]

      F --> G1[faq.json]
      E --> G2[product_page.json]
      E --> G3[comparison_page.json]

  ## 4.2 ParserAgent  

  **Purpose:**  
  Convert raw product data into structured internal models and initialize the shared PageContext.

  **Responsibilities:**  
  - Parse raw dictionary into a `Product` object.  
  - Create a `FictionalProduct` for comparison.  
  - Initialize `PageContext` containing:  
    - base product  
    - fictional product  
    - empty lists for generated questions  
    - empty list for FAQ items  
    - placeholders for product & comparison pages  

  **Output:**  
  A fully structured PageContext object passed to the next agent.

  ---

  ## 4.3 QuestionGeneratorAgent  

  **Purpose:**  
  Generate realistic customer-style questions derived from the product attributes.

  **Responsibilities:**  
  - Analyze product characteristics.  
  - Generate ≥15 categorized questions across:  
    - Informational  
    - Usage  
    - Safety  
    - Purchase  
    - Comparison  
  - Populate `context.generated_questions`.  

  **Output:**  
  A comprehensive question set for FAQ creation.

  ---

  ## 4.4 ContentPlannerAgent  

  **Purpose:**  
  Convert generated questions into FAQ items using reusable logic blocks.

  **Responsibilities:**  
  - Map each question to the appropriate logic block.  
  - Produce a structured FAQItem containing:  
    - question  
    - answer  
    - category  
  - Append formatted FAQ entries into `context.faqs`.  

  **Output:**  
  A complete list of FAQ items ready for page assembly.

  ---

  ## 4.5 PageAssemblerAgent  

  **Purpose:**  
  Build the final JSON-ready product, comparison, and FAQ pages using templates.

  **Responsibilities:**  
  - Apply templates:  
    - `product_template`  
    - `comparison_template`  
    - `faq_template`  
  - Combine logic block outputs + PageContext data.  
  - Fill:  
    - `context.product_page_data`  
    - `context.comparison_page_data`  
    - `context.faq_page_data`  

  **Output:**  
  Final assembled dictionaries for all three pages.

  ---

  # 5. JSON Output Specification  

  The system generates **three structured JSON outputs**, each satisfying the assignment’s requirements.

  ## 5.1 product_page.json  
  Contains:  
  - product name  
  - concentration  
  - skin suitability  
  - ingredient list  
  - benefits summary  
  - usage instructions  
  - safety notes  
  - price  

  ## 5.2 faq.json  
  Contains:  
  - at least 5 FAQ items  
  - each containing:  
    - question  
    - answer  
    - category  

  ## 5.3 comparison_page.json  
  Contains:  
  - summary of GlowBoost  
  - summary of fictional Product B  
  - structured comparison of:  
    - ingredients  
    - benefits  
    - usage  
    - price  

  ---

  # 6. Assignment Compliance Summary  

  The system fully meets all engineering and design expectations outlined in the assignment.

  **✓ Multi-agent modular architecture**  
  Each agent has a clearly defined responsibility and isolated processing logic.

  **✓ Reusable content logic blocks**  
  Benefits, safety, usage, and comparison blocks ensure consistent text generation.

  **✓ Template-based JSON page generation**  
  All outputs follow predefined templates ensuring predictable structure.

  **✓ Complete automation pipeline (orchestrator)**  
  A single orchestrator executes all steps in sequence.

  **✓ Outputs three clean JSON pages**  
  Required: product, FAQ, and comparison pages.

  **✓ No external data sources used**  
  Only the provided GlowBoost dataset is referenced, as required.

  **✓ Documentation includes diagrams, explanation, and architecture**  
  Satisfies the assignment’s documentation standard.

  **✓ Extendable and production-ready structure**  
  Future LLM/AI integrations can be plugged in without redesign.

