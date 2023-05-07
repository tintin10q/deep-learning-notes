# Residual connections 

Residual connections give a neural netork the ability to give information of a layers input straith to the next layer. 

>option to copy the input if the function F(x) is not informative. So the layer becomes identity function. 

People found that it is effective if you take the input before one or more convolution layers and also merge this input with a later layer. So you take the identity of a previous layer before you do another operation to it and add this information to generate a new input. 

![Residual connections. The layer is added the the output of the result after 2 weight layers](Pasted%20image%2020220610213923.png)

The reason for doing this is that when you get deeper, the network at some point suffers from the vanishing gradient and learning becomes slow or zero in back propagation. By using residual connections you are devaluing the work done by the convolution layer but you are also providing new information which means the network can go deeper. 

## Resnet

Resnet is an implementation of a neural network which you can see uses risidual connections, and it works well.
![Example of residual connections beign used](Pasted%20image%2020220610214427.png)


## Highways
A residual connection is a highway connection when you also have a gate (some sort of function which is learnable) which you apply on the residual connection before you merge the input layer with the output of the convolution.  An example below:

![Another example of residual connections](Pasted%20image%2020220610220826.png)

The highway connections still aply some functions to the residual connection. Here it is done to not turn things into zero, maybe.

## Disadvantages

A neural network with residual connections has to make sure that the input and the later hidden layer you add the hidden layer to have to be the same size, or you have some other sheme to add the layers together. 

Using residual connections increases computation needed