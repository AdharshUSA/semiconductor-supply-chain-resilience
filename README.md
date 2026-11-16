# Resilience Assessment of Semiconductor Supply Chains using Arena Simulation

Course project for **IE 580: Systems Simulation**, Purdue University. Team of five (Group 3). Built in Arena 16.20.

## Question

Chip manufacturing is concentrated in a few regions, which makes the supply chain efficient and fragile at once. This project builds a process-level discrete-event model from chip design to packaged chip, shows where bottlenecks form, and tests whether targeted changes in capacity and tool reliability raise output.

## Model

Each chip is an entity with attributes (node type, start times, defect counters). Every station uses queue, seize, process, release logic. Chips are either legacy node (28 nm and above) or advanced node (6 to 27 nm), and advanced designs take longer. The run is 150 units of raw material over 1,000 simulated hours.

| Subsystem | Stations (capacity) | Processing times |
|---|---|---|
| Design | Global design (8), legacy design (7), advanced design (3) | Triangular: global (6, 8, 10), legacy (10, 12, 14), advanced (15, 20, 30) |
| Wafer prep | Slicing (1), polishing (1), wet bench (1) | Triangular: (6, 8, 12), (10, 12, 16), (12, 18, 24) |
| Front-end fabrication | Oxidation, lithography, etch, ion implant, thin-film deposition (1 each) | Exponential means: 25, 6, 7, 1.5, 5 |
| Die bank | Buffer between fab and back end | None |
| Assembly, test, packaging | Dicing, die attach, wire bonding, inspection, final packaging | Exponential means: 5, 6, 7.5, 3.5, 4.5 |

Lithography is the only tool with failures and repairs: time between failures EXPO(6) and repair time EXPO(0.9), about 87% availability. Processing times and failure parameters are assumptions drawn from the literature listed in the project report and from synthetic parameter sheets, not measured fab data. Stage and tool definitions follow the CSET ETO Chip Explorer data (Georgetown University).

## Baseline results

One replication of the baseline model (Arena output):

- **Throughput:** 33 chips completed in 1,000 hours (21 legacy, 12 advanced) from 150 units started.
- **Time in system:** design dominates. Average design time is 442 hours for legacy and 356 hours for advanced chips. Total average time in system is 622 hours (legacy) and 569 hours (advanced).
- **Saturated stations:** slicing (98% utilization), polishing (97%), wet bench (96%) and oxidation (92%). Average queue waits are about 249 hours at slicing, 237 at oxidation, 160 at polishing and 158 at wet bench.
- **Starved stations:** lithography is only 17% utilized in this baseline run, etch 24%, deposition 15% and ion implant 5%, because few wafers get through wafer prep and oxidation.

## Scenarios

Ten scenarios changed capacity and the lithography failure parameters. The target was at least 70 completed chips in 1,000 hours. Counts below are legacy plus advanced chips as reported in the project report (50 replications listed per scenario).

| Scenario | Failure param | Repair param | Litho | Oxidation | Slicing / polish / wet bench | Chips |
|---|---|---|---|---|---|---|
| 1 (baseline) | 6 | 0.5 | 1 | 1 | 1 / 1 / 1 | 35 |
| 2 | 6 | 0.5 | 2 | 1 | 2 / 2 / 2 | 35 |
| 3 | 6 | 0.5 | 2 | 4 | 3 / 3 / 3 | **112** |
| 4 | 12 | 0.5 | 2 | 4 | 2 / 2 / 2 | **93** |
| 5 | 6 | 0.5 | 2 | 1 | 2 / 2 / 2 | 35 |
| 6 | 6 | 0.5 | 1 | 1 | 2 / 1 / 2 | 36 |
| 7 | 10 | 0.3 | 2 | 2 | 1 / 2 / 2 | 69 |
| 8 | 6 | 0.5 | 1 | 3 | 2 / 2 / 1 | 48 |
| 9 | 6 | 0.5 | 1 | 2 | 1 / 2 / 2 | 68 |
| 10 | 6 | 0.3 | 2 | 2 | 4 / 4 / 4 | **72** |

Scenarios 3, 4 and 10 met the target. Scenario 3 had the largest, most targeted capacity increases and the lowest total time in system (about 360 hours legacy, 382 advanced). Scenario 4 mainly cut failures. Scenario 10 added the most wafer prep capacity, which cut design time to about 104 hours but left fabrication time near 304 hours, versus about 141 hours in Scenario 3 (oxidation capacity is 4 in Scenario 3 and 2 in Scenario 10).

## Regional impact

The report estimates US demand at roughly 150 to 200 units per week, with 30 to 50 per region. Even the best scenario falls well short, so upstream production, not distribution, is the limit. At 168 hours per week, 112 chips per 1,000 hours is about 19 per week.

## Takeaways

- Wafer prep and oxidation saturate first in the baseline. Adding capacity where queues form works better than uniform increases.
- Lithography failures and repair time matter once upstream congestion is relieved.
- A single line cannot deliver national resilience. It would take redundant critical tools and multiple plants.

## Limitations

- Baseline figures come from a single replication, and Arena reports no confidence half widths for it.
- Parameters are literature-based assumptions. The model has no explicit disruption events, cost or energy metrics, or geographic layouts. Those are the next extensions.
- The report and deck quote throughput per week using a conversion that does not match a 168-hour week, so this README states chips per 1,000 hours instead.
