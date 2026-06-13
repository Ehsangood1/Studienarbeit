# DSPy SST-2 Sentiment Classification Experiment

This repository contains the implementation for the thesis project **Auto-Optimize LLMs: Evaluating a DSPy-Based Pipeline for Systematic Prompt Optimization on a Selected NLP Task**.

The experiment compares a manually designed baseline prompt with DSPy-based approaches on the SST-2 sentiment classification task from the GLUE benchmark.

## Overview

The experiment evaluates four approaches:

1. **Manual baseline prompt**
2. **DSPy unoptimized program**
3. **DSPy optimized program** using `BootstrapFewShot`
4. **DSPy optimized balanced program** using balanced optimization examples

The language model is executed locally with **Ollama** using:

```text
llama3.2:3b
```

The main evaluation uses a fixed subset of **30 SST-2 validation examples**. The small evaluation size is intentional because the experiment was executed with a local model.

## Repository structure

```text
implementation/
├── baseline_vs_dspy_sst2.ipynb
├── requirements.txt
├── README.md
├── .gitignore
└── results/
    ├── final_30/
    │   ├── accuracy_results_30_extended_labeled.png
    │   ├── comparison_30_extended.csv
    │   ├── dspy_optimized_balanced_results_30.csv
    │   ├── dspy_optimized_results_30.csv
    │   ├── dspy_unoptimized_results_30.csv
    │   ├── experiment_config.csv
    │   ├── manual_baseline_results_30.csv
    │   ├── summary_results_30.csv
    │   └── summary_results_30_extended.csv
    └── pilot_10/
        ├── dspy_optimized_results_10.csv
        ├── dspy_unoptimized_results_10.csv
        ├── manual_baseline_results_10.csv
        ├── pilot_accuracy_results_10.png
        └── summary_results_10.csv
```

## Setup and Installation

This project was implemented with Python and a local language model runtime. The experiment uses the `llama3.2:3b` model through Ollama and does not require a paid cloud API key.

### 1. Install Ollama

Download and install Ollama from:

```text
https://ollama.com/download
```

After installation, open PowerShell and download the model:

```powershell
ollama pull llama3.2:3b
```

To test whether the model works, run:

```powershell
ollama run llama3.2:3b
```

Then enter a short test prompt, for example:

```text
Classify the sentiment as positive or negative: This movie was wonderful.
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

### 5. Result Files

The experiment stores its result files in:

```text
results/final_30/
```

The pilot test results are stored in:

```text
results/pilot_10/
```

The main result summary is stored in:

```text
results/final_30/summary_results_30_extended.csv
```

The final accuracy chart is stored in:

```text
results/final_30/accuracy_results_30_extended_labeled.png
```

### Important Note

The `.venv` folder is not included in the repository because it is machine-specific. The environment can be recreated using `requirements.txt`.

## Main result

The final 30-example evaluation produced the following accuracy values:

| Approach | Accuracy | Number of examples |
|---|---:|---:|
| Manual baseline prompt | 0.7667 | 30 |
| DSPy unoptimized program | 0.8333 | 30 |
| DSPy optimized program | 0.6667 | 30 |
| DSPy optimized balanced program | 0.7333 | 30 |

The unoptimized DSPy program achieved the highest accuracy in this local setup. The optimized variants did not automatically improve performance, which is discussed as an important finding in the thesis.

## Result files

The main result files are stored in:

```text
results/final_30/
```

The pilot smoke-test results are stored in:

```text
results/pilot_10/
```

## Reproducibility notes

The experiment uses a fixed random seed:

```text
42
```

The model is local, so results may depend on the Ollama version, hardware, and model runtime behavior. The evaluation size is limited because local inference with `llama3.2:3b` can be slow.
