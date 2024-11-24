[[Index - Data Science for Physical Scientists]]
Moments give us a way to describe the shape of a distribution. Moments are all about the distance to a reference point of every data point in the sample (Like an engineering moment!)
# First Moment (Mean)
The first moment tells us roughly where the data falls and is just the mean of the data set
$$ \mu'_1 = \frac{\sum x_i}{n}$$
Where $x_i$ is a given data point in the sample and $n$ is the number of data points in the sample
# Second Moment (Variance)
The second moment is about the shape of the data set, how spread out it is. The crude version of it is just the sum of the squares divided by the total number of data points. 
$$ \mu'_{2 \text{ crude}} = \frac{\sum x^2_i}{n}$$
Where $x_i$ is a given data point in the sample and $n$ is the number of data points in the sample. In this crude version, larger numbers correspond to more spread on the data, however it does not let us compare well between drastically different data sets. By subtracting off the first moment (the mean) and centering the data we can achieve a more useful measure of spread
$$ \mu'_{2 \text{ centered}} = \frac{\sum (x_i - \mu'_1)^2}{n} $$
Now, things are subtracted off of the mean and larger numbers correspond directly to how spread out the data is. 

# Higher Order Moments
Crude higher order moments can be achieved by simply squaring, cubing, or taking x to the 4th or higher powers. One can center and then standardize the 3rd and 4th moments in order to get skewness, and kurtosis
## Third Moment (Skewness)
$$ \frac{1}{n} \frac{\sum(x-\mu)^3}{\sigma^3}$$ 
Where $\sigma$ is the population standard deviation
## Fourth Moment (Kurtosis)

$$ \frac{1}{n} \frac{\sum(x-\mu)^4}{\sigma^4}$$
Where $\sigma$ is the population standard deviation. Interestingly, we don't have to normalize for the skewness because kurtosis does not rely on skewness as the lower 3 moments do. I.e. for 1st, 2nd, and 3rd order each lower order must exist for the next one to exist.

# Sampling Adjustments
When working with a data set that is a sample of the population, you only have the sample mean and std dev, so the above equations must be adjusted

#### 1st Moment
Does not change as there are no "population parameters"
$$ \mu'_1 = \frac{\sum x_i}{n}$$
#### 2nd Moment (sample variance)
Must change because $\mu'_1$ is a population parameter
$$ \mu'_{2 \text{ centered}} = \frac{\sum (x_i - \mu'_1)^2}{n} \ \longrightarrow \ \frac{\sum(x_i - \bar{x})^2}{n-1}  $$
What be $\bar x$

![[Pasted image 20230918132548.png|400]]
