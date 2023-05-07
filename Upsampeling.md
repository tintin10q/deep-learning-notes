# Upsampeling 

With upsampeling you want to incease the number of samples you have of an input. So if you have a matrix of MxN and you want to create the same matrix but larger. 

You start with one image, and you want to create a similar image which is larger.

![Upscaling images](Pasted%20image%2020220610163919.png)

![Upsampeling in signal processing](Pasted%20image%2020220610163933.png)

## Methods

There are multiple methods to do this. For instance, nearest neighbour:

![upsampeling-techniques1](upsampeling-techniques1.png)

There is also unpooling. 

### Max unpooling 
With max unpooling (during encoding) 

![Max unpooling](Pasted%20image%2020220610171549.png)
During the encoding stage, you take into account the position where the (max) pooling happens. So which of the indexes were used to generate the feature map. Once you have these positions, you use these positions to fill in the gaps of the output with unpooling.  

> Remember with max pooling you take the max value of a square and use that as the output of the kernel  


You start with a matrix of zero. Then you fill in the values which were the max on the encoding with the input. 