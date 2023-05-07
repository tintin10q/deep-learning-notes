# Bias

The bias is like the prior. It is the things you know before which influence your next. 

In the context of machine learning, when you have **low bias** you have fewer assumptions about the target function you try to learn and when you have **high bias** you have more assumptions about the function you try to learn. 

If you are **biased** then there are **too many** assumptions. 

![Bias and varience](Pasted%20image%2020220611121118.png)


One kind of bias is also at a higher level than the humans 

# Variance

Variance relates to how an estimated function would change if different training data is used. If you use the same model on a different data set, and you have very different results then you have high varience. Then the model learns too many details about the data and can't generalize.

- **Low varience**: Small changes with different training data. (Not memorizing noise) 
- **High varience**: Large changes with different training dataset (Memorizing noise).  

# Bias & Varience 

You want to have low bias and low varience. 

It is a tradeof between Bias & Varience. Because low bias makes your model flexible, and it will remember the dataset and then if you give another dataset it won't work. 

If you have high bias on the train set then make a bigger network, train longer or maybe use a different architecture of NN. 

If you have high varience to get more data, use [regularization](Regularization.md). Or maybe use a different architecutre of NN. 