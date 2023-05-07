# Properties of CNN

## Sparce connectifity 
Convolution kernel is much smaller.


With CNN you don't have a fully connected network. Only kernel size pixels are  connected to the next layer.  

![Left is MLP and right is CNN](Pasted%20image%2020220609191432.png)

So in this example, you only make the laters neuron be influenced by the previous one. With MLP you basically multiply the entire input with one matrix instead. With CNN you take one matrix and apply it to small parts of the input every time. That leads to less connections. This is much faster to train. Also, it makes sense. Pixels in one corner don't have impact on another corner you would expect. The bigger the kernel size the larger features you want to pick up on. 

## Parameter sharing
Kernel coefficients are identical for each input location. So with MLP you basically have one weight for every input but then again it is a dot product so everything sort of affects everything. 

Anyway because you apply the kernel multiple times and just moving it along by 1 (if stride is 0) then get the same input influencing the results multiiple times. 

![Parameter sharing](Pasted%20image%2020220609192403.png)

With MLP it doesn't share parameters. With CNN it does. The outputs are influenced by multiple neurons.

![2d parameter sharing](Pasted%20image%2020220609193009.png)

The image above makes more sense. So with MLP you would have 32x32 connections and you just multiply with a weights matrix/vector. With CNN you only have the filter and the same parameters go into multiple pixels on the output. 


## Equivarient reprentations 
Convolution value covaries with input value. If we apply transformation, folowed by convolution, its the same as if we applied convolution and then the transformation.  So the order of application does not matter. Just like 3 + 2 + 4 = 2 + (4 + 3 ).


# Techniques to apply
people have worked to reduce the impact of the vanishing gradient problem or the exploding gradient problem and reduce compitation. Techniques which worked are:

- [Residual connections](Residual%20connections.md)
- [Dilated Convolution](Dilated%20Convolution.md)
- ...

They don't know why these things work well. You just try things until it works. 

The reverse of convolution is [Transposed convolution](Transposed%20convolution.md).

