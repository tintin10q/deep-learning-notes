# Dilated Convolution

With dialated convolution, you have empty spaces in the convolution layer. Only the red points are considered. The ore dilation the more points you skip. 


![Diilated Convolutoin](Pasted%20image%2020220610150407.png)

The red points are the points which are considered by the kernel. So instead of making the kernel a direct square by just having a square, you take a square but actually take every dialation (l) points from that square as input to the kernel. So you skip inputs into the convolution. However, you still traverse the whole matrix. So its just a different input pattern into the kernel.  

The blue and greyish area show what the impact they have is on the feature map that gets extracted. So the dark area will be considered more often.  You still move these red dots every iteration. So you are still convoluting the entire image (matrix). Stride is separate from this. This makes sense, the corner in the left up is only considered once. 

1 dilated convolution is the normal case.

**You do dilated convolution to be able to run a bigger kernel more efficiently.** So you take into account a bigger square but with less iterations.

![Dilated convolution over the whole image with 2 dilated convolution](https://i.imgur.com/Um0lsPX.gif)
There are a lot of benefits of traversing an image with dilated convolution. 
- Less heavy computation.
- Exponential expansion of the receptive field without los of resolution or coverage.
- Larger receptive field with same computation and memory costs (as smaller kernel) while also preserving resolution. 
- Pooling and stride convolutions reduce the resolution; dilated convolution (with stide=1) doesn't.

You do traverse the whole image with the same kernel size but with a wider view. Every input is still considered, but in a different order. You can see the second pixel down from the top is considered later than without dilated convolution.  


So basically with stride you skip inputs and that is called loosing resolution. 



## The formula
The math of dilated convolution is: $(F *_{l}k)(p)=\sum\limits_{s+lt=p}F(s)k(t)$

## Stacked convolution
You can also have the same kernel but then have different filter layers with more dilation every time. 

![Stacked Dilated Convolution](Pasted%20image%2020220610163558.png)
