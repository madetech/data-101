### 1. Setting up the problem
Here, we describe the process that supervised learning entails. To this end, we start with giving a brief summary of the typical workflow starting with some data. 

The first step in supervised learning is to take a look at our data. This means doing some exploratory data analysis and conducting a cleanup (e.g. eliminating empty fields, other unusable pieces of data). 

Once we've got our data in order, we'll need to decide which variables we want to use and what we want to do with them. This initial examination of the problem is crucial and encapsulates much of the modelling aspects of the work (i.e. trying to establish associations between variables and observables and create a model that allows us to predict the observable given new data points.

This is where we start building our model, trying to establish relationships between variables and observations, all in an effort to make accurate predictions.

Finally, we'll need to declare our predictor space $X$ and our outcome space $Y$. These spaces can be either continuous or discrete, and will help us make predictions based on our model.

Then we define the samples that we use for the training task, i.e. $x^{(i)} ∈ X$, as well as $y^{(i)} ∈ Y$. At this point, we will also have to keep some samples aside for validation, the remaining data will constitute the training set. 

Now, for a given task with training set ${(x^{(i)},y^{(i)})}^N_{i=1}$, we want to find the function $f : X → Y$, whilst defining the loss function $L f(x), y$. The loss function can sometimes be thought of as a function that computes the error between the model’s estimates and the ground truth.


### The basic process for supervised learning is as follows:

(1) Start with data, as set of N (input, output) pairs:  $(x^{(i)}, y^{(i)}), i = 1, . . . , N$.

(2) Decide: Classification or regression?

(3) Define a loss function $L(y, f(x))$

(4) Minimise the mean sample loss: 

![Mean sample loss](./images/mean-sample-loss.png)

(5) Try to also achieve a reasonable expected test loss: 

![Expected test loss](./images/expected-test-loss.png)

 
In general, the solution for $f$ will be the result of some form of optimisation, minimising the in-sample loss (error) $L$. That is to say, we are aiming to minimise the following: 

![Optimisation](./images/optimisation.png)

 
However, we also want to avoid over-fitting, i.e. obtaining a model that fits the training data perfectly, but will not perform well on unseen data. This means that we need to ensure that the expected out-of-sample loss test is also small: 

![Overfitting](./images/overfitting.png)

This desirable property is also known as *generalisability*.

In addition, there are usually several hyperparameters in the models. These parameters are called ‘hyper’ because typically they specify the overall structure of the model or the algorithm. These are parameters that are chosen and remain fixed during the optimisation that is done on the training set. 

To try and avoid overfitting and to enhance generalisability, it is conventional to calibrate the hyperparameters on the results (a *hyperparameter search*), using a subset of the samples called the *validation set*.

The model trained on the training set with a particular choice of hyperparameters is used to predict the outcomes on the validation set. And this process is repeated with different values of the hyperparameters. 

This scanning (or optimisation) over the hyperparameters fulfills a double function: finding the optimal combination of hyperparameters, and providing us with some reassurance that the model is robust and has the potential to generalise well. 

After this validation step, we will choose a model with an optimised set of hyperparameters with good performance and robustness. This model is then used on the test data.
