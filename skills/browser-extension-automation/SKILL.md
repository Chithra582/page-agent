---
name: "browser-extension-automation"
description: "Operates across arbitrary third-party websites through the Page Agent Chrome extension content scripts."
---

# Browser Extension Automation

## Overview
This skill manages Page Agent when deployed as a Manifest V3 Chrome Extension, orchestrating cross-tab navigation, background service workers, and interactive chat drawer UI overlays.

## Key Capabilities
- **Universal Site Injection**: Injects automation scripts safely into any navigated HTTP/HTTPS web page.
- **Chat Drawer Management**: Controls the floating in-page conversational drawer UI.
- **Cross-Tab State**: Preserves multi-step task progress across page reloads and tab navigations.

## Operational Workflow
1. **Content Script Injection**: Initialize Page Agent runtime within the active browser tab.
2. **User Guidance Drawer**: Mount conversational chat drawer floating above page contents.
3. **Task Orchestration**: Execute user requests step-by-step with real-time progress indicators.
4. **Tab Lifecycle Handling**: Reconnect agent state if the page performs a full URL reload.
