# Activation functions 
They are what you do after you have multiplied the input vector with the weights vector and added bias in the perceptron and take the sum. Doing this gives you one number which you then feed into an activation function. The part before the activation function is $z$. So $z = \sum\limits_{i=1}{(w_{i}*x_{i})} + w_{0}$ see [Perceptron](Perceptron.md) if you did not know that.

The reasons for doing this are: 
  
- It is a bit like an on/off switch: do we turn this neuron on or off. If the output is high the neuron fires a lot and if it is low the neuron doesn't really fire. 
- The purpose of the activation function is to introduce non-linearity into the  
network  
- linear means that the output is reproduced from a linear combination of the  
inputs (which is the same as output that renders to a straight line--the word for  
this is affine).  
- Another way to think about activation functions in terms of non-linearity:  
without a non-linear activation function in the network, a NN, no matter how  
many layers it had, would behave just like a single-layer perceptron, because  
summing these layers would give you just another linear function.  
- An ideal activation function is both nonlinear and differentiable. The nonlinear  
behaviour of an activation function allows our neural network to learn nonlinear  
relationships in the data. Differentiability is important because it allows us to  
backpropagate the model's error when training to optimize the weights.

# Populair activation functions
## Step function

Binary step function is a threshold-based activation function which means after a certain threshold neuron is activated and below the said threshold neuron is deactivated.

![Step function](Pasted%20image%2020220613122941.png)

## Linear
![Linear activation function](Pasted%20image%2020220219161623.png)

- Easy to train but can't be very complex
- You can find the differentiation everywhere

## Sigmoid
![Sigmoid activation function](Pasted%20image%2020220219161632.png)

### Pros 
- More non linear
- Maps to a nice range of [0,1] so it can represent a percentage.

### Cons
- Vanishing Gradient problem: function is flat near 0 and 1 → during  
back-propagation, the gradients in neurons whose output is near 0 or 1 are nearly 0  
(a.k.a saturated neurons). It causes the weights in these neurons to update very  
slowly or not at all.  
- Output is not zero-centered: makes gradient updates go too far in different  
directions  
- Saturates and kills gradients  
- Slow convergence

The main reason to use it is the nice 0-1 mapping

## Hyperbolic tangent 

![](Pasted%20image%2020220219161905.png)

In the picture o should be z.  

Like the sigmoid but better. Its s-shaped but the range is from -1 to 1 instead of 0 to 1. 

  
The tanh non-linearity compresses the input in the range (−1,1). It provides an  
output which is zero-centered. So, large negative values are mapped to  
negative outputs (and not to zero as with the sigmoid). Similarly, zero-valued  
inputs are mapped to near zero outputs.  
- tanh function is symmetric with respect to the origin, where the inputs would  
be normalized. It can also be said that data is centered around zero for tanh  
(centered around zero is nothing but mean of the input data is around zero.  
Convergence is usually faster if the average of each input variable over the  
training set is close to zero.  
-  But still the vanishing gradient problem.

# ReLU

![](Pasted%20image%2020220219162202.png)

Tanh and sigmoid were among the most popular activation functions in 90’s but  
because of their Vanishing gradient problem and sometimes Exploding gradient  
problem they aren’t mostly used now.  

ReLU benefit over other activation functions is that it does not activate all the neurons  
at the same time.  

These days Relu activation function is widely used. Even though, it sometimes gets  into vanishing gradient problem, variants of Relu help solving such cases.  
This is simple and efficient, it is said to have 6 times improvement in convergence  compared to the hyperbolic tangent. Its range is from 0 to infinity.

Its better because in activation functions when we do [backpropagation](backpropagation.md) we have to take the derivatives for each Perceptrons activation function. With most function you get something like this:

![](Pasted%20image%2020220219162552.png)

This squashing happens because there is only change between 0-1. But relu does not have this. However during training the output can fall into below zero and then the output will always be 0. For that there is leaky relu which assigns small numbers to negative values. 

![](Pasted%20image%2020220219162728.png)