# Attention 

Attention in the context <span style=font-family:minecraft>of deep learning is</span> a technique to make a model consider more important parts of an input more. 

With attention, you have a model learn which part of the input is the most important. What to pay attention to. 

Basically you have the network learn which are of the important parts of your input so that it can reason about the input with respect to the important parts. So to focus more on the important parts and less on the less important part. 

You first have a network which learns which part of the input is the most important. Then you put this before the input to your neural network, so the neural network can focus on the most essential part. 

So the attention is a layer which you first learn. This layer then learns what to focus on in the input. When this layer is trained you use it to tell another neural network what to focus on. 

With NLP, you have a sentence and the attention layer will tell you which are the most important tokens from this sentence.

![Attention aritecture the face of the dog is the most important](Pasted%20image%2020220610231714.png)

So you have two neural networks. You use one nn first to extract the features (abstract representation of the input through convolution) and then you have an attention layer on these features which will learn which of the features are the most important. This is an attention mask with for instance values lower for features more important and higher for features of higher importance.

![Attention example](Pasted%20image%2020220610231917.png)

In the example 1x1 convolution is used to combine multiple channels.

## Attention with RNN

You use attention to learn what to focus on when updating the hidden layers.

First you train the attention layer like this:

![Training attention](Pasted%20image%2020220612030113.png)

This is a [CNN](CNN) which generates a feature map from an image. 

More unfolded version of above. 

![Attention with RNN](Pasted%20image%2020220612025948.png)

The  attention layer actually tells the hidden layer what part of the input is more important. But you can see **the attention layer gets input from h0**. So the attention layer will change what is important in the next state based on the previous state! This happens every step. This is how the attention layer is different every time! Because it gets a different hidden layer input every time at every time step of the sequence. 

This attention layer is usually trained beforehand. 

If you visualize this you get this. The lighter version is more attention:

![Attention](Pasted%20image%2020220612030953.png)

So you can see at first it goes for the stop sign and then for the road. 




## Attention seq2seq
With seq2seq models we often have encoder decoder architecture. First you represent a sequence of inputs a single embedding by taking the last hidden state of the model. Then you have the second part of the model which takes that embedding as input and generates a sequence from it which is the output. A many to one feeding into a one to many network! 

![seq2sec Encoder decoder](vlc_bjY9igPHm2.gif)



The context passed in the gif is one vector of numbers or a matrix. Often the size is something like (256, 512, 1024) this is an embedding. 

![More zoomed out encoder decoder](vlc_mtRH8PXFzK.gif)
At the end of this gif a end of sequence is generated. So you can see in the image that the last state of the RNN encoder is given to decoder as input. 

You usually update the states of the decoder (one to many) by feeding the generated output as input again like this:


![Encoder decoder](Pasted%20image%2020220612024153.png)


What if we could give all the states of the sequence to the decoder that would look something like this:

Like this:
![Encoder decoder with attention](vlc_XZiFSioGyr.gif)

So for instance we just concatenate all the hidden layers from the sequence or we add them together. However this would result in very high dimensionality. You also don't know how large the input is going to be if you concatenate because you don't know how large the sequence is.

These things are solved with an [attention](Attention.md) layer which looks at what part of the input is the most important to combine that input into a smaler embedding (but by doing this you do take in all the information about all the previous time step hidden states). 

That looks like this:

![Attention](Pasted%20image%2020220612132810.png)

So what is happening in that image? 

First of all you have the encoding stage which is the same as ever but how you can see the context which is passed to the decoder has $h_{1},~h_{2}~h_{3}$ in concatenated instead of only $h_3$ the last output.   

You can see that actually 2 layers are pictured the layer 4 and 5.  

We still feed the output of the RNN as input (gray arrow above $h_4$) but we also feed part of the hidden layer from the input. This part of the hidden layer input is generated by the action layer which learned to know what to focus on. In this case it is simple and the attention says weigh the first part of the sequence more than the later one. In this example that is based upon it now basically outputting the transformed first input of the sequence so the first hidden layer is the most important. You can see that first horizontal vector of Attention_4 is the most light so that means that one is weighted the highest. 

> With weighted it is meant multiply the entire input hidden layer states with a matrix. The matrix has higher values in the places which will multiply with the things the attention considers most important and thus cause those places to have a higher value after multiply with the attention matrix the which causes a higher activation in the activation function.

So it comes down to turning the encoded hidden layers into a smaller vector which can be death with an when doing it you want to weight certain part of it more. 

In the context of language the hidden states are generated by giving word [Embeddings](Embeddings.md) as input. The hidden states are a more abstract representation of the sequence which remembered the previous words as well. 

Now in more detail:

![Attention in more detail](Pasted%20image%2020220612134809.png)


So you see it is not that bad. You just take all the hidden states from the encoder, and **score** them *somehow* on how important they are. Then you [Softmax](Softmax.md) the scores so they all are between 0 and 1 and add up to 1. Then you multiply (weigh) the vector by the softmax score and then you just sum the vectors. Easy peasy. Combine the hidden state vectors into one and scale each of them depending on how important they are.

This approach can collapse an input of hidden states of **any size** in into one vector! 
This vector is then combined with the decoder hidden state at a timestep to generate the output sequence!

The question remains: **How do you generate the scores?** This is what the attention layer is trained upon. 

## How do you generate the attention scores?

A way of getting a score is to dot product with the hidden state of the decoder with each hidden state of the encoder. Because this is dot product between vectors you get a scaler. So multiplying purple vector with each orange vector to get the scores. The idea is that the vectors similar to the decoder hidden state  get a higher score because of dot product.

With words that looks like this:

![Attention result](Pasted%20image%2020220612143515.png)

So you can see for instance that because the embedding (or hidden state) representation of *Accord* is similar to *agreement* the resulting attention output vector will be mostly influenced by the accord embedding. Sometimes multiple words are similar like with zone and area, la. These embeddings are of course close because they mean the same and that is what the [Distributional hypothesis](Distributional%20hypothesis.md) says. But this captures the differences in grammar and it learns what is the most important word to focus on when translating

This is essentially what happens but a bit more involved.

To get the scores we first of all assign new names. We rename use the decoder hidden state to query, and the encoder hidden states to key/values. 

So now you say you multiply the query with the key and values and the more similar vectors get higher scores. 

![Keys and query on the image of before](Pasted%20image%2020220612144250.png)


![](Pasted%20image%2020220612144119.png)


See [Transformers](Transformers) for more details. 

So then back to this: You put the vector generated by the attention and the hidden state of the decoder together into an activation function to generate an output (softmax output for a vocabulary for instance).  
![Attention](Pasted%20image%2020220612132810.png)

Then you the output of layer 4 is fed into layer 5 at the bottom with the hidden state of the decoder and with that another input from the encoder is made with the attention by multiplying with the hidden state. 


--- 

So attention in practice as I now understand it is: Instead of feeding in only the last hidden input layer from the encoder to the decoder ($h_3$ which does have all the information of the sequence up till that point but less information about the start of the sequence and not all the details). You instead give all the hidden layers of the encoder (concatenated) and then use an attention layer to collapse that input to a smaller shape. In that process you weigh more the things which are deemed more important. For instance by dot product of hidden layers then doing softmax and scaling the hidden layers with that and then taking the sum. 

This enables you to encode all the hidden layers from the RNN encoder by paying more attention to what's the most important. 