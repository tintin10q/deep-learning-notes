# Questions 

## Why would one want to use a sliding window on the input instead of the whole input at a time?  

- Smaller inputs

## Below are some statements regarding the differences between CNNs and MLPs. Which of these statements are correct?  

- The main difference between a CNN and an MLP is that the convolutional neural network (CNN) has layers of convolution and pooling.  
- In an MLP all the input data is sent to each neuron in the first layer; then a dot product is performed between the input data and the neruons weights to produce a single number as output. These outputs are then concatenated and sent to the next neurons.  
- In a CNN the weights in the convolutional layers are a small matrix, i.e. the filter or kernel, which is dot producted with each window of input to produce a new single value.  
- All of the above  
- None of the above

## Calculate Sizes of layers and stuff

![Question](Pasted%20image%2020220609200810.png)


$w_{out} = \frac{W_{in}-f_{w}+2P}{S+1}$

Where $W_{in}$ is 


Size of filter: 4x4x1

Stride is 32x3?
Number of feature maps: 

Either 4x4 or 4x4x32

### Right answers 

Size of one filter: (4,4,3), So this is a 3d CNN (that was not clear)
Number of feature maps: 32 (number of filters you apply)


In this case padding is 0, so you get: $$w_{out} = \frac{W_{in}-f_{w}+2\cdot 0}{S}+1= \frac{W_{in}-f_{w}}{S}+1$$ 
Stride is 2, so you get: $$w_{out} = \frac{W_{in}-f_{w}+1\cdot 0}{2}+1= \frac{W_{in}-f_{w}}{2}+1$$
$W_{in}$ is the width of the input which is 64
$f_w$ is the width of the filter which is 4

So that gives you: $$w_{out} = \frac{64-4}{2}+1=\frac{60}{2}+1 = 20$$

$H_{in}=64,~W_{in}  = 64$   This is with and height of the input!

$f_{w} = 4,~f_{h}=4$   This is width and height of the filter/kernel

$P=0,~S=2$   This is padding size and stride 


$w_{1} = \frac{W_{in}-f_{w}+2\cdot P}{S}+1 = \frac{64-4+2\cdot 0}{2}+1=\frac{60}{2}+1 = 30 + 1 = 31$ 

$h_{1} = \frac{H_{in}-f_{w}+2\cdot P}{S}+1 = \frac{64-4+2\cdot 0}{2}+1=\frac{60}{2}+1 = 30+1=31$ 


Number of parameters of the conv layer:

Its the (filter size (4x4x3) + the bias (1)) x the number of filters (32) so: $(4\cdot 4 \cdot 3 + 1) \cdot 32 = 1568$



![Another question](Pasted%20image%2020220609231020.png)

one filter: 8x8x10
feature maps: 10
$w_{1}=\frac{100-8+2\cdot 0}{2}+1 = \frac{92}{2}+1=46$

$h_{1}=\frac{100-8+2\cdot 0}{2}+1 = \frac{92}{2}+1=46+1=47$
$p_{size}=(8\cdot 8 \cdot 10 + 1) \cdot 28 =17948$

So here $h_1$ and $w_1$ are the width and height of the feature maps the depth is the number of feature maps which is the same as the . 


## What is the difference between a convolutional layer and a pooling layer?

1. In practice there are no differences
2. The filters in a convolutional layer have higher dimension than the pooling. 
3. The convolutional layer has learnable parameters, while the pooling has not
4. Convolution is used in the inital layers. While pooling is used in the deeper layers of the CNN

Its c.

## Another one
![Another ](Pasted%20image%2020220610150201.png)


## How can dropout be implemented 

You make a random mask which you apply to the outputs of a layer and you set those outputs to 0. You also have to do scaling to the weights either during training or testing.


## Exponentially weighted averages

Why are the filtered versions shifted to the right?

![Exponentially weighted averages](Pasted%20image%2020220611174051.png)

Because you take into account more of the higher points from before.  


## Problem of RNN architecture

![Problem of RNN](Pasted%20image%2020220612005128.png)

The problem with this is that if you have long sequences with your [Recurent Networks (RNN)](Recurent%20Networks%20(RNN).md) you suffer from the [Vanishing gradient problem](Vanishing%20gradient%20problem.md) and learning (based on long range dependencies stops). 