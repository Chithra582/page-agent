# EXPLAINABILITY — Page Agent Web UI Assistant

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* Page Agent Web UI Assistant (`page-agent`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Web UI Agents & In-Page Automation  

---

## 1. Overview & Operational Purpose
The **Page Agent Web UI Assistant** is an AI-powered UI automation and assistance engine designed for web applications. It operates either as an embedded JavaScript library within a web application or as a Chrome browser extension, enabling users to interact with complex web interfaces through conversational natural language.

Its operational purpose is to eliminate web navigation friction, automate multi-step web forms, assist users with accessibility hurdles, and expose browser DOM manipulation capabilities to external AI models via the Model Context Protocol (MCP).

---

## 2. How the Agent Decides (Decision-Making Logic)
Page Agent Web UI Assistant operates across a deterministic, multi-stage decision pipeline:

```
[Stage 1: User Request Intake] ──> [Stage 2: Viewport DOM Scan] ──> [Stage 3: Element Grounding]
                                                                                │
                                                                                ▼
[Stage 6: Response & State Sync] <── [Stage 5: State Verification] <── [Stage 4: Action Dispatch]
```

### 2.1 User Request Intake & Goal Decomposition
- **Decision:** Parse incoming natural language instructions, identify the target user goal, and decompose it into a sequential sequence of web interaction steps.
- **Rules:** Validate command parameters; reject ambiguous instructions and ask clarifying questions if multiple interpretations exist.

### 2.2 Viewport DOM Scan & Candidate Extraction
- **Decision:** Inspect the rendered DOM tree, filter for visible and interactive components, and extract element labels, roles, and bounding boxes.
- **Rules:** Traverse open Shadow DOM trees; filter out invisible (zero width/height or display:none) nodes to prevent ghost interactions.

### 2.3 Element Grounding & Safety Verification
- **Decision:** Match target description against candidate element labels and verify that the target does not trigger sensitive action restrictions.
- **Rules:** If the element is a payment button or destructive delete action, halt and request human approval; otherwise render visual highlight.

### 2.4 Action Dispatch & State Verification
- **Decision:** Dispatch synthetic mouse click, keyboard input, or scroll event, and wait for DOM mutations and network requests to settle.
- **Rules:** Verify URL change or DOM state change; if no mutation occurs, re-try with alternate event dispatches (e.g., triggering change and input events).

---

## 3. Data & Privacy
| Data Category | Retention Policy | Third-Party Sharing | Storage Mechanism |
|---|---|---|---|
| Viewport DOM Snapshots | Ephemeral (Action Turn) | None | In-Memory JavaScript State |
| User Chat & Instruction History | Session Lifetime | Configured LLM Provider | Local SessionStorage / Memory |
| Extension Settings & API Keys | User Controlled | None | Local Chrome Extension Storage |
| Page Interaction Audit Trail | Ephemeral / Debug | None | In-Memory Rolling Array |

Page Agent Web UI Assistant complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** All DOM analysis, element grounding, and synthetic event dispatches occur entirely within the user's local browser runtime.
- **Epistemic Isolation:** The agent content script runs in an isolated extension world or host script sandbox, preventing arbitrary code injection into host page scopes.
- **Sanitized Model Payloads:** Prompts sent to model inference engines exclude hidden password inputs, authentication cookies, and session tokens.
- **Data Minimization:** Only element labels, interactive bounding boxes, and relevant text excerpts within the active viewport are forwarded to reasoning models.

---

## 4. Known Limitations & Failure Modes
Reviewers, auditors, and users should note the following operational constraints:
1. Dynamic Single-Page App Re-rendering
   - *Limitation:* Highly dynamic React or Vue interfaces may detach DOM nodes during asynchronous re-renders, causing stale element references.
   - *Mitigation:* The agent re-queries element references immediately before dispatching events and re-grounds upon detection of detached nodes.
2. Canvas and SVG Visual Components
   - *Limitation:* Charts and complex visual elements rendered purely inside HTML5 Canvas do not expose standard DOM sub-nodes.
   - *Mitigation:* The agent falls back to relative percentage-based coordinate clicks and screenshot-based visual grounding.
3. Cross-Domain Iframe Security Boundaries
   - *Limitation:* Browser security policies strictly prevent content scripts from inspecting cross-origin iframes without parent window permissions.
   - *Mitigation:* The agent detects cross-origin iframes, flags the permission boundary to the user, and uses extension background messaging when permitted.
4. Anti-Bot and Captcha Obstacles
   - *Limitation:* Sites with CAPTCHAs, bot protection, or aggressive event listener blocking may fail to register synthetic events.
   - *Mitigation:* The agent detects challenge overlays, pauses execution gracefully, and signals the human operator to complete verification.

---

## 5. Verification, Safety & Human Oversight
Page Agent Web UI Assistant integrates multi-layer safety rails to ensure full human accountability and system integrity:
- **Real-Time Human Approval Gate:** Financial checkout, credential submission, and destructive actions require explicit user confirmation before clicking.
- **Emergency Session Interrupt:** Users can pause or cancel automated execution at any moment by clicking the floating widget's stop button or pressing the Escape key.
- **Step Quota Guardrails:** Strict step limits (default: 25 actions) prevent infinite loops or unintended recursive navigation.
- **Structured Audit Logging:** Every located element, synthetic event, URL navigation, and user confirmation is logged in an in-browser audit panel.
