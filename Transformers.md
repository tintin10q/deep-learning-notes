# Transformers 

A transformer is a model which uses [attention](Attention.md) to boost the speed with which models can be trained. This allows to train the model in parallel instead of serial with [recurrent Networks (RNN)](Recurent%20Networks%20(RNN).md). 

Transformers were first introduced in the field of natural language processing. Later they were also used for images these types of transformers are called [[Vision Transformers (ViT)]]. Transformers are the state of the art and have replaced RNN in many domains since 2017. 

The idea is to stop using [[recurrence]] and instead only use attention + positional encoding to capture the sequence. This allows to train in parallel and you don't have to run through the entire sequence. Using attention also generates much more informative representations of the input. A downside of transformers is that there are a lot more parameters to learn with means more matrix multiplications and more money (but at least you can do a lot of them in parallel). 

This is how it looks (taken from original paper):

![A transformer architecture](Pasted%20image%2020220612151623.png)


Transformers have an encoder decoder architecture. This means that the first part of the network encodes an input into an abstract version and the second part decodes the abstract version to another domain. French to abstract version to English.

> This is the image described. As I know now.
> The **encoder on the left** and the **decoder on the right**. You take input embeddings add a positional encoding (whatever that is) then you feed into the multi head encoding (whatever that is) then you add the input embedding again trough a [residual connection](Residual%20connections.md) and [Normalization](Batch%20Normalization.md) then you go to a [Feed forward neural networks FFNN](Feed%20forward%20neural%20networks%20(FFNN)%20from%20CL.md) with also another residual connection. This is then given to the decoder (half way trough )  which has an output from the decoder before (I guess). 

## The encoder

All encoders are the same in structure (but do not share weights).

![](Pasted%20image%2020220612153902.png)
You can keep stacking these two layers. So there can be much more encoders on top of each other  which are stacked. Each encoder has two layers a self attention layer and a feed forward neural network. The last output at the top of the stack is given to the encoder but you can also at any point in this stack can you take the output at that point in the stack and give it to the encoder. This results in many vectors coming from the encoder. But it is not a sequence, it is a tower from one input. Just at different points in the tower the vector is taken. 

You can also feed the outputs of the encoder at different places to the decoder. 

![Decoder encoder with transformers](Pasted%20image%2020220612154909.png)


The encoder when given input looks like this:

![Encoder with input](Pasted%20image%2020220612155005.png)
The encoder gets the input (in this case words as word [Embeddings](Embeddings.md)) and these are combined in the self attention layer the output of which is given to the feed forward network. So here you give 3 vectors as input and get 3 vectors as output which is the input to the FFNN. Each z is computed by combining the embeddings but taking the one embedding as query. 

So how this this work?

First for each word embedding you create the query, key and value vectors. This is done by multiplying the embeddings by a special query matrix ($W^q$) to get the query input, a special key matrix ($W^k$) to get the key input and a special $W^v$ to get the value input.

![Pasted image 20220612144119](Pasted%20image%2020220612144119.png)


### Self attention thermology

Short detour from the cl summary to talk more about the attention terminology. 

In the example above, each word can play 3 roles. These are given more specific names to which allows us to be more specific. 

So each item in the sequence can be:
- A **query** (q), when it is compared to other items in the sequence. 
- A **key** (k), when it is part of the sequence to compare with. 
	- So the scores are written as $a_{i,j} = \text{score}(\mathbf{k}_{i},\mathbf{q}_{j})$
- A **value** (v) when used to compute the output:
	- $y_{5} = sum(a_{1.5}\mathbf{v_1},~a_{2.5}\mathbf{v_{2}},~a_{3}\mathbf{v_{3}},~a_{1}\mathbf{v_{4}},~a_{5.5}\mathbf{v_{5}})$

In the example $x_{5}$ plays the query role.  $x_{1}$ , $x_{2}$ , $x_3$  , $x_4$  , $x_5$ all play the key role and  $x_{1}$ , $x_{2}$ , $x_3$  , $x_4$  , $x_5$ play the value role. 

As you can see, each word can play multiple roles. **The key to self attention is to learn separate weights for each role** This way get separate weights for each role. Then if you want to use an input as a certain role, you can just multiply the input with the correct weights. More concrete, each input $x_i$ is transformed into its role by multiplying the embeddings with the corresponding weights. Like this:

- $q_{i}=W^Q\mathbf{x}_1$
- $k_{i}=W^K\mathbf{x}_1$
- $v_{i}=W^V\mathbf{x}_1$

These matrixes are obtained trough training. These matrixes give very fine-grained information about each item of the sequence (word in the sentence(s)). In practice, the representations of the words are different depending on the role the word is playing. 

So this means there are a lot of weights to learn. However, it is more efficient than recurrence as: 
- We get rid of the [Vanishing gradient problem](Vanishing%20gradient%20problem.md) 
- All the computations for each score are independent. This allows for large **parallelization**.

## What do you do with Query, Key and value?

When you have Query, key and value you feed them into this machine:

![Machine to take in Query, key and value](Pasted%20image%2020220612160327.png)
So you multiply the Q and K (to see how similar they are) then you scale that and optionally mask the output (only in the decoder) you still have vectors at that point. Then you use [Softmax](Softmax.md) to get probabilistic version and then you multiply that with the value vector. This then gives you a new vector now with the attention applied. 

So you can see that what is paid more attention to is based on how similar the key and the query are and then how similar the value is to that.

Here is a more practical example:

![Transformers practical example](Pasted%20image%2020220612161724.png)


- What you see in this image is that **you actually compare every input to each other input**
- First you take *thinking* as query and key and value and you calculate the score for thinking by multiplying key and query (q_1) 
- Then you only take the key of *machines* to calculate the score for machines as well (by multiplying the key of machines k_2 with the query for thinking q_1. 
- So that is the **score of the attention when taking thinking as query for thinking and machines**.  So you have two scores one for thinking and one for machines
- Then you multiply that normalize those scores 
- Then you multiply the scores with the values (blue vectors) of the word embeddings. So you multiply v_1 with the score of thinking and and v_2 with the score of machines.  
- As you can see the machines one is less blue so lower values = less attention because the score was lower with machines (because the query was more geared towards thinking).
- Then you just sum v1 and v2 to get z1. This is the attention output when focusing on thinking in the input sequence. So z1 has the information of thinking and machines in it.

So each word embedding is query once for the input sequence. 

So after you take thinking as query you do the same and take machines as query. 

**These calculations are done in matrix form for faster processing** so you just glew the vectors together into a matrix and then you can multiply in one go:

![Attention with matrix](Pasted%20image%2020220612172337.png)

So first you get the query, key and value matrixes. These W^ matrixes is what the model learns.  
Then the attention basically becomes:

![Attention calculation](Pasted%20image%2020220612172529.png)

So you do all the queries in one go to get all the z in one go. So this way represent the input sequence as  a matrix and that gives you a new matrix z with the attention applied. Every vector of z corresponds to one vector of x. 

## Multi headed attention

The idea of multi headed attention is to train multiple *heads* each with their own $W^q$, $W^k$ and $W^v$ matrixes. These matrixes are trained separately. The idea is that you get each head paying attention to different things. Hopefully things the other heads missed. This gives more flexibility because with only one head everything is only based on a single matrix for the query, key and value. 

![Multiple attention heads](Pasted%20image%2020220612173519.png)

Multi head attention gives the attention layer multiple representation subspaces. So per head you have different W^ matrices. You can see in the picture W_1 W_2 etc. The original paper had 8 heads.

However, this also gives you 8 z (output of the attention layer) matrixes. There are two solutions for this. Either **concatenate the z matrixes** or multiply the output matrixes with a final $W^o$ matrix which was trained with the model. Both methods give you a single smaller output again.

![Multiple heads multiple output problem](Pasted%20image%2020220612174152.png)


Now this is where the papalism comes in. You can calculate all those z matrixes (the attention output for each head) in parallel. That looks like this:

![Attention in parallel](Pasted%20image%2020220612175538.png)
So the big overview of multi headed attention:

![Transformer attention overview](Pasted%20image%2020220612175658.png)

This is then fed to the forward network. **You can just repeat an attention layer after a feed forward network!** This is called R in the picture above. That is still a serial process but all the heads you can do in parallel!

## Positional encoding
Because we don't feed in the sequence token by token but the whole sequence at once and then compute the attentions for each token mixing the other tokens the network looses the order information in the input. There is no sense of order. This is fixed by adding a vector to the embeddings which encodes the position in the sequence. This is called **positional encoding**. 

![Adding a position encoding](Pasted%20image%2020220612180324.png)


 The positional encoding looks like this for instance: 
 
![Positional encoding row](Pasted%20image%2020220612180535.png)

You have to read this as each y being a separate row which you add. So each y is a vector. They are all different which encodes the sequence. You can also see some line going from 0 forward every time and this sort of encodes the position. But these things are generated in a quite arbitrary way:

The paper makes these positional encodings like this: $$PE(pos,2i) = \sin{(\frac{pos}{10.000\frac{2i}{d}})}$$
- $PE$ stands for positional encoding. 
- Here $pos$ is the position in the sequence 
- $i$ is the position in the vector
- $d$ is ?? maybe a constant? 

The idea is that you have multiple sin waves with different periods and you add them together. Sometimes the *height* on the sin wave will be the same and other times it is different. But using a sin wave is nice because this way you don't get values which just keep increasing. The max value can only be the max of the sin waves. 


![Positional encoding visualized](Pasted%20image%2020220612181058.png)

So look at this image for a while.      So lets take the embedding $P_6$.  You first have $pos$ which is the position in the sequence. That is 6 for $P_6$ (this is the same for the whole vector). Then we want to get the positional encoding for the entire $P_6$. This you do by using the formula with $pos=6$ and filling in $i$ for every position of the vector. The $i$ causes a different sin wave to be generated and each of these sin waves will be at a different $y$ at $pos=6$. 

You repeat this for every vector but you of course change $pos$. Doing this will always generate unique vectors. 

> Basically for every vector in the sequence you take a different POS (different position on the sin wave), and for every POS you take a different i (different sin wave) at every position for the vector. 

You then add all the unique positional encodings the the input embeddings. You only add them at  the start (so only once) before the first encoder layer. After this the sequence information is just in there.

This is pretty clever but also arbitrary (like why divide by 10.000? To make it smaller?) but it works well.  


## The decoder  
So all that leaves us with:

![The Transformer encoder 1](Pasted%20image%2020220612183737.png)

Notice the [Residual connections](Residual%20connections.md). This is the layer norm step.

If you stack this and add the decoder you get:

![Stacked transformer](Pasted%20image%2020220612183918.png)

Notice that you only add the positional encoding once at the start. 

The output of the transformer at every stack is fed to the decoder at different parts of its stack. 

**The actual bottom input of the decoder does not come from the encoder but from the decoders previous output** (like with RNN). Remember the output of the decoder is actually words (or like a real interpretable output) again. For this reason we to feed it back into the decoder we have to convert it back to an embedding and it also gets **positional encoding added**.  

As you can see in the picture the encoded input is only added in the later attention layers. The encoded information is mixed in with the previous output (of the decoder) trough attention. 

We can keep stacking the decodes after that. 

### The final decoder layer

The final part of the decoder looks like this:

![Final decoder layer](Pasted%20image%2020220612185500.png)

The idea is to **run a linear function to expand the decoded matrix to the size of the number of all the classes** and finally you **run softmax** on that to get a percentage for each possible class as next in the sequence. Then you take the index of item in the vector with the highest probability and the class corresponding to that index is the next item in the sequence!

So with words this will generate a probability for each word in the vocabulary (actually the index of the final vector will correspond to a word in the vocabulary). Then you take the word with the highest probability. 

## More things in the decoder

If we look at the transformer image from the paper again:

![Decoder encoder picture from the paper](Pasted%20image%2020220612191451.png)
There are still two things in this picture which are not explained yet. The **masked** attention and the 3 arrows going into each attention layer. 

### 3 attention input 

Every attention layer in the picture has 3 inputs. These 3 inputs represent the key, value and query inputs. At the first layer the key, value and query all come from the previous output of the decoder. At the later attention layers  (at the red dot) the key and value are the inputs from the encoder and the input from the decoder is the query. The query is also fed trough separately with a residual connection. **So you take the query from the decoder and the key and value from the encoder.**

### Masked attention 

The reason why you have masked attention is that when you generate the output sequence you generate it one step (token, pixel whatever) at the time. This means that the output of the first attention is not so informative and there is garbage in there because in the beginning there is only one input. **To lessen the impact of the garbage they apply masked attention** -> You just set some of the outputs of the attention to zero by applying a mask. 


# Summary of transformers

This was on the last slide of the transformer lecture.

- While transformers have as many layers as there are words in the maximum length sentence. Just like RNN. These layers are disjoint, not time dependent.
- Transformers are not always better than traditional RNNs in all applications. RNNs still win in some contexts, but in those applications where transformers match or beat traditional RNNs they do so with lower computational cost. This is mainly dependent on how much data is available. 
- Transformers are non-sequential: sentences are processed as a whole rather then words by word and each head in the multi attention can be done at the same time. 
- **Positional encodings** are an innovation introduced to replace recurrence 
- Self-Attention: a 'unit' used to compute similarity scores between words in a sentence. 

## Trying out transformers

![Trying out transformers](Pasted%20image%2020220612204356.png)
