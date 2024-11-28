#  Evaluating Study Time Differences Across Schools       
      
## Table of Contents            
1. [Data](#data)   
2. [Results](#results)  
3. [Analysis Approach](#analysis-approach) 
4. [Conclusion](#conclusion)  

## Data
The dataset consists of the time (in hours) that students from three different schools spent studying or doing homework during an exam period:
- **School 1:** 25 observations
- **School 2:** 23 observations 
- **School 3:** 20 observations

## Results
Given the limited sample sizes, typical frequentist methods may not yield significant results. Therefore, we adopt a Bayesian approach. Using a normal model with a conjugate prior, we calculate the posterior means for each school, which are approximately:
- **School 1:** 9.29 hours
- **School 2:** 6.95 hours
- **School 3:** 7.81 hours

To construct 95% confidence intervals for these means, we implement a Monte Carlo procedure, drawing 1,000 random samples from the distributions. The confidence interval for School 1 does not overlap with those of the other schools, indicating a significantly higher average study time. In contrast, the confidence intervals for Schools 2 and 3 overlap, prompting further analysis of probabilities concerning which school's mean is greater.

The probability that the mean study time for School 3 is less than that of School 2, which is in turn less than 1, is approximately 0.4814 (the most likely outcome). The probability that the mean for School 2 is less than that of School 3 is 0.4719. Additionally, the probability that School 3 has a lower mean than School 2 is slightly greater than the complementary probability based on all outcomes.

<br>
<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/Graph1.jpg" alt="Study Time Differences Graph" style="width: 900px;" />


## Analysis Approach
1. **Posterior Means Calculation:** 
   - Use Bayesian methods to calculate the posterior means for each school based on prior distributions and observed data.
```
# Using monte carlo simulation for all three schools
# Mu0, var0, k0, v0 given
mu0 <- 5
var0 <- 4
k0 <- 1
v0 <- 2
#posterior mean for s1
n1 <- length(s1)
y_bar1 <- mean(s1)
var1 <- var(s1)
mu_1 <- (k0 * mu0 + n1 * y_bar1)/(k0 + n1)
```

2. **95% Confidence Intervals:**
   - Employ Monte Carlo simulations to derive confidence intervals for the means and standard deviations of study times for each school.

3. **Probability Comparisons:**
   - Assess the probabilities of the relationships between the means of different schools to determine which school has a statistically greater average study time.
<br>

<img src="https://github.com/RoryQo/Evaluating-Study-Time-Differences-Across-Schools/blob/main/Figures/graph2.jpg" alt="Study Time Differences Graph 2" style="width: 300px;" />


## Conclusion
The Bayesian analysis reveals distinct differences in average study times among the schools, with School 1 significantly outpacing the others. Further investigation into the overlapping confidence intervals for Schools 2 and 3 highlights the nuanced relationships between their average study times, suggesting that while School 1 is clearly superior, the competition between Schools 2 and 3 remains closely matched.
