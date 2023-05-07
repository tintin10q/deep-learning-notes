# Batch normalization
The idea of batch normalization is that you want to normalize the hidden layers to improve performance. The hidden layers are the input to the next layer so normalizing that would be better. But the problem is that you don't know how to normalize the hidden layers because you don't know what the numbers represent. 

![Batch Normalization](Pasted%20image%2020220611193128.png)



## How do you do it?

Remember that the output of a neuron is $z = w_0+\sum\limits_{i=1}^{N}w_{i }*x_{i}$ 
The idea is to calculate the mean and standard deviation of all the activations in the training [mini batch](Optimizers.md). 

So if you have 1000 images you can have batches of 100 and then every image in that batch will have activations for every neuron in a layer.  You calculate the mean and sd for those activations. 

Mean: $\mu=\frac{1}{m}\sum\limits_{i}z_{i}^{[l]}$
Standard Deviation: $\sigma^2=\frac{1}{m}\sum\limits_{i}{(z_{i}^{[l]}-\mu)}^2$
Then you normalize the neurons with that like this (before you compute the gradient): $$z^i_{norm}=\frac{z^{i}-\mu}{\sqrt{\sigma^2+\epsilon}}$$
The only reason i for index is on top is that we want to put norm at the bottom to say that this z is normalized. 

Then you use the normalized output in the activation function. Then you do the [backpropagation](backpropagation.md) with the activation function.  

## Scale and shift

Another option is this:

$$\hat{z}^{i}=\Upsilon z_{norm}^{(i)}+\beta$$
Here you just add 2 hyperparameters to the output  of $z_{norm}$. The hyper parameters are **scale** ($\gamma$) and **shift** ($\beta$) Basically this is just the linear equation. This is done to prevent the normalization to end up in zero. 

