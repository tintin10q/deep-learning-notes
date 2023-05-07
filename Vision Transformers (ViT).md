# Vision Transformer (ViT)

A vision transformer is a [Transformer](Transformers.md) which can work with images. Making transformers work with images initially presents a problem of how you will give input to the model. 

First of all images are square and not a sequence. But this is easily solved by just putting all the pixels behind each other. However with large images you have every pixel paying [attention](Attention.md) to every other pixel and it becomes computationally infeasible even with parallel processing. So people had find clever tricks. 

One such trick is by taking 16x16 squares out of the image flattening those squares and then using that as a vector input to the model. This allows you to split the image in parts of 16x16. 

That looks like this:

![Vision transformers](Pasted%20image%2020220612194942.png)

Notice that the positional encoding is added (purple numbers). They also a added a dummy input to the transformer at position one and that one actually predicted the class. They used a [Multi layer perceptron](Multi%20layer%20perceptron.md) to get an output class. 

Now it becomes really interesting because once you put the image into the transformer you can transform it to basically any other output sequence. Simplest output sequence is text. Like a label for the image. But how about transforming an image to another image? Also transforming text to an image is the coolest thing ever. 

Another cool thing about Vision Transformers is that you can visualize the attention on the image. You can visualize the numbers of the image and the higher numbers are kept after the attention pass. This gives an idea of what a model is focusing on:

![Attention in image](Pasted%20image%2020220612195615.png)
You can visualize the attention per head or after combining the heads or at different attention layers. These cool examples even show a segment of the image but these are the nice examples.

## Performance 

Below is a graph comparing the different transformer architectures. The baseline is a [CNN](CNN). 

![Performance of Transformers](Pasted%20image%2020220612200030.png)
The take away of the **first graph** is that the C**NN performs better than the transformers on a smaller data set** (with less preprocessing of the attention) but **with much more data the transformers get incredible performance. Better than CNN**. But not that much better. That JFT-300M is a dataset with 300.000.000 images. 

This performance is really cool but it now means that only parties with this kind of data and the money to train on it can make these really good models. This energy is often not green energy (renewable). Training on this amount of data requires a large amount of energy. The transformers require more energy (money) to train then CNN. 

The reason for this increased energy is not just more data. Transformers also have more parameters. CNN have those shared parameters to pick up on the bias in the matrix and transformers don't really have this. Transformers have more parameters to pay attention to the relationships in the sequence. So more parameters is more energy. However transformers can be cheaper to train than RNN (for instance because of the parallel).

This means that only parties with this kind of data and the ability to train on this kind of data can get the state of the art transformer. This is concerning because these parties can get a large scary advantage. Transformers can be really really good. Also keeping the model for yourself and renting it out can lead to  inequality. 

A cool thing you can see in the **second graph**. Here they tried training the models on only parts of that 300M images data set. One cool thing is that the performance difference between 300 and 100 million is small. It also seems that **the more heads the less data is needed**. In the second graph the squares are CNN and you can see the same pattern. They first perform better than transformers on less data. But with more data the transformers overtake the CNN (but not even by much). So for images CNN are very viable just not state of the art. For NLP transformers very good.

## Video Transformers

You also have video transformers. Below is just one approach:


![Video transformers](Pasted%20image%2020220612202813.png)

Somehow encode each frame of the video as tokens then you have multiple transformers for batches of frames and then you can see there is another transformer at the top which tries to pay attention to the temporal aspect of videos. Then this produces a class. That is pretty interesting because you can have a class like a Pinging sliding down a hill. 

You can also use multiple frames at once by having an input with many channels and each channel being a frame.

This is a very high level overview.