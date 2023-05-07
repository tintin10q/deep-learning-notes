# Sequence 

A sequence is a sequence of items. How do you model sequences? The differences between sequences and images is that with a sequence in theory everything which came before can influence what came after. 


![Sequence](Pasted%20image%2020220611223942.png)

With an image it is more simple. Basically only the pixels around pixels influence the other pixels and [Convolution](Convolutional%20Neural%20Network%20(CNN).md) takes good advantage of that. The bigger the square the more pixels you consider around another pixel as input which the models deals to learn with. 

However with something like a sentence or the music you listen to it is a sequence of data where everything from before a point in the time or point in the sequence influences what should comes next. This is hard to capture with Convolutional Neural Networks (CNN) so new machinery is needed. This is the recurrent network. 

One example is translation: 

![Translation of a sequence](Pasted%20image%2020220611223536.png)

Every word in the sentence has effect on the next word. You can go left to right and right to left with that. With short sequences you could in theory still use something like convolutional neural networks but with a longer sequence this becomes infeasible.

A sequence can be anything. 

 ![Caption for image generator](Pasted%20image%2020220611223731.png)

You also have the opposite where you have a sentence and then you create an image from the sentence. You can think about an image as a sequence of pixels as well. 


## Common Tasks with sequences 

- Create representations of sequences
- Go from one sequence to another (sequence-to-sequence)
- Generate sequences from a given representation

## Representations of sequences

With translation because the grammars are not the same you have to first represent your sequence in a more abstract form (encoding) and then from that version it is possible to go to another language (decoding). This pattern is often seen. You can't directly go from one to the other so you have to first encode as a more abstract version (this is what convolution does as wel) and from that you you can train another model which takes that abstract representation and turns it into what you want. 

Or going from text to speech. You first encode the text as an abstract representation and then from that you can go to the speech. 


Now all that still sounds like pure magic to me so how is it done? With [Recurrent neural network (RNN) from cl](Recurrent%20neural%20network%20(RNN)%20from%20cl.md). 

But lets start a new one.

