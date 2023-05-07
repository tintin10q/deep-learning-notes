# Transposed Convolution 

Transposed convolution is the opposite of [convolution](Convolutional%20Neural%20Network%20(CNN).md). You go from a smaller image to a bigger image. This is done by having a kernel wich takes in one input and then generates a matrix as output. 

Ofcourse, you could also take in 4 pixels and generate a bigger matrix still. 

![Transposed convolution](https://i.imgur.com/LWpOh3G.gif)

It works like this:

![Transposed convolution example](Pasted%20image%2020220610165248.png)

I don't know if all transposed convolution works like this, but here the idea is to take one square of the input and multiply that with the kernel and put the result in the bigger square. You sum everything for the output. So that 0 multipled with the kernel is the square of zero. The 3 is the square with [0, 3, 6, 9]. Then we just sum it. 

As this is a convolution layer you can learn the filter.

We want transposed convolution because we would like to have an encoder-decoder architecture. 

![Decoding Encoding](Pasted%20image%2020220610170319.png)

With an encoder decoder aritecture you have convolution on the left making the input smaller, and then you have transposed convolution on the right making the input bigger again. The decoder generates something from the abstract representation produced by the right. The convolution is downsampeling and the convolution is [Upsampeling](Upsampeling.md), but with learned filter. 

