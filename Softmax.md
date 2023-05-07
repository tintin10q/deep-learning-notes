# Softmax

For multi class problems like the one below, making the output probabilistic is hard. The left is what we have and the right is what we want. 

![Multiclass probem](../images/Pasted%20image%2020220603192841.png)

You make probabilistic by using the SoftMax function, with gives you the probability that your output is a certain class given the input. Or more general **turn a vector of numbers in a vector of probabilities**. 

This works by estimating different weights and biases for every class rather than a single vector, and then you divide every single vector by the sum of all the vectors. When doing this the activation formula becomes like this:  $$p(y=c|\mathbf{x}) = \frac{e^{\mathbf{w}_{c} \cdot \mathbf{x} + b_c}}{\sum\limits^{K}_{k~=~1}e^{\mathbf{w}_{k}~\cdot~x+b_{k}}}$$
This might look daunting, but it becomes more clear if we call it the SoftMax function and replace with the part where you multiply in input vector with the weights vector + the bias with z. 


$$softmax(z_{i}) = \frac{e^{z_{i}}}{\sum\limits^{K}_{k~=~1}e^{z_{k}}} 1\leq i \geq K$$
**The idea is to divide the result of one class/vector ($e^z$) by the sum of all the results, which is just calculating a probability.** Doing this will always give you a result between 0 and 1. So you get this (right):

![From sigmoid to probability](../images/Pasted%20image%2020220603200100.png)
