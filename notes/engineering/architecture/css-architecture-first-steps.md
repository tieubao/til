---
title: "CSS architecture - first steps"
date: 2016-07-30
captured: "2016-07-30T13:41:43Z"
tags: [css, architecture, frontend, BEM, SMACSS, ITCSS]
source: "GitHub issue tieubao/til#247"
aliases: []
status: refined
---

## Context

A primer from Cheesecake Labs on bringing architectural thinking to CSS. Without structure, CSS features like cascading, specificity, and inheritance create tangled, unmaintainable stylesheets. This article surveys the major CSS methodologies and proposes a practical starting structure.

**Source:** [CSS Architecture: First Steps](https://www.ckl.io/blog/css-architecture-first-steps/)

## The problem

Page-based CSS falls apart in responsive, component-driven applications. Selectors get tightly coupled to DOM structure. Specificity wars erupt. New developers add `!important` rather than understanding the cascade. The result: CSS that nobody dares refactor.

## Three core principles

1. **Modularity** - break code into smaller, scoped chunks rather than monolithic stylesheets
2. **Organization** - categorize rules by function (settings, base, layout, components)
3. **Meaningful naming** - selector names should convey intent and relationships

## Methodologies compared

**SMACSS** (Scalable and Modular Architecture for CSS) introduced five categories: Base, Layout, Module, State, and Theme. Good conceptual framework but light on naming conventions.

**ITCSS** (Inverted Triangle CSS) expands SMACSS with additional layers (Settings, Tools, Generic, Elements, Objects, Components, Trumps), ordered by specificity from low to high. The "inverted triangle" metaphor: broad, low-specificity rules at the top, narrow overrides at the bottom.

**BEM** (Block, Element, Modifier) provides concrete naming conventions:
- **Block**: independent component (`.card`)
- **Element**: inner part, double underscore (`.card__title`)
- **Modifier**: variation, double dash (`.card--featured`)

BEM reduces specificity conflicts because selectors stay flat and predictable.

## Recommended four-layer structure

- **Settings** - variables, mixins, helper functions (preprocessor-only, no output)
- **Base** - resets and default HTML element styling
- **Layout** - structural containers, grids, page scaffolding
- **Components** - reusable UI patterns (buttons, modals, cards), each in its own file

## Key takeaway

Use a CSS preprocessor (Sass, LESS) to split code into separate files. Adopt BEM naming to keep specificity flat. Abstract UI elements into standalone components only when they appear in multiple contexts. The methodology matters less than having one at all.

## Related

- [[software-architecture-guide-fowler]] - architecture thinking applied at every layer, including CSS
