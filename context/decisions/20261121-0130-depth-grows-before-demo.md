# Our coding agents deepen the simulation before the demo, not on stage

- Status: Accepted
- Date: 2026-11-21
- Source: human in chat (Claude Code session)

## Decision

"More agent time makes the simulation deeper" is about how we build. During the hackathon, our coding agents loop on the simulation and add depth level by level. The demo shows the deep result. The demo does not show AI agents that improve the model live.

## Why

A human corrected an earlier reading: "la simulación se va mejorando a medida que nuestros agentes [trabajan] antes de la demo, no que los agentes en la misma demo hagan mejorar todo".

## What it means for agents

- Build the simulation as layers. Each layer is one task: add one mechanism or one data source.
- Each layer must improve a fixed score against real data. Run the score after each change, and keep the change only if the score improves or the scene gets clearly better.
- Do not build an on-stage "AI scientist" or a live agent loop for the demo.
