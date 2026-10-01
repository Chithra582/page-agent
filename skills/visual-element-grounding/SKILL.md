---
name: "visual-element-grounding"
description: "Identifies interactive web elements through visual bounding boxes, semantic roles, and text labels."
---

# Visual Element Grounding

## Overview
This skill resolves natural language element references (e.g., 'the blue Submit button on the right') into precise DOM nodes by fusing accessibility tree attributes with visual bounding box geometry.

## Key Capabilities
- **Geometric Bounding**: Computes viewport-relative client bounding rects for visible components.
- **Semantic Text Matching**: Matches visible text, placeholder strings, and aria-label attributes.
- **Spatial Disambiguation**: Resolves ambiguous duplicate controls using positional proximity hints.

## Operational Workflow
1. **Viewport Scanning**: Enumerate visible interactive candidate nodes.
2. **Attribute Gathering**: Extract element tag, role, text, title, and bounding coordinates.
3. **Similarity Ranking**: Score candidate nodes against user descriptive intent.
4. **Target Selection**: Select top candidate and return unique element reference token.
