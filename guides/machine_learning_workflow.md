# A typical Machine Learning Workflow
It is no secret that understanding data is the key to unlocking hidden potential across our Public Sector organisations. Be it through optimisations in analysis, the monitoring of services or identifying previously unknown trends, the importance of data is unquestionable. So what part do Made Tech play in this brave new data driven world? Well, as ever, our role is one of guidance, education and reliable implementation. We must ensure that we poses the ability to mentor this burgeoning capability within the public sector and the confidence to lead them through the data landscape. 

The goal of this document is to provide a framework for a typical Machine Learning (ML) project. It will outline each step and briefly discuss its importance, what factors must be considered and what realities our clients (and our data team's) are likely to face. We will cover how to identify a business need and how to frame a problem as a question answerable by a machine learning model. We will explore the realities of developing a high quality data set and what is necessary to develop a robust ML solution. Throughout this process, we must consider how this development process will impact the business and how best we can demonstrate the value added to the client.

![A typical machine learning workflow](../images/ml-workflow.jpg)

Software development is always an iterative process, agile workflows and short sprint cycles are now well known and widely accepted as best practice within the public sector. Although this workflow is mirrored in the data world the nature of the work can often give the impression that very little is ever being done. The process of model development is an incredibly iterative one and will produce very little in terms of tangible outputs until it is complete. The cycle of feature engineering and model training is in many ways far more similar to a tech spike, trying new techniques and models until something yields the desired result. Where a typical sprint may add a selection of new features to an app, a successful ML sprint may only see several new columns (data features) added to a database. It may take several weeks to prepare data and develop features, only to find that the resulting model does not reach the required metrics and the whole process must begin once again. 

The implementation of ML is a timely and often costly process and there will always be the risk that a project may run out of time or money before a successful model can be produced. This makes open and honest communication incredibly important with any stake holder. 

## Identify and Framing the Business Problem 
The first objective of any speculative data project is convincing stake holders of the need for a data strategy in the first place. Highlighting the myriad of benefits that accompany a mature data platform is an incredibly exciting process, as development alone will immediately open so many possibilities and save the client time and money. Made Tech already has a track record of this and plenty of case studies to back it up. Once a conversation around data has begun, things can very quickly get really quite exciting. 

Machine Learning has an incredible range of problems it can solve, processes it can automate and insights it can generate. During these early conversations, it is a good idea to start talking to a member of the data team and get them involved, you will quickly find that we love talking about the art of the possibility and will have an idea of the potential they can unlock. Broadly speaking ML tasks are likely to fall into three basic catagories:

**forecasting**: Given a set of historical data, ML can be used to predict future outcomes. How many passengers are likely to use public transport at this time or over this weekend? Given previous trends, how many patients should a hospital expect, when are the likely surges and what will demand be? Predicting staff requirements for shift allocation, estimating supply demands or forecasting spending patterns, all of these are questions that can be tackled using machine learning and regression modeling.  

**Classification or recognition**: Here, the business is looking to identify or classify specific events based on input data. Identifying workers who are likely to leave, detecting when an application to a service is fraudulent, automatically tracking how many of a specific request type is submitted or filtering out spam from messaging services. Classification problems even extend into image recognition, automatically detecting contraband items in X-Ray images, checking for false identification documents at border crossings or extracting license plate information from images. When ever you are looking to apply a specific label to a series of inputs, chances are you can train a classification model to do the work for you.

**Clustering**: Finally we have clustering. This is a version of unsupervised learning that can be used to determine or specify user groups based on selected metrics. Highlight similarities between teams, departments, organisations and locations, or even find outliers in a data set that might otherwise go unnoticed.

All things considered, there are plenty of instances where the implementation of a ML model can deliver massive benefits to a department. It is important to remember that Made Tech has a wealth of people with a huge range verse experience! We are always looking to discuss any project you think may benefit from a data unlock! 

## Data collection and Preparation
Core to any ML project is the data, as this is what we use to train a model and evaluate it effectiveness. The overall success of any machine learning project relies heavily on there being enough high quality data available to analyse. Only from this can we hope to draw meaningful conclusions. As a rule of thumb, the bigger the initial data set the better. As we progress through the various development stages the aim will be to produce a core of very high quality data, this will then be segmented into training, validation and testing sets and fed to the model. Without a large initial data set, the project runs the risk of not having enough high quality data to be truly representative resulting in a poorly trained model that wont be reliable. 

It is important to consider the collection process. Is the data source reliable? Does the data set truly represent whatever you wish to analyse? If you are looking to perform time series analysis, does the data cover a long enough period of time? If you are looking to perform image recognition, do you have a large enough dataset of correctly labeled images? A model trained on a set containing mostly one demographic may have trouble identifying or dealing with others. Data collection is a time intensive and resource heavy process, is it part of the statement of work? In cases were we need to attain data ourselves, say from conducting user surveys or scraping logs, then the collecting, cleaning, sorting and storing of this data could even be considered a completely separate delivery. 

Once the data has been colleted then we must consider how easily accessible that data is and about the infrastructure supporting it. Data at the scale required for effective machine learning can rarely be stored in simple excel sheets and will become impossible to maintain as the project progresses. Building out a robust data infrastructure is a fantastic development for any business, and of course one that Made Tech has plenty of experience in delivering. The goal here is to move from single excel sheets held on an individuals computer, to a managed databases hosted on a suitable cloud platform that can be monitored, updated and maintained. 




<!-- does not end - it continues to evolve. 
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
Labels MUST be correct in order for the model to reliably train  -->

## Prepare the data
<!-- Is the data accessible? Can the data be easily and efficiently queried? Is there a data pipeline in place? Are there appropriate environments for the data to be analysed and explored? Is the data scientist required to develop locally or in a cloud environment? who owens the env? Is there sensitive or identifiable data that needs to be obfuscated? What steps do we need to enact in order to get the data to a reasonable state - Is there a clear data schema? -->



## Use visualisations and exploratory techniques to analyse the data 
<!-- Exploratory data analysis - Data statistics - data visualisations - how many features are there? are these features relevant? What can the domain experts tell you? 
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

Do we need to fill or filter missing data? Has data been miss labeled - are outliers typos? Are measurements correctly filled in and of the same / expected scale? Do we need to account for special characters within fields? Names? Text blocks? -->


## Feature Engineering, Model Training and Model Evaluation

Feature engineering is a fundamental component of any machine learning project. It is the process of selecting, manipulating, extracting and otherwise transforming raw data into features that can be fed into a model and used in the training process. When done correctly, this process will simplify the model, reducing the time it takes to train, while still retaining all the information. Effective feature selection is essential in improving model accuracy, given however that there are no hard and fast rules for this process, it is often considered as much an art as it is a scientific process. 

Hand in hand with this is model selection, training and evaluation. Every machine learning problem has a plethora of different algorithms that may be used to solve them. It may be that the nature of the data and the trends it contains responds better or worse to a certain algorithmic approaches. The nature of ML means that often we will not know until we try. There are also hyperparameters for each model that must be tuned that will again have an effect on how well the model preforms. 

All of this leads to a very time consuming process, often requiring many iterations before a final feature set and model choice is made. It may even transpire that the situation is not completely described by the current data set and it is necessary to return to the data collection process and for the cycle to begin again. It is vital that throughout this process the client is kept well informed of what is going on. Sometimes the result of a feature engineering sprint will simply be reporting what doesn't work, important information that translates to progress, but something that can be frustrating for a client looking for tangible deliveries. 

## Business Goal Evaluation
Once a feature set and model has been developed and is producing results to an acceptable level of accuracy, it is important to evaluate how well that model performs using live data and how well it meets the business objectives. The implementation of any ML solution has the potential to have a massive impact on the operation of any department, freeing up recourses, delivering deeper insight and taking months off delivery time scales, however often the road to this impact is difficult to navigate. 

At all times, but particularly when considering the sector in which Made Tech operates, it is incredibly important to keep in mind the down stream consequences of our solutions. A model can only ever provides an outcome with a certain level of accuracy, there will always be outlier cases that the model is not able to predict. Put simply, it will not be correct 100% of the time so we must consider the impact of those errors. Does the model over estimate a figure for a report, who is seeing that report and what decisions are being made? If that model is classifying fraudulent benefit claims, does the model show bias? What impact will a false negative have on a persons life? 

These are all questions that must be asked and deeply considered when building a service. Made Tech has a responsibility to deliver the best solution to a problem. Sometimes that may mean developing exciting machine leaning pipelines, other times the situation may be better served by taking the time to fully map out the business logic and "simply" programming that into the pipeline instead. 


## Predictions and deployment 
<!-- Where are the predictions being used
Are they being monitored for model drift with evolving live data / how are we monitoring/measuring reliability
How is this worked into a pipeline 
There is a difference between developing a model in a notebook and deploying a model to a live env
Heavy collaboration with data engineers
How does the client want to interact with the model 

MLmodels will require constant monitoring and development as the data will inevitably change shape over the course of time. 
Does the client have the requisite skills / developers in place to maintain a data pipeline / ML model
has up skilling been park of the delivery -->

## Hosting 

## Conclusions
If the client has data that they wish to be interrogated then there is likely something that a data scientist can do. This creates an incredible opportunity for Made Tech to step forward and offer to do more. Once a data scientist has been able to spend some time with the data, be that taken from years of excel spread sheets or scraped from the web, 