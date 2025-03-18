# AB-Testing-Udacity-Project

## Project Description 
This project is about A/B testing experiment conducted by Udacity, an e-learning platform. The experimental change done in this test is to trigger a screen which displays a message asking weekly time users can commit once they clicked on the  free-trial button. If the input value is less than 5, a message would appear mentioning ' Udacity courses take greater time commitment for successful completion and its suggests students might like to access the course material for free'.

 ## Project Goal 
The goal of conducting the A/B test with new experimental change is to reduce the number of users who leave the course at the end of the free-trial period by directing them to use free course materials while increase the retention after 14 days free trial period, by encouraging users only with enough time commitement to enrolled into free-trial plan.

## Background 
A/B testing is a statistical method which is use to help businesses make data-driven decisions regarding the impact of a new change on the business. A/B test composed of two variants in the experiment, which are control setup with default conditions and the experiment setup with the new chnage introduced. The test is performed to identify if there is a change in the measured value of the selected metric in both setups. The selected metric could be metrics such as KPI, signup rates, click through rates which  use to evaluate the business impact of the new change. A/B test stands at a gold standard in the hierarchy of evidence pyramid, which results gaining highest possible accuracy on a statistical test due to higher causation and less correlation or assumption basis.

## Data 
Dat for this project is provided by Udacity. It has provided ![baseline data](baseline_values.csv)  meassured at the start of the experiment as the reference point for comparison. The data collected for ![control group](control.csv) and ![experiment group](Experiment.csv) are provided in seperate datasets in order to conduct the A/B test.

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

Considering there are no other experiments perform simaltaneously during this time, when 100% of the traffic divereted into this experiment in which 50% of the users then will be in the treatment group:
- The days required  to conduct the experiment with net conversion, retention and gross conversion is :118.0
- The days required to conduct the experiment with net conversion and gross conversion is :17.0

The number of daily cookies given in the baseline dataset is 40000. ue to seasonality and risk of using 100% of traffic in the experiment, the duration of the gross conversion and net conversion can be further extended by 17 more days by diverting only 50% of traffic in this expxeriment.




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




