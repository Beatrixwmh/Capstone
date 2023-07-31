# An analysis of factors that contribute to opinions on legal reproductive rights
With the recent overturning of Roe v. Wade, the topic of legal reproductive rights have been more controversial and porlarizing than ever. Moreover, it is getting harder for organizations like Planned Parenthood to find the right demographic to advertise to. In this project, I will attempt to build a classification model using survey data from the General Social Survey, which gives information about the American demographic and their political opinions. The goal is to build a model that can predict whether someone is in favor of or against legal abortion based on facts about them, and to find characteristics those who share the same opinion have in common to give guidance to finding the right demographic for advertising. This allows Planned Parenthood to know where to focus their business efforts in and be more efficient with their resources.

![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/e1d1c918-2bfd-450f-8aa9-c0b27c532c82)

## Business and Data Understanding
### About the data
The data for the project was sourced from the [General Social Survey website](https://gss.norc.org/getthedata/Pages/Home.aspx), a research institution that surveys the american population yearly using full probability sampling with the aim of providing unbiased insight into various aspects of the general public. For the 2018 survey, there were over 2000 respondents, and each respondent was asked over 1000 questions about themselves and their opinions regarding different topics, giving researchers insight into the respondent's personal background and way of thinking. Moreover, full probability sampling ensures the data gathered is representative of the population. 
### Data Selection
The survey was structured such that respondents were free to skip questions they did not want to answer. To choose my independent variables such that there is minimal missing information for a more accurate analysis, I selected only questions with missing responses making up less than 3% of the dataset. I also referred to the [codebook](https://gss.norc.org/documents/codebook/gss_codebook.pdf) to understand what questions the columns represented and what their values mean, and did my best to choose variables that might give insight into the respondent's background and thus help the model make correlations between their attributes and their political opinions.
To create the right target variable for my model, I first needed to select the correct survey questions that are able to capture and reflect a respondent's opinion on the topic sufficiently. To do so, I consulted an article called [Updating A Time-Series of Survey Questions: The Case
of Abortion Attitudes in the General Social Survey](https://gss.norc.org/Documents/reports/methodological-reports/MR133%20Abortion.pdf), which was written by Sarah K. Cowan and published by NORC at the University of Chicago, the same institution that conducted the survey subject to this analysis. The article goes into detail discussing the correct questions to ask to capture public opinion about the ever-changing narrative surrounding the legality of abortion. It mentions a series of six hierichal questions that can be summed up as the Rossi scale, a six-point scale with one point given when a respondent responds 'yes' to the question. To demonstrate the hierichal nature of these questions, I will go through each of them and show the distribution of answers in the survey conducted in 2018. <br/>
**1. Should abortion be legal if the pregnant woman's health is at risk?** <br/>
![health](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/ad1ca000-8ab8-4cfb-bd78-0bece1071b27)

**2. Should abortion be legal if the pregnancy was a result of rape?** <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/f1fe0c12-fc2e-437e-a96d-52c7ea24c94d)


**3. Should abortion be legal if the fetus has a birth defect?** <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/194c4f3d-604b-4189-ad01-2a6b730cd0f8)

**4. Should abortion be legal if the woman wants no more children?** <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/254309e0-bd50-4845-bad2-038de8368373)

**5. Should abortion be legal if the pregnant woman is too poor to support a child?** <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/3215caea-ac10-4f55-a56c-09770c31d711)

**6. Should abortion be legal if the pregnant woman is single?** <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/23593f17-3133-46ea-b3c7-2f372c4a7c1d)

As is apparent, the distribution of responses evolve with every question, with the questions getting increasingly controversial. It therefore makes sense to put level of support on a scale based on one's answer to these questions. According to the article, this series of questions have proven to be reliable even fifty years after their creation. Since I am creating a binary classification model, I categorized those who responded yes to 4 or more questions as in favor of legal abortion. The resulting distribution is as follows. <br/>
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/d1a7018c-6fd3-411a-92b1-90550e72e771)
![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/1a97dae9-ef50-46fd-8011-9598001d1a32) <br/>
It is important to note that turning a 6-point scale into a binary classification inherently involves some degree of generalization and the giving up of certain nuances in opinion. However, given the distribution of repsonses of the last three questions were very similar at roughly a 50/50 split, it seems reasonable to assume anyone who scores a 4 or above would be supportive of legal abortion for most scenarios.

## Modeling and Evaluation 
Since my goal is to help Planned Parenthood identify people who are likely to support them, I will be prioritizing recall score for my models, since I would want my model to be able to capture as many potential customers as possible. That said, I would also keep precision into account so as to not mislead the company when it comes to giving pointers to finding the right demographic. <br/>
To build the model, I experimented with different classification algorithms, with decison trees as my baseline model. The models I tried include gradient boosted trees, logistic regression, and k nearest neighbors classifier. I cross validated all of them to find the algorithm that produced the highest accuracy, which turned out to be random forest classifier, then I tuned its hyperparameters to reduce its tendecny to overfit to the training set by using GridSearchCV, which takes in a grid of parameters, goes through all possible combinations of them, and returns the set of parameters with the best cross validated score. Finally, since there were 17 questions in my feature space, some of which were highly correlated to each other, which might affect the way the algorithm gives weight to each feature, I tried to simplify the feature space by combining or dropping certain columns based on their correlations to one each other, and adding columns to draw attention to minority groups. For instance, instead of keeping information about what relgion the respondent grew up in, and what religion the respondent is now in respectively, which are highly correlated variables, I instead added features that indicated who became relgious, who became athiest, and the relgions they stuck with or converted to respectively. This helped reduce the complexity of my input variables while preserving important information, which in turn helped the model's performance.
### The final model
After arriving at my final model, I evaluated its performance on predicting a set of unseen data, and here are the results. <br/>

![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/63e90598-ac57-4dad-9542-2a509397cf41)

Compared to the baseline model, the accuracy is better by almost 10%. Moreover, it has 71% recall and nearly 80% precision for positive cases, meaning it is able to correctly identify 71% of those who support legal abortion and only misclassified someone against abortion as someone for aboriton 20% of the time. <br/>
The shortcomings of this model would be correctly identifying those who are against legal abortion, with only around 50% precision, of all the people classified as against legal abotion, only half of them were classified correctly. Moreover, its performance in identifying negative cases did not improve with SMOTE. This could be because there is too much varaince in the characteristics of negative cases such that the patterns in the training data was not representative of unseen data. It could also be because political opinions are more complex than a dozen characterisics, and the model can only generalize to a limited extent.

## Conclusion
A look into the top ten features that helped the model differentiate between the two classes are as follows. <br/>

![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/848529c7-e989-4647-b981-b4c7ce5f0c31)

To translate these features into plain language, the top 10 most indicative charactersitics are: <br/>
* How frequently respondents pray <br/>
* Party identification {on a scale of 0 - 6, 0 indicating strong democrat and 6 indicating strong republican} <br/>
* The age of respondent
* Level of education
* overall satisfaction with life { social, financial, and quality of life in general}
* The number of children they have had
* Whether they were once religious and became athiest
* Whether they were a protestant from childhood to present
* Whether the respondent was male
* Whether the respondent was white <br/>
Additionally , to assist in getting a better understanding of how these variables relate to the target variable, I color coded the bars such that the lighter colors indicate a positve correlation with one's score on the Rossi scale, and the darker color indicates a negative correlation. <br/>
In light of the feature importances of this model, I would recommend Planned Parenthood to advertise in big urban cities, since they tend to lean towards being democratic. They could also advertise in college areas, as people living there are likely to be more highly educated,Moreover, rather than looking at the specific relgion a person follows, to look at how strict they are with their religious habits. <br/>
If they were to have access to demographic data, they could also use this classification model to sort through large groups of people to select those who are likely to be interested in their services.

## Caveats
There are certain limitations to this classification model. For instance, having around 70% accuracy is far from perfect. Since it serves to guess politcal stance of people based on rather limited information about them, it is expected that the model would be unable to classify perfectly. Moreover, since I am working with self report data, there is inherent bias and error arisig from truthfulness of repsondents and misinterpretation of questions, leading to potentially misleading data. <br/>
There might also be aspects of opinions surrounding abortion that this model is not able to identify. Namely, the Rossi scale, which is the target variable of this model, mainly tackles question about the legality of abortion and does not include topics of morality. A person that supports the legalization of abortion might not necessarily support the act of it, and would still not be the target demographic for Planned Parenthood.

## Next Steps
Moving forward, I would look into expanding the feature space to include more attribute about each respondent, which could potentially lead to better model performace. Seeing as the model's fault mainly lies in identifying those who are against abortion, I would also try to gather more data about respondents with that sentiment such that more reliable patterns would be recognized by the algorithm. <br/>
I would also explore more aspects of the discussion surrouding abortion beyond its legality to gain more insight into public opinion regarding this issue, and build a model that can classify level of support in a more holistic way.

## For More Information
Please review my full analysis in the [github repository](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis)) or [my presentation](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/blob/main/presentation.pdf). All the code used in this project can be run on google colab, access the notebook [here](https://colab.research.google.com/drive/17sWP3JexFAegoffSWVB3jqOd--Aetequ#scrollTo=oP3kd1ZbDKx3).
For any additional questions, please contact Beatrix Wong, beatrix.wmh@hotmail.com

## Repository Structure
```
├── README.md                           <- The top-level README for reviewers of this project
├── ab2018.ipynb                        <- Narrative documentation of analysis in Jupyter notebook
├── presentation.pdf                    <- PDF version of project presentation
```









   
