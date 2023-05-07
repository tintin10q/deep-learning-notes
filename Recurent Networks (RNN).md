# Recurrent networks

In Recurrent neural networks (RNN) the later layers of the network actually influence earlier hidden layers after processing an input to prepare for the next input. This works really well to model [Sequences](Sequences.md).

This is different from [Feed forward neural networks (FFNN) from CL](Feed%20forward%20neural%20networks%20(FFNN)%20from%20CL.md) in which this never happens. 

![Recurrent networks](Pasted%20image%2020220611230437.png)
So you can see in the image that the input to the hidden layer vector (**h**) in the feed forward is given by $h=f(x;\theta)$ the function takes the input vector x and the parameters. Two inputs.

With recurrent networks the hidden layer is given like this $h=f(h_{t-1};x;\theta)$.  Now the function has 3 inputs. Still the two inputs from before, the input from the previous layer ($x$), the parameters ($\theta$) and now also the previous output of the hidden layer $h_{t-1}$. This makes the hidden layer function a recursive function.  

The idea behind doing this is that because we are working with [Sequences](Sequences.md) we want to capture the information about the sequence. Now the previous hidden layer has that information so you just feed it again. **You want to generate an output taking into account not only the information at time t but also at t-1.** By doing this you also capture the information at t-2 etc because that information is in t-1 because we take t-1 every time. How long can you keep that information?

Now if we write this as a [Multi layer perceptron](Multi%20layer%20perceptron.md) we get:

$$h_0=0$$
$$h_{t}=\sigma(Ux_t+Wh_{t-1}+b)$$
So now instead of one matrix of parameters ($U$) to multiply with the input ($X$) you have another matrix of parameters ($W$) you multiply with the **previous hidden layer output** ($h_{t-1}$). After you multiplied you just add the results plus the bias. **This means that the U has to be the same size in one dimension as h and W has to be the same size in one dimension as x**. After that the results also have to be the same size otherwise you can't add them together. 
As you can see you do need to an [Initial value](Weights%20Initialization.md) for the hidden layer. This can be 0 (no effect) or you can learn it separately. 

The result of all that you then feed into an activation function ($\sigma$ in this case). 

## Unfold visualization

The visualizations so far were folded with the arrow going back into the hidden layer. You can also visualize unfolded with every state of the hidden layer separately. That looks like this:

![Unfolded recurrent neural network](Pasted%20image%2020220612001332.png)

So every time the hidden layer changes based on the previous input and the previous hidden layer. Remember that the the Weights you multiply the input with and the hidden layer don't actually change! (Only during the [optimizer step](Optimizers.md)). So that makes sense looking at the math. The U and W matrix don't change only the output of the hidden layer changes and that is saved for the next input from the sequence to the model. **This means that the back propagation only happens after you ran trough an entire sequence**. You also need a special back propagation which is back propagation through time. Besides that you probably want to do batches and then only after you did the whole back to an optimizer step. So **recurrent neural networks require more computation then CNN**. But that is obvious because you have to remember the hidden layer in the first place.  

Below the image also shows the output layer. In this case you compute an output from 3 inputs sequentially by multiplying the output of the hidden layer every time with a $V$ matrix. In this case each input from the sequence also maps to one output. 

![Recurrent Networks many to many](Pasted%20image%2020220612003553.png)

This representation also makes clear that **RNN are NOT penalizable and serial by their very nature.  

## Beam search

The output of the RNN is actually an (softmax) vector where each index of the vector represents a certain word in the dictionary which the network could predict. So if the probability of A is highest you predict A as the first word.

![Output of RNN](Pasted%20image%2020220928145840.png)

However just predicting the word with the highest probability every time might not always be the best idea. You can actually also try to predict a lower probability word and then see if the probability (certainty) of a word in the next timestep is actually higher. Doing this is called **beam search**.

![Beam Search - Take a lower probability at step 2 to get higher probability at 3 and 4](Pasted%20image%2020220928150321.png)
Take a lower probability at step 2 to get higher probability at 3 and 4. Then you get a search problem. Below is a bigger beam search.

![Bigger beam search](Pasted%20image%2020220928150410.png)

So then you can find the path which in the end will have the highest sum of the probability. This gives you the sentence with the most certain sentence. 

## Objective function (Loss)

How can you get a loss function for recurrent neural networks? Below one way is shown with **supervised learning** were you calculate the loss based on the expected outputs in the sequence and what was actually generated at every point of the sequence. I guess you then sum up the loss or something.

![Loss function with RNN](Pasted%20image%2020220612003811.png)

But how do you use this loss back the back propagation has to take into account the hidden layer changes at every time step. This is done with **back propagation through time**.

### The predicted probability vector loss

Another way is just to compare the generated vectors which indicate which word should be predicted (from the beam search section). With a vector of all zeros and 1 of the word that you wanted it to predict. Anyways using those generated vectors which predict the words is a good idea for the loss. 



## Back propagation through time

Back propagation through time works by combining  the each loss function from each time step into one big loss function and then taking the derivative of that. 

![Backpropagation Through Time](Pasted%20image%2020220612004409.png)

![Another back propagation through time image](Pasted%20image%2020220928135817.png)

This could become a large loss function. This increases computation and memory use but also causes the [vanishing gradient problem](Vanishing%20gradient%20problem.md). After a while the dependencies/influence of the first time step becomes very small and the gradients become vary small or go 0 and then you stop learning and the connection won't be there even it it is actually important. This means the length of the sequences is limited. Especially if your using sigmoid activation function for example.

To deal with the vanishing gradient problems with RNN we have **gated units**.

You can also have an **exploding gradient** where the gradient just gets bigger and bigger. This happens if the gradient is more than 1. We can solve this with gradient clipping. $\mathbf{g} \leftarrow \min(1,\frac{C}{||\mathbf{g}||})\mathbf{g}$

You just cut it of at some point. 

## Gated units

Gates units keep or throw away information in a hidden state for later. 

![Gates drawing](Pasted%20image%2020220612010126.png)

One way to do this is with the sigmoid function. 

![Gated Unit](Pasted%20image%2020220612005934.png)

If the sigmoid function goes to 0 from the gate input it multiplies with 0 and the information is destroyed. If the input goes to 1 it multiplies with 1 and the information is let through. If the information is destroyed it is also an option to replace it. 

If you go more advanced with gates you get something like this: 




## LSTM 

![LSTM](Pasted%20image%2020220612010410.png)

Another visualisation:

![LSTM](Pasted%20image%2020220928140810.png)

This way of doing it is called LSTM. 

By now it starts to look more like a CPU hardware diagram. There is a video explaining this in more depth. But the idea of the gates is to not keep all information to avoid the vanishing gradient problem. 

In this case x (input) and h (hidden layer) are concatenated for the input. There is a separate input c which stores the cell state. So in this case the recurrence happens through 2 inputs. In the end there is a hidden layer output and the cell state.  

The **f** function is defined like this: $f_t=\sigma(W_{f}x_{t}+U_{f}h_{t-1}+b_{f})$ 

Then this gate is used with (x) you multiply with the results of the sigmoid gate. This will set things to 0 and leave other things the same.

The **i** function is: $i_{t}=\sigma(W_{i}x_{t}+U_{i}h_{t-1}+b_i)$
The **c** function is: $\bar{c}_{t}=\tanh(W_{c}x_{t}+U_{c}h_{t-1}+b_c)$

**i** and **c** are then added together with the result of the (x) to fill in the gaps created. This creates the new cell state.

Then that cell state ($c_t$) is goes through a tanh and that is used to compute the next hidden state with the $h_{t-1}$ and $x_t$ concatenated and going through a $\sigma$ (to stop exploition). This goes on and on for everything in the sequence. We use [tanh](activation%20function.md) because the output of this function is always between -1 and 1 so it can go up or down. With a sigmoid it could only go up for instance. So this is nice. 

You can see that every function has separate matrices. We filter information and also replace it with new information. 
You can see the gates as masks. 

All the matrixes are then learned by back propagation through time. 

With PyTorch the gates are implemented. This LSTM is standard I think but you can change it. However doing it in code is not that bad as the formulas are just: 

- $F_{t} = \sigma(W_{f}\mathbf{x}_t+U_{f}h_{t-1}+b_f)$ 
- $I_{t} = \sigma(W_{i}\mathbf{x}_t+U_{i}h_{t-1}+b_i)$ 
- $\bar{c}_{t} = \tanh(W_{c}\mathbf{x}_t+U_{c}h_{t-1}+b_c)$ 
- $O_{t} = \sigma(W_{o}\mathbf{x}_t+U_{o}h_{t-1}+b_{o})$ 
- $C_{t} = f_{t} \circ c_{t-1} + i_{t} \circ \hat{c}_t$ 
- $H_t=o_{t} \circ tanh(c_{t})$ 

You also have to decide on the initial state. Backpropagation for initial state is better. 

This solves the vanishing gradient because the network can learn to decide to forget the previous state.

In the other course they gave us this:
![](Pasted%20image%2020220930095204.png)


So the idea of the forget gate is that you multiply with a vector of zeros to one and in between. Then you use add so there are no exploding gradients or vanishing gradients because you just make small changes. 

## GRU 

![GRU architecture](Pasted%20image%2020220612015304.png)

![Visualization from Nijmegen](Pasted%20image%2020220928143957.png)

The GRU architecture wanted to go less complicated with less gates and no cell state and no concatenation and more efficient. This was used for translation.

You can also stack these recurrent neural network boxes with gates. 

$R_t = σ(X_t W_{xr} + H_{t−1} W_{hr} + b_r )$
$Zt = σ(X_t W_{xz} + H_{t−1} W_{hz} + b_z )$
$H̃t = tanh(X_t W_{xh} + (R_t \circ Ht−1 ) W_{hh} + b_h )$

## Bi directional  RNN
With bi directional RNN you feed in the sequence from left to right and also from right to left. This increases performance. 

Going bi directional is useful because you might have dependencies in the sequence for something occurring now based on something which will occur in the future. For instance with sentences this can happen. This is statistical stuff we want to pick up on. 

![Bidirectional RNN](Pasted%20image%2020220612020144.png)
You add the green line. You could do this with LSTM together one which gets the information in normal order and the other one gets the information in reverse order. This is something you can run in parallel. This way you can encode information about the future and the past. 

While going through the sequence with 2 different RNN at the same time is separate you then **combine the hidden layer outputs of both RNN to generate the actual output**. So for the actual output you need to go through the whole sequence. This means that with bidirectional RNN **you can not work in real time** as you need to have the entire sequence you are going to feed in. 

## Higher dimensional RNN

With this you walk through your input which is in higher dimensions in a non linear way.

## Using RNN for NLP

How do you encode words for RNN? As vectors and these vectors are called [Embeddings](Embeddings.md). There are ways of doing this which you can find in [Vector semantics](Vector%20semantics.md). You can do it based on counting and also based on the hidden layer state of network tasked with predicting words.

The simplest ways is one hot encoding which just is a vector of size vocabulary and then it is 1 at the spot which is the word. 

This creates a [Vector Space](Vector%20Space.md). 

![Vector space projected down into 2 dimensions](Pasted%20image%2020220612021837.png)

The [Distributional hypothesis](Distributional%20hypothesis.md) states that words which are similar in meaning should show up in the same context and thus together in a good vector space. You can get this by [counting](Co-occurrence.md) or with neural networks. 

## Word2vec

One way of using neural networks is with [Word2Vec](Word2Vec.md).

High level of word2vec

![Word2Vec](Pasted%20image%2020220612022414.png)

You make ngrams from the words and then to one hot encoding on this. Then you feed that into a network which tries to predict that word using softmax. The ngram you gave in should get a high probability (after training with back propagation). If it does you can take the hidden layer which caused that prediction as embedding!
The idea is that because you use ngrams. The embeddings for Ngrams with overlapping words should be closer together than ngrams without overlapping words because with the overlapping words the networks have to predict the same ngram.


## Using embeddings

If you have the embeddings you can give them to the network as a sequence like this:

![Embeddings NLP and RNN](Pasted%20image%2020220612022148.png)

I think NLP here is one to one. 

## Many to many 

With many to many models you take a sequence of some length and then you output a sequence which is the same or different length than your input.

![Many to many](Pasted%20image%2020220612023048.png)

So with many to many you output something every time you have an input. I guess you could also have blank outputs if you need shorter outputs. But the encoder decoder makes more sense then that.

With many to many the input and output are the same lenght!

## Many to one 
With many to one you take in a sequence and then predict one class.

![Sequence to one output](Pasted%20image%2020220612023210.png)

With many to one you only have one output after you ran though the entire sequence. This is for instance the topic of a text. You don't care about the intermediate outputs. With this you can actually use the last output as a representation of the entire sequence!

The loss function with many to one only takes into account the final output: 

![Loss with many to one](Pasted%20image%2020220612023809.png)

So that V came from a RNN and this is the last output. y is the correct. 

## One to many

With one to many you generate many outputs from one input. How? You just keep going with the hidden layer and don't feed in new inputs. That is pretty cool. 

![One to many](Pasted%20image%2020220612023618.png)

So for instance. Generate text based on a topic? How about music from a (short sequence of) genres. 

With one to many the idea is that you actually **feed the output back as input**. Otherwise not much will happen. That looks like this:

![One to many output is input](Pasted%20image%2020220612024000.png)

## Sequence to Sequence (Seq2Seq)

You can combine the many to one and one to many to have a super cool encoder decoder model. You first create a representation of the entire sequence as one output. Then you have another model which takes that one output and generates a sequence from it!

![Encoder decoder](Pasted%20image%2020220612024153.png)

This is really great because now you deal well with the translation issue that the output is not the same length. 

With the decoder model you also need to have (beginning of sequence) BoS and end of sequence (EoS) tokens or start and stop tokens. If you don't the model does not stop. So the model has to learn when to stop.

So what you do is that the encoder RNN takes in all the input and then you take the final hidden state from that and give that hidden state to the decoder RNN and give the decoder a BoS to start generating something. 

Another example is the image to sentence model. 

![Go from image to sentence](Pasted%20image%2020220612024702.png)

You take the image and feed the pixels as sequence **or use convolution to feed an abstract version of the pixels** to the many to one RNN to get one embedding. Then this is given to a one to many RNN to generate the sentence!

The loss is generated by checking the embeddings of the words in the sequence generated to embeddings of the words in the correct sequence maybe with [Cosine](Cosine.md) and summing. 

So simpler even just use a CNN to get an abstract version of the image and feed that to a one to many RNN to generate a text. Woah its magic. 

![CNN output as input](Pasted%20image%2020220612025318.png)

To do this you have to back propagate the whole thing. 


The encoder decoder thing seems to be really really powerful like it solves a bunch of hard to do issues. 

## Loss functions for RNN

## Auto regressive training
You train RNN using auto regressive training. Here you make the network try to predict the next output it should give. Then you use this prediction as the next input. 

![Auto regressive training](Pasted%20image%2020220928135346.png)

This way you can use the rnn as a generator. 

## More examples of RRN seq2seq

![seq2sec Encoder decoder](vlc_bjY9igPHm2.gif)
The context passed in the gif is one vector of numbers or a matrix. Often the size is something like (256, 512, 1024) this is an embedding. 

![More zoomed out encoder decoder](vlc_mtRH8PXFzK.gif)
At the end of this gif a end of sequence is generated. So you can see in the image that the last state of the RNN encoder is given to decoder as input. What if we could give all the states of the sequence to the decoder that would look something like this:

Like this:
![Encoder decoder with attention](vlc_XZiFSioGyr.gif)

So for instance we just concatenate all the hidden layers from the sequence or we add them together. However this would result in very high dimensionality. You also don't know how large the input is going to be if you concatenate because you don't know how large the sequence is.

These things are solved with an [attention](Attention.md) layer which looks at what part of the input is the most important to combine that input into a smaler embedding (but by doing this you do take in all the information about all the previous time step hidden states). 

That looks like this:

![Pasted image 20220612132810](Pasted%20image%2020220612132810.png)

Continue in the [Attention](Attention.md) note to read more about this 


