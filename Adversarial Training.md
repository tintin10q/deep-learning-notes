# Adversarial Training 

Adversarial Training is the technique of taking a model which is already good more robust. 

People found out that if you add some (special) noise to inputs then you can fool the mode. See the image below. To make a model which can deal with noise you train it further on noisy images. 



![Adversarial Training](Pasted%20image%2020220611155505.png)

This basically an advanced form of [data Augmentation](Data%20Augmentation.md) where you generate noise with another network.

If you have access to the model you can use gradient descent to find a good noise.
So if we call the change you make to the input $\delta$ then you get $x + \delta$ . If you can fool the model and $\delta$ is really small than this is a problem. In this case small means like changing one pixel or the colours or a changing pixels in a way that humans would still classify it as the same class. 

So the attack model want to find a small $\delta$ so that $x+\delta$ is classified incorrectly. 

### Project gradient descent

To do gradient descent to make a noise we want to keep $\delta$ small so we do projection to keep it small.  There are different project functions. The size $\delta$ can maximally be we call $\epsilon$.    Like 

.... 

Usually one step of gradient descent is the good enough. 

Put the formulas in the break. 

The idea is that you find a mask which actually minimises a certain class in the input or it maximises another class. 

### Why does this happen
Normally you have:
Loss minimisation $\sum\limits{L(X_{i},y_{i},w)}$

But this might generate a decision boundary which can be changed very easily by a small change which we have seen. This small change might go over a decision boundary. 

Minimise $\sum\limits{L(X_{i}+\delta,y_{i},w)}$

Here you mean x + a lot of small changes. 

So one solution is just to apply a lot of data augmentation. 

## Black box attacks
So far for the technique above you need to have access to the model to make the adversarial input. But you can also do without. For instance you could use the same data if you have access to that. 

You can make models who generate the noise. The loss of this model is the inverse of the loss of the classifier model. 

Now you can also use a model to generate this noise. The loss of this model is a function of the inverse of the loss of your classifier. This model is then the adversarial network of your classifier. The one model tries to generate noise which will fool the other model. Then you tell the adversarial model why it failed and retrain it. 

## Defending

To defend against this instead of just making it more robust you can also train a network which makes a model which learns to check if images are real. This is [[gan]] or [Generative network](Generative%20network.md). 