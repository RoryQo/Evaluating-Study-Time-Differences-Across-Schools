# R-School-Differences-Bayesian-Analysis

#### Data
Each data point is the time in hours a student spent studying or doing homework during an exam period

+ School 1 has 25 observations
+ School 2 has 23 observations
+ School 3 has 20 observations

#### Results

  Because of the limited sample size a typical frequentist approach will not yeild any significant results.  Therefore we will continue with a bayesian apprach.  Using the normal model with a conjugate prior we can obtain the posterior means for each school as approximately 9.29,6.95, and 7.81 hours spent studying per student.
 
  To create 95% confidence intervals for these means we will implement monte carlo precedure (for each school) drawing 1000 random samples from our distributions and then view the 0.025 and 0.95 quantiles of the resulting distribution.  the confidence interval for school 1 does not overlap with any of the other schools, so it is clear that school has a significantly higher average time spent studying than the other.  The other school means confidence intervals overlap, so we will continue to find probabilities for which school mean is greater.
  
  Theta 3 is less than theta 2, which is less than 1, with a prabability of 0.4814 (this is the most likely outcome).  However, the probability that theta 2 is less than 3 is less than 1 is 0.4719.  The probability that theta 3 is less than theta 2 is 1% greater than the compliment based on all outcomes.
