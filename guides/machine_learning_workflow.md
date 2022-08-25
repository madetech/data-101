# A typical Machine Learning Workflow
It is no secret that understanding data is the key to unlocking hidden potential across our Public Sector organisations. Be it through optimisations in analysis, the monitoring of services or identifying previously unknown trends, the importance of data is unquestionable. So what part do Made Tech play in this brave new data driven world?

As ever, our role is one of guidance and education. We must ensure that we poses the knowhow to mentor this burgeoning capability within the public sector and the ability to lead them through the data landscape. The goal of this document is to provide a framework for a project and the steps required to ensure its successful delivery. What factors must be considered when developing a data solution and the realities of implementing Machine Learning (ML) into a product.

We will cover how to identify a business problem and how to frame a machine learning answerable question. We will explore the realities of developing a gold standard data set and what is necessary to develop a robust ML solution. At all times we must consider how this development process will impact the business and how best to demonstrate the value a data solution is able to add to any project. 

![A typical machine learning workflow](../images/ml-workflow.jpg)

Development is always an iterative process, agile workflows and short sprint cycles are well known and widely accepted as business practice. This is mirrored in the data world but still often underestimated or overlooked. It may take several weeks to prepare data and develop features, only to find that the resulting model does not reach the required metrics and the whole process must begin once again. The implementation of ML is a timely and often costly process and there will always be the risk that a project may run out of time or money before a successful model can be produced. This makes open and honest communication incredibly important with any stake holder. Implementing a data solution has the potential to have a massive impact on the operation of any department, freeing up recourses, delivering deeper insight and taking months off delivery time scales, however often the road to this impact is difficult to navigate. 

## Identify the Business Problem 
The first objective of any speculative data project is highlighting the need for a data strategy to begin with and communicating the myriad of benefits that accompany a mature data platform. The ability to identify a business case for ml implementation requires delivery partners and technology leads to understand the use cases for ml and the massive impact that it can deliver. 

Broadly speaking, machine learning is very good at three basic things, predicting a value: 
### forecasting 
Passenger movements, patient surges, estimating how many requests an IT service will have to deal with or when more officers will likely be required on shift. 
### Classification or recognition 
Identifying the cause of worker churn, when an application to a service is fraudulent or automatically tracking how many of a specific request type is submitted. Clustering: determining user groups based on specific metrics to improve the service provided or identifying entirely new user groups that were overlooked in the past. 

At all times, but particularly when considering the sector in which we work, it is incredibly important to also consider the consequences of implementing machine learning models. A model only provide a solution with a certain level of certainty, there will always be cases that the model is not able to predict. If this is overestimating a figure for a report, who is seeing that report and what decisions are being made? If that model is classifying fraudulent or non-fraudulent benefits claims, what impact will a false negative have on a persons life?

All things considered, there are  plenty of instances where the implementation of a ML model can deliver massive benefits to a department. It is important to remember that Made Tech has a wealth of people with massive amounts of experience in this area and we are always looking to discuss any project you think may benefit from a data unlock! 

<!-- 
all about problem formulation - starting point of any ML project - required to identify the problem
What are we trying to solve? Consiquences of using ML - what are the ramifications of incorrect answers. 
What is the business metric, cost reduction, increased customer base, improved efficiency. What is the correct metric - what is the impact - what quality of metric. Are there multiple metrics? is there a priority? Can they be linked? 
Can this be answered by business logic instead? 
Do we have enough data? Do we have High quality data? Needs high quality big data set
Is the data static or evolving over time?
Communication is key as these will all have implications on the time scale of the project.  -->


## Frame the problem as a Machine Learning problem
What kind of ML algorithm can we apply 
Supervised vs unsupervised or semisupervised?
Likely multiple models working together to answer a question. 
Establish the criteria for success - you can always tweak to improve
Always defer to the simpler model


## Collect and Integrate the data
does not end - it continues to evolve. 
Sometimes you need to find more data or find new data for new features. 
Need enough for A/B testing. 
examples of data collection - logs - APIs - public or private data
There is a lot of publicly available data - ONS - Census - Geographical 

Sampling - Select a subset of instances for training and testing
    Random
    Must be a good representation of the population 
    Must make sure there is not sampling bias
    stratified
    Apply random sampling to subpopulations
Labeling - Obtaining gold standard answers for supervised learning
Lookout for Seasonality or trends in data 
Does time of the day/week/year effect the data 
Do patterns shift over time?
Make sure there is no data leakage 

Labeling
Is the data labeled - can that labeling be trusted? Do we need human capital to perform labeling?
Labels MUST be correct in order for the model to reliably train 

## Prepare the data
Is the data accessible? Can the data be easily and efficiently queried? Is there a data pipeline in place? Are there appropriate environments for the data to be analysed and explored? Is the data scientist required to develop locally or in a cloud environment? who owens the env? Is there sensitive or identifiable data that needs to be obfuscated? What steps do we need to enact in order to get the data to a reasonable state - Is there a clear data schema?


## Use visualisations and exploratory techniques to analyse the data 
Exploratory data analysis - Data statistics - data visualisations - how many features are there? are these features relevant? What can the domain experts tell you? 
Numerical
is it normally distributed? are there any obvious features? Are there outliers? Quartile analysis - averages for back filling? 

Ordinal
What is the relationship? 

Categorical 
Most frequent - least frequent - percentage of set - size of set - does the set cover all the possibilities? - 

Text / Language
are there key words to identify - Are we looking to analyse sentiment or identify themes? 

The use of visulisations is vital to identify correlations and relationships within the data set 

Establish the quality of data wether we need more to build a bigger / better feature set

Do we need to fill or filter missing data? Has data been miss labeled - are outliers typos? Are measurements correctly filled in and of the same / expected scale? Do we need to account for special characters within fields? Names? Text blocks?


## Feature Engineering

## Model Training

## Model Evaluation

## Business Goal Evaluation

## Predictions and deployment 