# Neural Network for Story Generation

## Overview
This project implements a neural network model to predict the next words in a sequence, enabling it to generate text that continues an input seed text. The model is trained on a text corpus, tokenized, and converted into numerical sequences for learning. The architecture uses embeddings, LSTMs, and feed-forward layers to predict multiple next words in a sequence.

---

## Steps in the Pipeline

### 1. **Data Preprocessing**
- **Corpus Loading**: The dataset is loaded using Python's `pickle` module.
- **Tokenizer**:
  - A custom tokenizer preprocesses text by adding spaces around punctuation and mapping words to unique indices.
  - The `Tokenizer` class includes methods to preprocess text, fit the tokenizer on a corpus, and convert text to sequences of indices.

### 2. **Dataset and DataLoader**
- **TextDataset**:
  - Converts the tokenized corpus into input-output pairs for training. For each sequence, `n-gram` sequences are created where a portion of the sequence is input, and the subsequent tokens are the target for prediction.
  - The dataset supports multi-word prediction through a `predict_steps` parameter.

- **DataLoader**:
  - Handles batching, shuffling, and padding sequences to ensure that batches can be efficiently processed by the model. A custom `collate_fn` function is used for padding.

### 3. **Model Architecture**
The **NextWordPredictor** model is designed to handle multi-word predictions and consists of the following components:
- **Embedding Layer**:
  - Converts input tokens into dense vector representations of size `embed_size`.
- **LSTM**:
  - A two-layer LSTM processes the input embeddings, capturing temporal dependencies in the sequence.
- **Layer Normalization**:
  - Normalizes the output of the LSTM's final hidden state for improved stability.
- **Feed-Forward Layers**:
  - A series of fully connected layers (optionally with BatchNorm) process the hidden state to generate predictions.
- **Final Linear Layer**:
  - Outputs a tensor of shape `(batch_size, predict_steps, vocab_size)` containing predictions for multiple words.
- **Custom Weight Initialization**:
  - Xavier initialization is used for weights, and biases are initialized to zero for better convergence.

### 4. **Loss Function**
A custom loss function (`multi_word_loss`) computes the average cross-entropy loss across the predicted steps.

### 5. **Inference**
- The model generates text by recursively predicting the next tokens for a given seed text.
- Predictions are translated back to words using the tokenizer's `index_word` dictionary.

### 6. **Training**
- The training loop uses the Adam optimizer with a learning rate of `0.0001` and trains the model over multiple epochs.
- The average loss is logged for each epoch.
- Periodically, the model's performance is tested by generating text from sample seed phrases.

---

## Usage

### **Training the Model**
Run the provided script to:
1. Load the dataset.
2. Train the model on the dataset.
3. Periodically save checkpoints and generate text predictions.

### **Generating Text**
After training, you can generate new stories by providing a seed text to the `predict_sequence` function.

---

## Features
- Handles multi-word predictions.
- Customizable architecture:
  - Embedding size, LSTM size, feed-forward layers, and more can be adjusted.
- Flexible tokenizer with preprocessed text.
- Trains efficiently using `DataLoader` with padding support.

---

## Dependencies
- Python 3.8+
- PyTorch
- tqdm
- scikit-learn
- numpy
- Anaconda (recommended for managing the environment)

---

## YAML File: Anaconda Environment Configuration

Save the following YAML file as `environment.yml`, and run `conda env create -f environment.yml` to create the environment.

```yaml
name: story_gen
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.8
  - pytorch=1.10
  - torchvision
  - torchaudio
  - pytorch-cuda=11.3
  - tqdm
  - scikit-learn
  - numpy
  - pyyaml
  - pip
  - pip:
      - wikipedia-api
```

---

### **Command to Activate the Environment**
```bash
conda activate story_gen
```

### **Run the Script**
Once the environment is activated, you can run the script:

```bash
python your_script_name.py
```

---


