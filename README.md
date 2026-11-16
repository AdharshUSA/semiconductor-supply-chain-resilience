# Resilience Assessment of Semiconductor Supply Chains using Arena Simulation

Course project for **IE 580: Systems Simulation**, Purdue University. Team of five (Group 3).

## Question

Chip manufacturing is concentrated in a few regions, which makes the supply chain efficient and fragile at once. This project builds a process-level discrete-event model from chip design to packaged chip. It shows where bottlenecks form and tests whether targeted changes make the system more resilient.

## Model

Each chip is an entity with attributes (node type, start time, defects). Every stage uses queue, seize, process, release logic. Processing times and failure behavior come from synthetic data and literature.

| Subsystem | Detail |
|---|---|
| Design and wafer prep | Global, legacy and advanced design paths, then slicing, polishing and wet bench in sequence |
| Front-end fabrication | Oxidation, lithography, etch, ion implant, thin-film deposition. Lithography is the only tool that fails and is repaired. |
| Die bank | Buffer between fab and back end |
| Assembly, test and packaging (ATP) | Dicing, die attach, wire bonding, inspection, final packaging |

## Results

- **Baseline:** 33 chips completed in 1,000 hours (about 2 per week) from 150 wafers. Target was at least 70.
- **Bottlenecks:** slicing, polishing, wet bench and lithography. Design dominates time in system.
- **10 scenarios tested.** Three met the target:

| Scenario | Chips in 1,000 h | Approach |
|---|---|---|
| 3 | 112 | Largest, most targeted capacity increases at the longest queues |
| 4 | 93 | Focus on reducing failures |
| 10 | 72 | Similar total capacity to Scenario 3, allocated less effectively |

- **Regional impact:** even the best scenarios cannot cover US demand of roughly 150 to 200 units per week. Upstream production is the real limit.

## Takeaways

- Fix wafer prep and lithography directly. Uniform capacity increases do much less.
- A single line cannot deliver national resilience. It takes redundant critical tools and multiple plants.

## Tools

Arena Simulation Software

## Team

Jaemin Park, Valentina Aguirre Garrido, Monisha Jayashankar, Adharsh Rajasekar, Diya Sen
