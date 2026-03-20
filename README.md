# csci455-assignment2

# LSTM Code Summarization

This project implements an LSTM-based sequence-to-sequence model for Java code summarization. The model generates short natural-language summaries for Java methods using data mined from GitHub repositories.

## Data Sources and Preprocessing

Training data was constructed by mining public GitHub Java repositories. Java source files were collected and parsed to extract method-level code snippets. When available, nearby documentation comments were used as natural-language summaries.

The dataset preparation pipeline performs the following steps:

- filters non-Java files
- extracts Java method bodies
- pairs methods with nearby documentation comments
- normalizes whitespace in code
- lowercases summaries
- removes duplicates
- splits the dataset into training and validation sets

The resulting processed files are stored in:
````
processed_data/
train_code.txt
train_summary.txt
val_code.txt
val_summary.txt
````
Each line in a code file corresponds to the summary on the same line in the paired summary file.

## Embeddings

The model uses embeddings generated from the pretrained CodeT5 model (`Salesforce/codet5p-220m`). The provided embedding script generates token IDs and an embedding matrix saved in artifact files used by the LSTM model.

## Model

The model is a sequence-to-sequence LSTM architecture:

- an encoder LSTM processes tokenized Java method code
- a decoder LSTM generates summary tokens
- a linear layer projects decoder outputs to the vocabulary

The embedding layer is initialized using the pretrained CodeT5 embedding matrix.

## Installation

Install required Python packages:
````
pip install torch transformers sentencepiece
pip install sacrebleu bert-score sentence-transformers
pip install pandas numpy nltk tqdm
````
If using NLTK (for METEOR):
````
import nltk
nltk.download("wordnet")
````
## Running the Notebook

Run the notebook:
````
assignment-2-LSTM.ipynb
````

The notebook performs the following steps:

1. dataset preparation
2. embedding generation
3. LSTM training
4. validation analysis
5. final evaluation on the held-out test set
6. computation of BLEU, METEOR, BERTScore, and SIDE metrics

## Training Notes
Full training (10 epochs on ~48k samples) was completed prior to submission.

Due to runtime constraints:
- Full training is not rerun by default
- CPU training is significantly slower than GPU

Typical performance:
- GPU (Colab): ~10 minutes per epoch
- CPU (local): 30–90+ minutes per epoch

Evaluation metrics shown in the notebook are from the completed training run.

## Outputs

Generated summaries and evaluation results are saved in:
````
outputs/
test_predictions.csv
````

The notebook also prints all evaluation metrics at the end of execution.

## Test Set

The provided test dataset contains Java code–summary pairs used for final evaluation. This dataset is not used during training or validation and is evaluated only once for final metric reporting.
