# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Page Agent Web UI Assistant** (`page-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Page Agent Web UI Assistant (`page-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Web UI Agents & In-Page Automation  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The Page Agent Web UI Assistant is an AI-powered UI automation and assistance engine designed for web applications. It operates either as an embedded JavaScript library within a web application or as a Chrome browser extension, enabling users to interact with complex web interfaces through conversational natural language. Its operational purpose is to eliminate web navigation friction, automate multi-step web forms, assist users with accessibility hurdles, and expose browser DOM manipulation capabilities to external AI models via the Model Context Protocol (MCP).

### 1. Decision Architecture

The user prompt intake, semantic DOM element indexing, interaction sequence formulation, and visual feedback pipeline operates across a deterministic, five-stage architecture:

```
User Voice / Text Request (In-Page Instruction / Form Fill Request / Navigation Goal)
    │
    ▼
[Stage 1: Ingestion & DOM Accessibility Tree Extraction]
    │  - Captures in-page interactive elements from active browser document context
    │  - Normalizes semantic accessibility roles (button, link, input, combobox)
    │  - Filters hidden, occluded, or non-interactive background elements
    ▼
[Stage 2: Semantic Element Localization & Handle Assignment]
    │  - Maps accessibility nodes to compact numeric handles (`@1`, `@2`, `@3`)
    │  - Computes spatial bounding boxes and viewport scroll positions
    │  - Renders lightweight visual indicator highlights on target web elements
    ▼
[Stage 3: Interaction Sequence Planning & Boundary Validation]
    │  - Formulates step-by-step action sequence (click, type, scroll, select)
    │  - Validates element interactability and checks for modal overlay obstructions
    │  - Intercepts sensitive form actions requiring explicit human confirmation
    ▼
[Stage 4: Synthetic DOM Event Dispatch]
    │  - Dispatches native trusted DOM events (MouseEvent, KeyboardEvent, InputEvent)
    │  - Waits for asynchronous network responses and DOM mutation updates
    │  - Verifies expected state transitions (dropdown expansion, page transition)
    ▼
[Stage 5: Visual Feedback & Trajectory Logging]
    │  - Renders non-intrusive in-page conversational tooltips explaining completed actions
    │  - Scrubs private user passwords, credit card inputs, and session cookies
    │  - Commits structured interaction traces to local browser storage for auditing
    ▼
Validated In-Page Action Execution & Auditable Web Assistant Trajectory Record
```

### 2. Decision Logic & DOM Localization Formulations

Page Agent evaluates element target matching, visual saliency, and interaction safety using deterministic mathematical models:

1. **In-Page Element Relevance Score ($S_{\text{element}}$)**:
   $$S_{\text{element}}(e) = (w_r \cdot R_{\text{role}}) + (w_t \cdot T_{\text{text}}) + (w_v \cdot V_{\text{viewport}})$$
   where:
   - $R_{\text{role}} \in \{0, 1\}$ represents exact accessibility role matching.
   - $T_{\text{text}} \in [0, 1]$ represents semantic string similarity against element labels or placeholder text.
   - $V_{\text{viewport}} \in [0, 1]$ measures element visibility inside the current visible scroll window.
   - Weights: $w_r = 0.40, w_t = 0.40, w_v = 0.20$ ($\sum w_i = 1.0$).

2. **Action Safety & Occlusion Index ($I_{\text{safe}}$)**:
   $$I_{\text{safe}}(e) = 1 - O_{\text{occlusion}}(e)$$
   where $O_{\text{occlusion}}(e)$ detects overlapping modal dialogs or transparent overlays, preventing misclicks on obscured elements.

### 3. Thresholding & Refusal Decision Criteria

Page Agent Web UI Assistant enforces strict operational safety and user privacy boundaries:
- **Refusal to Auto-Submit Sensitive Financial Forms**: Form submissions triggering real-money payments or credit card processing require explicit human confirmation (`ERR_PAYMENT_SUBMISSION_REQUIRES_APPROVAL`).
- **Refusal of Password Inspection**: Password fields (`<input type="password">`) and CVV numbers are deterministically masked at the DOM level (`WARN_PASSWORD_MASKED`).
- **Turn Ceiling Enforcement**: In-page interactive loops enforce a ceiling of `max_turns: 25` to prevent infinite navigational loops (`WARN_TURN_BUDGET_REACHED`).
- **Cross-Origin Boundary Isolation**: The assistant operates strictly within the current origin; cross-origin iframe exfiltration is blocked (`ERR_CROSS_ORIGIN_SECURITY_VIOLATION`).

### 4. Fallback Decision Mechanism

Continuous web interaction support is maintained through multi-tier fault recovery:
- **Model Cascade Failover**: When the primary foundation model experiences latency spikes or HTTP 429 rate limits, the orchestrator cascades automatically between `claude-3-5-sonnet`, `gpt-4o`, and `gemini-2.0-flash`.
- **Selector Fallback Cascade**: If accessibility tree handles fail due to dynamic shadow DOM rendering, the engine cascades to XPath and CSS selector fallbacks.
- **Graceful Conversational Guidance**: If automated synthetic clicking fails on a custom web component, the agent highlights the element visually and prompts the user to click manually.

### 5. Human-in-the-Loop Governance

Human users retain complete operational primacy and visual oversight:
- **Live Visual Highlighting**: Users see glowing bounding boxes and tooltip highlights on web elements before and during agent interactions.
- **Emergency Session Kill Switch**: Users can halt agent actions instantly by clicking anywhere on the screen, pressing `Esc`, or issuing the `/stop` voice command.
- **Inspectable Interaction Trajectories**: Every DOM action, synthetic event, and network transition is recorded in structured browser logs for transparency.

---

## The Data It Uses

Page Agent operates under strict privacy, data minimization, and local browser isolation standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill in-page UI assistance:
- **User Voice & Text Commands**: In-page chat prompts and natural language navigation goals.
- **Accessibility DOM Snapshots**: Structural JSON representations of visible DOM nodes, labels, and roles.
- **Viewport Dimension Data**: Screen widths, scroll offsets, and element bounding client rects.

### 2. Configuration & Reference Data

- **Model Context Protocol (MCP) Schemas**: JSON tool definitions enabling external LLMs to drive in-page actions.
- **Highlighting CSS Palettes**: Clean, non-intrusive styling rules for in-page visual overlays.
- **Origin Permission Matrices**: Security rules defining allowed web domains and sensitive form inputs.

### 3. Base Model & Inference Lineage

- **Deterministic DOM Engines**: Native JavaScript DOM traversal, MutationObserver listeners, and synthetic event dispatchers executed natively in the browser (100% deterministic with zero LLM variance).
- **Foundation LLMs**: High-capability frontier models (`claude-3-5-sonnet`, `gpt-4o`, `gemini-2.0-flash`) utilized for intent parsing, DOM comprehension, and conversational response generation.
- **Zero Training on User Web Sessions**: User form data, browsing histories, and authenticated page contents are never transmitted to external cloud servers or used for model training.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection via adversarial web content, credential harvesting, and cross-site scripting (XSS).
- **Local Browser Storage**: All interaction logs, user preferences, and transient DOM states reside exclusively in the user's browser `localStorage`.
- **Automated Password Masking**: Password fields, tokens, and credit card numbers are masked prior to model ingestion.
- **Zero Commercial Monetization**: User web interactions, browsing trajectories, and form entries are never monetized, aggregated, or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of Page Agent is essential for web deployment.

### 1. Complex Canvas & WebGL Controls
- **Limitation**: Applications rendering UI elements inside opaque HTML5 `<canvas>` tags (e.g., Google Maps, Figma) lack accessibility nodes.
- **Mitigation**: The assistant pairs coordinate-based visual estimation with OCR text detection for canvas-rendered interfaces.

### 2. Deeply Nested Closed Shadow DOMs
- **Limitation**: Web components encapsulated in closed shadow roots (`attachShadow({mode: 'closed'})`) prevent external DOM traversal.
- **Mitigation**: The agent utilizes custom injected script bridges where application host permissions allow, or guides the user visually.

### 3. Dynamic Asynchronous Re-Renders
- **Limitation**: Complex single-page applications with rapid client-side re-renders can invalidate element handles between planning and clicking.
- **Mitigation**: The engine re-queries element handles immediately prior to event dispatch to ensure element attachment.

### 4. Non-Standard Custom Drag-and-Drop
- **Limitation**: Proprietary complex multi-step drag-and-drop interactions can fail under standard synthetic mouse events.
- **Mitigation**: The agent offers step-by-step visual coaching, highlighting start and destination coordinates for the user.

### 5. Multi-Window and Popup Management
- **Limitation**: Workflows spawning separate detached browser windows can disrupt in-page extension context.
- **Mitigation**: The extension tracks window focus events and re-injects the assistant overlay onto newly focused active tabs.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & DOM localization formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested user commands, DOM snapshots & viewports | Section 1 | Verified |
| - Configuration, MCP schemas & origin permissions | Section 2 | Verified |
| - Base model lineage & deterministic DOM engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Complex canvas & WebGL controls | Section 1 | Verified |
| - Deeply nested closed shadow DOMs | Section 2 | Verified |
| - Dynamic asynchronous re-renders | Section 3 | Verified |
| - Non-standard custom drag-and-drop | Section 4 | Verified |
| - Multi-window and popup management | Section 5 | Verified |
