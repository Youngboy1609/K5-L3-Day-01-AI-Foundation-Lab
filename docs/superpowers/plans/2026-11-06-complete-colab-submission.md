# Complete Colab Submission Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Complete the student's assigned Embedding challenge and final submission artifacts directly in the existing notebook and report directory, while preserving Colab `Run all` compatibility.

**Architecture:** Keep the existing notebook pipeline unchanged through the generated challenge selection. Add only the assigned challenge solution and written reflection content to `report/individual/day01_lab.ipynb`, then synchronize the evidence checklist and report narrative in the student's Markdown file. Generate `lab1_report.json` only from the notebook's existing report-generation path so its schema remains compatible with the lab rubric.

**Tech Stack:** Jupyter Notebook JSON, Python cells, NumPy/PyTorch/SentenceTransformers, Markdown, PowerShell validation.

---

### Task 1: Inspect the assigned challenge interfaces

**Files:**
- Read: `report/individual/day01_lab.ipynb`
- Read: `src/README.md`
- Read: `RUBRIC.md`

- [ ] **Step 1: Identify the existing model variables and helper signatures**

  Confirm the notebook exposes the trained BoW model, embedding classifier, prediction helpers, `MY_CHALLENGE`, `SEED`, and final report export cell before inserting challenge code.

- [ ] **Step 2: Identify the final report schema**

  Locate the existing `lab1_report.json` export cell and preserve its keys (`seed`, `checks`, `predictions`, and `results`) rather than creating a parallel schema.

- [ ] **Step 3: Validate the current notebook JSON**

  Run:

  ```powershell
  Get-Content -LiteralPath 'report\individual\day01_lab.ipynb' -Raw | ConvertFrom-Json | Out-Null
  ```

  Expected: exit code `0`.

### Task 2: Complete the assigned Embedding challenge in the notebook

**Files:**
- Modify: `report/individual/day01_lab.ipynb` at the challenge code cell and challenge answer Markdown cell

- [ ] **Step 1: Add four deterministic challenge messages**

  Use two natural spam messages that avoid obvious `free`/`win` keywords and two ham messages containing `free` or `win`. Keep labels and explanations in a small list so the output is reproducible and easy to discuss.

- [ ] **Step 2: Run both prediction paths**

  Call the notebook's existing BoW prediction helper and Embedding + Logistic Regression helper for each message. Print a table containing message, expected label, both predictions, and a short explanation. Do not introduce a second model implementation or `sklearn` dependency.

- [ ] **Step 3: Add the written answer**

  Explain each mismatch or agreement, specifically noting BoW keyword sensitivity and embedding semantic generalization. Keep claims tied to the printed predictions.

- [ ] **Step 4: Add the final reflection answers**

  Fill the three required product/reflection prompts using the notebook's measured F1, latency, throughput, and cost values. Describe a router using the existing probability/uncertainty outputs and record AI assistance honestly in the existing AI-log table.

### Task 3: Regenerate and verify submission artifacts

**Files:**
- Modify: `report/individual/2A202603729_HoTrongNghia.md`
- Create or modify: `report/individual/lab1_report.json`
- Verify: `report/individual/day01_lab.ipynb`

- [ ] **Step 1: Run the notebook from the first cell through the final export cell in Colab**

  Use Google Colab's `Runtime -> Run all`, provide `STUDENT_ID=2A202603729`, and configure any required package installation or Colab Secret without putting credentials in the notebook.

- [ ] **Step 2: Save the executed notebook**

  Download/replace `report/individual/day01_lab.ipynb` with the executed notebook so outputs remain present.

- [ ] **Step 3: Save `lab1_report.json`**

  Download the JSON produced by the notebook's final report cell into `report/individual/lab1_report.json`.

- [ ] **Step 4: Update the personal report**

  Mark the notebook and JSON evidence as present, preserve the existing hand-calculation checkbox until the image is supplied, and copy measured results rather than inventing metrics. Complete the challenge and product sections from the notebook output.

- [ ] **Step 5: Scan for secrets and personal data**

  Run:

  ```powershell
  rg -n -i 'sk-[A-Za-z0-9]{20,}|ghp_[A-Za-z0-9]{20,}|AIza[A-Za-z0-9_-]{20,}|[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' report\individual
  ```

  Expected: no real API keys, email addresses, or phone numbers.

### Task 4: Validate Colab readiness and repository state

**Files:**
- Verify: `report/individual/day01_lab.ipynb`
- Verify: `report/individual/lab1_report.json`
- Verify: `report/individual/2A202603729_HoTrongNghia.md`

- [ ] **Step 1: Validate notebook structure and outputs**

  Confirm valid notebook JSON, at least one code cell with output, and no traceback output in executed cells.

- [ ] **Step 2: Validate JSON report keys**

  Confirm `seed`, `checks`, `predictions`, and `results` exist and the report parses as JSON.

- [ ] **Step 3: Check the required submission directory**

  Confirm the directory contains the renamed report, executed notebook, JSON report, and either the hand-calculation image or an explicitly marked missing item.

- [ ] **Step 4: Review the final diff**

  Run:

  ```powershell
  git status --short
  git diff --stat
  ```

  Expected: only the intended notebook/report artifacts and this plan are changed.
