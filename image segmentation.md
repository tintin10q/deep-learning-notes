# Image segmentation 

Image segmentation is the task of creating the outline of the objects in an image. This is pretty clever. The idea is the use a [CNN](CNN) and when it is at a very abstract state after some convolution layers you want the outline to be there. Then you use [Transposed convolution](Transposed%20convolution.md) to make the image larger again and back to the size of the image you had. Than you just overlay the overlay the network created to get the outline? How do you train the network to create the outline? You just use the softmax function on each of the pixels of the outline image with [backpropagation](backpropagation.md) on that. Then it becomes a classification like problem where some cells should be 1 and others shouldn't. The [Softmax](Softmax.md) on the generated image is really clever. Also it is unlike other classification problems where you want one output to be high and the other low but here you actually want a lot of cells to be high and lots to be low. But instead of high and low it is more like a binary. So there is no single labels but a ton of labels. 512x512 labels for a 512x512 image. Then a combination of these labels is what you are after and then you generate a mask with that.

![Image segmentation](Pasted%20image%2020220921150455.png)

The loss for this is $L = \sum\limits_{P \in allpixels}{y \log \hat{y}}$ 

Then you can create a lot of masks for different things like bikes and trees and different kinds of animals. Then you put all the masks on the original images in different colors.