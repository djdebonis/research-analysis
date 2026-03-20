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

### Independent T-Test

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

# you will want to print means and stds at the same time so you have all of the relavant
# data at the same time
print(f"Exercise Group: Mean: {exercise_scores_post.mean()}, std: {exercise_scores_post.std()}")
print(f"Sedentary Group: Mean: {sedentary_scores_post.mean()}, std: {sedentary_scores_post.std()}")
print(t_result_post_cong_incong_combined)
print("t-statistic:", t_result_post_cong_incong_combined.statistic)
print("p-value:", t_result_post_cong_incong_combined.pvalue)
```

And, the output I get is:

```
Exercise Group: Mean: 722.810606060606, std: 146.06517947482098
Sedentary Group: Mean: 746.9957627118644, std: 142.53799157319776
TtestResult(statistic=-1.0941352910900843, pvalue=0.2753415969243852, df=182.0)
t-statistic: -1.0941352910900843
p-value: 0.2753415969243852
```
So, just looking at the summary statistics, you can see the two groups are pretty close together. However, we can't just vibe it out and go by what "seems" close. We need to use a t-test to determine if they're stasticially different.

So, because our p-value is above 0.05, we fail to reject the null hypothesis. This means there is *not enough evidence* to suggest there is a stastically significant difference between these groups.

In this case, this is actually not bad for the research, because remember we are only using this for example. With this data, we are more interested in *difference between* pre and post data, as well as differences in the congruent vs incongruent data (Stroop Effect).


##  Paired T-Test (Same Group)

Remember, this is
Paired T-Test
- same participants (paired data)
- numerical data
- differences roughly normal

So, for this one, I am curious if exercise intervention had a stastically significant effect on incongruent scores (scores where the ink color of the word is different from the denotation of the color word displayed). As a simple interpretation of Stroop Data, this might suggest that low intensity exercise could improve inhibitory control directly after. I still ahve my pre_average and post_average from before, so now I will just compare them based on group. First I'll filter

```python
# now, we will look at exercise incongruent pre vs exercise incongruent post
# and see if there is a stastically significant difference
# let's filter our dataframe so we are only looking at incongruent scores
# for the exercise group. This is for simplicity.
stroop_data_incong_ex = stroop_data[
    (stroop_data["score_type"] == "incongruent") & 
    (stroop_data["group"] == "exercise")] 


```

Now I will run the test

```python
stroop_data_incong_ex_pre = stroop_data_incong_ex["pre_average"]
stroop_data_incong_ex_post = stroop_data_incong_ex["post_average"]

t_result_post_incong_ex = stats.ttest_rel(stroop_data_incong_ex["pre_average"], 
                                          stroop_data_incong_ex["post_average"])

# you will want to print means and stds at the same time so you have all of the relavant
# data at the same time
print(f"Pre Scores: Mean: {stroop_data_incong_ex_pre.mean()}, std: {stroop_data_incong_ex_pre.std()}")
print(f"Post Scores: Mean: {stroop_data_incong_ex_post.mean()}, std: {stroop_data_incong_ex_post.std()}")
print(t_result_post_incong_ex)
```

and we get this as an output:

```
Pre Scores: Mean: 861.2272727272727, std: 160.8618919926486
Post Scores: Mean: 779.7878787878788, std: 149.7222444177379
TtestResult(statistic=5.51417457025686, pvalue=4.449254900088657e-06, df=32)
```

So, although there appears to be a difference between pre and post scores, our test statistic and pvalue suggest that it is not a stastically significant difference.









