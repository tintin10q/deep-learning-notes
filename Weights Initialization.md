# Weights Initialization

Weights Initialization is about how you initialize the weights of your network before you start training. 

You have to start somewhere with the weights of a network. They can't be 0 because then the multiplication doesn't work. So what do you put them?  

Normally you just do a random initialization. But it can not be just whatever number because you can have **vanishing gradient** and **exploding gradient** problems. 


**vanishing gradient** - When you have weights are close to zero, the gradients in your later zeros vanish (go to zero)
**exploding gradient** - When you have weights higher than 1 the gradients may explode (grow exponentially)

So you need to somewhere between 0 and 1. Turns out the best way to do it is drawing numbers from a normal distribution with zero mean and a specific variance. The most common used methods are **Xavier** and **He**. 

It depends on the activation function. 

## Xavier
With Xavier you have initialize the weights by drawing from a normal distribution with mean = 1 and variance is $Var(w_{i)} = \frac{1}{\text{fan\_in}}$  with fan_in being the number of incoming neurons to the layer. 

This method works better with Sigmoid or Tanh activation functions.

## He
With He you have initialize the weights by drawing from a normal distribution with mean = 1 and variance is $Var(w_{i)} = \frac{2}{\text{fan\_in}}$  with fan_in being the number of incoming neurons to the layer. 

This method works better with Relu or  activation function.



