# EPU Attention Benchmark: Two-Stage Evaluation of Long-Context Attention in LLMs

This repository contains the code, outputs, and figures for an attention benchmark built on the human-audited Economic Policy Uncertainty (EPU) newspaper dataset. The benchmark studies whether large language models preserve the same main-task judgment when surrounding conditions change, even though the target article stays the same.

The benchmark evaluates four attention challenges:

- **Q1 — Distraction:** Does salient but irrelevant context change the model’s judgment?
- **Q2 — Neutral length stress:** Does performance change as more neutral filler text is added?
- **Q3 — Task-order sensitivity:** Does the order of subtasks in a multi-step prompt affect the main EPU label?
- **Q4 — Buried information:** Can the model still make the correct judgment when the key article is buried in a larger dossier?

## Why this benchmark matters

Many evaluations test models only in clean settings. This benchmark tests whether a model can keep the **same grounded decision rule** when context changes around the same article.

A robust system should not:
- change its judgment because irrelevant but salient text is added,
- drift because more neutral text occupies the context window,
- depend heavily on whether the main task is asked first or last,
- lose judgment quality after the correct evidence has already been retrieved.

This makes the benchmark useful for studying progress toward more stable long-context reasoning.

## Dataset

This benchmark uses part of the human-audited **Economic Policy Uncertainty (EPU) newspaper dataset**, prepared under the supervision of **Scott R. Baker et al.**

Balanced samples are used so the task is not biased toward one class.

- **Stage 1:** approximately **600** balanced rows
- **Stage 2:** approximately **500** balanced rows

The benchmark focuses on the binary **EPU** classification task and uses the original human labels as ground truth.

## Two-stage design

### Stage 1: broad benchmark sweep
Stage 1 provides the first benchmark pass across the four questions. It asks whether model behavior changes under distraction, longer neutral context, task-order changes, and buried information.

### Stage 2: deeper mechanism analysis
Stage 2 strengthens the benchmark by adding more controlled analysis so that failure modes can be separated more clearly.

Examples:
- **Q1:** richer distractor bank, difficulty levels, and article-level flip analysis
- **Q2:** multiple neutral filler families, target-position checks, and truncation diagnostics
- **Q3:** a length-matched single-task control to separate prompt-bulk effects from true order effects
- **Q4:** explicit retrieval diagnostics to separate retrieval from final judgment quality

Stage 2 is therefore not just a rerun. It is a deeper mechanism-focused analysis of the same four attention questions.

## Main findings

Across the four questions, models that look similar in headline accuracy often fail for very different reasons.

- Some models are more vulnerable to **salient distraction**.
- Some are more sensitive to **context-window burden**.
- Some are affected mainly by **task order**.
- Some can **retrieve the correct evidence** but still fail to use it reliably in the final judgment.

A central result of the benchmark is that **attention is not one ability**. Long-context performance depends on stable attention, stable judgment, and the ability to keep relevant evidence central to reasoning under realistic information overload.

## Repository structure

```text
epu-attention-benchmark/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── 01_stage1_q1_distraction.ipynb
│   ├── 02_stage1_q2_neutral_length.ipynb
│   ├── 03_stage1_q3_task_order.ipynb
│   ├── 04_stage1_q4_buried_information.ipynb
│   ├── 05_stage2_q1_distraction_deeper.ipynb
│   ├── 06_stage2_q2_neutral_length_deeper.ipynb
│   ├── 07_stage2_q3_task_order_control.ipynb
│   └── 08_stage2_q4_burial_retrieval_judgment.ipynb
│
├── results/
│   ├── stage1/
│   │   ├── q1/
│   │   ├── q2/
│   │   ├── q3/
│   │   └── q4/
│   └── stage2/
│       ├── q1/
│       ├── q2/
│       ├── q3/
│       └── q4/
│
├── graphs/
│   ├── graph1_two_stage_overview.png
│   ├── graph2_q1_q2_comparative.png
│   ├── graph3_q3_order_control.png
│   └── graph4_q4_retrieval_vs_judgment.png
│
└── docs/
    └── notes.md
