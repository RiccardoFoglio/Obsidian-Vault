Sequence data = data that comes as ordered items
![[Screenshot 2025-11-23 at 4.11.51 PM.png]]



![[Screenshot 2025-11-23 at 4.13.52 PM.png]]
i = index of sentences
t = index of elements in each sentence

representing words: start with a vocabulary (or dictionary), to each word is assigned its position
![[Screenshot 2025-11-23 at 4.48.32 PM.png]]
sequence constructed by a matrix
![[Screenshot 2025-11-23 at 4.49.46 PM.png]]
special tokens for characters or text that is not expected in our vocabulary
![[Screenshot 2025-11-23 at 4.50.23 PM.png]]
Problems:
- Inputs/Outputs can be different lengths in different examples
- Doesn't share features learned across different positions of text
- Doesn't include mechanisms akin to memory

Recurrent Neural Networks (RNN) can solve these problems
- Input will consider the condensed version of what the network has seen the previous step

![[Screenshot 2025-11-23 at 4.58.12 PM.png]]
2 functions for each layer:
- what to output at that step (Y hat)
- what to propagate in the network (a\<n>)

only flow: from beginning to the end

Forward propagation:
the network will learn the weights and if the past is more or less relevant wrt the input
![[Screenshot 2025-11-23 at 5.05.07 PM.png]]

Backward propagation:
difficult to train, because it starts from the end
![[Screenshot 2025-11-23 at 5.07.38 PM.png]]
Loss function for each element of the sequence, then sum all to have a final value
![[Screenshot 2025-11-23 at 5.08.36 PM.png]]
![[Screenshot 2025-11-23 at 5.09.06 PM.png]]
propagating the final loss thru the entire sequence until we reach the 1st element.
now done by systems but helpful to find insights on why there are some limitations

# Architectures and Applications

![[Screenshot 2025-11-23 at 5.13.50 PM.png]]

many-to-one: simplified architecture, we disregard all outputs except the last one

![[Screenshot 2025-11-23 at 5.15.27 PM.png]]

one-to-many: only take one input, then system is autonomous, giving itself the output as input

Many to Many where length of input is different from the length of the output.
Suppress the output, process input, produce a compressed space, then next section produces output
![[Screenshot 2025-11-23 at 5.18.15 PM.png]]

## Language modeling
Probability of each sentence actually being said:
![[Screenshot 2025-11-23 at 5.23.04 PM.png]]

Training set: large corpus of text
![[Screenshot 2025-11-23 at 5.25.00 PM.png]]

![[Screenshot 2025-11-23 at 5.25.40 PM.png]]

the input of the prev step is narrowing down a bit all the possible words that could follow the word, so probability is shifted

The network will learn a distribution of the probability of all words in the vocabulary

Sample from the probability we calculated in training
	to autocomplete the next word, roll the dice with the probabilities we have

Statistical plausibility doesn't mean it's correct

## Modeling Dynamical Systems

Dynamical system is a mathematical object (model) used to describe time evolution of the variables characterizing a phenomena under study

Continuous-time dynamical systems the model is described by means of ordinary differential equation

![[Screenshot 2025-11-24 at 2.57.46 PM.png]]

while a discrete-time dynamical system when the model is described in terms of difference equations

![[Screenshot 2025-11-24 at 2.58.13 PM.png]]

Dynamical systems can be used to describe different kind of natural or man-made systems
- mechanics
- aerospace
- robotics
- electronics
- finance
- social media
- biology

# Vanishing Gradients and Solutions

singular subject must be recalled when it's time for the verb, even with a long sentence separating the two.
![[Screenshot 2025-11-24 at 3.00.25 PM.png]]

Summing all singular losses, the gradient of each is used for the total
![[Screenshot 2025-11-24 at 3.03.40 PM.png]]

the issue here is: the loss at each step is dependent on the previous one, so each one has to back propagate to all previous layers. This means that back propagation is basically multiplying the weight by itself a lot of times
![[Screenshot 2025-11-24 at 3.06.59 PM.png]]
exploding gradients can be fixed: put a cap
vanishing gradients are harder, because you can't recover the learning process

Units used to avoid vanishing gradients:

Current RNN unit: ![[Screenshot 2025-11-24 at 3.09.06 PM.png]]
gating functions for input and output, will emphasize 

GRU (simplified)
![[Screenshot 2025-11-24 at 3.11.20 PM.png]]
memory cell, update gate is used decide if has to be updated with the new value or it's too close to 0.
So only update for useful information in the sentence, not for junk in the middle of the sentence.
$g_r$ gating function will learn how relevant information are

trial and error to develop these gating functions
![[Screenshot 2025-11-24 at 4.24.57 PM.png]]
Long short-term memory : LSTM is older, further gates and elements
difference with GRU: 2 gates instead of just one ($g_u$ and $g_f$) and activation is calculated on another gate ($g_o$) so activation is not useful for output generation --> more degrees of freedom
![[Screenshot 2025-11-24 at 4.26.56 PM.png]]

Output cannot depend on information that has not been seen yet, but when we read we look ahead --> Bidirectional RNN
![[Screenshot 2025-11-24 at 4.36.44 PM.png]]

Deep RNN: Stacking more than one RNN (2-3 layers becomes very heavy), to make the function more complex (and therefore more flexible)

![[Screenshot 2025-11-24 at 4.41.23 PM.png]]

# Attention Models

attention = way to modulate information in network flow

translating: give some words in the sentence more importance than others, dampen some of the inputs that are not relevant

FR : Jane s'est rendue en Afrique en septembre dernier
EN : Jane went to Africa last September
![[Screenshot 2025-11-24 at 4.47.23 PM.png]]

Attention mechanisms also introduced with images for image captioning:
![[Screenshot 2025-11-24 at 4.48.52 PM.png]]

![[Screenshot 2025-11-24 at 4.50.02 PM.png]]

![[Screenshot 2025-11-24 at 4.52.33 PM.png]]

grid of vectors will analyze each part of the image and suppress the info of parts of the image that are not relevant
![[Screenshot 2025-11-24 at 4.53.19 PM.png]]

soft attention = weights are between 0 and 1, but not completely suppressing it (so never too close to the extremes)

## Transformer Model: attention is all you need

Transformers = alternative to RNN that use only attention
the building block is the scale dot-product attention

$Attention(Q,K,V) = softmax(QK/ \sqrt(d_k))V$

![[Screenshot 2025-11-24 at 4.58.53 PM.png]]

- Queries and keys are vectors of size $d_k$
- Attention measure how compatible queries and keys are
- Softmax ensures that all weights sum to 1

multi-head attention consists of several attention layers running in parallel
Key, value and query are linearly projected by multiplying with learnable weight matrices W^Q, $W^K$, $W^V$
![[Screenshot 2025-11-24 at 5.01.41 PM.png]]

Self-Attention: queries and keys come from same sequence, each vector of system can be considered as either one (all combinations)
we may or may not look into the future, if we want to prevent looking we need to mask

self-attention layers learn "it" could refer to different entities in different contexts
![[Screenshot 2025-11-24 at 5.05.48 PM.png]]
looking into the future is needed to find out which meaning
in some real-time applications this is not possible, so in training you have to mask the looking into the future

Transformer Architecture:
![[Screenshot 2025-11-24 at 5.07.37 PM.png]]
output: probabilities of seeing a certain word
entire sequence analyzed in parallel, expensive in memory

**problem** : output of attention layer doesn't depend on order!
**solution** : positional embedding encode position of each token and are concatenated to the input embedding

