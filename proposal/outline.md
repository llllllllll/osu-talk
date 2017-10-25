Many publicly available examples of time series prediction with neural networks
use fake or random data. Other examples, particularly in finance, present poorly
performing models. It is very hard to learn good practices when only presented
with toy examples. Instead, this talk aims to teach the full process of using a
neural network for time series prediction by walking through a real problem from
start to finish.

We will begin by explaining the concrete problem we would like to solve and how
to frame our problem in a way that we can model. Once we understand our problem,
we will discuss how to collect the needed data. We will discuss the process of
reducing our input data into important features for the model to consume. We
will then learn how to use Keras to implement our neural network. Once we have a
working model, we will cover some tricks to improve its performance.

At every step, we will cover problems faced while working on this model. We will
show how to use data visualization to aid in model development and catch
problems early. We will also cover tips for using numpy to work with time series
data efficiently.

By the end of the talk, audience members will:

- know how to frame a problem in a way that a neural network can model
- know how to think about feature selection
- be familiar with the Keras API for time series predictions
- understand that the hardest problems come before you even get to Keras
