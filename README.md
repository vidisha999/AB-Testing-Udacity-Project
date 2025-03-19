# AB-Testing-Udacity-Project

## Project Description 
This project is about A/B testing experiment conducted by Udacity, an e-learning platform. The experimental change done in this test is to trigger a screen which displays a message asking weekly time users can commit once they clicked on the  free-trial button. If the input value is less than 5, a message would appear mentioning ' Udacity courses take greater time commitment for successful completion and its suggests students might like to access the course material for free'.

 ## Project Goal 
The goal of conducting the A/B test with new experimental change is to reduce the number of users who leave the course at the end of the free-trial period by directing them to use free course materials while increase the retention after 14 days free trial period, by encouraging users only with enough time commitement to enrolled into free-trial plan.

## Background 
A/B testing is a statistical method which is use to help businesses make data-driven decisions regarding the impact of a new change on the business. A/B test composed of two variants in the experiment, which are control setup with default conditions and the experiment setup with the new chnage introduced. The test is performed to identify if there is a change in the measured value of the selected metric in both setups. The selected metric could be metrics such as KPI, signup rates, click through rates which  use to evaluate the business impact of the new change. A/B test stands at a gold standard in the hierarchy of evidence pyramid, which results gaining highest possible accuracy on a statistical test due to higher causation and less correlation or assumption basis.

## Data 
Dat for this project is provided by Udacity. It has provided ![baseline data]('baseline_values.csv')  meassured at the start of the experiment as the reference point for comparison. The data collected for ![control group]('control.csv') and ![experiment group]('Experiment.csv') are provided in seperate datasets in order to conduct the A/B test.

Each column in the dataset is defined by Udacity as:

- `Pageviews`: Number of unique cookies to view the course overview page that day.
- `Clicks`: Number of unique cookies to click the course overview page that day.
- `Enrollments`: Number of user-ids to enroll in the free trial that day.
- `Payments`: Number of user-ids who who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)
## Experiment Setup 
### Unit of Diversion 
The unit of diversion is a unique cookie as provided by the Udacity. This cookie tracks the unique users who enroll on the free-trial option on each day by assigning them a user-id. For the users who don't enroll, were not given a user-id to track them even though they were signed in when they have to visit the course overview page.

### Initial Hypothesis
Based on the project challenge, following initial hypothesis were initially derrived. But these were later changed after data analysis and selection of metrics to perform the test.

- H0: The treatment has no effect in the number of students who enroll in the free-trial.
- H1: The treatment reduces the number of sudents who enroll in the free-trail.
----
- H0: The treatment has no effect in the number of students who leave at the end or during the free-trial.
- H1: The treatment reduces the number of students who leave during or after the end of free-trial.
----
- H0: The treatment has no effect in the number of students who retain after the end of free-trial period.
- H1: The treatment increases the probability students who stay after the end of free-trail.
----
- H0: The treatment has no effect in the number of students who use free-course material .
- H1: The treatment increases the probability students who start using free- course material.
### Metric Choice 
Udacity has provided a list of metrics which can be used as either an evaluation metric or an invariant metric ,with their practical significance level. Both types of metrics are important in conducting an A/B test.

**Invarinat Metric** :

Metrics that don't chnage during the whole duration of the experiment and can be considered as control variables. They are important metrics which are use for internal and external sanity checks before performing complex evaluation of the AB test or deploy the test in the production environment.

- *Number of cookies* : Number of unique daily cookies to browse course overview page (**dmin=3000**)
- *Number of clicks* : Number of unique daily cookies to click the free-trial button (**dmin=240**)
- *Click through probability*: (Number of clicks/ Number of cookies) (**dmin=0.01**)

**Evaluation Metric**: 

Metrics which can be meassurable easily and should be sensitive enough to pickup intended chnages and robust to unimportant changes. Once users enrolled in the free-trial plan , they are tracked through unique user-id. Hence the number of user-ids are only meassured after the treatment is introduced,it is not evenly distributed across control and experiment groups, causing a non-normal distribution and so that it's not considered as an evaluation metric.

- *Gross Coversion* : (The number of user-ids to complete checkout and enroll in free-trial plan / Number of unique cookies to click on the free-trial button)(**dmin =0.01**)
- *Retention* : (The number of user-ids to remain enrolled past free-trial plan/ Number of user-ids completed checkout and enrolled in free-trail) (**dmin=0.01**)
- *Net Conversion* : The number of user-ids to remain enrolled past free-trial plan/ Number of unique cookies to click free-trial button(Gross conversion * Retention)(**dmin=0.0075**)

When deciding the evalutaion metrics, there are few parameters which use to identify if the selected metrics can cause a meaningful difference in the experiment.

- **Statistical Power (1-β)** : The probability of detecting the value of a pre-determined size or higher that value, if the treatment has caused a difference in the meassured metrics.
    - **β** here is the **False Negative probability** or **Type II error** in a statistical test.
    - **False Negative** : Fail to reject the null hypothesis and mistakenly conclude there is no difference in the meassured metrics caused by the treatment, when it really does.

- **Significance Level(α)** : False positive probability or Type I error in a statistical test.
     - **False Positive** : Mistakenly rejects the null hypothesis and concludes there is a difference in the meassured metrics caused by the treatment, when there is no real effect.
     - **Minimum Detectable Effect**- The minimum size of the metric that is considered significant to the business in order to change the default actions.Refer as the practical siginificance level

### Meassuring variability of Metrics
Baseline data is used to  perform severla pre-processes and pre-analysis needed to perform before implementation of the A/B tests. The  variability of each evaluation metrics is determined by calculating the standard error, based on the sample sizing suggested by Udacity. Then the experimental sample size was calculated to find the total count of pageviews require to test each of the hypothesis defined at the begining of the test. With the results gained for the count of pageviews, the experiment duration and exposure was determined. 

In order to perform above processes, parametric statistical tests were used and they were based on assumptions made to the dataset. 

***Assumptions***

- The fundamental assumption that is made when performing parametric tests is assuming a normal distribution in sample distribution of the chosen metric, in order to allow compute p-values and significance levels.
- To perform analytical estimate of the standard error of each evaluation metric, the sample distribution of them should be known; binomial or normal and a large enough sample size allowing central limit theorem(CLT).
- This get rids of the need of doing empirical estimation which require bootstrapping samples which is computationaly expensive.

***For the A/B test***:

- The unit of diversion is same as the unit of analysis (denominator) in all evaluation metrics. So, that outcomes can be model as bernouli trials for each entity making the sample proportion follow a binomial dristribution.
- According to Central Limit Theorem(CLT), the binomial distribution of the sample proportion approaches a normal distribution when the sample size is large enough, and the assumption of normal distribution can be made even though the underline distribution is binomial.
- Since 5000 cookies, 400 clicks and 83 user_ids are large enough sample size, its assumed that the sample proportion distribution of each evaluation metric approaches a normal distribution. It can be further verified using the formula ```np≥10 and n(1−p)≥10```
  
All three analytical metrics passed the valuduty test for the assumptions, so it can proceed with resizing the data sample based on the sizing of 5000 cookies provided by Udacity and then calculate the estimated standard error for each of the metrics using the formula ```sqrt(p(1-p)/n)```.Below tables show the calculated sample sizes of invariant metrics and derrived statndard error of evaluation metrics. 

**Sample size of Invariant Metrics**

|Metric|Sample size|
|:----|:----|
|cookies|5000|
|clicks|400|
|user id|82.5|

**Estimated Standard Error of Evaluation Metrics**

|Metric|Standard Error|
|:---|:---|
|gross conversion|0.020231|
|retention|0.054949|
|net conversion|0.015602|

***Experimnetal Sample Size***

Given the statistical power of the test as **0.80**, hence **β=0.20** and **α=0.05** , the experimental sample size needed to meassure each evaluation metric is calculated, using **two-proportions z-test** under the normal approximation of binomial distributions. The below table shows the values derrived for the experimental sample sizes of the evaluation metrics.

|Metric| Experimental Sample size|
|:---|:---|
|gross conversion|51115.224893|
|retention|78173.219325|
|net conversion|54826.675793	|

### Experiment Exposure and Duration

Considering there are no other experiments perform simaltaneously during this time, when 100% of the traffic divereted into this experiment in which 50% of the users would be in the treatment group:
- The days required  to conduct the experiment with net conversion, retention and gross conversion is :**118 days**
- The days required to conduct the experiment with net conversion and gross conversion is :**17 days**

The number of daily cookies given in the baseline dataset is 40000. Due to seasonality and risk of using 100% of traffic in the experiment, the duration of the gross conversion and net conversion can be further extended by 17 more days by diverting only 50% of traffic in this expxeriment.
- The days required for the experiment with 50% traffic diverted for gross conversion and net conversion, is :  **34  days**


### Multiple Hypothesis

As multiple hypothesis were initially made for this project, if it was decided to perform the A/B test with multiple hypothesis, then statistical parameters such as p value should be compared against **Family Wise Error Rate(FWER)**.

- **FWER** : The probability of making at least one Type I error (false positive rate)
- **Boneferroni correction method**: The correction to the significance level(α) to control the FWER

If performing multiple hypothesis tests, it require more pageview count which is 118 days. Doing this could be risky due to several reasons. It can cause high opportunity cost due to long duration of the experiment. As well as if the treatment harms the user experience, it can lead to degradation of conversion rates and have the trend unnoticed for a long time leading a huge business risk. Considering all above conerns,we will remove retention as an evalutaion metric and use only gross conversion rate and net conversion as only evaluation metrics. Especially since net conversion is a product of rentention and gross conversion, so that we might be able to draw inferences about the retention rate from the other two.

Using only gross conversion and net conversion simplify the A/B test to use only Hypothesis III.

## Data Analysis

**Sanity checks for internal and external test validity**

To ensure the experiment will run properly, sanity checks for internal test validity should be performed on the invariant metrics. As invariant metrics should stay constant throughout the duration of the experiment, there should be no significant difference between these metrics in the control and experiment group. The invariant metrics selected for this project are:

- Number of clicks
- Number of cookies
- click through probability

***Cookies***

The number of unique cookies of the experiment is the count of pageviews which is also the sample size of each group.The experiment group has a larger sample size than the control group, causing a imbalanced data distribution between two groups at a glance. It is important to determine if the effect of this imbalance distribution has a small effect or larger effect. The presence of the **Sample Ratio Mismatch (SRM)** of the two groups can be determined by usig a **Chi_square goodness of fit test** or by calculating **Sample Size Ratio**.

Method I :

```Sample Size Ratio = max(n_A, n_B) / min(n_A, n_B)```

- If Ratio >0.8 , then sample sizes are balanced and test should perform well.
- If 0.5 < Ratio ≤ 0.8, then sample sizes are moderately imbalanced.
- If Ratio ≤ 0.5, then sample sizes are highly imbalanced.

Method II

 ```chi² = Σ ((Oᵢ - Eᵢ)² / Eᵢ)```

- if p_value < significance level, then a SRM maybe present in the experiment
- if p_value >= significance level, then a SRM may not presert

The sample size ratio calculated using *Method I* is **1.0025**  and is larger than 0.8. When chi-square goodness fit was performed using *Method II*, the resulted p_value was **0.289** and is less than significance level **(alpha=0.05)**. Using both methods, it can be determined there is no presence of sample ratio mismatch in the test groups. So the sample distribution between control and experiment groups is considred as a balanced distribution and should be good to perform the A/B test.

***Clicks***

In order to check if the count of clicks was evenly distributed among both groups and to validate the randomization, several statistical tests can be performed.If considerd, being assigned to the control group as a success, the binominal distribution can be used to model the number of successes in the given sample (treatment+control). For this, a **binomial test** along with calculating **confidence intervals(CI)** can be performed as a sanity check.

Method I :

```CI = [p̂ - Z(1 - α/2) * SE, p̂ + Z(1 - α/2) * SE]```
where:

- p̂ = sample proportion
- Z(1 - α/2)= critical value of the confidence interval
- SE = standard error of the proportion

If proportion of number of successs in the sample is within the interval,then there is no significance difference in the data distribution between test groups.

Method II :

Alternatively, **one-proportion z-test** can be performed as a sanity check to determine if any of the control or experiment group has got lesser or higher than the 50% of the data which is the total count of clicks when distributed evenly among groups:

```Z = (p̂ - p₀) / sqrt((p₀ * (1 - p₀)) / n)``` 

where:

- p̂= sample proportion (number of successes divided by the total sample size)
- p₀= hypothesized population proportion,
- n= sample size.

If P value >= significance level, then SRM may not present in the experiment.

By performing bionmial test using *Method I* , the calculated p_value is **0.500** and it lies within the confidence interval of **(0.496, 0.505)**. When **one-proportion z test** was performed as in *Method II*, the calculated p_value is **0.824** and its larger than alpha =0.05. As it was proved using both these methods, there is no significant difference in the data distribution between groups, and SRM may not present.

***Click Through Rate (CTR)***

Since the normal approximation can be assumed due to large sample size, **two proportions z test** can be used to determine if the population of the each groups are significantly different.Alternatively **confidence intervals** can be used as another satistical test for validity of the sanity check, by assuming a binomial distribution in the underline data distribution of each group.

 ```Z = (p̂₁ - p̂₂) / sqrt(p_pooled * (1 - p_pooled) * (1/n₁ + 1/n₂))```
  
Where:

- p̂₁ = Observed proportion in group 1
- p̂₂= Observed proportion in group 2
- p_pooled= Pooled proportion
- n₁,n₂= Sample sizes of each groups

The p_value calculated using **two_proportion z test** method is **0.932** and its greater than alpha. So, that CTR proportions are not significantly different between groups and they have a balanced distribution.

Now,since the experiment passed sanity checks of internal test validity for all invariant metrics,its ready to proceed with the A/B test to analyze the evaluation metrics.

## Test Analysis

Payments were tracked 14 fewer days than the other metrics in the dataset, given their start date.Looking at the loaded data, the payments are tracked for 37 days, which is 23 days to get enrolled and 14 days afterwards to make a payment. To be fully accountable for the 14 days free trial period only a period of 23 days is considered as the duration of the experiment. So that first 23 entries of the each datasets is enough to compute statistical meassures. This could alter the targeted sample size of the experiment. The calculated true sample size of **423,525** should be used for statitical computations in the evaluation metrics.

**Hypotheis Revised**

Gross Conversion(GC)

H₀: GC_experiment = GC_control
H₁: GC_experiment ≠ GC_control

Net Conversion (CN)

H₀: NC_experiment = NC_control
H₁: NC_experiment ≠ NC_control

**Graphical Evaluation of the evaluation metrics**

Based on the both plots ![variability of Net conversion between control and experiment]() and ![variability of gross conversion between control and experiment](), on Oct 24 th , there was a huge peak in the both conversion rates among both groups, and it needs further investigation about this day to check if there were any special offers or discounts for the users browsing the website.

**Practical and Statistical Significance**

*Statistical Siginificance*

  - In order to check if the observed difference between the sample and experiment groups are statisticaly significant, statistical method , **two proportions z test** can be used to compute the **p_value** associated with the Z_statistic and compare it against the given siginificance level (alpha).
  - 95 % **confidence intervals** can be computed for the difference between metrics to check if near 0 value lies within the interval, in order to determine the statistical significance of the test.

*Practical Significance*

- However, having the test statistically siginificant is not enough to launch the targeted features.The impact must be higher than the **minimum detectable effect (dmin)** in order to considre the effect as practical significant.
- If the dmin is not given then **cohens'h** metric can be calculated to evaluate the size of the effect and determine if its meaningful.
- If the dmin is given, the practical siginificance of the test can be found out by checking if the observed difference is higher or lower than the dmin and if the dmin contains or overlap with 95% confidence interval.

        - if dmin is positive:
            - Check if difference (d) is greater than dmin⁡.
            - Ensure that the entire confidence interval lies to the right of dmin⁡.
            - If both conditions hold → practically significant.

        - if dmin is negative (decrease in meaningfulway):
            - Check if difference (d) is smaller than dmin⁡.
            - Ensure that the dmin⁡ is outside and to the right of confidence interval.
            - If both conditions hold → practically significant.


|Metric|Observed Difference|dmin|p_value|confidence interval| statitical significane| practical significance|
|:--|:--|:--|:--|:--|:--|:--|
|gross conversion|-0.0205|-0.01|2.578e-06|(-0.029,-0.0119)|yes|yes|
|net conversion|-0.0049|0.0075|0.1558|(-0.0116,0.0019)|No|No|

The above table shows, the observed difference made by the gross conversion metric in control and experiment groups is both statisticaly and practically significant, while neither of significance was made by the net conversion metric.
## Seasonality Analysis 

Udacity requires to complete ths result analysis by runing a sign test. Sign test is a non-paramteric test that is use to determine whether there's a consistent directional difference between the control and experiment groups without assuming the normal distribution. Performing this test would violate the fundamental assumptions made in this A/B test in regards to normality of the sample distriibution. Instead of the sign test, an analysis can be performed on the seasonality of the test by breaking down the user acitivity for the day of the week.

|Day|GC_difference|NC_difference|
|:--|:--|:--|
|Mon|-0.020228|-0.007257|
|Tue|-0.024932|-0.006806|
|Wed|-0.017096|0.009921 |
|Thu|0.017814|-0.005035  |
|Fri|-0.025138| -0.021883 |

Based on the above table, most days of the week results a negative effect on the observed difference, which means treatment group had less gross conversions compared to control group as expected .But on **Thursday** , a positive effect was observed and it is important to further analyze the website traffic divertion on Thursdays.

For net conversion, many days results a negative effect on the observed difference, which is not expected and not allign with the main goal of introducing this feature. **Wednesday** is the only day of the week when a positive effect was observed.It is also in a small amount of **0.9 %** more users in the treatment group than control group made payments.

## Results Analysis
Based on the results gained from the statistical tests for the gross conversion and net conversion metrics, the effect made by the gross conversion was negative, which means the observed gross conversion in treatment group is around 2% smaller than that of the control group. Since the minimum detectable effect (dmin) for gross conversion falls outside the entire confidence interval  and it is smaller than the lower limit of the interval, the observed difference in the gross conversion is considered both statisticaly and practically significant. So, that launching the new features may make a business impact, considering the gross conversion metric.

Looking at the net conversion, the statistical test results shows fail to reject the null hypothesis , hence the observed difference is not statisticaly significant.Therefore, it doesn't have a practical significance. The observed net conversion in experiment group is about 0.45% smaller than the control group with a small negative effect.

## Conclusion 

Based on the result analysis, this test would help make clearer expectations among students upfront. However since only the gross conversion is statisticaly and practicaly significant, the expectations aimed for increase of the net conversion as in the initial hypothesis wouldn't be acheived by launching this feature. This experiment is effective for reducing the free-trial enrollements but that wouldn't help imporove the payaments made by users. A potential cause of negative effect made by the net conversion could be due to, users who diverted to the use of free course material have improved as a result of the decrese of the gross conversion. Although this test provide students with a clearer expectations when making the enrollment decision, from a business perspective it would have a negative effect on the number users who make payments beyond the free trial period. If the expectation of the Udacity is to improve the revenue alongside of minimizing number of students who leave the free trial plan, I would recommend not to launch this feature. 

## Future Improvements

1. Reduce free-trial cancellations:

 The main expectation of the Udacity is to reduce the users leaving the free-trial plan before making the first payment.This effect can be meassured at two reference points, which are before and after enrolling to the free-trial plan. In this experiment we made the hypothesis IV regarding the improvement of users who enroll in free course material. Somehow give data has only meassured user activities after enrolling in the free-trial plan and therefore we had to eliminate testing hypothesis IV. With the decrease of the gross conversion rate, there is definete possibility of increase of the users who enroll in the free course materials. When students have enough understanding from the course material, there's a less probability of them leaving the course for being frustrated if they decided to enroll the paid plan later. So, goal should be to convert the users who use free course material to paid plan later.

  For this change, instead of only adding time commitment question to the form, few more questions related to pre-requiste and goal of earning a certification of completion should be added to the form. So, that students who enrolled in the free-trial plan are the ones who fulfill all these criteria and it can guarantee they will retain until the end of the cousre to earn the certificate. Users who don't enroll or enroll in free trial plan can later join the paid plan to earn certificate if they don't fulfill all criterias listed in the form. 

   To implement this new chnage to the test, we should meassure click through rate of free-course material tab as an evaluation metrics. This would be the number of unique daily cookies that click on the free-course matrial tab ddivided by unique cookies that visited teh course overview page on that day. By collecting a new dataset with thes changes, we can test the experiment for the hypothesis IV and can draw results based on that aspect as well.

2. Improve the current features

For the users who alreday enrolled in the free-trial plan, their user experience can be further improved by providing one on one tutoring support and assigning students into groups. So, that they can share each others their thoughts and knowledge on solving difficult problems or work on course projects as a group. It would also improve the colloarative mindset of students which would pre-train them on working in an industrial environment.




