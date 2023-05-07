# 1 by 1 Convolution 

1 x 1 convolution is when you have a convolution layer which is 1 in two of its dimensions. This is done to reduce the dimensionality of the input. 

1 by 1 convolution is done to combine information of multiple channels. You do 1x1 convolution to increase the depth and width of the network while keeping the computational buget constant. 


With 3x3 or 5x5 convolution, you take part a matrix, and you transform it to a scaler. This is done by multiplying the input matrix with another matrix and summing the results. The numbers you multiply with is learned. 

With 1 by 1 convolution, your kernel size is 1x1. So in theory you perform a linear operation by just muliplying every pixel of an image with one value and that is the next value. The size of the output does not change. However, you can have 1x1x3 convolution. This way you sort of merge 3 channels of input together. 

So 1 x 1 convolution makes sense to do with 3d cnn or when you have multipe channels . 



![1 x 1 convolution example](Pasted%20image%2020220610211618.png)

If you look at the image above, it seems like 1 × 1 convolution means 1 and 1 in at least 2 dimensions. So in the image above you're not merging channels but instead just take a row. 

It seems however that this always has the effect of reducing the dimensionality by a lot. You compress it.

![Reducing dimensionality](Pasted%20image%2020220610213155.png)

You can also apply this with inception architectures. 

## Inception architecutes

You use the 1 x 1 to reduce the size of the input without too much computation. The idea is to do muliple types of convolution and just concatanate them together. 

![comparing 1x1 and dimensionality reduction](Pasted%20image%2020220610211333.png)
The 1 × 1 method works great because you can reduce the size of the input. In the inception architecture, they concatonate the layers together of these different ways of doing it. 




