# When Does Latent English Appear?

This repository contains the code and data for the seminar paper:

**When Does Latent English Appear? Lexical Category Effects in Qwen2.5 3B**

The project studies whether English readable intermediate predictions in a multilingual language model vary across lexical categories. It uses the logit lens to compare target language token probabilities and English approximation token probabilities across layers in Qwen2.5 3B.

## Project overview

The experiment uses 54 cloze prompts in Chinese, German, and French. Each language contains 18 items, divided into three lexical categories:

* concrete words
* abstract words
* culture specific concepts

For each item, the model is evaluated at the position immediately before the target word. The hidden state from each layer is projected through the language modeling head. This gives a layerwise next token distribution. The analysis then compares the probability of the target language token set with the probability of an English approximation token set.

The main dependent measure is English dominance area, defined as the positive layerwise difference between English approximation probability and target language probability, summed across layers.

## Main analyses

The notebook performs the following analyses:

* tokenization check for target and English approximation terms
* layerwise logit lens extraction for Qwen2.5 3B
* English dominance area calculation
* category summaries for concrete, abstract, and culture specific items
* metric sensitivity analysis using sum, mean, and max English approximation metrics
* language stratified bootstrap contrasts
* language specific category summaries
* regression checks with category and language predictors
* figures and tables used in the paper and appendix

## Repository contents

Recommended file structure:

```text
README.md
requirements.txt
experiments.ipynb
lexical_items.xlsx
3b
```

The notebook reads the input file:

```text
lexical_items.xlsx
```

The main output folder is:

```text
3b
```

The output folder contains result tables, bootstrap summaries, regression checks, and figures used in the paper.

## Input data

The input file contains the lexical items used in the experiment. Each item includes a target language prompt, a target word, lexical category information, language information, and English approximation terms.

The prompts contain no English text. This ensures that English tokens found in the layerwise predictions come from the model readout rather than from the input prompt.

## Model

The experiment uses Qwen2.5 3B as the main model. The model is loaded through the Hugging Face Transformers library during execution. Model weights are obtained through the normal model loading process and are excluded from this repository.

## How to run

Install the required packages:

```bash
pip install -r requirements.txt
```

Open the notebook:

```text
experiments.ipynb
```

Update the root directory in the setup cell if needed:

```python
ROOT_DIR = "/content/drive/MyDrive/logit"
```

Then run all notebook cells from top to bottom. The notebook will load the data, run the logit lens analysis, save the result tables, and generate the figures.

## Main output files

The notebook generates the following types of output:

```text
layerwise_results_qwen25_3b.csv
metrics_by_item_qwen25_3b.csv
metrics_by_item_long_qwen25_3b.csv
summary_by_category_qwen25_3b.csv
summary_by_category_language_qwen25_3b.csv
bootstrap_category_differences_qwen25_3b.csv
bootstrap_category_differences_by_language_qwen25_3b.csv
metric_sensitivity_summary_qwen25_3b.csv
metric_sensitivity_by_language_qwen25_3b.csv
regression_checks_qwen25_3b.csv
results_workbook_qwen25_3b.xlsx
```

It also generates the figures used in the paper:

```text
figure1_layerwise_probabilities_qwen25_3b.png
figure2_metric_sensitivity_qwen25_3b.png
figure3_language_specific_sum_qwen25_3b.png
appendix_language_metric_grid_qwen25_3b.png
figure3_english_dominance_heatmap_qwen25_3b.png
```

## Requirements

The main Python packages are:

```text
torch
transformers
accelerate
pandas
openpyxl
matplotlib
numpy
scipy
statsmodels
tqdm
jupyter
```

## Notes on reproducibility

The notebook is written for Google Colab, but it can also be adapted to a local Python environment. The main path that may need adjustment is `ROOT_DIR`.

The analysis uses first token probabilities for target and English approximation terms. It does not estimate full expression probabilities for multi token expressions. The statistical analysis is exploratory and should be interpreted together with the small item count per language and category.

## Author

Yuqing Li
Seminar: Model Analysis and Interpretability for Natural Language Processing
University of Zurich
Spring term 2026
