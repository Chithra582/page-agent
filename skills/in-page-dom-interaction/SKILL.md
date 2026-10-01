---
name: "in-page-dom-interaction"
description: "Interacts with live web page DOM elements, Shadow DOM, and dynamic form inputs directly in-browser."
---

# In-Page DOM Interaction

## Overview
This skill executes reliable synthetic user interactions directly within live web application DOM trees, accommodating modern frontend frameworks (React, Vue, Angular) and deep Shadow DOM hierarchies.

## Key Capabilities
- **Event Dispatching**: Dispatches standard trusted PointerEvent, MouseEvent, and InputEvent sequences.
- **Shadow DOM Piercing**: Traverses nested open shadow roots to reach encapsulated custom elements.
- **Form Automation**: Enters text, triggers change notifications, and selects options in dynamic dropdown menus.

## Operational Workflow
1. **Target Identification**: Locate the element using semantic locators and spatial bounding boxes.
2. **Visibility Check**: Verify element interactability and scroll into view if off-screen.
3. **Visual Highlighting**: Paint temporary highlight marker on the target element.
4. **Event Sequence Dispatch**: Emit pointerdown, pointerup, click, and input events in succession.
5. **Mutation Observation**: Wait for resulting network requests and DOM updates to stabilize.
