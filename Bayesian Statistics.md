#Statistics #Bayesian
[[Index - Data Science for Physical Scientists]]

Based on bays theorem
$$ P(\text{model}|\text{data})P(\text{data}) = P(\text{data}|\text{model})P(\text{model})$$
"I have a model, what is the probability that my model matches this data". Does not require fair data.

![[Pasted image 20230915161323.png|center]]

**Likelihood** - the probability of my data according to the model
**Prior** - The probability that my model is correct
**Marginal** - is absurd and basically never known
**Posterior** - how probable our result is given all the stuff

**Because we only want to compare data**
![[Pasted image 20230918153032.png|300]]

**Def** - Support 
The number space that the probability exists on (the `linspace`, the number line, etc...)

**The core idea** -> By multiplying what we see in the data, by what we expect in the data we can get the probability of what we see