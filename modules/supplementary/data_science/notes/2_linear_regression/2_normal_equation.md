### Linear regression and the Normal Equation

In the case of linear regression, we assume that our observations follow a linear relationship with respect to the input variables. This assumption could be based on some prior knowledge about the data; it could follow from our exploratory data analysis; or it could be based on a known model with a basis in physics, biology, economics, etc. 

For simplicity we will restrict ourselves to the case where the observable variable is univariate and real: $y^{(i)} ∈ R$.

The data will look like: 

![Linear data](./images/linear-data.png)

We then write the following linear model for our observations: 

![Observations](./images/observations.png)

where the vector $β$ contains the $(p + 1)$ parameters of the model, which need to be found from the training data. 

![Vector B](./images/vector-b.png)

For every point in our data, we will then have a predicted value

![predicted value](./images/predicted-value.png)

Ideally, we would want to find the values of the parameters $β$ such that the prediction $\hat{y} (i)$ is equal to the observation $y (i)$ for every single data point:

![every point](./images/every-point.png)

Linear regression solves the least squares problem, essentially it finds a set of parameters such that our predictions are close to the observations in a precise sense. To see how this problem is solved in generality, we rewrite the above in matrix-vector form. Where matrix $X$ contains the descriptor variables and vector $y$ contains the observed variables:

![matrices](./images/matrices.png)

So $N × 1$ vector of predicted values $\hat{y}$ is given by: 
    $\hat{y} = Xβ$

Since we want to find the parameters such that prediction and observations are close, we need to quantify the error of our model: 
    $e = y − \hat{y} = y − Xβ$. 

The error is illustrated in Figure 2.1. The vector $e_{N×1}$ contains all the deviations between the predicted values and the observed outcomes.

![Figure 21](./images/figure21.png)


Least squares finds a solution of an associated system that minimises the mean squared error (MSE). In our matrix-vector notation, the MSE can be written compactly as: 

![mean squared error](./images/MSE.png)

The linear regression model that minimises the MSE is obtained by solving this least-squares problem

![least-squares](./images/least-squares.png)

To understand what this least squares solution means we need to perform an optimisation which can be solved both explicitly and numerically, these proofs are outside our scope for now. 
 