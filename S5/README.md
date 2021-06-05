# Sentiment Analysis on Stanford Sentiment Bank

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/sudo-rickroll/END2/blob/main/S5/main.ipynb)

This directory contains the "main.ipynb" notebook prepared for training the Stanford Sentiment Bank for Sentiment Analysis, using the LSTM Model and Data Augmentations such as "Random Augmentations" from EDA and "Back Translation".

## Stanford Sentiment Bank Overview

The Stanford Sentiment Bank contains 11855 sentences. Each sentence is assigned an ID and this ID is mapped to labels ranging from values 1-5, with the following sentiments:
<ul>
  <li>1 - Very Negative</li>
  <li>2 - Negative</li>
  <li>3 - Neutral</li>
  <li>4 - Positive</li>
  <li>5 - Very Positive</li>
</ul>

These 11855 sentences are initially broken down to phrases and these phrases are mapped to IDs. These have labels attached to them, that determine their sentiment. These sentiment labels on individual phrases determine the final sentiment label of the sentence when all these phrases are combined to form sentences.

The <b>pytreebank</b> library (https://pypi.org/project/pytreebank/) contains the sentences, phrases and their corresponding labels in a tree structure, with the sentence and its label present at the top of the tree.

For the purpose of sentiment analysis as part of this directory, only the root sentences have been used and the individual phrases making up the sentences have been discarded. Hence, only the root node from every tree of the pytreebank has been considered. 


## Data Pre-Processing

### Dataset Preparation

After the sentences are fetched from the root node of the trees from the pytreebank, they are read onto a dataframe using the `to_labeled_lines()` method of the pytreebank. This produces a dictionary with three keys, namely - `train`, `test` and `dev`. As the name suggests, `train` is the train set, `test` is the test set and `dev` is the validation set.

The dataframe produces by these three dictionaries are then combined to obtain the complete dataset. This complete dataset is then again split to Train Dataset and Validation Dataset, with the desired proportion of data in each of them. Augmentations are done on the Train Dataset and later, with this augmented Train Set and the original (untouched) Validation Set are then used to build the Dataset objects from the <i>torchtext.legacy.data</i> module. 

### Dataset Augmentation

As mentioned in the <i>Dataset Preparation</i> section, augmentations are performed on the Train Dataset. Here, a certain portion of the Train Dataset is chosen and augmentations are performed on them and the resulting augmented sentences are added to the dataset (original sentences are not replaced by the augmented sentences) to provide invariance and to also add data to the dataset.

For the analysis present in this directory, three types of augmentations are performed :
<ol>
  <li>Random Augmentations:</li>
  <ul>
    <li>Random Swap</li>
    <li>Random Delete</li>
  </ul>
  <li>Back Translation</li>
</ol>

Random Augmentations [[1]](#1)

> ## References
  >><a id="1">[1]</a> 
    Jason Wei and Kai Zou (2019). 
    <a href="https://arxiv.org/abs/1901.11196v2">EDA: Easy Data Augmentation Techniques for Boosting Performance on Text Classification Tasks. </a> 
    arXiv:1901.11196v2 [cs.CL].