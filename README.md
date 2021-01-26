# PROJECT-2 

___
## OBJECTIVE
Predicting worldwide movie revenue at upon domestic opening. 
Potential use: in a situation where a movie is doing less than expected at the domestic release, the stakeholders may want to review their expectation and estimate of total revenue globally using  different tool(s) than what were used to sanction the production of the movie. 

## METHOD & TOOLS

### WEB_SCRAPING

The data required for making a prediction model was scraped using BeautifulSoup.

The web scraping started at a specific movie page, in my case I scraped this movie [Star Wars: Episode VII - The Force Awakens](https://www.boxofficemojo.com/title/tt2488496/credits/?ref_=bo_tt_tab#tabs) where from the information about the movie such as  was scraped and put into a dictionary. The information that we can get from the movie page includes, but not limited to: production budget, movie (domestic opening, domestic, international and worldwide)revenues, rating, runtime, genres, release date, crew and casts. 

More actual data are required for making a prediction model. The Box Office Mojo actually has several type of movie lists that we can use. I used the All-Time Charts for Top [Lifetime Grosses](https://www.boxofficemojo.com/chart/top_lifetime_gross/?ref_=bo_cso_ac) which includes 1000 movie data. The list of the movie was scraped from this page and for each movie in the list, the information from each movie page is appended by calling the link to each movie. The data were collected in a dictionary and transformed into a data frame when all information of the movies in the list were collected.

### EDA

Data from years prior to year 2000 were removed.
Rows with null values on target and features were also removed.

Ideally the prediction model should initially use and investigate as much as possible all information available upon the domestic release such as: production budget, domestic opening gross and general information about the movie (e.g., director, actor, distributor, movie genres, rating, runtime and release date). However for this project only the following were used:

- production budget
- domestic opening gross revenues
- worldwide gross revenues
- release date (month)
- runtime
- movie rating

The worldwide gross revenues variable will be used as target and the rest as predicting features.

### LINEAR REGRESSION
 
#### Initial assessment
The data were first assessed by visualizing their correlations using Seaborn heatmap and pairplot graphs and running the statsmodels ols modelling to get the initial R^2 value before any transformation of the features and/or target.

#### Feature engineering & preselection
The predicting model construction started by verifying if a simple model can be achieved by removing one feature one by one and compare the ols R^2 score to the initial value, keeping only the features that did not decrease ols score.

#### More feature engineering & Cross Validation
More feature engineering to include/evaluate transformed, polynomial and interacting features were carried out through sklearn KFold cross validation.
For each additional feature that increased the R^2 validation scores a candidate model were created. Upon the completion of the cross validation process, the R^2 validation score were compared and the model with the highest validation score was selected as the predicting candidate model.

This is the outcome from my cross validation process:
|      MODEL    |                       FEATURES                            | MEAN R^2 VAL SCORE  |
| :------------ | :-------------------------------------------------------: | ------------------: |
| initial model | simple(all simple numerical features)                     |        0.737        |
| MODEL 1       | simple + opening gross^2                                  |        0.737        |
| MODEL 2       | simple + budget^2                                         |        0.715        |
| MODEL 3       | simple + runtime^2                                        |        0.741        |
| MODEL 4       | simple + runtime^2 + month^2                              |        0.741        |
| MODEL 5       | simple + runtime^2 + year^2                               |        0.741        |
| MODEL 6       | simple + runtime^2 + c1 (budget x runtime)                |        0.741        |
| MODEL 7       | simple + runtime^2 + c2 (opening gross x runtime)         |        0.730        |
| MODEL 8       | simple + runtime^2 + c3 (opening gross / year)            |        0.743        |
| MODEL 9       | simple + runtime^2 + c4 (budget / year)                   |        0.744        |
| MODEL 10      | simple + runtime^2 + c4 + c5 (month x runtime)            |        0.745        |
| MODEL 11      | simple + runtime^2 + c4 + c5 + c6 (month x year)          |        0.743        |
| MODEL 12      | simple + runtime^2 + c4 + c5 + c7 (month x opening_gross) |        0.753        |
| MODEL 13      | simple + runtime^2 + c4 + c5 + c7 + rating(PG + PG_13 + R)|        0.760        |

Note : simple feature includes budget, opening_gross, runtime, month and year

The selected model from the cross validation process is MODEL 13 with mean validation score of 0.76. The R^2 the test score however was much lower at 0.511. Further investigation was required to address this  overfitting potential and/or complexity issues. This was carried out using Lasso Regularisation.


#### Lasso Regularisation
Lasso regularisation was carried out for the selected MODEL 13. However, for the sake of comparison, I also did Lasso regularisation on the model that include simple and categorical features without any polynomial and interacting features.

- Case 1: LR for the selected MODEL 13
- Case 2: LR for the model including only initial features without any 2nd degree or interacting features.

Following the project presentation, additional cases were investigated to deal with outliers, e.g., top grossing movies at the movie release and  the low end revenues.

- Case 3: LR for the Case 1 but with data for movie domestic opening gross more than 200 Million USD removed to verify if a better prediction model can be achieved. 

- Case 4: LR for the Case 3 with data for domestic opening revenues less than 0.5 Million USD dropped.


## RESULTS & CONCLUSIONS

|    CASE      |  R^2 TEST SCORE  |    MAE (Million USD)     |
| :----------- | :--------------: | -----------------------: |
| CASE 1       |       0.523      |          118.7           |
| CASE 2       |       0.505      |          116.0           |
| CASE 3       |       0.714      |           96.9           |
| CASE 4       |       0.638      |          105.0

The best predicting model was achieved for Case 3 and the coefficients after LR are as follows:

|    FEATURE    |   COEFFICIENT (ROUNDED)   | 
| :------------ | ------------------------: |
| opening gross |       161,859,254.6       |
| budget        |        69,804,824.6       |
| year          |         9,624,883.5       |
| PG            |        17,621,363.4       |
| runtime^2     |         8,019,851.7       |
| c5            |        38,607,514.3       |



![Case 3 Actual vs Prediction](case_3a_lr_s.pdf)



