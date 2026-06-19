# DSPy SST-2 Sentiment Classification Experiment

This repository contains the implementation for the scientific work **Auto-Optimize LLMs: Evaluating a DSPy-Based Pipeline for Systematic Prompt Optimization on a Selected NLP Task**.

The experiment compares a manually designed baseline prompt with DSPy-based sentiment classification approaches on the SST-2 sentiment classification task from the GLUE benchmark. The language models are executed locally through Ollama.

## Overview

The final experiment evaluates three approaches:

1. **Manual baseline prompt**
2. **DSPy unoptimized program**
3. **DSPy optimized balanced program** using `BootstrapFewShot` and balanced optimization examples

The final experiment uses three local Ollama models:

```text
llama3.2:3b
gemma3:4b
phi4-mini:3.8b
```

The main evaluation uses a fixed subset of **100 SST-2 validation examples**. The evaluation subset is selected with a fixed random seed to support reproducibility.

## Model Selection Note

During development, additional local models were considered and tested. The model `qwen3:4b` was tested, but it produced reasoning-heavy outputs and timeout problems in the local Ollama setup. Gemma 4 variants were also considered, but they were too heavy or unstable for the available hardware. Therefore, the final evaluation uses the three models listed above.

## Repository Structure

```text
implementation/
├── baseline_vs_dspy_sst2.ipynb
├── requirements.txt
├── README.md
├── .gitignore
└── results/
    └── final_100/
        ├── accuracy_all_models_labeled.png
        ├── classification_reports_all_models.csv
        ├── experiment_config.csv
        ├── summary_all_models.csv
        ├── llama3_2_3b/
        │   ├── comparison_results.csv
        │   ├── dspy_optimized_balanced_results.csv
        │   ├── dspy_unoptimized_results.csv
        │   └── manual_baseline_results.csv
        ├── gemma3_4b/
        │   ├── comparison_results.csv
        │   ├── dspy_optimized_balanced_results.csv
        │   ├── dspy_unoptimized_results.csv
        │   └── manual_baseline_results.csv
        └── phi4-mini_3_8b/
            ├── comparison_results.csv
            ├── dspy_optimized_balanced_results.csv
            ├── dspy_unoptimized_results.csv
            └── manual_baseline_results.csv
```

## Setup and Installation

This project was implemented with Python and a local language model runtime. The experiment uses Ollama and does not require a paid cloud API key.

### 1. Install Ollama

Download and install Ollama from:

```text
https://ollama.com/download
```

After installation, open PowerShell and download the required models:

```powershell
ollama pull llama3.2:3b
ollama pull gemma3:4b
ollama pull phi4-mini:3.8b
```

To check whether the models are available, run:

```powershell
ollama list
```

To test one model manually, run:

```powershell
ollama run llama3.2:3b
```

Then enter a short test prompt:

```text
Classify the sentiment as positive or negative: This movie was wonderful. Return only one word.
```

The model should return a positive sentiment answer.

### 2. Create a Python Virtual Environment

In the project folder, create a local virtual environment:

```powershell
python -m venv .venv
```

Activate the environment:

```powershell
.\.venv\Scripts\activate
```

If PowerShell blocks activation, run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\activate
```

### 3. Install Required Python Packages

Install the dependencies from `requirements.txt`:

```powershell
pip install -r requirements.txt
```

The main packages used in this project are:

```text
dspy
datasets
pandas
scikit-learn
matplotlib
jupyter
ipykernel
```

### 4. Start the Notebook

Open the project folder in Visual Studio Code and open:

```text
baseline_vs_dspy_sst2.ipynb
```

Select the Python kernel from the local `.venv` environment. Then run the notebook cells from top to bottom.

The main experiment cell evaluates:

```text
3 models × 3 approaches × 100 examples
```

Therefore, the final evaluation can take a long time on local hardware.

## Result Files

The final experiment stores its result files in:

```text
results/final_100/
```

The main result summary is stored in:

```text
results/final_100/summary_all_models.csv
```

The classification reports are stored in:

```text
results/final_100/classification_reports_all_models.csv
```

The experiment configuration is stored in:

```text
results/final_100/experiment_config.csv
```

The final accuracy chart is stored in:

```text
results/final_100/accuracy_all_models_labeled.png
```

For each model, the repository also stores separate CSV files for the manual baseline, the unoptimized DSPy program, the optimized balanced DSPy program, and a combined comparison table.

## Main Result

The final 100-example evaluation produced the following accuracy values:

| Model          | Approach                | Accuracy | Number of examples |
| -------------- | ----------------------- | -------: | -----------------: |
| llama3.2:3b    | Manual baseline         |     0.78 |                100 |
| llama3.2:3b    | DSPy unoptimized        |     0.87 |                100 |
| llama3.2:3b    | DSPy optimized balanced |     0.71 |                100 |
| gemma3:4b      | Manual baseline         |     0.85 |                100 |
| gemma3:4b      | DSPy unoptimized        |     0.89 |                100 |
| gemma3:4b      | DSPy optimized balanced |     0.88 |                100 |
| phi4-mini:3.8b | Manual baseline         |     0.83 |                100 |
| phi4-mini:3.8b | DSPy unoptimized        |     0.81 |                100 |
| phi4-mini:3.8b | DSPy optimized balanced |     0.90 |                100 |

The results show that the DSPy-based approaches do not improve performance uniformly across all models. The unoptimized DSPy program performs best for `llama3.2:3b` and `gemma3:4b`, while the optimized balanced DSPy program performs best for `phi4-mini:3.8b`. This model-dependent behavior is an important finding of the experiment.

## Runtime Observations

The experiment was executed locally with Ollama. Runtime depends on the selected model, hardware, Ollama runtime behavior, and the number of generated tokens.

The measured runtimes for the final evaluation are included in:

```text
results/final_100/summary_all_models.csv
```

The local runtime setup makes the experiment reproducible without a paid cloud API, but it also limits the number of models and examples that can be evaluated practically.

## Reproducibility Notes

The experiment uses a fixed random seed:

```text
42
```

The dataset subset, optimization examples, and evaluation examples are selected deterministically. However, local language model behavior can still depend on the Ollama version, model quantization, available hardware, and runtime settings.

The `.venv` folder is not included in the repository because it is machine-specific. The environment can be recreated using:

```powershell
pip install -r requirements.txt
```

## Important Note

The local Python virtual environment is intentionally excluded from GitHub. The `.gitignore` file should include:

```text
.venv/
__pycache__/
.ipynb_checkpoints/
*.pyc
.DS_Store
```
