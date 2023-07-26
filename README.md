# An analysis of factors that contribute to opinions on legal reproductive rights
With the recent overturning of Roe v. Wade, the topic of legal reproductive rights have been more controversial and porlarizing than ever. Moreover, it is getting harder for organizations like Planned Parenthood to find the right demographic to advertise to. In this project, I will attempt to build a classification model using survey data from the General Social Survey, which gives information about the American demographic and their political opinions. The goal is to build a model that can predict whether someone is in favor of or against legal abortion based on facts about them, and to find characteristics those who share the same opinion have in common to give guidance to finding the right demographic for advertising. This allows Planned Parenthood to know where to focus their business efforts in and be more efficient with their resources.
## Business and Data Understanding
### About the data
The data for the project was sourced from the [General Social Survey website](https://gss.norc.org/getthedata/Pages/Home.aspx), a research institution that surveys the american population yearly using full probability sampling with the aim of providing unbiased insight into various aspects of the general public. Each respondent was asked over 1000 questions about themselves and their opinions regarding different topics, giving researchers insight into the respondent's personal background and way of thinking. Moreover, full probability sampling ensures the data gathered is representative of the population. 
### Data Selection
The survey was structured such that respondents were free to skip questions they did not want to answer. To choose my independent variables such that there is minimal missing information for a more accurate analysis, I selected only questions with missing responses making up less than 3% of the dataset. I also referred to the [codebook](https://gss.norc.org/documents/codebook/gss_codebook.pdf) to understand what questions the columns represented and what their values mean, and did my best to choose variables that might give insight into the respondent's background and thus help the model make correlations between their attributes and their political opinions.
To create the right target variable for my model, I first needed to select the correct survey questions that are able to capture and reflect a respondent's opinion on the topic sufficiently. To do so, I consulted an article called [Updating A Time-Series of Survey Questions: The Case
of Abortion Attitudes in the General Social Survey](https://gss.norc.org/Documents/reports/methodological-reports/MR133%20Abortion.pdf), which was written by Sarah K. Cowan and published by NORC at the University of Chicago, the same institution that conducted the survey subject to this analysis. The article goes into detail discussing the correct questions to ask to capture public opinion about the ever-changing narrative surrounding the legality of abortion. It mentions a series of six hierichal questions that can be summed up as the Rossi scale, a six-point scale with one point given when a respondent responds 'yes' to the question. To demonstrate the hierichal nature of these questions, I will go through each of them and show the distribution of answers in the survey conducted in 2018. <br/>
1. Should abortion be legal if the pregnant woman's health is at risk?
![health](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/ad1ca000-8ab8-4cfb-bd78-0bece1071b27)

2. Should abortion be legal if the pregnancy was a result of rape?\
![rape]()![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/f1fe0c12-fc2e-437e-a96d-52c7ea24c94d)


3. Should abortion be legal if the fetus has a birth defect?
![defect](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/194c4f3d-604b-4189-ad01-2a6b730cd0f8)

4. Should abortion be legal if the woman wants no more children?
  ![no more](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/254309e0-bd50-4845-bad2-038de8368373)

5. Should abortion be legal if the pregnant woman is too poor to support a child?
![poor](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/3215caea-ac10-4f55-a56c-09770c31d711)

6. Should abortion be legal if the pregnant woman is single?
![single](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/23593f17-3133-46ea-b3c7-2f372c4a7c1d)

As is apparent, the distribution of responses evolve with every question, with the questions getting increasingly controversial. It therefore makes sense to put level of support on a scale based on one's answer to these questions. According to the article, this series of questions have proven to be reliable even fifty years after their creation. Since I am creating a binary classification model, I categorized those who responded yes to 4 or more questions as in favor of legal abortion. The resulting distribution is as follows. <br/>
![Rossi](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/d1a7018c-6fd3-411a-92b1-90550e72e771)
![binary](![image](https://github.com/Beatrixwmh/Reproductive_Rights_Analysis/assets/108293459/1a97dae9-ef50-46fd-8011-9598001d1a32) <br/>
It is important to note that turning a 6-point scale into a binary classification inherently involves some degree of generalization and the giving up of certain nuances in opinion. However, given the distribution of repsonses of the last three questions were very similar at roughly a 50/50 split, it seems reasonable to assume anyone who scores a 4 or above would be supportive of legal abortion for most scenarios.

## Modeling and Evaluation 
Since my goal is to help Planned Parenthood identify people who are likely to support them, I will be prioritizing recall score for my models, since I would want my model to be able to capture as many potential customers as possible. That said, I would also keep precision into account so as to not mislead the company when it comes to giving pointers to finding the right demographic. <br/>







   
