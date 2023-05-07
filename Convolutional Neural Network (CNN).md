# CNN
Convolutional Neural Networks are networks which takes the input (a matrix) and apply convolution to on multiple layers by applying filters (kernels) to areas of the matrix. They are optimized for learning things about grid data. 

You only apply the filters to a small square of the matrix every time, and then you move it around. 

When you have smaller squares, you can feed them into a normal feed forward neural network. 

![What is ](Pasted%20image%2020220609115209.png)

## Convolution 
The idea of convolution layer is to take smaller squares of (kernel size, kernel size) out of 
the bigger image and turn those squares into a single pixel with a filter.  The smaller squares are called the filter window. We say that we have a sliding window.  Then you move the filter window forward to the next position and make another pixel. After this you have a new square which is (probably) smaller wich is a more abstract version of the previous square.  

![Convolution high level](Pasted%20image%2020220609144805.png)

You do convolution to make the image smaller because if you don't, and you just put all the pixels behind eachother with fully connected layers the networks become too big. We need to run the functions through an activation function to learn with back propagation. The idea is to make a smaller, more abstract version of the input, so you can still run the activation function effectifly on the input.  

![Convolution example](Pasted%20image%2020220609144444.png)

With CNN you actually learn the filters or the kernel! So here you have basically simple kernel where you just multiply the value from the matrix with the value from the kernel for each kernel, and then you take the sum. This way you can learn the values of the kernel!  

In a convolution layer you can either have one kernel which you then apply sliding window style to the whole image or you can also learn seperate filters for each part of the image. 

![The same kernel (W) for every part of the image](Pasted%20image%2020220609151322.png)
Instead of just doing one convolution layer you can also run multiple kernels at the same point in the network. This gives you multiple new channels. This way you can create n feature maps. See the image 

![You go from 32x32x3 to 28x28x6](Pasted%20image%2020220609153029.png)

So you get different feature maps from the same input. This sort of extracts different features from the input into seperate channels. But you also combine the filters together right away to get a scaler from multiple channels. So you could have a kernel which is 3x3**x3** to get one number from a rbg. 

![One value after combining the result of filters](Pasted%20image%2020220609161656.png)

![Bigger kernel. Each kernel layer can be different](Pasted%20image%2020220609161611.png)


### Stride

The stride is how much you move the filter window over the matrix. 

![stride2](stride2.gif)

![Stride](Pasted%20image%2020220308120805.png)

The stride affects the output size 


## Padding
If you take a stride which causes you to jump out of bound, then you get problems (3x3 with stride of 2). For this reason we have padding which makes the image larger to fit. 

![Kenrel not fitting without padding](Pasted%20image%2020220609200147.png)

Without padding, you also filter (apply kernel) the values at the edges of the matrix less times because the filter only moves over it only once. This is another reason to add padding as then it only moves over the padded once, but we don't care about the padding.  


![Pading](Pasted%20image%2020220308121226.png)

![Padding](Pasted%20image%2020220308121243.png)

The values of the padded cells do not matter much because you only look at them once mostly. 

The idea is now to learn the filters that are needed to get the best results based on a [loss function](loss%20function.md).

![Another example of padding the kernel will consider the border pixel equal times](Pasted%20image%2020220609200311.png)

### Padding values
The actual values you put in the padded cell don't matter that much because it taken into account less. What you actually put in there could be anything often it is 0 or 1 or the max of the neighbours. 

You also have [[Dilated Convolution]].

## Activation function
After you have done convolution, you run the activation function on it. This is what introduces non linearity and what allows the model to learn because you can use back propagation to learn the weights and this is what actually improves the model.

This is often the Rectified Linear layer (ReLu).

### Locality 
One additional advantage of convolution is that you get what is known as locality. After convolution you have merged a small group of values in the matrix together. If you than don't make a fully connected network than you have certain neurons only looking at a certain part of the output. So because of this you can think of convolution as *weight sharing*. You give a number of cells the same weight. 

### Notation 

Convolution is always written a $\star$.



# Pooling

Pooling is a simple version of convolution. It is more of a transformation. With pooling you don't learn anything, you just make a transformation. 

![Pooling](Pasted%20image%2020220308122133.png)

Diffent types of pooling are 
- Max pooling
- Average pooling

The pool size is how large the filter is.  The difference with convolution is that you don't learn the filter, you just predefine it. 

![Pooling to reduce the size](Pasted%20image%2020220610000616.png)

It is a make things smaller transformation. For instance a convolution layer but instead of multiplying with a kernel you do some operation like average of the 2x2 cells. Or max pooling just takes the max of the pooling cell.

## Kernels from computer vision 
![Intresting kernels from computer vision](Pasted%20image%2020220609151821.png)

These are cool kernels which tell you things about the image. $[-1, 1]$ is also a an edge detector. 

## Different types of data

There is basically 1D data, 2D data and 3D data.


![different types of data](Pasted%20image%2020220609181758.png)

### 1D
This is like a sound wave but squised into one point or time data with events happening at certain time. Sequences of numbers.
![1 Dimensional data](Pasted%20image%2020220609182122.png)

- sound

### 2D
This is like audio waves or images. 

![2d data](Pasted%20image%2020220609182214.png)

With images we still talk about 2d networks. This is because the filters are still 2d even if you have many filters on top of eaother you still have each single filter being 2d.

- images

### 3D
This is like a body scan for instance is a 3d image. Videos are also 3d because you have many images on top of eachother. 

So a 3d cnn traverses in 3d. So it takes in a cube instead of a plain to generate a scaler. 

So the 3d kernels also build a 3d vector space. This is different from applying multiple 2d kernels to make multiple feature maps. So with 3d you get a 3d feature map from 1 fitler. Because it might look the same. The difference is that you combine the data from mutliple slices trough the third dimension instead of just doing one slice. So with 3d you take a cube in one go and turn it into one cube instead of a slice and turning it into a pixel.  

![3d cnn](Pasted%20image%2020220609182356.png)

So a 3d filter is kernel which takes in both the red and green and blue channels all at once. With 3d you would have the filters take only one of those channels and then the x,y. With 3d you can take them all in. Altough this is actually possible by just rotating the 2d filter along the 3d.

- video analysis 
- medical imaging
- point clouds
- volumetric data 

So this is really like the dimension of each layer. This is different than having color channel in an image. There every color channel is still 2d.


### Channel

Sometimes the same data is made out of multiple channels of the same size to represent it. Like color channels of an image for example. All those channels are still 2d. 

--- 
So you have a matrix and you have convolution which becomes input for an activation function 

![Properties of CNN](Properties%20of%20CNN.md)



### Math
The math of convolution is: $(F * k)(p) = \sum\limits_{s+t=p}F(s)k(t)$
F is the filter, and you multiply it by the other function then you sum it up. 

p = the point which you take = stride + t (time? step?)

