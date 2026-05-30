# FCPR: Failure-aware Fuzzy Closed-loop Prompt Refinement for Promptable Medical Foundation Segmentation

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)]()
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-orange)]()
[![License](https://img.shields.io/badge/License-MIT-green)]()
[![Paper](https://img.shields.io/badge/Paper-Preprint-red)]()

Official implementation of **FCPR: Failure-aware Fuzzy Closed-loop Prompt Refinement for Promptable Medical Foundation Segmentation**.

FCPR is a failure-aware fuzzy closed-loop prompt refinement framework for promptable medical image segmentation. It addresses the reliability problem of MedSAM under inaccurate box prompts by converting prompt-induced segmentation failures into an iterative fuzzy control process.

Instead of treating prompt generation as a one-shot localization problem, FCPR refines box prompts through prediction feedback, failure-aware descriptors, Gaussian membership functions, fuzzy rule activation, TSK-style consequents, bounded box updates, and an adaptive stop decision.

---

## Highlights

- Failure-aware fuzzy closed-loop prompt refinement for promptable medical segmentation.
- Frozen MedSAM is used as the controlled segmentation backend.
- A lightweight Prompt Generation Module provides the initial box prompt.
- The Fuzzy Prompt Controller refines the box prompt through iterative prediction feedback.
- The controller uses failure-aware descriptors, fuzzy membership functions, rule activation, TSK consequents, and adaptive stopping.
- Optional FCPR-LoRA is provided to examine compatibility with lightweight decoder adaptation.
- Evaluated on five polyp segmentation benchmarks and a supplementary Prostate158 MRI setting.

---

## Framework Overview

FCPR consists of three main components:

1. **Prompt Generation Module (PGM)**  
   Generates an initial box prompt from a coarse localization map.

2. **Frozen MedSAM Backend**  
   Uses the MedSAM image encoder, prompt encoder, and mask decoder as the controlled segmentation backend.

3. **Fuzzy Prompt Controller (FPC)**  
   Observes the current prediction, current box, and refinement history, then outputs:
   - a bounded box update;
   - a stop probability;
   - interpretable fuzzy rule activations.

The closed-loop refinement process is:

```text
Image → Coarse Localization → Initial Box Prompt
      → MedSAM Prediction
      → Failure-aware Descriptor Extraction
      → Gaussian Membership Functions
      → Fuzzy Rule Inference
      → TSK-style Box Update + Stop Decision
      → Refined Prediction
