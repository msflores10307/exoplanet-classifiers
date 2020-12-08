# machine-learning-challenge

### Introduction

- The aim of this project was to use machine learning techniques to create models that could tell apart actual exoplanets from false positives, and compare the results. I chose two methods to compare: Support Vector Machine and Random Forest. These each have distinct srengths and weaknesses, and required looking at the data differently in order to make the most of them. The information below describes my approach and my findings.

### Feature selection Strategy

- The code I used for feature selection exists in FeaturesSelectionJamSesh.ipynb. In this notebook, I try to transform the data in a variety of ways and explore different ways to visualize the features in the model. My first task was to isolate the numerical features from the binary ones. I did this by inspecting the data types. I created a set of boxplots for each feature, showing distributions for Candidates, False Positives, and Confirmeds side by side. This made it easy to see which features were  helpful for telling apart these categories, and which were noisy and insignificant. 
- I initially thought that scaling the results would change the way these visualizations looked, and that perhapse, I'd find a few more features worth pulling in, but I found that after using standardscaler() and minmaxscaler() the boxplots looked the same.
- There were various features that looked to be correlated with other features, so I doubted whether they were helpful to add. To assess these cases, I created scatter plots and in some cases linear regressions of the features in question. With the exception of one or two features, I found that even when distributions appeared to overlap significantly in the box plots, the correlation between two potentially collinear variables did not seem strong enough to justify throwing one out and keeping another. 
- I stored my feature selection rationale in the spreadsheet titled featureSelectionNotes.xlsx. 


### Data Pre-Processing

- Prior to creating any models I performed minmax scaling on the data. I also removed any features that were not selected, and performed the appropriate scaling.  I stored the data in a csv file called 'processed.csv', and imported this into the corressponding notebooks to create the ML model. Once in the model notebook, I removed any NaN values from the data.

### Support Vector Machine Model
- model_SVM.ipynb
- To perform the SVM model, I set the variable koi_pdisposition as my target. This variable is binary and contains only the Candidates and False Positives. Because the candidates category contains objects that may prove to be false positives later as well as exoplanets that are not yet confirmed, I thought that this model would perform poorly. However, that wasn't the case. Even before grid search optimization, the accuracy of predicted values was at around 98%. 
- Best Parameters: {'C': 10, 'gamma': 0.0001}




### Probability Tree and Random Forest Models
- model_RandomForest.ipynb
- To perform the random forest model, I set the variable koi_disposition as my target. I thought this model would be ideal since it would allow me to classify results into three categories (Candidates, Confirmed, False Positives), not just a binary output. However, I found that because the candidates category often overlaps with both the confirmed and false positive results, the accuracy was lower than I expected it to be. This underscores that ML is often about understanding the limitations of the data. 
- With an initial accuracy of 84%, the grid search optimization was not able to bring total accuracy above 89.5%! 
- The precision for Candidate and confirmed were .77 and .79, respectively, but .99 for False positive. 
- Despite lower quality results for both Candidate and confirmed precision and recall for False Positive were .99 and 1.00 respectively. This meanse we guessed false positive whenever false positive came up!
- Best Parameters: {'bootstrap': True, 'max_depth': 110, 'n_estimators': 300}


### Challenges
- The most time consuming part of this process was feature selection. I mostly used a visual criteria to tell which parameters were most likely to be significant meaningful. In the future, it would be great to do something more rigorous like performing t-tests or linear regressions to rate features quantitatively and have a more robust understanding about the relationships models reveal.
- The other challenge was that gridsearch took a very long time to run, and I had to adjust the number of parameters in the grid to be able to iterate through all of them in a reasonable amount of time. If I had more time, I would perform grid-search more thoroughly. 

### Conclusion
- Despite the lower overall accuracy, upon closer inspection, the random forest model identifies false positives better than the SVM. Both had a recall for false positives of 1, meaning whenever a result was "false positive" it was correctly classified. But, the Random Forest had a higher precicion for false positives. This means that it was less likely to assign false positive to a KOI when it was in reality a candidate for exoplanet status. For that reason, the random forest model was chosen as the best model, and saved in the repo.
- Each of my approaches had different strengths and weaknesses. To make the most of each, some compromises had to be made. For example, using the SVM required turning the target results into binary output. Also, It was frustrating having to let go of search precision in the name of performance. This underscores how machine learning is as much about engineering as it is about the math and computer science. In the future, i will try to be mindful of technical constraints when deciding on my ML strategy!
