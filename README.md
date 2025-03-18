# AB-Testing-Udacity-Project

## Project Description 
This project is about A/B testing experiment conducted by Udacity, an e-learning platform. The experimental change done in this test is to trigger a screen which displays a message asking weekly time users can commit once they clicked on the  free-trial button. If the input value is less than 5, a message would appear mentioning ' Udacity courses take greater time commitment for successful completion and its suggests students might like to access the course material for free'.

 ## Project Goal 
The goal of conducting the A/B test with new experimental change is to reduce the number of users who leave the course at the end of the free-trial period by directing them to use free course materials while increase the retention after 14 days free trial period, by encouraging users only with enough time commitement to enrolled into free-trial plan.

## Background 
A/B testing is a statistical method which is use to help businesses make data-driven decisions regarding the impact of a new change on the business. A/B test composed of two variants in the experiment, which are control setup with default conditions and the experiment setup with the new chnage introduced. The test is performed to identify if there is a change in the measured value of the selected metric in both setups. The selected metric could be metrics such as KPI, signup rates, click through rates which  use to evaluate the business impact of the new change. A/B test stands at a gold standard in the hierarchy of evidence pyramid, which results gaining highest possible accuracy on a statistical test due to higher causation and less correlation or assumption basis.

## Data 
Dat for this project is provided by Udacity. It has provided ![baseline data](baseline_values.csv)  meassured at the start of the experiment as the reference point for comparison. The data collected for ![control group](control.csv) and ![experiment group](Experiment.csv) are provided in seperate datasets in order to conduct the A/B test.

Each column in teh dataset is defined by Udacity as:
Pageviews: Number of unique cookies to view the course overview page that day.
Clicks: Number of unique cookies to click the course overview page that day.
Enrollments: Number of user-ids to enroll in the free trial that day.
Payments: Number of user-ids who who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)

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




