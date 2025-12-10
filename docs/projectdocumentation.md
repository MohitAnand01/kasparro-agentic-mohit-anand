
  # 📘 Multi-Agent Content Generation System — Technical Documentation
  ### Kasparro AI Engineer Challenge  
  ### Author: Mohit Anand  

  ---

  # 1. Overview

  This document provides the complete technical specification and explanation of the **Multi-Agent Content Generation System** developed for the Kasparro AI Engineer Assessment.

  The goal of the system is to take a small raw product dataset for **GlowBoost Vitamin C Serum** and automatically generate:

  - `product_page.json`
  - `faq.json`
  - `comparison_page.json`

  This is achieved through:

  - Multi-agent collaboration  
  - Reusable logic blocks  
  - Template-based rendering  
  - A central PageContext data model  
  - A well-defined Orchestration pipeline  

  The system is modular, easily extendable, and satisfies all assignment requirements.

  ---

  # 2. High-Level System Architecture

  ```mermaid
  flowchart TD

  A[Raw Product Input] --> B[Parser Agent]
  B -->|Creates PageContext| C[Question Generator Agent]
  C -->|Generates Customer Questions| D[Content Planner Agent]
  D -->|Creates FAQ Items| E[Page Assembler Agent]
  E -->|Uses Logic Blocks + Templates| F[JSON Writer]

  F --> G1[product_page.json]
  F --> G2[faq.json]
  F --> G3[comparison_page.json]
