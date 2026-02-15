# Notes from 'The Illustrated Transformer' by Jay Alammar

_(These notes were prepared while studying Jay Alammar’s well-known article, ['The Illustrated Transformer'](https://jalammar.github.io/illustrated-transformer/).<br>
A significant portion of the text closely follows or directly quotes the original content, with the intent of preserving its accuracy and clarity. I have added emphasis and brief commentary in certain sections that I found particularly insightful or worth revisiting.<br>
The goal of this document is to summarize the main flow and core ideas presented in the article, serving as a compact reference rather than a replacement. I have not reproduced the article’s illustrations — the visuals in the original piece convey the explanations far better than I could. Readers are strongly encouraged to refer to the original work alongside these notes for the most complete understanding.)_


## Overall structure
+ The transformer is consisted of two major components - 
    1. an encoding component - consisting **Encoders**
    2. a decoding component - consisting **Decoders**

+ The encoding component is basically a stack of encoders (the [original paper ](https://arxiv.org/abs/1706.03762) stacks 6 of them on top of each other). The decoding component is a stack of decoders of the same number.
+ Each encoder has 2 components/sub-layers:
    1. A **self-attention** layer
    2. A feed-forward neural network (FFNN)
+ Each decoder has the same 2 components but there is an extra laye:
    1. Self-attention
    2. **Encoder-decoder attention (extra)**
    3. FFNN

## Input vectors

+ First, we turn each input word into a vector using some word-embedding. 
+ All encoders will receive **a list of vectors**, each of size 512
    - Only the bottom-most encoder receives the word embeddings
    - The other encoders receive the output of the encoder below

<u>Note:</u> The size of the list is a hyperparameter. This is basically  the length of the largesr sentence in the training dataset.

+ In self-attention layer, the path of flow of the word in each position has inter-dependencies. But in the FFNN, they are independent of each other and hence, can be run in parallel.

## Steps of Self-attention:

### Step 1:
For each input vectors (here, let's assume that the vectors are the word embeddings, like they will be for the **first** or bottom-most encoder only), we create 3 vectors:

1. A **Query** vector
2. A **Key** vector
3. A **Value** vector

<u>Note:</u> These vectors are created by multiplying the embedding by the matrices that we **train** during the training process.

It will be like this:<br>
+ We have a a vector (word), X<sub>1</sub> of size 512, or, X <sub>1</sub><sup>(1 x 512)</sup>

+ We have a weight matrix, **W <sup>Q </sup>** (for Query) of size 512 x 64
+ We multiply these two and get the Query vector, **q<sub>1</sub>**, of size 64, or q<sub>1</sub><sup>(1 x 64)</sup>.

Exact similar operations are done for obtaining Key vector(k<sub>1</sub>) and Value vector (v<sub>1</sub>) for the first word X<sub>1</sub>.

### Step 2:

Now we calculate some 'scores' for the input word. We will calculate the score of each word in the input sentence against this word. These scores determine how much focus to place on other parts of the input sentence as we encode a word at a certain position.<br>
So, if our current word is X<sub>1</sub>, we will calculate the scores of the words X<sub>1</sub>, X<sub>2</sub>, X<sub>3</sub>.... w.r.t X<sub>1</sub>.

The score of each word (say, X<sub>i</sub>) in the sentence for a given word (say, X<sub>1</sub>) <br>
= query vector of the given word (q<sub>1</sub>) $\cdot$ key vector of the word X<sub>i</sub> ( k<sub>i</sub> )

So, for processing the first word, the first score will be q<sub>1</sub>  $\cdot$ k<sub></sub> , the second score will be q<sub>1</sub>  $\cdot$ k<sub>2</sub>, and so on...


### Step 3:

Next we divide the score by 8 ( = $\sqrt{64}$ = $\sqrt{\text{dimension of the key vector}}$ )

### Step 4:

Next we do **Softmax** over the score.<br>

<u>Note:</u><br> The softmax score determines how much each word will be expressed at this position.<br> 
Clearly, the word **at this position itself** will have the highest softmax score, but it is useful sometimes to attend to another word that is relevant to the current word.

### Step 5:
Multiply each **value vactor** with the softmax score.<br>This will suppress the values of the vectors/words which are irrelevant.

### Step 6:

Sum up the weighted value vectors. This is the output of the self-attention layer for <u>**this word**</u>.


## Matrix Equivalence

The whole operation shown in above 6 steps are actually done in matrix format to speed things up.

Suppose we have only two vectors (words) in the input sequence (sentence). Then the input matrix of that sentence will be a matrix of size 2 x 512.

This input matrix, X, is multiplied by:
*  W<sup>Q</sup> to get Q<br>
$$\left[ X^{2 \times 512} \cdot W^{Q \, (512 \times 64)} = Q^{2 \times 64}\right]$$
* W<sup>K</sup> to get K<br>
$$\left[ X^{2 \times 512} \cdot W^{K \, (512 \times 64)} = K^{2 \times 64}\right]$$
* W<sup>V</sup> to get V<br>
$$\left[ X^{2 \times 512} \cdot W^{V \, (512 \times 64)} = V^{2 \times 64}\right]$$

### The softmax step:

$$\text {softmax} \left[ \frac{Q^{2 \times 64} \cdot K^{T (64 \times 2)}}{\sqrt{64 (= \text{dimension of K})}} \right]
= \text {softmax} \left[ M^ {2 \times 2} \right]$$

This **M** is the matrix we obtained using the operation in **Step 4** mentioned above.

### Last step:
(combining Step 5 and Step 6 from above)
<br>
<br>
$$M^{2 \times 2} \cdot V^{2 \times 64} = V^{2 \times 64}_{\text{weighted}} = Z^{2 \times 64}$$

Each row of Z is the sum vector we want.

## Multi-headed Attention
" The paper further refined the self-attention layer by adding a mechanism called “multi-headed” attention. This improves the performance of the attention layer in two ways:"

1. It expands the model’s ability to focus on different positions. 

2. It gives the attention layer multiple “representation subspaces”.

"With multi-headed attention we have not only one, but multiple sets of Query/Key/Value weight matrices. The paper uses eight attention heads, so we end up with eight sets for each encoder/decoder."

So, We basically have 8 different Q, K, V matrices and hence 8 differenr Z matrices for each word/vector.

**But**, the FFNN does not expect 8 matrices. It is expecting a single matrix (a vector for each word). <br>

So, we concatenate all Z<sub>i</sub>'s into a single matrix of size 2 x [64 x 8] or 2 x 512.<br>

Then we multiply it by an additional matrix W<sup>O</sup> of size 512 x n (say).<br>

The resultant Z matrix is sent to the FFNN.

## Positional Encoding

To address the order of the words in the input sequence, the transformer adds a vector (positional encoding) to each input embedding.

If the word is $$X_1 ^{1 \times 512}$$ and the positional encoding vector for time at 1 is $$t_1 ^{1 \times 512},$$
then the updated vector will be <br>
$$X_1 ^{1 \times 512} + t_1 ^{1 \times 512} = X_{1 (updated)} ^{1 \times 512}$$

## Residual and Layer Normalization

Each sub-layer (self-attention, ffnn) in each encoder has a residual connection around it, and is followed by a layer-normalization step.

[This image](https://jalammar.github.io/images/t/transformer_resideual_layer_norm_2.png) from the article gives a very clear idea of what happens here. 

## Decoder Part

+ The output of the top encoder is then transformed into a set of attention vectors K and V. These are to be used by **each decoder** in its Encoder-Decoder Attention layer.<br> 
This helps the decoder focus on appropriate places in the input sequence.

+ The output of each step is fed to the bottom decoder in the **next time step**. The decoders passes them upwards just like the encoders did.<br>
We also add positional encodings to the decoder inputs.

+ This process is repeated for each vector until we reach a special symbol (< end of sentence >).

<u>Note:</u><br>
The self attention layers in the decoder is only allowed to attend to **earlier positions** in the output sequence, **unlike the encoder**. This is done by masking future positions (setting them to -inf) before the softmax step in the self-attention calculation.

The Encoder-Decoder Attention layer works similar to multiheaded self-attention, except it 
 1. creates its Queries matrix from the layer **below** it
 2. takes the Keys and Values matrix from the **output of the encoder stack**.


