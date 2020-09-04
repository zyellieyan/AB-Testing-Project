# AB-Testing-Project
## Experiment Overview: Free Trial Screener  

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.


In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.

<img src="Experiment_Screenshot.png">

## 1. Experiment Design
### 1.1 Unit of Diversion (provided by Udacity)
The unit of diversion is a **cookie**, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

### 1.2 Initial Hypothesis 
The hypothesis was that this might set clearer expectations for students upfront, thus **reducing the number of frustrated students who left the free trial** because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course. (Provided by Udacity)  
  
Based on the information above, we can set some initial hypotheis: (these are just initial hypothesis and we will revise them further)

  1. H0: the change has no effect on the number of students who enroll the free trial.  
     H1: the change reduces the number of students who enroll the free trial.
   
   
  2. H0: the change has no effect on the number of students who leave the free trial.  
     H1: the change reduces the number of students who leave the free trial.  
    
    
  3. H0: the change has no effect on the probability of students who continue the free trial after 14 days.  
     H1: the change increases the probability of students who continue the free trial after 14 days.  
     (since we cannot say the number will be increased or decreased here, we use probability.)
     
### 1.3 Metric Choice
there are seven choices from Udacity below.

- **Number of cookies**: That is, number of unique cookies to view the course overview page. (dmin=3000)
- **Number of user-ids**: That is, number of users who enroll in the free trial. (dmin=50)
- **Number of clicks**: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)
- **Click-through-probability**: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)
- **Gross conversion**: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
- **Retention**: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
- **Net conversion**: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)  


dmin means the practical significance boundary for each metric, that is, the difference that would have to be observed before that was a meaningful change for the business, is given in parentheses. All practical significance boundaries are given as absolute changes.

#### 1.3.1 Choosing Invariant Metrics
The invariant metrics (control variables) are metrics that we expect not to change between experiment and control group. And they will be used for sanity check

Based on the metrics above, we would like to treat these three as invariant metrics: 
- **Number of cookies**
- **Number of clicks**
- **Click-through-probability**: Number of clicks / Number of cookies

Because these are the data that we can collect before the change, so they must nearly be same in both test and control group. And these three are all dependent on cookies, which is the unit of diversion for this project.  
Also, for the click through probability, though it covers both number of cookies and number of clicks, it might be unexpected vary during sanity check, so it is much safer to track those three.


#### 1.3.2 Choosing Evaluation Metrics
For evaluation indicators, we should not only care the statistical significance, but also whether the change of the metric is significant in practice (dmin), because the treatment may not be worth implementing (although there is statistical significance).  
    

We consider these three metrics below can be used for our A/B testing project.
- **Gross conversion**: we expect this metric will be decreased in treatment group. (users get the question and will have a second to consider enroll or not, this will filter some users who are not willing to checkout after free trial period).  

- **Retention**: we expect this metric will be increased in treatment group. (since the question has already filtered some uses who are not willing to checkout after free trial period, the retention rate should get higher).  

- **Net conversion**: since it is the product of the two above, we cannot expect its direction, which may vary.   
  
  
  
- **Attention:** 
  **Number of user-ids**: for this metric, we won't use it because:
    - User-ids are tracked only after enrolling in the free trial, so we cannot expect the equal distribution between the control and experimental group. It isn't normalized.
    - Also, the information about number of user-ids can be inculded in the gross conversion. 

#### 1.3.3 Hypothesis Revised
Based on the metrics we choose and the initial hypothesis, we can revise our hypothesis.

 1. H0: Gross Conversion(control) = Gross Conversion(treatment)  
    H1: Gross Conversion(control) != Gross Conversion(treatment)
 2. H0: Retention(control) = Retention(treatment)  
    H1: Retention(control) != Retention(treatment)
 3. H0: Net Conversion(control) = Net Conversion(treatment)  
    H1: Net Conversion(control) != Net Conversion(treatment)
    
## 2. Measuring Variability  

This csv file below contains rough estimates of the baseline values for these metrics (again, these numbers have been changed from Udacity's true numbers).  

For each of the metrics the standard deviation is calculated for **a sample size of 5000 unique cookies** visiting the course overview page.

### 2.1 baseline values
