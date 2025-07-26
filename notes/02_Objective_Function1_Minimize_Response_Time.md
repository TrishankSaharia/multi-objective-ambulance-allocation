## MSc Final Project: Optimizing City Ambulance Allocation Using Real-Time Data

### Student: Trishank Saharia

### Guide: Dr. Rajashree Mishra, KIIT University

### Duration: Aug 2025 – April 2026

---

## Stage 0: Setup and Planning

### Project Title

Optimizing City Ambulance Allocation Using Real-Time Data: A Multi-Objective Approach

### Objectives

- Allocate ambulances optimally across a city
- Use real-time data such as traffic and emergency calls
- Balance multiple objectives like response time, coverage, and fairness

### Storage and Access Plan

- Chat-based notes: Delivered step-by-step in ChatGPT
- PDF Notes: Exported for each completed stage
- GitHub Folder: Will contain final code, report, and visualizations (to be created in Stage 4)

### Tools Required

- Python (Jupyter Notebooks)
- GitHub (for code and version control)
- Excel (initial EDA, tabular modeling)
- Overleaf (for final report writing in LaTeX)

### Timeline Overview

| Stage   | Focus                     | Time Required |
| ------- | ------------------------- | ------------- |
| Stage 0 | Setup + Planning          | 1 week        |
| Stage 1 | Theory + Core Concepts    | 3–4 weeks     |
| Stage 2 | Data + Assumptions        | 3–4 weeks     |
| Stage 3 | Model Design + Objectives | 4–5 weeks     |
| Stage 4 | Implementation + Testing  | 6–8 weeks     |
| Stage 5 | Report + Finalization     | 4 weeks       |
| Buffer  | Feedback & Rework         | 4+ weeks      |

---

## Stage 1: Core Concepts & Background

### 1. What is Optimization?

Optimization is the process of selecting the best solution from a set of feasible solutions under given constraints.

#### Key Components

- Objective function (e.g., response time)
- Decision variables (e.g., ambulance placement)
- Constraints (e.g., ambulance availability, coverage zones)

#### Types of Optimization

- Linear Programming
- Integer Programming
- Non-Linear Programming
- Dynamic Optimization
- Stochastic Optimization

---

### 2. What is Multi-Objective Optimization?

Multi-objective optimization (MOO) handles multiple conflicting goals — like minimizing response time while minimizing cost.

It results in a set of optimal trade-offs (Pareto-optimal solutions) instead of a single answer.

---

### 3. What is the Pareto Front? (Story-Based Explanation)

Imagine you're working with the **Bhubaneswar City Health Department**. They say:

> "We want two things:
>
> 1. 🚑 Faster ambulance response times
> 2. 💰 Lower monthly operating cost"

These two are in conflict:

- More ambulances → faster but costly
- Fewer ambulances → cheaper but slower

You try 4 ambulance allocation plans:

| Plan | Avg Response Time (min) | Monthly Cost (₹ lakh) |
| ---- | ----------------------- | --------------------- |
| A    | 6.0                     | 12                    |
| B    | 5.0                     | 14                    |
| C    | 4.8                     | 18                    |
| D    | 5.0                     | 15                    |

🧠 Analysis:

- A is cheap but slow
- B is balanced
- C is fast but expensive
- D is worse than B in all aspects ⇒ **dominated**

✅ Plans A, B, C form the **Pareto Front**:

> You cannot improve one without worsening another.

```
 Response Time ↓
      |
  6.0 |● A
      |
  5.0 |● B        D ← dominated
      |
  4.8 |● C
      |_________________________
        12   14   15   18 → Cost ₹
```

---

### 4. What is Ambulance Allocation?

It’s the science of placing ambulances across the city so they:

- Reach emergencies quickly
- Cover high-risk areas
- Minimize idle time
- Adjust to dynamic city data

---

### 5. Why Use Real-Time Data? *(To be expanded in Stage 1.3)*

- Real-time traffic helps avoid congested routes
- GPS tracks ambulances and caller location
- Hospital load data helps reroute in emergencies

---

### 6. Real-World Applications *(To be expanded in Stage 1.3)*

- **London Ambulance** uses real-time GPS + predictive routing
- **New York EMS** uses historical demand prediction
- **Delhi NCR** tested AI-based hotspot prediction

---

## Stage 1.2 – Objective Functions and Constraints

In this sub-stage, we mathematically define how your ambulance allocation model will work.

### 🎯 Objective Functions

#### Objective 1: Minimize Average Response Time

**Goal**: Reduce the average time taken by ambulances to reach demand zones.

Let:

- \(T_{ij}\): Estimated travel time from ambulance base \(i\) to zone \(j\)
- \(x_{ij}\): Binary variable = 1 if ambulance from base \(i\) is assigned to zone \(j\); 0 otherwise
- \(D_j\): Relative demand (weight or frequency of calls) in zone \(j\)

**Mathematical Formulation:** \(\text{Minimize: } f_1(x) = \sum_{i,j} T_{ij} \cdot x_{ij} \cdot D_j\)

**Explanation of Terms:**

- \(T_{ij} \cdot x_{ij}\): If an ambulance from base \(i\) is assigned to zone \(j\), this gives the travel time
- \(D_j\): Gives higher importance to zones with more emergencies
- The summation aggregates the weighted travel time across all zones and bases

**Example:** If:

- Zone 1 needs 2x more ambulances than Zone 2 (i.e., \(D_1 = 2, D_2 = 1\))
- Base A is 5 min from Zone 1, 10 min from Zone 2
- Ambulance from A is sent to Zone 1: \(x_{A1} = 1\), \(x_{A2} = 0\)

Then:

$$
  f_1(x) = (5 \times 1 \times 2) + (10 \times 0 \times 1) = 10
$$

The objective is to **minimize** this total value across the city.

#### Objective 2: Minimize Ambulance Idle Time

Let:

- \(A_i\): idle time of ambulance \(i\)

\(\text{Minimize: } f_2(x) = \sum_i A_i\)

#### Optional Objective 3: Maximize Demand Coverage

Let \(C_{ij}\) = 1 if ambulance \(i\) can reach zone \(j\) within 8 minutes:

\(\text{Maximize: } f_3(x) = \sum_{i,j} C_{ij} \cdot x_{ij} \cdot D_j\)

---

### 🔗 Constraints

#### Constraint 1: Limited Ambulances

\(\sum_{i,j} x_{ij} \leq N\)

#### Constraint 2: Each Zone Must Be Covered

\(\sum_i x_{ij} \geq 1 \quad \forall j\)

#### Constraint 3: Binary Decisions

\(x_{ij} \in \{0, 1\} \quad \forall i,j\)

#### Constraint 4: Optional Time Limit

\(T_{ij} \cdot x_{ij} \leq T_{\text{max}} \quad \forall j\)

---

You now have a complete optimization model structure with detailed derivation of the first objective.

