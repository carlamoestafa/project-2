# PROJECT-2 PLAN
___
## BACKGROUND
With increased interest in veganism (articles, vdo streaming, social media, dedicated sections for vegan products in the supermarkets etc...), I would like to first confirm if my perception is correct and unbiased. Furthermore, I would like to see if there are any correlations with other factors such as income, health, education level and environment.
## METHOD
Data required will be acquired by web scraping. 
Although data recording the number or proportion of vegans in the various populations is very limited, [GoogleTrends data for vegan](https://trends.google.com/trends/explore?date=all&geo=RU&q=vegan) is available. These data will be used and analysed based on the assumption that increasing search trends means increasing interest and vice versa. For the correlation analysis, feature data will be chosen from indices recorded and maintained by [OECD Organisation](https://www.oecd.org).</br>
Selected indices from OECD comprise of the following:
- GDP or average wages (to be decided later)
- Meat consumption
- Crop production
- Adult education level
- Better Life index
- Health indicators: life expentacy at 65, suicides rates, deaths from cancer
- CO2 emissions

The GoogleTrends data for the vegan term will be collected and analysed and then collated with the selected OECD Countries feature data for EDA and regression analysis.
  
## TOOLS

Tools to be used:
- pandas
- matplotli
- seaborn
- beautiful soup
- stastmodels
- scikit-learn

## POSSIBLE IMPACTS
If the results of the  analysis show a desired correlation(s) between increased interest for veganism and feature indicators, the analysis can be used by governments and veganism advocacy groups as a basis upon which to collect more robust and reliable data to further study the correlation(s) between number of vegans in the population and various indicators to develop and implement better policy and public health awareness campaigns.