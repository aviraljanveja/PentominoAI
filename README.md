
# Eye tracking based learning analytics for German schools.

This project demonstrates an AI system that analyzes **student gaze behavior during spatial reasoning tasks** to detect whether a student may require teacher intervention while solving digital pentomino puzzles.

The system combines **eye‑tracking data, puzzle interaction logs, and machine learning** to support classroom learning analytics.

---

# Chapter 1 — Problem Overview

## Objective

Build a machine learning system that predicts whether a student is:

| Prediction | Meaning |
|------------|--------|
| Doing Fine | Student is actively engaged in solving the puzzle |
| Needs Help | Student appears disengaged or stuck |

The goal is **engagement detection**, not puzzle completion.

---

## Educational Context

Students solve **digital pentomino puzzles** which help develop:

- Spatial reasoning  
- Geometric intuition  
- Pattern recognition  

Teachers often cannot monitor every student individually in real time.  
This system provides **automatic signals indicating when a student may need help**.

---

## Core Idea

Eye movements reflect **attention and cognitive effort**.

The ML system learns the relationship between:

| Input | Output |
|------|------|
| Eye‑tracking behavior | Student engagement state |

Deployment workflow:

1. Eye tracker records gaze behavior
2. ML model predicts engagement state
3. Teacher receives intervention signal

---

# Chapter 2 — Experimental Setup

## Eye Tracking Hardware

Eye movements are recorded using classroom eye trackers such as **Tobii**.

| Parameter | Value |
|-----------|------|
| Sampling Rate | 60 Hz |
| Data Recorded | gaze coordinates, pupil dilation |

Meaning:

60 gaze samples per second

---

## Student Session Design

| Parameter | Value |
|----------|------|
| Session Length | 20 minutes |
| Puzzles per Session | 4 |
| Time per Puzzle | 5 minutes |

---

## Raw Data Generated Per Session

60 samples/sec × 1200 seconds = **72,000 gaze samples**

| Metric | Value |
|------|------|
| Raw gaze samples per student | ~72,000 |

---

# Chapter 3 — Study Scale

| Parameter | Value |
|----------|------|
| Schools | 50 |
| Students per School | 50 |
| Total Students | 2500 |

Each student attempts **4 puzzles**.

2500 students × 4 puzzles = **10,000 ML samples**

| Metric | Value |
|------|------|
| Training Samples | 10,000 |
| Features per Sample | 5 |
| Classes | 2 |

---

# Chapter 4 — Raw Eye‑Tracking Dataset

Eye tracking produces **high‑frequency time‑series data**.

Example dataset:

| student_id | timestamp | gaze_x | gaze_y | pupil_size | AOI |
|-----------|-----------|-------|-------|------------|-----|
| S01 | 0 ms | 421 | 312 | 3.2 | puzzle_board |
| S01 | 16 ms | 425 | 315 | 3.3 | puzzle_board |
| S01 | 32 ms | 430 | 318 | 3.2 | piece_panel |

AOI = **Area of Interest**

| AOI | Description |
|----|----|
| puzzle_board | puzzle solving region |
| piece_panel | available pieces |
| controls | UI controls |

---

# Chapter 5 — Data Preprocessing

Raw gaze data contains noise and requires preprocessing.

## Data Cleaning

Remove:

- blink artifacts
- missing gaze points
- out‑of‑screen coordinates

## Signal Smoothing

Noise reduction using:

- moving average
- Savitzky–Golay filter

## Fixation & Saccade Detection

| Event | Meaning |
|------|------|
| Fixation | eye stable on a point |
| Saccade | rapid eye movement |

---

# Chapter 6 — Feature Extraction

Each **5‑minute puzzle attempt becomes one ML sample**.

Selected gaze features:

| Feature | Interpretation |
|--------|---------------|
| Mean Fixation Duration | cognitive processing effort |
| Fixation Count | attention level |
| Mean Saccade Amplitude | visual search behavior |
| AOI Transition Count | gaze exploration |
| Mean Pupil Dilation | cognitive load |

Example feature vector:

[fixation_mean, fixation_count, saccade_amplitude, AOI_transitions, pupil_mean]

---

# Chapter 7 — Label Generation

Labels are derived **from pentomino puzzle interaction logs**, not eye‑tracking data.

This avoids **label leakage**.

Signals used:

| Metric | Meaning |
|------|------|
| puzzle solved | successful attempt |
| move count | engagement |
| idle time | disengagement |
| repeated invalid placements | confusion |

Example rule:

If idle_time > threshold OR move_count < threshold → **needs_help**  
Else → **doing_fine**

---

# Chapter 8 — Machine Learning Models

## Baseline Model

**Support Vector Machine (SVM)**

Reasons:

- strong performance on medium‑size datasets
- robust with small feature sets
- common in gaze analytics research

Kernel: **RBF Kernel**

---

## Advanced Model

**Deep Multilayer Perceptron (MLP)**

Architecture:

Input (5 features)  
↓  
Dense Layer (32 neurons)  
↓  
Dense Layer (16 neurons)  
↓  
Output Layer (binary classification)

---

# Chapter 9 — Training Strategy

| Method | Purpose |
|------|------|
| Train / Validation / Test Split | evaluation |
| Cross‑validation | stable SVM training |
| L2 Regularization | reduce overfitting |
| Dropout | neural network generalization |
| Early Stopping | prevent overtraining |

Typical split:

- 70% training  
- 15% validation  
- 15% test  

---

# Chapter 10 — System Architecture

Full ML pipeline:

Eye Tracker (60 Hz)  
↓  
Raw gaze stream (~72k samples)  
↓  
Data cleaning  
↓  
Fixation / saccade detection  
↓  
Feature extraction  
↓  
Feature dataset (~10k samples)  
↓  
SVM baseline model  
↓  
MLP model  
↓  
Prediction: **Doing Fine / Needs Help**

---

# Deployment Concept

Eye tracker records gaze  
↓  
Model predicts engagement state  
↓  
Teacher receives alert  
↓  
Teacher intervention

Key value:

The system predicts **student struggle before the final puzzle outcome is known**, enabling proactive classroom intervention.

---

# Technologies Used

| Category | Tools |
|------|------|
| Programming | Python |
| Data Processing | Pandas |
| Machine Learning | Scikit‑learn, PyTorch |
| Model Serving | FastAPI |
| Data Storage | SQL |

---

# Future Extensions

- real‑time gaze streaming inference
- temporal sequence models (LSTM / Transformers)
- adaptive tutoring recommendations
- large‑scale classroom learning analytics
