# Eluvio takehome challenge: 

[Google colab link](https://colab.research.google.com/drive/1-D35eqSa7Nh4jZC44Tz6r216FCaaQFXn?usp=sharing)


## Steps

- Understand the data: In this section, I tried to plot the time series only one sample to find out some insights. 

- Try to use some simple classifier to fit the training set, beacuse the first section shows that the similarity_diff might be helpful in the classification task. The problems are:
    1. unbalanced dataset if using this method.
    2. caused a high precision and low recall problem

- Using NN to do classification task. In this section, use three methods.
    1. LSTM
    2. attention_model
    3. CNN 1D
 all of the models are fitting using the original features. However although the precision is performing very well, they are struggling with the low recall problem. I think the reason is that we need to consider the similarity of the continuous shots. However, none of them could make it.

- Using similarity matrix of the features as the training set.
    1. The only one problem in this section is that each of the sim-matrixes has a different shape. If we use this matrix as a sequence-feature, not only the seq_len is changing, but also the feature-dim is changing. So I use SVD to decompose the matrix, so we can make them have the same feature-dim
    2. Build the model using Conv1d and Bi-LSTM. Conv1d is used to extract the feature in some continuous features similarities. LSTM is used to do the classification, because this is literally a time-series problem


## Something about preprocessing
- add a '0' in front of the groud_truth. 
- when padding in a batch, instead using 0-vec to pad, I use the last vector in each features (place, cast, action, audio). The reason is 0 has no meaning, using the last vector means the scene is not changing any more.
- when compute the similarity, some continuous shots' features are all zero-vectors. This will give us nan, so we need to replace nan with 0.

## Something about the model
- while choosing the model, we should consider two things: 
    1. how to get the features of some continuous shots
    2. how to include the time-series features.

    so finally , I choose conv-1d and BI-LSTM
    Self-Attention model is a good attempt, although it is not working very well. The idea of the attention model comes from Transformer's encoder part.
