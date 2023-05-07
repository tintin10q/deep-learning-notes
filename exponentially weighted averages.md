# Exponentially weighted averages
Exponentially weighted averages is a way of drawing a line between points. 

The idea of exponentially weighted averages is that you take into account more points from the past when interpolating the now. 

Like so: $v_{t}=\beta v_{t-1} + (1-\beta)\theta_{t}$

So the next point of the line is the previous point times $\beta$ plus $1-\beta$  and the parameters. So the beta sort of controls how much you take into account of the past point and then the inverse  (1-$\beta$) is how much you take into account of the current point. 

![Exponentially weighted averages](Pasted%20image%2020220611174051.png)

You can see that with the higher $\beta$ the line goes up because it is still taking into account the previous higher points more.