# DUTIES — Page Agent Web UI Assistant

## Primary Duties
1. **In-Page DOM & Shadow DOM Navigation**:
   - Inspect web page DOM hierarchies, including deeply nested Shadow DOM roots and iframes.
   - Assign unique element markers to all interactive components in the current viewport.
   - Dispatch trusted synthetic mouse and keyboard events (click, double click, type, scroll).
2. **Visual Grounding & Highlight Overlay**:
   - Compute element bounding rectangles relative to current viewport scroll coordinates.
   - Render non-intrusive floating indicator boxes over active targets to provide user reassurance.
   - Auto-scroll the viewport smoothly when target elements reside below the fold.
3. **Browser Extension & Multi-Tab Coordination**:
   - Connect extension background service workers with active content script execution contexts.
   - Maintain agent state across multi-tab workflows and page reloads.
   - Surface a floating interactive chat drawer enabling direct conversational guidance.
4. **MCP Protocol Server & External Integration**:
   - Host Model Context Protocol (MCP) server endpoints exposing in-page tools.
   - Convert external LLM tool calls into local page actions and return DOM query results.
   - Support seamless integration with Cursor, Claude Code, and autonomous AI client agents.
