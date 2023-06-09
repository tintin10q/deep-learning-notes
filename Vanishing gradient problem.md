# Vanishing gradient problem 
When using [recurrence](Recurrence.md), the effect an input has on the current prediction lowers the further it is in the past. This causes the **vanishing gradient problem**. This is not so noticeable for short to medium length sequences, but for long sequences, RNN will stop improving. The reason for this is that with the backpropagation trough time algorithm, the gradients of the earliest parts of the sequence go to 0. Basically, the further in the past a part of a sequence is, the less impact it has on the current prediction. If you go too far away, the impact will become 0 and back propagation stops working.  STM & GRU architectures mitigate this issue by forgetting about parts of sequence too far in the past, but this causes information to be lost through recurrence. 

The problem of vanishing gradient was already known about in 1997.

[Transformers](Transformers.md) attempt to solve this issue without forgetting. 