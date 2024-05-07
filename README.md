# Multiple Choice Question Answering Artifacts

This repository is the official implementation of the in-progress paper: "Artifacts or Abduction: How Do LLMs Answer Multiple-Choice Questions Without the Question?", which can be viewed on Arxiv [here](https://arxiv.org/abs/2402.12483).

<p align="center">
  <img src="/images/figure.png"></img>
</p>

## Overview

This repository contains the code and dataset to run our experiments related to choices-only LLMs in MCQA, memorization, individual priors and choices dynamics, and the meta-strategy of abductive question inference

## Setup

Python 3.10.0, pip 23.2.1, and conda 23.5.0 were used when running the code in this repository. A list of requirements can be found in `requirements.txt`, which can be installed through the following command:
```
pip install -r requirements.txt 
```

The most important files in this repository are as follows:
* `/model/`: Contains the code for running all experiments with LLMs
* `/prompts/`: Contains the prompts used in the independent full and indepdent choices-only prompt experiments
* `/evaluation/`: Contains the code for generating our plots
* `/scripts/`: Sample bash scripts to run our code

## Model Usage

There are six relevant files:
* `/model/run_hf.py`: Code to run all possible prompt formats, except the second step of the inferring the question strategy and the random question prompt
* `/model/run_hf_remote.py`: Version of `run_hf.py` that works with models on the huggingface hub where you need to trust remote code (i.e. Phi)
* `/model/extract_generated_questions.py`: After running step one of the inferring the question strategy in the above two Python scripts, this will extract the questions generated by the LLM. This must be run before either of the last two Python scripts
* `/model/extract_random_questions.py`: This will sample a random set of questions from the test set. for the LLM to use. This must be run before either of the last two Python files
* `/model/run_hf_question.py`: Code to run the second step of the inferring the question strategy and the random question prompt
* `/model/run_hf_question_remote.py`: Version of `run_hf_question.py` that works with models on the huggingface hub where you need to trust remote code (i.e. Phi)

Samples bash scripts for `run_hf.py`, `run_hf_remote.py`, `run_hf_question.py` and `run_hf_question_remote.py` can be found in the `/scripts/` folder. All of these files require the following arguments, which are detailed in the bash scripts:
- `model_name`: Nickname of the model
- `model_name_hf`: Huggingface model name
- `experiments`: List of experiments to run, identified by ProomptType enum
- `datasets`: List of datasets to run, identified by DatasetName enum
- `partition`: Partition of the dataset to run (e.g. `full`, `first_half`)
- `hf_token`:  huggingface token (for downloading gated models
- `load_in_8bit`: load the model in 8bit? ("False" or "True")
- `load_in_4bit`: load the model in 4bit? ("False" or "True")
- `use_20_fewshot`: use a 20-shot prompt? (only applies to ARC, as it it normally 25-shot)
- `res_dir`: Directory pointing to the results folder
- `prompt_dir`: Directory pointing to the prompt folder
- `cache_dir`: Directory pointing to the cache folder for saving the downloaded models

In addition, `run_hf_question.py` and `run_hf_question_remote.py` have the extra parameter:
- `use_random_question`: Should the two-step question in "Inferring the Question" be a randomly-generated question or a model-generated question? ("True or "False")

To run `extract_generated_questions.py` and `extract_random_questions.py`, it is best to specify the following parameters at the top of the file:
- `res_dir`: Directory pointing to the results folder
- `DATASETS`: List of datasets to extract the questions from
- `MODELS`: Models to extract the questions from

## Evaluation Usage

There are two relevant files:
* `/evaluation/plot_accuracy.py`: Create bar charts for all models except the independent classification prompts
* `/evaluation/plot_accuracy_individual.py`: Create bar charts for only the independent classification prompts

The files use the following parameters specified at the top of the file:
- `res_prefix`: Directory pointing to the results folder
- `out_dir`: Output directory to save the plot

The file `plot_accuracy.py` also has the following parameter:
- `EXPERIMENTS`: List of experiments to show in the bar chart
