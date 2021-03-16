# Early Warning Financial Distress Classification System

In normal, non-stressed environment, it is very hard to predict failing banks as it is a very rare event equivalent of anomaly detection. There was a significant increase in the number of failed banks from 2009 to 2014 what produced enough data for effective classification. Additionally, it was also important to creat comparable risk profiles. Below are annual counts of regulated banks, healthy in blue and failed in red.

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/all_banks.png) 

The primary objective is to develop an early warning system, i.e. binary classification of failed (`'Target'==1`) vs. survived (`'Target'==0`), for US banks using their quarterly filings with the regulator. Overall, 137 failed banks and 6,877 surviving banks were used in this machine learning exercise. Historical observations from the first 4 quarters ending 2010Q3 (`./data`) are used to tune the model and out-of-sample testing is performed on quarterly data starting from 2010Q4 (`./oos`).  For more information on methodology please refer to `CAMELS.md` file included in the repository, below are annual failed banks counts.

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/failed_banks.png)

## Project Set Up and Installation
*OPTIONAL:* Explain how to set up this project in AzureML.

## Dataset

### Overview

Approximately 2,000 preliminary features were obtained for every bank instance from "Report of Condition and Income" (CALL report, example included here `'data/CALL_175458.PDF'`) using publicly available SOAP APIs on https://banks.data.fdic.gov/docs/. Eventually, only 14 financial metrics were used for the actual classification:


    `selected_features = {
        'RIAD3210' : 'Total equity capital', 
        'RCON2170' : 'Total assets', 
        'RCON3360' : 'Total loans', 
        'RCON3465' : '1-4 family residential loans', 
        'RCON3466' : 'Other real estate loans', 
        'RCON3387' : 'Commercial and industrial loans',
        'RCONB561' : 'Credit cards', 
        'RCON3123' : 'Allowance for loan losses',
        'RIAD4093' : 'Total noninterest expense', 
        'RIAD4300' : 'Net Income before',
        'RCON2215' : 'Total transaction deposits', 
        'RCON2385' : 'Total nontransaction deposits', 
        'RCON1773' : 'Available-for-sale Fair Value',
        'RIAD4150' : 'Number of full-time employees' }`

For more information about CALL reports please visit the regulator's website at https://cdr.ffiec.gov/public/ManageFacsimiles.aspx. Detailed description is available here: https://www.investopedia.com/terms/c/callreport.asp

### Task

Selected financial ratios were used to produce unique risk profiles according to CAMELS valuation framework, that is explaind in detail in suplemental `CAMELS.md` file. This framework is used to assess performance along 6 risk dimensions, namely Capital, Assets, Management, Earnings, Liquidity, and Sensitivity to market risk. It was assumed that a failed bank will exceed its risk capacity along several diminsion and eventually would face a liquidity crises. 

For more information about CAMELS framework please visit the regulator's resource here https://www.fdic.gov/deposit/insurance/assessments/risk.html or an unofficial explanation here https://en.wikipedia.org/wiki/CAMELS_rating_system

Financial metrics recorded in the last reports of the failed banks should have predictive power that is needed to forecast future failures. Due to significant class imbalances and taking into account costs accosiated with financial distress, the model should aim to maximize the recall score. In other words, accuracy is probably not the best metrics, as Type II error needs to be minimized.

Basic benchmark model was created in order to better understand the requirements. Sklearn `traing-test-split` was used with `StandardScaler` to prepare for Gradient Boosting tree-based `GridSearch`, optimizing for recall. The resulting model performed reasonably well on the testing dataset with AUC of 0.97. Out-of-sample results were also very promising as recall scores  were ranging from 0.76 to 1. Out of 138 banks that failed during the period from 2010Q4 to 2012Q4 the benchmark model correctly flags 124 failed banks based solely on the information from their last CALL reports. With time the number of failed banks decreases sharply and so does predictive power of the model.

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/oos_GBM.png)

### Access
*TODO*: Explain how you are accessing the data in your workspace.

Training data with respective CAMELS features was stored in CSV format `'data/camel_data_after2010Q3.csv'` and uploaded to Azure storage to be accessed with `dataset = ws.datasets['camels']`. 

The data from last reports of failed banks is collected for 4 preceeding quarters prior to 2010Q3 as noted in `'AsOfDate'` column. All healthy or surviving banks are taken as of 2010Q3 quarter-end. This means that the failed banks are represented by a panel dataset with observations from diffirent quarters. For example, if failed Bank A submitted its last report in 2010Q1 and another failed Bank B submitted its last report in 2010Q2, both banks will appear in the training dataset. Healthy Bank C could have submitted reports in Q1 and Q2 as well, but only features as of 2010Q3 will be used for trianing, as it survived after 2010Q3 reporting cycle. This important choice was neccessary in order to partially mitigate imballanced classes.

Out-of-sample testing data covers 9 quarters starting in 2010Q4 and contains reports submitted by failing and surviving banks as of respective quarter-end. Obviously, failed banks will not appear in the next quarterly reporting cycle, but not all missing banks are automatically recorded as failed banks. There are numerous reasons why reports can dissaper from the sample, for example consolidation, change of charter or mergers and acquisitions. These out-of-sample quarterly datasets are stored in respective CSV files, for example `'oos/camel_data_after2010Q4_OOS.csv'` contains CAMELS features reported by all existing banks in Q4 of 2010.

 

## Automated ML
*TODO*: Give an overview of the `automl` settings and configuration you used for this experiment

### Results
*TODO*: What are the results you got with your automated ML model? What were the parameters of the model? How could you have improved it?

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters.

## Hyperparameter Tuning
*TODO*: What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search


### Results
*TODO*: What are the results you got with your model? What were the parameters of the model? How could you have improved it?

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters.

## Model Deployment
*TODO*: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:
- A working model
- Demo of the deployed  model
- Demo of a sample request sent to the endpoint and its response

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
