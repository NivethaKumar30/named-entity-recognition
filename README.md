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
```
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from tensorflow.keras.preprocessing import sequence
from sklearn.model_selection import train_test_split
from keras import layers
from keras.models import Model

data = pd.read_csv("ner_dataset.csv", encoding="latin1")
data.head(50)

data = data.fillna(method="ffill")
data.head(50)
print("Unique words in corpus:", data['Word'].nunique())
print("Unique tags in corpus:", data['Tag'].nunique())

words=list(data['Word'].unique())
words.append("ENDPAD")
tags=list(data['Tag'].unique())

print("Unique tags are:", tags)

num_words = len(words)
num_tags = len(tags)
num_words
```
```
class SentenceGetter(object):
    def __init__(self, data):
        self.n_sent = 1
        self.data = data
        self.empty = False
        agg_func = lambda s: [(w, p, t) for w, p, t in zip(s["Word"].values.tolist(),
                                                           s["POS"].values.tolist(),
                                                           s["Tag"].values.tolist())]
        self.grouped = self.data.groupby("Sentence #").apply(agg_func)
        self.sentences = [s for s in self.grouped]

    def get_next(self):
        try:
            s = self.grouped["Sentence: {}".format(self.n_sent)]
            self.n_sent += 1
            return s
        except:
            return None

getter = SentenceGetter(data)
sentences = getter.sentences
len(sentences)
sentences[0]

word2idx = {w: i + 1 for i, w in enumerate(words)}
tag2idx = {t: i for i, t in enumerate(tags)}
word2idx

plt.hist([len(s) for s in sentences], bins=50)
plt.show()

X1 = [[word2idx[w[0]] for w in s] for s in sentences]
type(X1[0])
X1[0]
max_len = 50nums = [[1], [2, 3], [4, 5, 6]]
sequence.pad_sequences(nums)

nums = [[1], [2, 3], [4, 5, 6]]
sequence.pad_sequences(nums,maxlen=2)

X = sequence.pad_sequences(maxlen=max_len,
                  sequences=X1, padding="post",
                  value=num_words-1)
X[0]
y1 = [[tag2idx[w[2]] for w in s] for s in sentences]
y = sequence.pad_sequences(maxlen=max_len,
                  sequences=y1,
                  padding="post",
                  value=tag2idx["O"])

X_train, X_test, y_train, y_test = train_test_split(X, y,test_size=0.2, random_state=1)
X_train[0]
y_train[0]

input_word = layers.Input(shape=(max_len,))

embedding_layer = layers.Embedding(input_dim = num_words,output_dim = 50,input_length = max_len)(input_word)
dropout_layer = layers.SpatialDropout1D(0.1)(embedding_layer)
bidirectional_lstm = layers.Bidirectional(layers.LSTM(units=100, return_sequences=True,recurrent_dropout=0.1))(dropout_layer)
output = layers.TimeDistributed(layers.Dense(num_tags, activation="softmax"))(bidirectional_lstm)

model = Model(input_word, output)
model.summary()

model.compile(optimizer="adam",loss="sparse_categorical_crossentropy",metrics=["accuracy"])

history = model.fit(
    x=X_train,
    y=y_train,
    validation_data=(X_test,y_test),
    batch_size=32,
    epochs=3,
)

metrics = pd.DataFrame(model.history.history)
metrics.head()

print("NIVETHA.K 212222230102")
metrics[['accuracy','val_accuracy']].plot()

print("NIVETHA.K 212222230102")
metrics[['loss','val_loss']].plot()

i = 20
p = model.predict(np.array([X_test[i]]))
p = np.argmax(p, axis=-1)
y_true = y_test[i]
print("{:15}{:5}\t {}\n".format("Word", "True", "Pred"))
print("-" *30)
for w, true, pred in zip(X_test[i], y_true, p[0]):
    print("{:15}{}\t{}".format(words[w-1], tags[true], tags[pred]))
```
## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

![Screenshot 2024-04-22 011728](https://github.com/NivethaKumar30/named-entity-recognition/assets/119559844/4bbb6e7c-83dc-4b44-9d7b-7faafd73b666)


![Screenshot 2024-04-22 011734](https://github.com/NivethaKumar30/named-entity-recognition/assets/119559844/8808d3e5-0a80-43b6-90fc-9d049f1cea1a)


![Screenshot 2024-04-22 011752](https://github.com/NivethaKumar30/named-entity-recognition/assets/119559844/e950165e-6977-449a-b172-961d3d5e2107)


### Sample Text Prediction


## RESULT

Thus an LSTM-based model for recognizing the named entities in the text is developed .
