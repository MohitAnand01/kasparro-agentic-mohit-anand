# 📘 Multi-Agent Content Generation System  
### Kasparro – Applied AI Engineer Challenge  
### Author: Mohit Anand  

---

# 1. Problem Statement    

Design and implement a **modular agentic automation system** that transforms a small product dataset into three structured, machine-readable content pages:

- FAQ Page (minimum 5 Q&As)  
- Product Description Page  
- Comparison Page (GlowBoost vs fictional Product B)  

The system must satisfy these requirements:

- Use **multiple independent agents**, not a monolith  
- Demonstrate an **automation flow / orchestration graph**  
- Use **reusable content logic blocks**  
- Use your **own template engine**  
- Output **clean JSON files**  
- Use **only the provided dataset** (no external facts or research)  

This challenge evaluates **system design**, **architecture**, **automation**, and **agent orchestration**, not content writing.

---

# 2. Solution Overview  

The solution is built as a **deterministic four-agent system** guided by a central orchestrator. Rather than writing content directly, each agent performs a single responsibility, transforming structured data through a controlled pipeline.

### The four agents are:

1. **ParserAgent**  
   Converts raw input into strongly typed internal models and initializes PageContext.

2. **QuestionGeneratorAgent**  
   Generates ≥15 categorized user questions based solely on input data.

3. **ContentPlannerAgent**  
   Converts questions into structured FAQ items using reusable logic blocks.

4. **PageAssemblerAgent**  
   Uses templates + logic blocks to generate final JSON pages.

Supporting components:

- **Logic Blocks** → benefit extraction, safety synthesis, usage rules, product comparison  
- **Template Engine** → user-defined JSON shaping for all pages  
- **Orchestrator** → executes the multi-agent workflow end-to-end  

This design is modular, testable, and easily extendable (LLMs, APIs, UI).

---

# 3. Scopes & Assumptions  

### ✅ In Scope

- Parsing only the provided GlowBoost dataset  
- Deterministic question generation  
- FAQ creation  
- Template-driven page construction  
- JSON-only outputs  
- Multi-agent workflow coordination  

### ❌ Out of Scope

- External data sources, APIs, or internet lookup  
- LLM or GPT content generation  
- UI / website / frontend  
- Creative rewriting or fact invention  

### Assumptions

- Dataset will always follow the provided schema  
- System must run fully offline  
- Future upgrades should require minimal architectural changes  

---

# 4. System Design   

This system follows a **four-stage agentic pipeline**, with each stage producing structured intermediate artifacts stored in a shared **PageContext** object.  
The architecture ensures strict single-responsibility and well-defined input/output boundaries.

---

## 4.1 Architecture Diagram  

```mermaid
flowchart TD

    A[Raw Product Data] --> B[ParserAgent<br/>Create Models + PageContext]
    B --> C[QuestionGeneratorAgent<br/>Generate 15+ Categorized Questions]
    C --> D[ContentPlannerAgent<br/>Map Questions → FAQ Items Using Logic Blocks]
    D --> E[PageAssemblerAgent<br/>Build Product + Comparison Pages via Templates]
    E --> F[FAQ Template<br/>Generate FAQ Page]

    F --> G1[faq.json]
    E --> G2[product_page.json]
    E --> G3[comparison_page.json]

