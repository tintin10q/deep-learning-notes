# Multi layer perceptron
Because every [perceptron](Perceptron.md) takes a list of inputs and then gives one output we can connect the outputs of many perceptrons together to form networks. This is called a multi layer perceptron. If you do this then you no longer need to have one perceptron solve the entire problem, but you can break up your problem into smaller parts first which will then be solved by a final perceptron. This is actually a requirement for more complicated problems because one perceptron can only make linear solutions. 

![Linear solutions](Pasted%20image%2020220219152253.png)

This turns out to be a great way of solving problems. Although it is hard to get 100% correct which would be possible with rule based systems but for some problems the rule based systems are too hard to make. So perceptrons are just another tool on your tool chest.

## One layer
You can start by giving the same input to multiple perceptrons but this still gives linear results.

![Multi layer Perceptron](Pasted%20image%2020220219151951.png)

No you basically get a matrix multiplication instead of a vector. 

![Inputs go to different perceptrons](Pasted%20image%2020220219152038.png)

You have an input vector and dot product with the matrix.

So that gives us something more like:

$$o = f(Wx +b)$$ 

W is a matrix now and its a vector of biases now.

## More layer 
We can just string the perceptron together. They don't know where their input vector comes from.

![MLP](Pasted%20image%2020220219152433.png)

Now each layer is like the one layer calculation. Each layer gives $a^{l}= f(W^{l} \cdot a^{l-1} + b^l)$

Here l is which layer you are on every time and a is the output of a layer. This like a recursive formula. But what is at the bottom of the recursion? This is the x we had before the input. So $a^{0}= x$ or $a^{l}= x$ for $l = 0$. The output can be math written down as $o=a^n$ where $n$ is the number of layers.

# Weights for perceptron on every layer?
How do you get the weights for every layer? This is done with the [Back Propagation](backpropagation.md) algoritm.


## Cost
Because there are multiple perceptrons in the last layer we have to have a function that will tell us the error. This also gives flexibility because you can put anything there. This function is called $\mathcal{L}$. This is often also called the [loss](loss function.md) or the cost instead of the error. It calculates the distance between the output and the expected output. Some examples of cost function are:
- Mean squared error (MSE) -> $\mathcal{L} = \frac{1}{N}\cdot \sum\limits_{i=1}^N(o_i-y_i)^2$
- Mean absolute error (MAE) -> $\mathcal{L} = \frac{1}{N}\cdot \sum\limits_{i=1}^N|o_i-y_i|$
- Cross Entropy -> $\mathcal{L} = -\sum\limits_{i=1}^{N}(y_{i}\cdot \log{o_i})$

## Backprop
Once we have the error function we can propagate the error backwards. 

![Propagate error backwards](Pasted%20image%2020220219155149.png)

To goal is to know how each parameter contributes to the error.  Thus the derivative of the error with respect to each parameter is needed. Because the ultimate goal of back propagation is to find the change in the error with respect to the weights in the network.

We do this by applying the chain rule. The chain rule says that when you are trying to find the derivative of something that if there is another formula in what you are trying to derivative that you can just multiply by the derivative of internal formula. 

So if $\mathcal{L} = f(a)$ and $a = g(z)$ then the derivative of $\mathcal{L}$ is $\frac{\delta \mathcal{L}}{\delta a}*\frac{\delta a}{\delta z}$ so that is $$\frac{\delta \mathcal{L}}{\delta z} = \frac{\delta \mathcal{L}}{\delta a}*\frac{\delta a}{\delta z}$$

### Chain Rule Example:
So in the formula, we want to compute the derivative of the loss with respect to z,  
which we can do by computing the derivate of the loss with respect to a and multiply  
that by the derivate of a with respect to the z.

![](Pasted%20image%2020220219160933.png)

### Continue backprop
So with the chainrule the part you multiply gives is really the equation for that layer that you have to optimize. So this gives you how the loss contributes to each layer. Which is really the loss for each layer. Then you can use this loss for every layer to update every single perceptron based on the output and learning rate. 

![](Pasted%20image%2020220219161133.png)