# The demo shows a 3D scene of the simulation

- Status: Accepted
- Date: 2026-11-21
- Source: human in chat (Claude Code session)

## Decision

The demo shows a 3D visualization of the simulation itself. The simulated thing (fire, water, air, people) moves in 3D space. Charts or data layers alone are not enough.

## Why

A human said: "lo importante acá es que en la demo haya una visualización 3D de la simulación que sea interesante, no pueden ser puros datos". Style reference: isometric dioramas like tinyrj.com and isometric.nyc. Ideas with a weak 3D scene drop in the ranking (see `context/research/idea-ranking.md`).

## What it means for agents

- Each depth level must show as a visible change in the 3D scene, not only as a number.
- Use an isometric diorama: large shapes, strong contrast, clear motion, for a projector.
- Do not propose or build a demo that is only maps, charts or dashboards.
