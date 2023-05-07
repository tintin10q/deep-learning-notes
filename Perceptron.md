# Perceptron

A perceptron is a simple machine which takes in a vector of numbers and then multiplies it with its own vector of numbers. A vector is just a simple list. After this it runs a function to turn the vector into one number. This is usually just the sum of all the numbers in the vector. This function is the [activation function](activation%20function.md).

So ultimately the perceptron takes a list of numbers and turns it into 1 number. This is most often done with multiply with another list and then sum. This is dot product. Often in the final sum there is another hard coded value added which is called the bias. This can be looked at as the prior in basian statistics. 

![[Pasted image 20220219131833.png]]

This picture can be captured in the formula:



$$output = \sum\limits({inputs * weights}) + bias$$
Which is really the same as the dot product:
$$output = inputs \cdot weights + bias$$

Now if we want to write this down shorter we get:

$$o = w_{i}\cdot x_i+w_{0}$$

Here o is now the output of the perceptron. $w_0$ is the bias. $x$ is the vector of inputs and $w$ is the vector of weights. So really we say to start i from 1 in the dot product to have the bias be left out. To make this more explicit we can write it like this to take the dot product apart again. Because we can write $i = 1$ under the sum symbol:

$$o = \sum\limits_{i = 1}(w_{i} * x_{i})+ w_{0}$$

We can also do multiplication instead of sum no problem or any other function really after the weights have been multiplied:

$$o = \prod\limits_{i = 1}(w_{i} * x_{i})+ w_{0}$$


But what do we do with the output? On it own it doesn't mean anything. That is where the f comes in at the end. This is a decision function that decides what the final output should be based on the result of the perceptron. It interprets the result of the perceptron or scales it. 

![[Pasted image 20220219131448.png]]

Now if we want to write this down we get:

$$o = f(w_{i}\cdot x_i+w_{0})$$
Which is:
$$o = f(\sum\limits_{i = 1}{(w_{i} * x_i)}+w_{0})$$

## How do you get the weights. 
When actually using perceptrons you want it to predict something. This will work if you have the weights correct. How do you know what weights you put? That is what we reduced our problem to so far. This problem can be solved by using a learning algorithm. 

### Learning algorithms 
A learning algorithm comes down to an optimizing problem. You have a goal and some method of getting to the goal. You execute the method, and it gives you the result. On the first try this result will almost always have error in it. The idea is then to make changes to the method and see if the error reduced. If the error reduced than you keep the changes if it didn't you don't keep the changes. So really in a very general form it is:

1. Define an expected result.
2. Define a method to get the expected result from an input.
3. Make an attempt with input.
4. Find the error in your result. (Difference between the result you got and the expected result) 
5. Make changes to the method.
6. Make another attempt with input. 
7. If your changes decrease the error your method improved. Keep the changes.
8. If your changes increased the error or the error stayed the same your method did not improve. Discard the changes.
9. Repeat from step 5 till you are happy with the method. 

One option is to just make random changes to the method and keep them if they decrease the error/improved the method. This can actually work really well, and it is called an evolutionary algorithm. 

Random changes can work well but if you have a lot of variables than it can be a bit slow. Now what if you can actually have a formula that will give you the error from the input? Then you can instead of making random changes try to minimize the error formula to find the inputs that provide the least amount of error. So you can do this by taking the derivative of the error function and find a minimum/maximum by setting the derivative to zero. This will give you points on the function where the change is 0 which is a minimum or maximum. This could give you the best weights in one go.

However, if the formula is to complicated then we can't just go directly to a 0 point. In this case there will be too many minimum and maxima but here you can try to use gradient decent and a lot of different starting points. Gradient decent is a technique that uses the derivative to find the best direction to move the method (weights) in towards a local minima or maxima. In this case a minimum because we want to minimize the error. 

The process of finding the best weighs is also called **training**. For one perceptron the training can be done by multiplying every weight by the error. This gives you a vector of new numbers that represents the change you should make to the weights. You then often multiply this entire vector by a learning rate. The learning rate scales back the change to the weights because if you make changes that are too large you might overshoot but sometimes overshooting is not bad because you might find indication of another even lower minima. If you overshoot and there is no other local minima later than you will just "roll back down into the canion". In math the $\Delta$ symbol means change of.

$$\Delta w = \eta * (y-o)x$$

Here y is the expected output and o is the output you got. x is the inputs you gave. $\eta$ is the learning rate. Because the bias is not part of the input vector you have to calculate the change in the bias separately, but this is really just a detail. For the bias it is calculated the same way but no input.  

$$\Delta w_0 = \eta * (y-o)$$




