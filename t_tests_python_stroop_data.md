# T-Tests in Python

By this point, your spreadsheet should be cleaned and ready to go. You should also already have a general understanding of your data from running descriptive statistics.

Next, we are going to run a t-test.

A t-test helps us figure out whether the difference between two groups is likely meaningful or whether it may have happened just by chance. This is the hypothesis texting we discussed earlier in the semester.

For example, you might want to know:

Did the walking group score differently than the sitting group?

Did students score differently on the pretest compared to the posttest?

Did one class period perform differently than another?

A t-test does not just tell us whether the numbers are different. It helps us test whether the difference is statistically significant.

Please note: as with the descriptive statistics section, what I am showing here is not always the most advanced or compact way to code. However, it is designed to help you clearly understand what is happening.

## Step 1: Import scipy.stats

To run a t-test, we need to import a function from a library called scipy.

`from scipy import stats`

This gives us access to statistical tools, including t-tests.

## Step 2: Understand the two main kinds of t-tests

There are different kinds of t-tests, but for most of our work, you will likely use one of these:

### Independent samples t-test

Use this when you are comparing two separate groups.

Examples:

- walking group vs. sitting group

- control group vs. treatment group

- class period 1 vs. class period 2

These are different people in different groups.

### Paired samples t-test

Use this when you are comparing two scores from the same people.

Examples:

- pretest vs. posttest

- before intervention vs. after intervention

- heart rate before exercise vs. after exercise

These are matched scores because they come from the same participants. The statistical implications are slightly different.

### Step 3: Prepare Data (Independent T-Test)

We are comparing two groups in the Stroop dataset:

`"exercise"`

`"sedentary"`

First, I am going to create a total score for `pre` and for `post.` The way I will do this is averaging pre and post data. **(Note: this is not really the proper use of stroop test data, as I'm combining congruent and incongruent. However, this example is likely closer to how your data will look if you're comparing two independent groups.)**

```python
# add columns averaging the two pre scores and the two post scores to create
# one cumulative pre score and one cumulative post score
stroop_data["pre_average"] = (stroop_data["pre1"] + stroop_data["pre2"]) / 2
stroop_data["post_average"] = (stroop_data["post1"] + stroop_data["post2"]) / 2
```
Let's say I'm just curious if the two groups have differences in their post scores, independent of their pre scores. I would use an independent samples t-test for this. So I could just create two groups and compare. 

#### Create two groups

```python
# create two dataframes, one for exercise scores post and one for sedentary scores post
# this is exemplary, and not best practice for stroop data (unless you were seeing if they were
# close enough to measure up against one another)
exercise_scores_post = stroop_data[stroop_data["group"] == "exercise"]["post_average"]
sedentary_scores_post = stroop_data[stroop_data["group"] == "sedentary"]["post_average"]
```
So, remember, *before we ever run* our tests, we should determine our p-value threshold. This helps us to be unbiased in our evaluation of the data. So, let's use the classic p-value of p < 0.05

Now, we will run our t-test. We will use the independent t test because they are two separate groups:

```python
t_result_post_cong_incong_combined = stats.ttest_ind(exercise_scores_post, sedentary_scores_post)

print(t_result_post_cong_incong_combined)
print("t-statistic:", t_result_post_cong_incong_combined.statistic)
print("p-value:", t_result_post_cong_incong_combined.pvalue)
```

And, the output I get is:

```
TtestResult(statistic=-1.0941352910900843, pvalue=0.2753415969243852, df=182.0)
t-statistic: -1.0941352910900843
p-value: 0.2753415969243852
```
So, because our p-value is above 0.05, we fail to reject the null hypothesis. This means there is *not enough evidence* to suggest there is a stastically significant difference between these groups.

In this case, this is actually not bad for the research, because remember we are only using this for example. With this data, we are more interested in *difference between* pre and post data, as well as differences in the congruent vs incongruent data (Stroop Effect).








