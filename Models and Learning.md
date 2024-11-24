Summary Table Types:

| Supervised Learning | Unsupervised learning    |
| ------------------- | ------------------------ |
| Classification      | Understanding sturucture |
| prediction          | anomaly detection        |
| Feature Selection   | Dimension reduction      |

# Supervised Learning

**Classification vs Regression**
Classification --> For categorical data
Regression --> For continuous data
## Regression
The optimal parameters for a line fit to data without uncertainties is:
$$ (X^T \cdot X)^{-1} \cdot X^T \cdot y = \begin{bmatrix} a \\ b \end{bmatrix}$$
Its linear algebra! Can be solved with $y=ax+b$ in simple cases. Basic idea, take some data and fit a line to it.

You can either solve this like above or you can use `sklearn` and solve it with ML

**Vocab**
- Hyperparameter -> configuration variables
- Parameters -> variables in the model (parameters are adjusted to match the model to the data)

### How it actually works
The parameters are optimized using something like Gradient Descent

**Gradient Descent**
Minimization/Maximization of some target (or objective) function that is related to both the data and the model and describes something useful to minimize/maximize. Selecting a function is the tricky and important part. A few examples include:

>$L_1$  ->First order absolute value of error from the data to the predicted value
>$L_2$  -> Second order square of the error from the data to the predicted value (Weights outliers more)
>$\chi^2$ -> Big fancy guy that compares the data to the probability it is normal (Weights the outliers less)

The higher the power the more outliers are prioritized 🤯

![[Pasted image 20231013155837.png|600]]

**Stochastic Gradient Descent**
Adds in stochastic variability by only taking one random batch of data every iteration. This helps converge towards true minima and not just local minima.
![[Pasted image 20231013155908.png|200]]
![[Pasted image 20231013155938.png|600]]


**Montecarlo Markov Chain**
Gives you the parameter values and their probabilities at the same time

Markovian process -> no hysteresis, not dependent on prior state, kinda like holonomic

The biggest and trickiest part here is deciding how to take the next step

