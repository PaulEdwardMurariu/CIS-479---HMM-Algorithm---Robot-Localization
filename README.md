# 🤖 HMM Algorithm – Robot Localization

This project implements a **Hidden Markov Model (HMM)**–based robot localization algorithm in Python.  
It was developed as part of the **CIS 479 course** (Artificial Intelligence) at the University of Michigan–Dearborn.

---

## 🧠 Project Overview
The goal of this project is to simulate how a robot can **determine its position in a grid environment** using imperfect motion and sensor data.  
The algorithm estimates location probabilities through **recursive Bayesian filtering**, combining prediction and correction (filtering) steps.

**Authors:** Paul Murariu and Dominic Baughman  
**Date:** November 5 2023  
**Language:** Python 3  
**Course:** CIS 479 — Artificial Intelligence  

---

## 🧩 Core Concepts
### Hidden Markov Model (HMM)
The algorithm models the environment as a **7 × 7 grid** of states.  
Each state represents a potential robot position (open cell = 0, obstacle = 1).  
The model uses two primary probabilities:
- **Transition probability** — likelihood of moving from one cell to another
- **Sensor probability** — likelihood of observing a specific environment feature from that cell  

The localization loop alternates between:
1. **Prediction (motion update)** — accounts for drift and uncertainty when moving
2. **Filtering (sensor update)** — corrects probabilities using sensory input

---

## 🧮 Implementation Details
### Maze Definition
A 7×7 maze defines obstacles (`1`) and open spaces (`0`):
```python
maze = [
 [0,0,0,0,0,0,0],
 [0,1,0,1,0,1,0],
 [0,0,0,0,0,1,0],
 [0,0,0,1,0,0,0],
 [0,1,0,0,0,0,0],
 [0,1,0,1,0,1,0],
 [0,0,0,0,0,0,0],
]
```
---

## Probability Parameters
###Parameter	               ###Description	                                      ###Value
correct_obstacle	           Probability of correctly identifying an obstacle	    0.90
correct_open_space	         Probability of correctly identifying open space	    0.95
incorrect_obstacle	         Probability of misreading an obstacle	              0.05
incorrect_open_space	       Probability of misreading open space	                0.10
straight	                   Probability robot moves as intended	                0.75
left	                       Probability of drifting left	                        0.15
right	                       Probability of drifting right	                      0.10
