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
**Course:** CIS 479 — Intro to Artificial Intelligence  

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

### 🎲 Probability Parameters

| Parameter | Description | Value |
|------------|-------------|-------|
| `correct_obstacle` | Probability of correctly identifying an obstacle | 0.90 |
| `correct_open_space` | Probability of correctly identifying open space | 0.95 |
| `incorrect_obstacle` | Probability of misreading an obstacle | 0.05 |
| `incorrect_open_space` | Probability of misreading open space | 0.10 |
| `straight` | Probability robot moves as intended | 0.75 |
| `left` | Probability of drifting left | 0.15 |
| `right` | Probability of drifting right | 0.10 |

---

## ⚙️ Key Functions

#### `transition(curr_state, action)`
Calculates the next grid cell after moving in the direction `action` (`0 = W`, `1 = N`, `2 = E`, `3 = S`).  
If movement would hit a wall or leave the grid, the robot stays in place.

#### `transitional_prob(state, action)`
Returns a set of possible new states and their probabilities (straight, left, right).  
Accounts for drift and directional wrapping using modulo arithmetic.

#### `moving(move_direction)`
Updates the probability distribution after motion, applying drift probabilities to simulate uncertainty.

#### `sensing(row, col)`
Simulates a robot’s sensor by returning the perceived environment around a given cell (`West, North, East, South`).

#### `filtering(visual, next_action)`
Implements the evidence correction (sensor update) step.  
Compares expected sensor readings with observed ones and updates the probability distribution accordingly.

#### `maze_print(maze, prob_maze)`
Displays the maze layout and the robot’s probability of being in each cell as percentages.

---

## 🔁 Execution Flow

The program alternates between **filtering** (sensor updates) and **prediction** (movement updates):

```python
actions_list = [
    ([0, 1, 0, 0], None),   # Sense
    ([0, 0, 0, 0], 'E'),    # Move East
    ([0, 0, 0, 0], None),   # Sense
    ([0, 0, 0, 0], 'N'),    # Move North
    ([1, 0, 0, 1], None),   # Sense
    ([0, 0, 0, 0], 'N'),    # Move North
    ([0, 1, 0, 0], None),   # Sense
    ([0, 0, 0, 0], 'W'),    # Move West
    ([0, 1, 0, 1], None)    # Final Sense
]
```

Each iteration updates the prob_maze to reflect the most likely robot position after movement or sensing.

---

## 📘 Algorithm Components (From Report)

### 🔄 Transitional Probability
Describes the **movement uncertainty** by combining straight, left, and right drift probabilities.  
If movement hits an obstacle or boundary, the robot remains in its current position.

---

### 🧩 Evidence (Conditional Probability)
Compares observed sensor readings with expected maze readings:

- ✅ **Correct readings** → boost probability *(0.9 or 0.95)*
- ❌ **Incorrect readings** → penalize probability *(0.05 or 0.10)*

---

### 🧮 Filtering
Applies sensor updates to refine the robot’s belief of its location.  
Normalization ensures the probabilities in the grid always sum to **1**.

---

### 🚀 Prediction
Computes new position probabilities using **motion commands**, **drift**, and the **transition model**.

---

## 🖥️ Output Example

At runtime, the program prints the initial probability map and updates it after each sensing or motion step.

```text
Initial Probabilities
(0,0): 2.50%   (0,1): 2.50%   (0,2): 2.50%   (0,3): 2.50% ...
(1,0): 2.50%   (1,1): 0.00%   (1,2): 2.50%   (1,3): 2.50% ...
(2,0): 2.50%   (2,1): 2.50%   (2,2): 2.50%   (2,3): 2.50% ...
...
Programmed by pmurariu and baughboy
```

---

## 📄 Files

| File | Description |
|------|-------------|
| [`HMM_Localization.py`](./HMM_Localization.py) | Python source code implementing HMM-based robot localization |
| [`HMM Algorithm.pdf`](./HMM%20Algorithm.pdf) | Summary of algorithm structure and explanation |
| [`HMM Localization Report.pdf`](./HMM%20Localization%20Report.pdf) | Full written report including analysis and component breakdown |

---

## 🏁 Results

The program successfully demonstrates how probabilistic models (HMMs) can be used for robot localization in uncertain environments.
It incorporates both sensor error and motion drift to produce a realistic simulation of autonomous navigation.

© 2023 Paul Murariu & Dominic Baughman — CIS 479 Robot Localization Project

---
