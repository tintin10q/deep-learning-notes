# Regularization 

Many strategies used in machine learning are explicitly designed to reduce the test error, possibly at the expense of increased training error. These strategies are known collectively as regularization.

Regularization often works by trading an increase in bias for a reduction in variance. An effective regulizer is one that reduces variance significantly while not overly increasing bias. 

![Regulization in action](Pasted%20image%2020220611134419.png)

With regularization you make the model focus less on the noise. You change the learning of a model to make it satisfy certain constraints or preferences. 

## Common strategies 

### Constraints in the weights 
The idea behind this is to say you can't learn any kind of weighs. This

### Additional terms on the objective (loss) function
Adding another term in the loss function to make it harder and punish just memorizing the data. 

That looks like this: 

$$\hat{J}(\theta,X, y)=J(\theta;X,y) + \alpha\Omega(\theta)$$

The first part is:

X is the input (matrix like image)
y is the labels 

Which goes into a function $J$ the regulized version is $\hat{J}$  to get your results. Instead of hat it is ~ on top.  J is the loss function you minimize.

When you regulize you add the alpha omega $\theta$ to it. 

You basically add ($\alpha\Omega$) a penalty. 

Also: $\alpha \gt 0$ 

So you make your loss more complicated based on the weights. So this for instance stops one weight getting really big. 

#### L2 regularization
One example is $L_{2}$ regularization. That looks like this: 
$$\Omega(\mathbf{w}) = \frac{1}{2}||\mathbf{W}||^2$$
So then the regulized loss function becomes:
$$\hat{J}(\mathbf{w};\mathbf{X},y)=J(\mathbf{w};X,y)+\frac{\alpha}{2}\sum\limits_{i}\mathbf{w}^2_i$$
what is going on here is that you have hte normal loss function and you just add the sum of the weights to the power 2 times a constant $\alpha$ which you can set yourself divided by 2. So this increases the loss if the value of the weights get really high. . This means the model gets higher loss if weights get high. (You square the weights to also punish large negative weights and then you half the alpha to account for this)

So your making the loss higher not based on the results but based on the behaviour of the model trying to punish certain behaviour. 


## L1 regularization

With L1 regularization it looks like this:  $$\hat{J}(\mathbf{w};\mathbf{X},y)=J(\mathbf{w};X,y)+\alpha\sum\limits_{i}|\mathbf{w}_i|$$
So you don't divide alpha in half anymore and you don't take the square instead you take the absolute value of the weights. So then you don't have to divide by 2. Same idea as L2 just different execution. 

L1 sometimes can cause sparse weights with some weights being high and many low as that is the best option.

![Comparing L1 and L2](Pasted%20image%2020220611141630.png)

Because with L1 you just take the absolute value everything is treated the. With L2 you punish larger weights more then smaller weights. So L2 encourages  having multiple weights in the middle. L1 + L2 is a mix. You combine them by just having +

L2 is normally implemented in the optimizers you use. 


## Early stopping 

The idea of early stopping is to keep track of training and test performance of the model. You might get to a point where the training error goes down further but at a certain point the test error goes up again. When you notice this happening you can just stop early. The graph below shows this well:

![Early stopping](Pasted%20image%2020220611142235.png)

So you stop when you start overfitting. 

These ideas of early stopping are related to the model capacity.

![Model capacity](Pasted%20image%2020220611142537.png)

It does turn out though that if you go a lot further the accuracy will improve again. This is called double decay. 

![Double decay](Pasted%20image%2020220611144328.png)

## Dropout

With the dropout regularization technique you randomly remove units for each training iteration. 

![Dropout with a neural network](Pasted%20image%2020220611145438.png)

With dropout you train an ensemble consisting out of subnetworks that can be constructed by removing non-output units from an underlying base network.

So you do some training with random weights off and then you do training with other random weights off. With turn of we mean making the output of a neuron 0. 

This makes each weight more robust. Also you could just leave them off if the model is the same performance and then you have a more efficient model. This you can think of as your network being an ensemble smaller networks trained separately.

- Dropout encourages modular representations which work well in the absence of some parts of the network
- One advantage of dropout is that it is very computationally cheap to apply (dropout is cheap doesn't make the model cheaper per se)

You can do dropout by multiplying the output of layer with a mask which puts some of the outputs to 0. One extra calculation. 

This gives the $p_{hidden}$ hyper parameter which says how likely a neuron is masked. This often draws from a Bernoulli distribution. Also you have to define to which layers you want to apply dropout. 

## Problems

The catch with dropout is that you might get many neurons doing the same thing and that reduces bias. 

The model learns with less information. Very practically if you have a matrix as output and a lot of zeros are there because of the mask during training and then take the sum you get 50. The models learns with that. Well during training you will get no mask and after the sum you might now always get over 50 so the neurons will always fire stronger basically. 

So when you get to test it has to suddenly deal with more information.

This can be solved in 2 ways:

- **Scale** expected total input to a given unit at test time similar to the expected total input to that unit at train time (when some units were missing)
- Do **inverse-scaling** during training, and no scaling for testing. 

