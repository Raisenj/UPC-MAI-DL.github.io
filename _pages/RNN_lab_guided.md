---
permalink: /rnn-lab-guided/
---


This page contains the **guided laboratory of the RNN topic** for the Deep Learning course at the Master in Artificial Inteligence of the Universitat Politècnica de Catalunya.

You can download the code and data for this examples from the following
github repository [https://github.com/bejar/DLMAI](https://github.com/bejar/DLMAI)


## Task 1: Time Series Regression (Wind Speed Prediction)

The goal of this example is to predict the wind speed of a geographical site
given a window of the previous measurements.

The data for this example has been extracted from the NREL
[Integration National Dataset Toolkit](https://www.nrel.gov/grid/wind-toolkit.html)

The NREL dataset includes metereological information for more than 126,000 sites
across the USA for the years 2007-2013 every 5 minutes. 

The dataset included with this task has data for 4 sites and includes the
variables **wind speed at 100m**, **air density**, **temperature** and
**air pressure**. The original data is sampled every 5 minutes, to reduce
computational cost the dataset is sampled every 15 minutes.

For this task we are going to use only the **wind speed** variable for one
site. You will use the rest of the data during the autonomous laboratory.

The data and the code are in the `\Wind` directory. The file `Wind.npz`
contains the data matrices for all four sites. The data is in `npz` numpy format,
this means that if you load the data from the file you will have an object that
stores all the data matrices. This object has the attribute `file` that tells
you the name of the matrices. We are going to use the matrix `90-45142`.

The code of the example is in the `WindPrediction.py` file. This code reads the
matrix of the first site and splits the data in a training set with 200,000 data
points and a test set with the rest.

Only the first variable (**wind speed**) is used. The data matrix is obtained
generating windows of size `lag`+1 moving the window one step at a time. The first
$lag$ columns of the matrix are the input attributes and the value to predict is
the last one.

Because the recurrent layer needs its input as a 3-D matrix, the data matrix is
transformed from 2-D to 3-D to obtain the shape (examples, sequence, attributes),
in this case the sequence has size $lag$ and attributes has size $1$.

The architecture for this task is composed by:

* A first RNN layer with input size the length of the training window with an
 attribute per element in the sequence.
* Optionally several stacked RNN layers
* A dense layer with one neuron with linear activation, that is the one that
computes the regression as output.

The optimizer used is `RMSprop` (Keras documentation recommends it for for RNN),
the loss function is the mean square error (MSE).

Elements to play with:

* The size of the windows
* The type of RNN (LSTM, GRU, SimpleRNN)
* The dropout
* The number of layers
* Batch size
* Number of epochs
* Use an adaptive optimizer like `adagrad` or `adam`

- - - -

## Task 2: Time Series Classification (Electric Devices classification )

The goal of this task is to classify a set of time series corresponding to
the daily power consumption of household devices to one of seven categories.

The data has been _borrowed_ from the
[UCR Time Series Classification Archive](http://www.cs.ucr.edu/~eamonn/time_series_data/)
(ElectricDevices dataset)

Each example has 96 attributes corresponding to a measure of the power consumption
of a household device every 15 minutes for a whole day. The classes correspond to
computer/television, dishwasher, fridge, heater, kettle, oven and washing machine.

All the details can be found in the paper:

> Lines, J.; Bagnall, A.; Caiger-Smith, P., Anderson, S.
**Classification of Household Devices by Electricity Usage
Profiles**, Intelligent Data Engineering and Automated Learning, IDEAL 2011:
 12th International Conference, Norwich, UK,
September 7-9, 2011. Proceedings, Springer Berlin Heidelberg,
2011, 403-412

The data and the code are in the `\Electric` directory. There are two datafiles:

* `ElectricDevices_TRAIN.csv`
* `ElectricDevices_TEST.csv`

The code of the example is in the `ElectricClass.py` file. The code loads the
 training and test sets and trains the RNN.

The architecture for this task is composed by:

* A first RNN layer with input size the length of the sequence (96) with an
 attribute per element in the sequence.
* Optionally several stacked RNN layers
* A dense layer with softmax activation of size the number of classes (7)

The optimizer used is `SDG`,
the loss function is the categorical crossentropy.

Elements to play with:

* The type of RNN (LSTM, GRU, SimpleRNN)
* The dropout
* The number of layers
* Batch size
* Number of epochs
* Use other optimizer like `RMSprop`, `adagrad` or `adam`

- - - -

## Task 3: Sequence Classification (Twitter sentiment analysis)

The goal of this example is to classifiy the sentiment of a tweet according to
if it is positive, negative or neutral.

The data and the code are in the `\Sentiment` directory.

There are two datasets `Airlines.csv` and `Presidential.csv`. The first one
contains tweets about US airlines and the second are tweets about the
2016 US Republican party presidential candidates debate. Both have three classes
(positive, negative and neutral)

The code is in `Sentiment.py`. This code reads the tweets file and generates
a vocabulary with all the different words that appear, everything that does not
correspond with letters a-z is discarded.

The size of the vocabulary can be controled with the `numwords` variable. Words
are recoded as numbers and the tweets are recoded accordingly maintaining the
order of the words in the tweets. The tweets are transformed to a matrix with
the length of the longest tweet as the number of columns, so shorter tweets are
padded with zeros.

The architecture for this task is composed by:

* A first `Embedding` layer that transforms the vectors corresponding to the
tweets to an embedded space
* At least one RNN layer
* A dense layer with softmax activation of size three (the number of classes)

The optimizer used is `SDG`,
the loss function is the categorical crossentropy.


Elements to play with:

* The number of words in the vocabulary of the tweets
* The size of the embedding
* The type of RNN (LSTM, GRU, SimpleRNN)
* The dropout
* The number of layers
* Batch size
* Number of epochs
* Use other optimizer like `RMSprop`, `adagrad` or `adam`

- - - -

## Task 4: Sequence Classification (Text Generation)

This example has been _borrowed_ from the Keras examples.

The main idea of this example is that we can generate text basically by
predicting for a sequence of letters the next most probable letter.

This is a simplification of a more computationally expensive problem that would
be to predict the next word of a sequence of words. The good thing about this
approach is that the number of classes to predict is just the number of characters
in the text, instead of a very large vocabulary.

The data used for the example corresponds to poetry selected from different
English authors. The main reason for choosing poetry instead of narrative is
mainly that poetry has a more relaxed grammar, sentences are shorter and usually
the main topic of the text is expressed in a limited number of words.

For this example four datasets of different length have been generated
(poetry1.txt to poetry4.txt). Each dataset is included in the next one.

The data and the code are in the `\TextGeneration` directory.

The data is transformed to a classification task assuming
that the number of classes is the number of unique characters in the text and the
attributes are windows of the text of a specific length.

The code is in `TextGenerator.py`. This code reads the data file (by default
`poetry4.txt.gz`) and generates
a training set by sequentially extracting windows of size `maxlen` moving
the window `step` number of characters every iteration. For each window the class
correspond to the next character.

The attributes of the sequences and the classes are transformed to a one-hot
encoding of the size the number of different characters in the text.

After training the model, the text is generated providing a random seed of size
`maxlen` to the model to obtain the prediction, the prediction is added to the
seed and the leftmost character is discarded. This is repeated several times
to obtain the text.

 The prediction of the next character is obtained as a probability distribution,
 this distribution is not used directly but is sampled to be able to obtain
 more or less diversity from the text, rebalancing the probabilities obtained from the
 model (see function `sample`).

The architecture for this task is composed by:

* A first RNN layer with input size the length of the training window with as
 many attributes per element in the sequence as unique characters.
* Optionally several stacked RNN layers
* A dense layer with softmax activation of size the number of characters

The optimizer used is `adam`,
the loss function is the categorical crossentropy.

Elements to play with:

* The length of the window and the step used to move the window
* The size of the input file (use for example the small data file)
* The type of RNN (LSTM, GRU, SimpleRNN)
* The dropout
* The number of layers
* Batch size
* Number of epochs
* Use other optimizer like `SGD`, `RMSprop` or `adagrad`

There is no good way for validating the results of this task, we can judge
subjectively the quality of the generated poetry, but judging the quality
of poetry is a hairy matter.

We can look for more objective things like the variety of the vocabulary, the
distribution of the words or the number of incorrect words, for example.

- - - -

## Task 5: Sequence to sequence (Addition)

This example has also been _borrowed_ from the Keras examples.

The idea is to show a simple example that learns associations between sequences.

The input sequences correspond to additions that have a maximum length assuming
that the figures added have a limited number of digits, for example `1234+54`.
The output sequences are the correct answer to the addition.

The main advantage of this taske is that it is very easy to generate examples
for the problem and it is easy to converge to a solution with  more than a 99% of accuracy.

The dataset is generated as one-hot encoding vectors of the sequence representing
the addition, the corresponding sequence (the result) is coded in the same way. There is
a maximum number of digits (variable `DIGITS`) for the numbers to add, so
the sequences have a maximum length, the ones that are shorter are padded at the
end.

The program has also the possibility of reversing the input sequences because
this seems to work well in other similar tasks.


The architecture for this task is the following:

* A first RNN layer (encoder) with input size the size of the longest addition with as
 many attributes per element in the sequence as unique characters.
* The output of this layer is repeated using a `RepeatVector` layer with an output
size of the maximum number of digits  (`DIGITS`) plus one (plus one because for example 2+9=11), 
that is the size of the ouptput sequence
* Optionally several stacked RNN layers (decoder) that return the whole sequence of states
* A time distributed layer that passes all the time slices generated by the RNN for each input to the next layer
* A dense layer with softmax activation of size the number or characters, so we have
a softmax classifier for each element of the sequence.

The optimizer used is `adam`,
the loss function is the categorical crossentropy.

Elements to play with:

* The size of the additions
* The number of examples generated
* The type of RNN (LSTM, GRU, SimpleRNN)
* The dropout
* The number of layers of the decoder
* Batch size
* Number of epochs
* Use other optimizer like `SGD`, `RMSprop` or `adagrad`
