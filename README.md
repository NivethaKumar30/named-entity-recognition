# Named Entity Recognition

## AIM

To develop an LSTM-based model for recognizing the named entities in the text.

## Problem Statement and Dataset

Named Entity Recognition (NER) is pivotal in natural language processing (NLP), involving the identification and classification of entities like persons, organizations, locations, dates, and more within text. The objective is to label each word with its corresponding entity tag. This project aims to develop an LSTM-based model for NER, capable of predicting entity tags for each word in input sentences. Accurate NER facilitates diverse NLP applications, including information extraction, question answering, and sentiment analysis.

Dataset
-> Word: English dictionary words from the sentences.

-> POS (Parts of Speech): POS tags indicating the grammatical category of each word.

-> Tag: Standard named entity recognition tags, including:

ORGANIZATION

PERSON

LOCATION

DATE

TIME

MONEY

PERCENT

FACILITY

GPE (Geo-Political Entity)

## Neural Network Model

Include the neural network model diagram.

## DESIGN STEPS

STEP 1:
Load and preprocess the NER dataset, handling missing values and determining unique words and tags.

STEP 2:
Implement the SentenceGetter class to extract sentence-word-tag tuples from the dataset and visualize sentence length distribution.

STEP 3:
Index words and tags to numerical values, pad sequences to a maximum length, and split the dataset into training and testing sets.

STEP 4:
Define the LSTM model architecture using Keras functional API, including embedding, LSTM, and dense layers.

STEP 5:
Compile, train, and evaluate the model, monitoring training/validation metrics and assessing prediction accuracy on test data.

## PROGRAM

Include your code here

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

Include your plot here

### Sample Text Prediction
Include your sample text prediction here.

## RESULT
