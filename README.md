# Evaluating Study Time Differences Across Schools - Bayesian Analysis  
  
<table align="center">  
  <tr>  
    <td colspan="2" align="center" style="background-color: white; color: black;"><strong>Table of Contents</strong></td>
  </tr>
  <tr>
    <td style="background-color: white; color: black; padding: 10px;"><a href="#data" style="color: black;">Data</a></td>
    <td style="background-color: gray; color: black; padding: 10px;"><a href="#4-posterior-predictive-check" style="color: black;">Posterior Predictive Checks</a></td>
  </tr>
  <tr>
    <td style="background-color: gray; color: black; padding: 10px;"><a href="#1-bayesian-framework" style="color: black;">Bayesian Framework</a></td>
    <td style="background-color: white; color: black; padding: 10px;"><a href="#5-probability-of-ordering-the-means" style="color: black;">Probability of Ordering the Means</a></td>
  </tr>
  <tr>
    <td style="background-color: white; color: black; padding: 10px;"><a href="#2-posterior-means-calculation" style="color: black;">Posterior Means Calculation</a></td>
    <td style="background-color: gray; color: black; padding: 10px;"><a href="#6-bayesian-inference-summary" style="color: black;">Bayesian Inference Summary</a></td>
  </tr>
  <tr>
    <td style="background-color: gray; color: black; padding: 10px;"><a href="#3-95-credible-intervals-monte-carlo-simulation" style="color: black;">95% Credible Intervals</a></td>
    <td style="background-color: white; color: black; padding: 10px;"><a href="#results" style="color: black;">Results</a></td>
  </tr>
  <tr>
    <td colspan="2" align="center" style="background-color: gray; color: black; padding: 10px;">
      <a href="#conclusion" style="color: black;">Conclusion</a>
    </td>
  </tr>
</table>





## Data
The dataset consists of the time (in hours) that students from three different schools spent studying or doing homework during an exam period:
- **School 1:** 25 observations
- **School 2:** 23 observations
- **School 3:** 20 observations

The goal of this analysis is to evaluate and compare the average study time across these three schools. Given the relatively small sample sizes, Bayesian methods are employed to incorporate prior knowledge and account for uncertainty in the estimates of average study times.

## Analysis Methodology

### 1. **Bayesian Framework**
Bayesian analysis is used to estimate the posterior distributions of the average study times for each school. The key idea behind Bayesian inference is that we update our prior beliefs about a parameter (in this case, the average study time) using observed data to obtain a posterior distribution. This distribution incorporates both the prior beliefs and the observed data, allowing us to make more informed inferences about the parameter.

- **Prior Distribution:**
  A conjugate normal prior is assumed for the means of the study times. This means that the prior distribution for the average study time in each school is a normal distribution, with a known mean and variance.
  
  We also assume a conjugate prior for the variance, which is an inverse gamma distribution. This allows the posterior distribution for the mean to also be normal, and the posterior distribution for the variance to be an inverse gamma.

  The priors are chosen based on reasonable assumptions:
  - Mean prior  $\mu_0 = 5$ hours (a rough initial belief about the average study time).
  - Variance prior $\text{var}_0 = 4$ (reflecting a moderate degree of variability in the study times).
  - Degrees of freedom  $\nu_0 = 2$ and scale parameter $\( k_0 = 1 \)$ , which represent the strength of the prior.

- **Likelihood Function:**
  The likelihood is assumed to be normally distributed, representing the data's uncertainty in terms of the sample mean and variance. The observed data from each school informs us about the sample mean $\bar{y}$ and sample variance $\ s^2$.

  The likelihood function combines the prior and the observed data using Bayes' theorem to calculate the posterior distribution of the mean study time for each school.

### 2. **Posterior Means Calculation**
Bayesian methods combine the prior distribution with the data to calculate the posterior distribution of the mean study time for each school. The posterior mean is the expected value of the parameter given the data, which represents the best estimate of the average study time after incorporating both the prior beliefs and the observed data.

The formula for calculating the posterior mean for each school is:
```math
\mu_{\text{posterior}} = \frac{k_0 \mu_0 + n \bar{y}}{k_0 + n}
```

where:
- $\mu_0$ is the prior mean.
- $n$ is the sample size for the school.
- $\bar{y}$ is the sample mean for the school.
- $k_0$ is the prior weight (which determines how much the prior influences the posterior).

```
#posterior mean for s1
n1 <- length(s1)
y_bar1 <- mean(s1)
var1 <- var(s1)
mu_1 <- (k0 * mu0 + n1 * y_bar1)/(k0 + n1)
```

<p align="center">
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/means.jpg" alt="Posterior Predictive Check for School 1" style="width: 250px;" />
</p>


### 3. **95% Credible Intervals (Monte Carlo Simulation)**
To quantify the uncertainty in the posterior mean estimates, we use Monte Carlo simulations to generate posterior samples. This is done by sampling from the posterior distributions of the mean and variance.

- **Generating Posterior Samples:** 
  For each school, the posterior distribution for the mean is a normal distribution with a mean equal to the posterior mean $\mu_{\text{posterior}}$, and a variance equal to the posterior variance (which depends on the observed data and the prior).

  - **Posterior Variance:** 
    The posterior variance is calculated using the formula:
$$\sigma^2_{\text{posterior}} = \frac{1}{n + k_0}$$

    where $( n )$ is the sample size for the school, and $k_0$ is the prior weight.



  - **Monte Carlo Sampling:**
    10,000 posterior samples of the mean and variance are drawn using the normal and gamma distributions. These samples are then used to compute the 95% credible intervals by calculating the 2.5th and 97.5th percentiles of the posterior samples for each school.

    This provides a range of plausible values for each school’s mean study time, taking into account both the prior beliefs and the observed data.

```
# 95% CI for s1 theta (mean)
set.seed(100)
vn1 <- v0 + n1 
kn1 <- k0 + n1
s1n1 <- (1/vn1) * (v0 * var0 + (n1-1) * var1 + ((k0 * n1 )/kn1) * (y_bar1 - mu0)^2 )
s1_postsample <- 1/rgamma(10000, vn1/2, vn1 * s1n1 / 2)
theta1_postsample <- rnorm(10000, mu_1, sqrt(s1_postsample/(n1 + k0)))
```

<p align="center">
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/Graph1.jpg" alt="Posterior Predictive Check for School 1" style="width: 775px;" />
</p>

### 4. **Posterior Predictive Check**

#### Posterior Predictive Check - Graph 1: School 1
To assess the quality of the model fit to School 1's data, we plot the posterior predictive check alongside the observed data for School 1.  All three schools appear to have well fit models based on our PPCs.

```
# Generate posterior predictive samples for School 1 (mean and variance)
theta1_pred <- rnorm(10000, mu_1, sqrt(s1_postsample/(n1 + k0)))
```

<p align="center">
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/s1.jpg" alt="Posterior Predictive Check for School 1" style="width: 600px;" />
</p>


#### Posterior Predictive Check - Graph 2: School 2
A similar analysis is done for School 2, and the posterior predictive check is visualized for comparison:

<p align="center">
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/s2.jpg" alt="Posterior Predictive Check for School 1" style="width: 600px;" />
</p>


#### **Posterior Predictive Check - Graph 3: School 3**
Similarly, the posterior predictive check is performed for School 3, showing the comparison between observed and simulated data.

<p align="center">
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/s3.jpg" alt="Posterior Predictive Check for School 1" style="width: 600px;" />
</p>


### 5. **Probability of Ordering the Means**
In addition to calculating the posterior means and credible intervals, we are also interested in comparing the means of the schools. Specifically, we calculate the probabilities of different orderings of the means for the schools.

- **Pairwise Probability Calculations:**
  We compute the probabilities that one school has a higher mean study time than another, based on the posterior samples. For example, we calculate the probability that the mean study time for School 1 is greater than that of School 2, and the probability that the mean for School 3 is greater than School 2.

We do this by counting the number of times that a given ordering occurs in the posterior samples and dividing by the total number of samples. This allows us to quantify the likelihood of different orderings of the means.

```
prob = function(small, medium, large){
 sum = 0
 for(i in 1:10000){
 if(medium[i] > small[i]){
 if(large[i] > medium[i]){
 sum = sum + 1
 }
 }
 }
 sum/10000
}
```


<p align="center">
  <img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/graph2.jpg" alt="Posterior Predictive Check for School 1" width="300"/>
</p>


### 6. **Bayesian Inference Summary**
After conducting the above steps, we summarize the findings:
- **Posterior Means** provide a point estimate of the average study time for each school.
- **95% Credible Intervals** give a range of plausible values for the means.
- **Pairwise Comparison Probabilities** offer insights into how likely one school’s average study time is to be greater than another.
- **Posterior Predictive Checks** help us assess the quality of our model fit to the observed data.

## Results
Given the limited sample sizes, typical frequentist methods may not yield significant results. Therefore, we adopt a Bayesian approach. Using a normal model with a conjugate prior, we calculate the posterior means for each school, which are approximately:

School 1: 9.29 hours
School 2: 6.95 hours
School 3: 7.81 hours

We implemented a Monte Carlo procedure to construct 95% confidence intervals for these means, drawing 1,000 random samples from the distributions. While the confidence intervals for the means overlap between all Schools, the probability analysis indicates a clearer picture.

After computing the probabilities, we find that there is almost a 95% chance that School 1 has the largest average study time. Furthermore, there is about a 2% higher likelihood that School 2 has a larger average study time than School 3. If we were to rank the schools based on the probabilities, the most likely ordering of the schools would be School 1 > School 2 > School 3. School 2 has a larger average study time than School 3 52% of the time, so it is approximately 2% more likely to have higher study times than school 3. While schools 2 and 3 are still very close together, we can establish school 1 is most likely the school with the highest average study time.

## Conclusion
The Bayesian analysis provides a more nuanced understanding of the study time differences across schools. School 1 most likely has a higher average study time than the other two schools. However, Schools 2 and 3 have very similar average study times. The Bayesian methods used here—especially the posterior distributions, credible intervals, and probability comparisons—allow us to incorporate uncertainty and make more informed inferences about the data.

By employing a Bayesian framework, we are able to account for prior knowledge and obtain more reliable estimates, especially in the context of small sample sizes. This methodology allows for a more robust analysis and deeper insights into the relationships between the study times of the different schools.
