# transient-queue-simulation-staffing
A Python-based stochastic simulation engine designed to optimize workforce scheduling in high-variance service environments. Models transient queue dynamics, non-linear labor costs (overtime), and service rate decay (fatigue) to maximize net profit.


# Stochastic Service Capacity Optimization

## Project Overview
This project simulates high-variability service operations (e.g., QSR/Cafe environments) to determine optimal staffing policies. Unlike traditional steady-state queueing models ($M/M/k$), this engine uses a **Fluid Flow Approximation** to capture **transient queue behavior** during peak arrival surges.

The simulation quantifies the trade-offs between **Labor Cost Efficiency** (Wage Bill) and **Service Level Reliability** (Lost Revenue due to Balking), while accounting for human factors like fatigue and overtime penalties.

## Theoretical Framework
The model addresses specific shortcomings of static capacity planning in stochastic environments:

1.  **The Flaw of Averages:** Demonstrates why staffing for "Average Demand" ($\lambda_{avg}$) leads to catastrophic service failure during peak hours.
2.  **Transient Analysis:** Captures the "Carryover Effect," where a queue backlog formed in Hour $t$ degrades service quality in Hour $t+1$.
3.  **Human Constraints:**
    * **Service Rate Decay ($\mu$):** Models the drop in service speed as staff exceed 6 hours of continuous work (Fatigue).
    * **Non-Linear Costs:** Incorporates Overtime Penalties to penalize inefficient shift scheduling.
    * **Psychological Balking:** Implements a "Hard Limit" on queue depth ($N=10$), modeling immediate customer loss upon seeing a full line.

## Simulation Logic
The engine runs a **Monte Carlo Simulation** (N=80,000 runs) across hourly blocks using the following logic:

* **Arrival Process:** Non-homogeneous Poisson Process ($\lambda_t$ varies hourly).
* **Service Process:** Dynamic capacity $C_t = s_t \times \mu_{state}$, where $\mu$ shifts between `MU_FRESH` (24/hr) and `MU_TIRED` (20/hr).
* **Flow Logic:**
  $$Queue_{t+1} = \max(0, Queue_t + Arrivals_t - Capacity_t)$$
  *(Subject to Balking Limits)*

## Key Scenarios Analyzed
The repository compares two distinct staffing strategies under a fixed headcount constraint ($N=4$ staff available):

### 1. The Constant Strategy ("Lazy Manager")
* **Policy:** Maintain 2 staff members all day (Open to Close).
* **Outcome:** Minimizes scheduling effort but fails during peaks (8 AM & 12 PM). Triggers overtime due to long shift duration.

### 2. The Reactive Strategy ("Chase Demand")
* **Policy:** Adjust staffing levels (1-3 staff) to match hourly $\lambda$.
* **Outcome:** Allocates labor to revenue-generating peaks. Minimizes fatigue by keeping shifts shorter.

## Results & Visualization
The simulation produces financial impact reports visualizing:
* **Net Profit:** The ultimate objective function.
* **Opportunity Cost:** Revenue lost due to queue balking.
* **Wage Utilization:** Regular vs. Overtime pay distribution.

## Tech Stack
* **Python 3.x**
* **NumPy:** Poisson generation and array operations.
* **Pandas:** Data structuring and metric aggregation.
* **Matplotlib:** Visualization of financial trade-offs.

## How to Run
1. Clone the repository.
2. Install dependencies:
   ```bash
   pip install numpy pandas matplotlib
3. Run the simulation:
   ```Bash
    python simulation_engine.py
## Future Scope
* Implementation of Discrete Event Simulation (DES) for minute-level granularity.
* Integration of Genetic Algorithms to solve for the global optimal schedule automatically.
