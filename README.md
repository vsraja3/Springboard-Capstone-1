# Springboard-Capstone-1

## Introduction

New York City is the most populous city in the United States and amongst the most diverse racially and economically and community distinctions with the city being divided into 5 boroughs with a total of 59 community districts spread amongst them. One of the natural challenges that large cities and urban areas in general have is managing waste and consumption in ways that are efficient and effective in being  sustainable yet practical. An example of a waste management standard that many New York residents find less than ideal is the disposal of garbage bags to be collected on streets due to the lack of alleys. This is an issue that is more or less inevitable as a result of the limited space and crowded urban layout of Manhattan island in particular. 

The NYC city government’s Department of Sanitation (DSNY) has been working continuously towards improving existing programs and practices as well as implementing new solutions to challenges faced across the city in waste management. While the issue of garbage being collected on the streets is one that is less likely to be resolved due to space limitations, one area that can always be improved is to promote existing recycling programs and find areas of improvement. The DSNY has been collecting data on recycling rates and producing reports throughout most of the last decade. While the data is useful in that it is broken down into recycling rates for each of the 59 community districts and through time (monthly from 2016 through 2019), there would be further room for analysis if additional data regarding the differences in makeup between the community district were available. Fortunately, the NYC Planning website provides a large amount of data on community district indicators including socio-economic and demographic data for each district. Using these two datasets, it should be possible to draw insights into determining what factors regarding the makeup of a community contribute to the success levels of recycling and waste management programs and build a model that could predict recycling potential with new data, perhaps even new communities and other cities.  

## [Data Wrangling](https://github.com/vsraja3/Springboard-Capstone-1/blob/master/Capstone%201%20-%20NYC%20Recycling%20-%20Wrangling%20Report.ipynb)

The first step in the process of developing a data science project involves data acquisition, which I began by exploring open data websites that allowed quick access to datasets to download. I began by searching using Data.gov as my primary source for identifying a dataset as it houses a variety of U.S. government data and seemed the most likely place to find the type of data I was seeking.

When initially searching for data related to recycling or other environmental impact and sustainability metrics, I stumbled upon the NYC Recycling Diversion and Capture Rates (hyperlink) dataset on Data.gov. While ideally I would have liked to gather data on a national level  or at least include other major cities, ultimately there was limited public data readily available from other areas. Moreover when taking into consideration the size and diversity of a city like New York, it is evident that the city provides a sufficient sample size and as good variability as possible to analyze the factors that could effect recycling rates. The dataset contained a breakdown of two metrics that the City of New York uses to measure the success of their recycling programs across the cities - recycling diversion rates (the fraction of amount of material recycled over the total waste collected) and capture rates (the fraction of material recycled against the total recyclable material in the waste stream). The data was recorded for every month from years 2016-2019, and broken down for each of New York City’s 59 community districts across their 5 boroughs. 

The DSNY recycling rates dataset presents a strong foundation for data science applications as it contained two metrics for recycling measurement along with variables including time in months of each year and a breakdown by community district. However, in order to tie in the hypothesis that a diverse population of people could potentially effect recycling rates differently based on differences between the proportion of subgroups based on factors like socio-economic status and race, additional data would be needed. Since the NYC recycling data was broken down primarily by community district on the initial dataset, I sought to find additional datasets that yield district level data and fortunately the NYC planning (hyperlink) website offers open access to datasets containing demographic, economic, social, and community indicators broken down by each of the city’s 59 community districts.

With both datasets being available in CSV format and having district level data separated by row with the variables including recycling rates and demographic percentages at the column level, the data wrangling process was more or less straightforward, albeit with a modest amount of cleaning and wrangling in order to produce a data frame that is easy to work with to start exploratory data analysis (EDA) and build and implement machine learning algorithms. 

I worked primarily with Jupyter Notebook and Pandas to construct the Data Wrangling report, which you can find in the repository here. There were several steps taken to clean up the two datasets and combine them that can be seen more specifically in the report. The first component which was simple enough was reading in the two CSV files using Pandas built in ‘read_csv’ function and storing them as Pandas data frame objects named as ‘recycling’ and ‘district’. 

The next steps involved cleaning up the missing data and removing any unnecessary data. Fortunately, the missing data was limited with only a handful of blank fields in columns that measured proportion (ex. percentage of Males in a community district). These values were assumed to be 0 percent based on the sum of the remainder of the fields summing to 100 percent, and as such, were simply replaced using ‘fillna’. There also were several columns in the community district dataset, which contained a total of 184 columns, that were either redundant or unnecessary based on the description of the metric in the data dictionary. Some of these included contact information for community district  board, websites and administrative information, as well as metrics pertaining to more than just a specific districts such as population of NYC or the borough a district is in. As such, a filtered data frame was created by selecting only the columns that could potentially be useful for exploratory data analysis.

The next component involved aggregating a few of the district indicators columns to find other categories for analysis. This included finding the total proportion of Male and Female of any age group since the raw data only included the proportion of each gender in each age group. 

Additionally, since the raw data included narrow age group breakdowns for each 5 years, this would likely be too narrow to draw conclusions from. To specify, rather than analyze the amount of residents between ages 5-10 in a particular districts, it would be more insightful to find the total proportion of minors or anyone under 20 since their behavior of each subgroup in that span is unlikely to be different. As such, I decided to add additional columns to break down the age group proportion into larger subgroups that represent potential generational differences for each 20 years with minors (under 20), young adults (20-39), middle aged (40-59), and seniors (60+) simply by taken the sum of each age group within.

After cleaning up the two data frames individually, the next step was to combine the two data frames by the common community district field (the column labeled ‘District’ in the recycling data frame and ‘cd_short_title’ in the districts data frame). This was done by creating a dictionary to map the district labels into standardized formats and then using a simple Pandas merge on the standardized district label.

With the combined data frame of recycling rates and district indicators created, the final component to the wrangling was to account for the fact that the recycling data was broken down into data for each month from years 2016 through 2019. While it would be ideal for the time periods of the recycling data and district data to line up, unfortunately the district level data was only recorded as a single measurement as an average estimate across the 5-year period of 2012-2016 as opposed the recycling data was recorded monthly through time. As such, I aggregated the monthly data into annual data while taking the mean of the recycling rates across months in a year and isolated the 2016 data frame since it aligns closet to the district level estimates. I additionally kept the annual and monthly data in two other data frames in order to at least explore a univariate analysis of rates changing over time. The ending result are three data frame with the average recycling rates broken down by year for each community district, which we can analyze over time in one instance and across the 2016 year against demographics in another. 

## [Data Story & Statistical Analysis](https://github.com/vsraja3/Springboard-Capstone-1/blob/master/Capstone%201%20-%20NYC%20Recycling%20-%20Data%20Story%20%26%20Statistical%20Analysis.ipynb)

By examining the wrangled data, there are several questions that can be asked regarding the relationships between variables, differences between subgroups of data, and trends through time that can be answered through exploratory data analysis (EDA). These questions and the resulting analysis and visualizations can be examined in greater detail in the Data Story & Statistical Analysis Report (hyperlink).

One of the first questions that comes to mind is to answer whether recycling rates have been trending upwards or downwards through time in general. The expectation is that the rates would trend upward as a result of sustainability initiatives and general awareness leading to increased use of eco-friendly recyclable material, which we would expect to increase the diversion rate, and the increased education towards proper recycling practices leading to a higher capture rate. Using the monthly data available and plotting in a quick scatter plot, we can confirm that a slightly upward trend of both diversion and capture rates, albeit with some ups and downs. Taking a statistical approach and examining the Pearson correlation and p-value, the values suggest a weak positive correlation for both diversion and capture rates when compared against time in months across all districts. Moving forward, we similarly are able to take a look at the trends of time at a higher level by using a box plot which allows us to see the percentile distribution of data including the median and middle 50 percent of the rates. 

Observing the box plot distribution and overlaying a swarm plot on top allows us to see the exact points and observe any outliers. Based on the color coding of the swarm plot to indicate which NYC borough a point belongs to, we can see that data points from certain boroughs may pull the data upwards or downwards. This begs the question as to whether there are significant differences in the subgroups of data, namely New York City’s five boroughs. It can be reasoned that there would be a difference simply based on the fact that the boroughs are geographically different and likely to have a different distribution of residents as a result. Using an additional box plot overplayed with a swarm plot allows us to see that the median rates in each borough have distinct differences. 

This is further explored by viewing the percentage breakdown of residents by racial group and age group within each borough, which highlights some high level insights such as the higher proportions of white New Yorkers in Manhattan and Staten Island, two of the zones which had higher median recycling rates. This leaves with us to explore further the relationships between race and recycling, which we will get too shortly.

The pie charts which display the age breakdowns of the 4 broader generational groups are less conclusive as they imply more or less even splits across the boroughs. Since the  broader age groups can not tell us as much, we are still left with the question as to if there is are certain age groups that recycle more, for which we can use the original data which included narrow age breakdowns in 5 year increments. By computing the Pearson correlation of each age group against the diversion and capture rate and using a simple bar graph to visualize this, we see that around age 25 and over there is a mostly steady positive correlation to the rates potentially implying a tendency towards better recycling ability habits in people over a certain age. 

While we were able to gain some insights into higher level questions regarding age, race, and borough differences, there still remain several potentially relevant variables in the dataset that may have a relationship to recycling rates. In order to take a quick look at each of these relationships we used a simple loop and joint plots  to get a quick look at each of the variables in a scatter plot, a histogram of the distribution of points, and correlation levels all in one image. The joint plots provided a quick overview of these relationship and help identify a few potentially strong indicators, such as a positive correlation to the percentage of population with a bachelor’s degree and negative to poverty rate. 

With there being dozens of metrics available as potential variables to sort through, I was able to narrow down the relevant variables by creating a filtered dictionary of their correlation and p-values. The ‘pearsonr’  function within the ‘scipy.stats’ package outputs a tuple of both the Pearson correlation coefficient and the p-value, which represents the probability of the null hypothesis that the relationship between two variables is random. Typically a p-value of less than 0.05 is said to be statistically significant, indicating that the relationship between variables is attributed to more than chance. This criteria was used alongside a dictionary comprehension to determine the variables that are statistically significant and thereafter separate into 4 subgroups - economic, community, race, and age - to help better distinguish what type of indicator they represent. 

Another question that comes to mind when categorizing the indicators into groups is whether any of these variables are correlated to one another. For example, within economic indicators we would anticipate significant overlap between the proportion of residents unemployed and the poverty rate based on the assumption that those in poverty likely do not have an income. Similarly, it is possible that places with higher rent burdens would be likely to have a higher percentage of clean streets since the rent may be higher in clean areas. A simple yet powerful visualization tool we can apply to answer these is to apply the heat map function from the Seaborn library, which displays the correlation between each variable against one another. Alongside the correlation, I set up the heat map to employ a Red-Yellow-Green colormap which display strong positive correlations as darker green and stronger negative correlations as darker red. Using this enables us to answer the previous questions regarding correlated variables while also getting an idea as to what variables have a strong relationship to diversion and capture rate in a univariate analysis. Moving forward to building Machine Learning algorithms such as linear regressions and random forests, it will be important to keep in mind what variables are independent and dependent on one another in order to select the features that will maximize the accuracy of the models.

## [Machine Learning Analysis](https://github.com/vsraja3/Springboard-Capstone-1/blob/master/Capstone%201%20-%20NYC%20Recycling%20-%20ML%20Report.ipynb)

After answering several high level questions regarding the data with exploratory data analysis, we are provided with an overview of the dataset with regards to differences between subgroups and univariate relationships between district indicator data and recycling rates. When coupling that with the application simple statistical methods such as measuring the Pearson correlation coefficient and p-value to isolate statistically significant data and using a correlation heat map to visualize relationships between x-variables, we have a strong foundation on which to gain greater insights by applying Machine Learning techniques to perform multivariate analysis on the relevant data to determine feature importance and prediction potential. 

With a multitude of different packages built within Python that implement various machine learning techniques, building algorithms and fitting them through data has become increasingly accessible. One of the most popular libraries is Sklearn, which contains sub packages for various different types of regressors and classifiers along with ensemble methods, regularizations, and evaluation metrics. We will primarily use variety of Sklearn packages but also include another library for ensemble learning that has become increasingly popular.

We begin by determining what type of possible ML techniques to apply to our recycling data in hopes of being able to determine feature importance of demographic data in predicting the recycling rates in a community. Given that the output or Y-variable we’re predicting is a continuous number, the problem is clearly a regression problem. The natural first model that comes to mind is to test a simple linear regression model. However, linear regression models have their limitations which is why it would be beneficial to apply other regression related techniques in and evaluate their accuracy metrics against one another to determine the best model. As such, we will implement, test and evaluate a simple linear regression model along with a Lasso regression, gradient boosting tree with XGBoost library, and a Random Forest model.

The first model implemented is the simple linear regression model. For all the models, we need to break down the data frame into matrices that include the features (X-variables) and target (Y-variable), which was done using simple Pandas functions. One important consideration when working with arrays with a large number of features, as is the case with our district indicators, is not to add features that are not relevant to the model and help improve the performance. While we used statistical metrics such as the p-value to determine what features have a correlation to the recycling rates independent of other variables and filtered out these features, it is important to consider that ML models such as linear regression may not improve with more features. As such, we used feature selection techniques to determine what features to select for the two regression models. The simple linear regression applied the technique of Recursive Feature Elimination (RFE), which works by iteratively removing features and building a model with the ones that remain and using evaluation criteria to determine the optimal number of features while ranking their importance. The results yielded an optimal number of features of 12, and when instantiating the same linear regression and fitting on the test data yielded a fairly high R-squared score of .74.

The second model built is a variation of a linear regression model called Lasso where the feature selection component is done through a regularization technique that evaluates each variable against the model and assigns a coefficient while using the technique of shrinkage to reduce the coefficient of any irrelevant variables to 0 in order to maximize the training potential and subsequent accuracy of the model. When employing the Lasso regularization method to our do, we can see that 8 of the variables have been shrunk to 0 leaving 8 variables left, with slightly different results than the previous simple linear regression with recursive feature elimination employed first which selected 12 variables. This is possibly due to the difference in computation between the two methods and the regularization accounting for correlated features. Ultimately, the model resulted in a higher score than the previous with an R-squared of .797.

The next two models built deviate from the linear regression techniques of linear regression and Lasso regularization by employing ensemble techniques, or algorithms that combine several machine learning techniques in order to improve performance and reduce variance. In addition to using linear regression as a technique to predict recycling rates, it is also possible to employ decision tree methods or a combination of the two in an ensemble method. 

The first of the two models we will use utilizes the technique of gradient boosting, which forms prediction by iteratively testing weaker models and using the results to improve the next model until the input parameters are satisfied. One of more popular libraries for gradient boosting that has often been successful in producing the best model performance, particularly with unstructured data, is XGBoost. Although we are working with structured data in this instance, XGBoost still draws intrigue as a result of its growing popularity to warrant testing. To account for the variance in performance that could result from different inputs of hyper parameters, we used grid search to determine that a linear booster and 19 estimators would be optimal based on R-squared score. We then applied the XGB grid search best estimator to fit to the test data. The results when measuring the root mean squared error as well as R-squared were ultimately lower than that of the simple linear regression and Lasso model. This suggests that with our limited dataset of structured data, the XGBoost model does not perform as well in comparison to Sklearn models.

The final model that was tested is the second of the ensemble methods - a random forest. This technique essentially utilizes bagging, or bootstrap aggregating, which is an approach that involves running a series of decision trees in parallel and taking the mean (or mode in classification problems) of the trees. Decision trees individually are simple but weaker models that can be an ideal option for simple classification problems, but have a tendency to overfit and have lower accuracy when applied to more complex regression problems such as ours. As such, using a random forest allows us to test the ensemble method of bagging in contrast to the boosting techniques applied in XGBoost previously. The Random Forest regressor is another tool available within the Scikitlearn library and, similar to XGBoost, requires the input of hyper parameters such as the max depth of each decision tree and the total number of estimators. As such, we run a grid search to find the optimal hyper parameters and fit the estimator to the testing data in a similar fashion as the previous exercise. Ultimately, the accuracy metrics of root mean squared error and R-squared score do indeed perform amongst the best of the 4 models tested.

Now that all 4 of the models have been built, trained, and tested against the same split of data (based on the constant random state parameter set) and evaluated based on their accuracy metrics, we can compare models against one another and determine which one is the most effective. The two metrics computed are the root mean squared error (RMSE), which measures the average difference between the recycling rates predicted by the model and the actual rates observed, and the R-squared score, a statistical metric that measures the proportion of variance of the Y-variables results from the variance of all the X-variables in a model. Both metrics are calculated as a function of sum of squared errors, but while RMSE is largely a measure of the goodness-of-fit of the model with lower values representing more accurate model, the R-squared score is rather a measure of the ability of a model and its input variables to influence the output with the value being represented as a percentage. Given that the primary goal of our analysis of NYC recycling data is to examine what features in the makeup of a population have the highest impact on improving recycling conversion rather than to predict future recycling rates across the city, the R-squared score is a more relevant measure for our purpose.

As such we can review the metrics calculated during each model’s test and see that the best performing model for goodness-of-fit (ie. the one with the lowest root mean squared error) is the Lasso model with an error of approximately 1.55. However, the best performing model based on the R-squared score, which as described earlier measures the percentage of the variance of the target variable that can be explained by the model, looks to be the Random Forest model with an R-squared score of .81.

After identifying the Random Forest model as the optimal model built, we can further examine the model to visualize how it ran and determine what features in the district data have the highest impact on the model in terms of positive and negative influence. This can be done by plotting the feature importance, a built-in attribute of the random forest model, in a simple horizontal bar graph. Doing so we can see that there are three features that are particularly significant with scores above .10 in poverty rate, percentage black population, and percentage clean streets. Just behind those three we can see that percentage with a bachelor’s degree and percentage white are also fairly high. 

These results are similar to the expectations of features with high importance based on the univariate analysis of measuring each variable’s Pearson correlation against the recycling rates, with the highest ranking variables being amongst those with high absolute value of correlation. However, there are other variables that were expected to be high in feature importance based on correlation that were instead amongst the lowest, such as the unemployment rate and senior percentage. This is potentially due to the covariance between features that influence the proportions in a community, such as the poverty rate and unemployment rate having overlap, as well as senior proportion and minors being inversely correlated. Moreover, when comparing the feature importances of the Random Forest with the other models, there appears to be significant overlap with the Lasso regression model with the regards to their coefficients. This is encouraging as the two models yielded the highest scores comparatively of the four tested.

## Conclusion

Ultimately, the results of this analysis lead to a number of discoveries regarding the relationships between recycling rates and the breakdown of population parameters within the communities of New York City. While there is limited evidence of any definitive conclusions regarding the behavior of different subgroups of population, there is a lot that can be inferred by the statistical analysis and machine learning models we have built such as the features of a community that can be expected to yield an increase or decreasing trend in recycling rates. 

The biggest takeaway to consider is the influence of the features with the highest importance as it provides insights into what proportional indicators have the highest influence on recycling while also taking into consideration any confounding dependent variables that vary alongside them. These features provide a starting point towards an examination of what the city and DSNY can do to target improvement of the city’s overall waste management and recycling potential. The top four variables are indicators which are examples of three different categories of community indicators. 

Poverty rate is a socio-economic indicator which has the highest feature importance. While the feature importance does not tell us the direction in which a variables is influencing our target (ie. do higher poverty rates lead to higher or lower diversion rates), we will use the direction of the univariate relationship between the two having a strong negative Pearson correlation to assume that the influence is negative. Based on that assumption, a potential course of action that can be taken to improve recycling rates in communities higher in poverty include providing monetary incentives for disposing recyclable material such as a bottle deposit program, tax relief for purchases and sales of renewable material for local grocery stores, and a targeted improvement to the sustainability education programs in schools and community centers. These are all examples of potential solutions that target a specific group as it can be discerned that any sort of incentive that provides financial relief is likely to be particularly appealing to communities struggling financially, and even possibly a solution likely to incite action among all groups. 

The second highest feature importance that pertains to racial breakdown is the percentage of black population in a district. Based on our statistical analysis using the correlation heat map shown previously, there is some overlap between proportion of black population and poverty rate with a slight positive correlation. As such, the solutions recommended towards targeting communities in poverty could potentially be effective in those with a higher proportion of black residents. Beyond those solutions there are a number of options including outreach towards promoting recycling programs within churches, recreation centers, and other and other areas that involve congregation of racial groups. 

The third highest feature importance is a community environmental indicator in the percentage of clean streets. Once again, there is a high correlation between this and poverty rate so the suggested solutions would similarly have a positive impact on these communities as well. There is also the natural relationship between areas that are not maintained well by street cleaning efforts and higher instances of littering being clearly less successful in recycling efforts. Another potential solutions that could target these areas is organize voluntary or paid street cleaning efforts, and improve upon existing ones by encouraging the additional step of separating recyclable materials (such as plastic) on the streets and disposing of and recycling accordingly. Naturally, a monetary incentive tied to this would be beneficial.

The fourth highest feature with significantly lower importance but representing another category of age group proportion is the proportion of minors, or residents aged under 20 using our 20-year splits. Due to the high correlation between proportion of minors and features like clean streets and poverty rate, all the previous suggestions can be applied to target areas with higher youth population, along with the addition to targeted improvement in sustainability education at public schools in communities with more youth. 

Taking all our findings into consideration, we end up with a test run of four different machine learning models with the benefits of grid search and cross validation to account for combinations of hyper parameters and splits in data. The strongest model was determined based on the accuracy metric of R-squared score, which measures the variance of the target variable attributed to the model and input variables. This ended up being the Random Forest model with the biggest takeaway from the model being the feature importances of each district indicator when combined with the variance of the rest of the model. Beyond the conclusions we have drawn so far, there is still plenty of area for additional exploration and research. 

One of these possibilities is to utilize the models built as a predictive tool that can be applied to NYC community districts in the future or even other cities across the country to predict their expected recycling rates. This type of predictive information can be used on a larger scale by the sanitation departments in other cities to target weaker performing communities for promoting sustainability education and prioritize the curbside pickup recycling routes of the more successful communities to maximize conversion rates and minimize pollution from fuel emissions. 

Advanced analysis and application such as these options would clearly go beyond the scope of this project and require additional data and resources to overcome the limitations of the data worked with here. I’ve noted the limitations that should be highlighted as I worked through the project. One of this is the lack of accessibility of community district data that spans through time rather than being averaged through a 5-year span to match up with the recycling measurements which were captured through time monthly. If there were a way to collect this data it may improve insights by adding time as a variable and potentially running time series analyses to record increases and decreases through time more closely. Another limitation is the size of the dataset with it being limited to a time averaged recording across 59 community districts, ultimately only leaving a sample size of 59 unique observations. Going along with size limitations, the use of proportional data to draw conclusions is useful but less effective than individual recordings. An example to elaborate on this is having a recording of the recycling rates of an individual or household rather than the aggregate amongst a community. Having individual data points would increase the sample size and allow the potential for stronger conclusions into what factors contribute to an individual’s ability to recycle. Obviously recording recycling data on an individual level is a largely impractical option due to the amount of additional collection efforts required, so it would have to be explored on the basis of how necessary it is to acquire this data and if the potential benefits could outweigh the costs. I believe there is a large potential for further research into this topic I would consider with additional time and resources, but leave the existing conclusions in this report as a starting point.
