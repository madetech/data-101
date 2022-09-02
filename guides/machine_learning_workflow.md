# A typical Machine Learning Workflow
It is no secret that understanding data is the key to unlocking hidden potential across our Public Sector organisations. Be it through optimisations in analysis, the monitoring of services or identifying previously unknown trends, the importance of data is unquestionable. So what part do Made Tech play in this brave new data driven world? Well, as ever, our role is one of guidance, education and reliable implementation. We must ensure that we poses the ability to mentor this burgeoning capability within the public sector and the confidence to lead them through the data landscape. 

The goal of this document is to provide a framework for a typical Machine Learning (ML) project. It will outline each step and briefly discuss its importance, what factors must be considered and what realities our clients (and our data team's) are likely to face. We will cover how to identify a business need and how to frame a problem as a question answerable by a machine learning model. We will explore the realities of developing a high quality data set and what is necessary to develop a robust ML solution. Throughout this process, we must consider how this development process will impact the business and how best we can demonstrate the value added to the client.

![A typical machine learning workflow](../images/ml-workflow.jpg)

Software development is always an iterative process, agile workflows and short sprint cycles are now well known and widely accepted as best practice within the public sector. Although this workflow is mirrored in the data world the nature of the work can often give the impression that very little is ever being done. The process of model development is an incredibly iterative one and will produce very little in terms of tangible outputs until it is complete. The cycle of feature engineering and model training is in many ways far more similar to a tech spike, trying new techniques and models until something yields the desired result. Where a typical sprint may add a selection of new features to an app, a successful ML sprint may only see several new columns (data features) added to a database. It may take several weeks to prepare data and develop features, only to find that the resulting model does not reach the required metrics and the whole process must begin once again. 

The implementation of ML is a timely and often costly process and there will always be the risk that a project may run out of time or money before a successful model can be produced. This makes open and honest communication incredibly important with any stake holder. Implementing a data solution has the potential to have a massive impact on the operation of any department, freeing up recourses, delivering deeper insight and taking months off delivery time scales, however often the road to this impact is difficult to navigate. 

## Identify the Business Problem 
The first objective of any speculative data project is convincing stake holders of the need for a data strategy in the first place. Highlighting the myriad of benefits that accompany a mature data platform is an incredibly exciting process, as development alone will immediately open so many possibilities and save the client time and money. Made Tech already has a track record of this and plenty of case studies to back it up. Once a conversation around data has begun, things can very quickly get really quite exciting. 

Machine Learning has an incredible range of problems it can solve, processes it can automate and insights it can generate. During these early conversations, it is a good idea to start talking to a member of the data team and get them involved, you will quickly find that we love talking about the art of the possibility and will have an idea of the potential they can unlock. Broadly speaking ML tasks are likely to fall into three basic catagories:
### forecasting 
Given a set of historical data, ML can be used to predict future outcomes. How many passengers are likely to use public transport at this time or over this weekend? Given previous trends, how many patients should a hospital expect, when are the likely surges and what will demand be? Predicting staff requirements for shift allocation, estimating supply demands or forecasting spending patterns, all of these are questions that can be tackled using machine learning and regression modeling.  
### Classification or recognition 
Here, the business is looking to identify and classify specific events based on a specific set of input data. Identifying the cause of worker churn, detecting when an application to a service is fraudulent, automatically tracking how many of a specific request type is submitted or filtering out spam from messaging services. Classification problems even extend into image recognition, automatically detecting contraband items in X-Ray images, checking for false identification documents at border crossings or extracting license plate information from images. When ever you are looking to apply a specific label to a series of inputs, chances are you can train a classification model to do the work for you.

### Clustering
Determining user groups based on specific metrics or identifying entirely new user groups that were overlooked in the past. Automatically identifying search or message topics. Even anomaly detection, finding outliers in the data set that might otherwise go unnoticed.  

At all times, but particularly when considering the sector in which we work, it is incredibly important to keep in mind the consequences of implementing machine learning models. A model only provide a solution with a certain level of certainty, there will always be cases that the model is not able to predict. If this is overestimating a figure for a report, who is seeing that report and what decisions are being made? If that model is classifying fraudulent or non-fraudulent benefits claims, what impact will a false negative have on a persons life?

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
Feature engineering is a fundamental topic in machine learning. It is the process of selecting, manipulating, extracting and otherwise transforming raw data into features that can be fed into a model and used in the training process. When done correctly, this process will simplify the model, decreasing the time it take to train, while still retaining all the information contained within the raw data, improving the model accuracy. 


As much an art as a science. 
build features required to train the model 
iterate with model training several times 
This is a massive time sink/allocation

Transformations, extractions, external sources, 


## Model Training
Iterative with feature engineering. 
Model selection
Hyperparameter tuning


## Model Evaluation
How well does the model perform. 
Train validate test data sets

## Business Goal Evaluation
Does the trained model answer the question we set out to answer 
Was the question framed correctly to begin with 
Has the scope changed over the course of the project
What are the business metrics and how do we measure success / failure

## Predictions and deployment 
Where are the predictions being used
Are they being monitored for model drift with evolving live data / how are we monitoring/measuring reliability
How is this worked into a pipeline 
There is a difference between developing a model in a notebook and deploying a model to a live env
Heavy collaboration with data engineers
How does the client want to interact with the model 

MLmodels will require constant monitoring and development as the data will inevitably change shape over the course of time. 
Does the client have the requisite skills / developers in place to maintain a data pipeline / ML model
has up skilling been park of the delivery