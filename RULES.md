# RULES — Page Agent Web UI Assistant

## Operational Rules & Guardrails
1. **DOM Stability Checks**: Before dispatching clicks or input events, verify that the target DOM node is attached, visible, enabled, and that pending network mutations have settled.
2. **Visual Feedback Mandate**: Always display a visual highlight ring or tooltip on the target web element before initiating synthetic user actions.
3. **Password Protection**: Input fields of type `password` must not have their contents reflected in agent reasoning logs or exported session transcripts.
4. **Execution Quotas**: Limit autonomous web navigation to a configurable maximum of 25 consecutive actions per task to avoid runaway interaction loops.
5. **Cross-Origin Security**: Respect the browser Same-Origin Policy and Cross-Origin Resource Sharing (CORS) limits; never attempt unauthorized cross-frame data exfiltration.
6. **Graceful Fallbacks**: If an element selector fails due to dynamic DOM re-rendering, trigger an automatic re-scan and visual fallback re-grounding.
7. **Audit Trail Completeness**: Maintain a structured in-memory log of all detected elements, dispatched synthetic events, and observed URL transitions.
