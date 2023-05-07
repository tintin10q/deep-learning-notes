# Generative network 

A generatieve network generates input based on a label. Instead of a discriminative network which generates a label for a class. The discriminative networks learn through backpropagation. 

![Generative model](Pasted%20image%2020220613001836.png)

The generative model has to learn how to create an input which will generate a certain class (in a discriminative model). 

![Going from a label to input](Pasted%20image%2020220613001903.png)


![Disgriminative vs Generative](Pasted%20image%2020220613085959.png)

How do you train such a network?

Firstly how can we get **different outputs?** Because if you ask for dog and the model finds a dog why would it generate another dog. 

![The same dog?](Pasted%20image%2020220613002124.png)

This is solved in one way by adding **random noise** when inputting a label to the model. This way the input is always different and the model has to deal with that. 


How to even **evaluate generative** models? If an input makes the right class it might not be a good input. Like no one told the network the picture shouldn't be blue.

![Evaluating class](Pasted%20image%2020220613084059.png)


## Distributions

You think about a generative model problem by sampling from a distribution. You assume that the inputs to model which generate the class (lets say images) are drawn from some hidden probability distribution in the universe from which you can draw samples and that will generate the right class .


![](Pasted%20image%2020220613003200.png)




The idea is that you first learn the distribution from which you sample and then you sample the dog picture from that. If you take a different sample you get a different image.

![Sampling gaussian distribution](Pasted%20image%2020220613084332.png)


In the picture above it is a Gaussian distribution to illustrate but it would be a much more complicated distribution. The model has to convert the sample of the distribution to some data we care about (for example an image). So the green line is a NN which approximates this function. 

![A generative function](Pasted%20image%2020220613084600.png)

So because you have the random input from before you can see the generative model as learning a function which takes an input sampled from some hidden distribution and then transforms it into data we care about. Like this:

However it doesn't actually work like this. We don't know what to sample because it is too complicated. If we did know how to do that it would be called **disentanglement**. That is the idea that you know what part of the sampled distribution wil do what in the output. 

If we can make generative models we can generate new data. 

The probability distribution with generative models is called the **hidden distribution** or **latent variables**. So if $X$ is the label and $Z$ is the class then we can get the probability like this: $P(X,Z) = P(X|Z)P(Z)$ however we don't know P(Z) nor P(X|Z). So that is what you learn. 

So:

1. Estimate the hidden distribution 
2. Learn to generate data points from a hidden distribution. 

There are two architectures presented:

- Generative Adversarial Networks (**GAN**) focus on goal 2
	- They assume the distribution and try to generate data
- Variational autoencoders (**VAE**) focus on goal 1 and 2
	- Learn both distribution parameters 
	- With the distribution learn how to generate data

# Variational Auto Encoder VAE
These models Given a set of training  data points X:

- Learn how to estimate the distribution $P_{\theta}(Z)$
- Learn how to estimate $P_{\theta}(X|Z)$


To do this we have a encoder decoder architecture. But now you try to first encode an input and then the decoder will try to go back to the same input. This is unsupervised. 

![VAE high level](Pasted%20image%2020220613091636.png)
So the data is a vector you create the abstract representation and then the decoder has to learn the inverse function from the abstract representation. 

![VUE](Pasted%20image%2020220613091713.png)

The trick is to make the abstract representation (Z) have a **lower dimensionality** than X. So you make the vector smaller to force the model to encode the image in a smaller representation. The loss function is the difference between X and reconstruction of X for a lot of images.  

This is the idea: 

![Auto encoder](Pasted%20image%2020220613092022.png)

Now to make it next level you make the encoder output the parameters of a distribution. **We assume this probability to be Gaussian**. So the mean ($\mu_z$)  and the standard deviation ($\sum_Z$). These two should approximate a gaussian shape of a prior $z_\theta$. 

We then only give the decoder the inputs to create the distribution and then the decoder will sample from that distribution and it has to reconstruct the output again. This will make the decoder generate different numbers because it will always sample different data from the distribution. Hopefully if the encoder has a really good distribution at some point we don't need it anymore. 

An issue that arises here is that you need to get a distribution which will only generate the image that you want. If the distribution of human distribution overlaps with images for cats then you get a human-cat image.

![Cat human](Pasted%20image%2020220613093849.png)

(Also the top review of this movie)

![Top review of cats](Pasted%20image%2020220613093923.png)



So that green blob looks like this:

![Green blob inside](Pasted%20image%2020220613093533.png)


The variance can never be negative so you need an activation function that does that. You can also use log for that. 

In practice there are many Gaussian distributions.

## Prior distribution

You have to make the model actually sample from the distribution. Because it could be that the model just puts 0 for one of the variables of the distribution and just threats it as an embedding instead. This is done by comparing the distribution generated to the **prior distribution**. So that is $z_{\theta}=[\mu_{\theta},\sum_{\theta}]$ and you compare that to the generated distribution: $Z|X = [\mu_{z}, \sum_{z}]$ 

![Comparing prior and generated distribution](Pasted%20image%2020220613095011.png)

You want the distribution to look like the prior so we know the model is actually *thinking* about distributions. But you also don't want it to be too close because then the probability distribution can become the same between multiple classes and we know it isn't. So $z= \mu_{z}+\sigma_{z}*\epsilon$ where $\epsilon \in n(0,1)$

Then we sample from that and reconstruct the image. Then we can train those distributions with backpropagation. 

To measure the difference between data X and the reconstruction P(X|Z) the log likelihood is used. $\mathcal{L} = \log P(X|Z)$. The log likelihood calculates how likely it is that the image comes from the distribution. You have to use this as the loss function. If you don't and use something like MSE then you are not using VAE anymore because then the decoder does not *interpret* the input as a distribution. The complete loss function looks like this: $$\mathcal{L}_{\text{VAE}}=\mathcal{L}_{KL} - \mathcal{L}_x$$
The loss likelihood of the VAE is (I am not sure)
$\mathcal{L}_{KL}$ = the likelihood the image came from  gaussian distribution
$\mathcal{L}_{x}$ = the likelihood the image came from encoder distribution  

So we want it to go towards coming from a gaussian distribution. We want the difference to become 0. So this causes the backpropagation to minimize $\mathcal{L}_{KL}$ and maximize $\mathcal{L}_{x}$ 

### Applications of VEA

![motion-forecasting](motion-forcasting.png)

![anonoaly-detection](anonoaly-detection.png)

## Generative Adversarial Nets (GAN)
This is a different way of addressing the problem of generating data that generates a class.

However with GAN you only generate the data. 

![GANS](GANS.png)
A GAN consists out of two two networks which are working to beat each other.  

The idea is that you have one model, **the generator** which generates data which should be classified as a certain class. This data is given to the other model with real data.


Then the other model, the **discriminator** tries to figure out if data was generated or is real. 

The loss is of the generator is tied directly to the performance of the discriminator. 

![Gans architecture](Pasted%20image%2020220613103317.png)

The discriminator could be a binary classifier. If the generator can fool the discriminator than you must have real images or picked up on a flaw in the discriminator. (This is also how cryptographic algorithms are designed, it is a powerful mechanism). 

With GAN you just assume that the distribution exists. 

When you do training you have two stages:

1. Update parameters of the discriminator
2. Update the Generator

### Update the discriminator

The discriminator will output a 0 if its a fake image and 1 if its a real image. 

![Discriminator update](Pasted%20image%2020220613103643.png)

> - $x_i$ is a data point
> - $D$ is the discriminator function
> - $G$ is the generator
> - $Z$ is the input to the generator
>So its just a binary classifier loss function. So if it was the generator d = 0 and than the second part is affected (because of the 1 -) and with the real data d = 1 so the first part is affected. Both have to be right to maximize the function (more and more correct).  

You feed the discriminator the real and fake images and the loss function is based on how many images are classified correctly than you do [backpropagation](backpropagation.md) on that. Important is that you don't take the back propagation back to the generator. 

## Update the generator 

When updating the generator you have a loss function tied to the performance of the discriminator. You want the discriminator to output 1. Important is that with the backpropagation you don't update the parameters of the discriminator. You do backpropagate through the discriminator though. 

![Update the generator](Pasted%20image%2020220613105428.png)

Here the loss function is to minimize the success of the discriminator. The cool thing is that the backpropagation goes though the discriminator. This actually gives the generator an idea of what to change and what set it off. 

## Minimax game

This adversarial process never converges. But at some point both models are both very good and good enough and you just stop.  

This is called a **minimax game**. One model maximizes a function and the other model tries to minimize (part of the) same function. 

Now apparently in practice it is more complicated than this but this is the principle behind it. For instance the discriminator does not have to be limited to binary classification. The generator can also take an input that is not sampled randomly but, for example an image (and generate a new image). The generator loss can be formed by more than one discriminator. 

## Applications 

Generating faces of people:

![age-faces-gan](age-faces-gan.png)

For instance here you have the discriminator also checking age. Or one discriminator checking if it is a human and another checking for age. If you then have an input which can decide on the age you got some disentanglement. 

![pix2pix](pix2pix.png)
This one has a generator generating an image from the sketch or outlines of an image. That is pretty cool.

![image-super-resolution](image-super-resolution.png)
upscaling images with ai. This is like the FBI ENHANCE!

This is the architecture: 

![Super resolution](Pasted%20image%2020220613111539.png)

Another approach to gan is 

![Noise](Pasted%20image%2020220613112339.png)

Here the idea is that you have a network which has a constant input but adds random noise to that. This "A style based generator Architecture for Generative Adversarial networks". 