# Loading-and-Preprocessing-Data-with-TensorFlow
## Data science with tensorflow

Data API: you can create a data-set object, tell it where to get the data, then transform it in any way you want.


TFRecord is a flexible and efficient binary format based on Protocol buffers

Features API:- lets you easily convert features to numerical features that can be consumed by your neural network.

TF Transform:- makes it possible to write a single preprocessing function that can be both in batch mode on your full training set, before training.


TF Datasets:- provides a convenient function to download many common datasets of all kinds, including large ones like ImageNet.



## The Data API

The whole data api revolves around the concept of a dataset: which represents a sequence of data items.


## Chaining Transformation
Once you have a dataset, you can apply all sorts of transformations to it by calling its transformation methods.


## Shuffling the Data

To do this, you can just use the shuffle() method. It will create a new dataset, then whenever it is asked for an item, it will pull one out randomly from the buffer, and replace it with a fresh one from the source dataset, until it has iterated entirely through the source dataset

## Preprocessing the Data

## Putting Everything Together

list_files() -> repeat() -> interleave() -> shuffle() -> map(preprocess()) -> batch() -> prefetch()


## Prefetching

By calling prefetch(1) at the end, we are creating a dataset that will do its best to always be one batch ahead.

In other words, while our training algorithm is working on one batch, the dataset will already be working in parallel on getting the next batch ready.

This can improve the performance dramatically


## The TFRecord Format
This is a tensorflow format for storing large amounts of data and reading binary records of varying sizes (each record just has a length, a CRC checksum for the data).

## Compressed TFRecord Files.

It can sometimes be useful to compress your TFRecord files, especially if they need to be loaded via a network connection. 

## TensorFlow Protobufs

The main protobuf typically used in a TFRecord file is the Example protobuf, which represents one instance in a dataset.

It contains a list of named features, where each feature can either be a list of byte string s, a list of floats or a list of integers.


## Loading and Parsing Examples
To load the serialized Example protobufs, we will use a tf.data.TFRecordDataset once again, and we will parse each Example using tf.io.parse_single_example().

This is a tensorflow operation so it can be included in a TF function.. It requires at least two arguements: a string scalar tensor conatininng the serialized data, and a description of each feature.


## Handling Lists of Lists Using the SequenceExample Protobuf

A sequence conatains a Features object for the contextual data and a FeatureLists object which contains one or more names FeatureList objects.

Each FeatureList just contains a list of Feature objects, each of which may be a list of byte strings, a list of 64-bit integers or a list of floats.

Building a SequenceExamole, serializing it ans parsing it is very similar to building, serialzib=ng and parsing an Example, but you must use tf.io.parse_single_sequence_example() to parse a single SequnceExample ot tf.io.parse_sequence_example() to parse a batch, ans both functions return a tuple containing the contaxt features.,



## The Features API

Preproessing your data can be f=perfored in many ways: it can be done ahead of time when preparing you data fle, using any tool you like.

Or you can preprocess your data on the fly when loading it with the Data API


## Categorical Features

categorical_column_with_identity()

categorical_column_with_vocabulary_list()

categorical_column_with_vocabulary_file()

## Crossed categorical Features

If you suspect that two or more categorical features are more meanigful when used jointly, then you can create a crossed column.



## Encoding Categorical Features Using One-Hot Vectors

A one-hot vector enoding has the size of the vocabulary length, which is fine if tehre are just a few possible categories, but if teh vocablary is large, you will end up with too many inputs fed to your neural network: it will hae too many weghts to learn and it will probably not perform well

As a rule of thumb, if the number of ategories is lower than 10, tehn one-hot encoding is generally the way to go.

If the number of categories is greater than 50, then embeddings are usually preferable.


## Encoding Categorical Features Using Embeddings

An embedding us a trainable dense vector that represents a category.

By default, embedding s are initialized randomly,.

Since these embedding s are trainable, they eill gradually imporve during training, and as they represent fairly similar categories, Gradient Descent will certainly end up pushhing them closer together.

Unfortunately, the ebedding matric can be quite large, especially when you have a large vocabuary: if this is the case, the model can only learn goof representation for the categories for which it has sufficient trainng data.

To reduce the size of the embedding matrix, you can of course try lowering the dimension hyperparamter, but if you reduce this paramter too much, the representation may not be as good.


## TF Transform

You define your preprocessing function just once (in Python), by using the TF Transform functions for scaling, bucketizing, crosiing features, and more.

Importantly, TF Transform will also genearte an equivalent TensorFlow Function that you can plug into the model you deploy.


## The TensorFLow Datasets Project

The TFDS projects makes it trivial to downlaod common datasets, from small ones like MNIST or Fashion MNIST, to huge datasets like ImageNet
