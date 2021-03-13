# Banks Classification 

*TODO:* Write a short introduction to your project.

The primary objective is to develop an early warning system, i.e. binary classification failed (`'Target'`==1) vs. surviving (`'Target'`==0), for regulated banks using their quarterly filings with the FDIC. There was a significant increase in the number of failing banks from 2009 to 2014.  Overall, 137 failed banks and 6,877 surviving banks were used for the modeling. Historical data from the first 8 quarters ending 2010Q3 (`./data`) is used to train the model and out-of-sample testing is performed on quarterly data starting from 2012Q4 (`./oos`).  For more information on methodology please view `CAMELS.md` file included in the repository.

## Project Set Up and Installation
*OPTIONAL:* If your project has any special installation steps, this is where you should put it. To turn this project into a professional portfolio project, you are encouraged to explain how to set up this project in AzureML.

## Dataset

### Overview
*TODO*: Explain about the data you are using and where you got it from.

Approximately 2,000 preliminary features were obtained for every bank instance from "Report of Condition and Income" (CALL report) using publicly available SOAP APIs. Eventually, only 14 financial metrics were used for the actual classification. For more information about CALL reports please visit https://www.investopedia.com/terms/c/callreport.asp


### Task
*TODO*: Explain the task you are going to be solving with this dataset and the features you will be using for it.

Selected financial ratios were used to produce unique risk profiles according to CAMELS valuation framework. This framework is used to assess performance along 6 risk dimensions, namely Capital, Assets, Management, Earnings, Liquidity, and Sensitivity to market risk, hence the abbreviation. It was assumed that a failing bank will exceed its risk capacity along several diminsion and eventually would face a liquidity crises. For more information about CAMELS framework please visit https://en.wikipedia.org/wiki/CAMELS_rating_system

Financial metrics recorded in the last reports of the failing banks should  have a predictive power that is needed to forecast future failures. Due to significant class imbalances and taking into account costs accosiated with financial distress, the model should aim to maximize recall score. In other words, accuracy is probably not the best metrics, as Type II error needs to be minimized.

Benchmark model was created in order to better understand the requirements. Sklearn `traing-test-split` was used with `StandardScaler` to prepare for Gradient Boosting tree-based `GridSearch`, optimizing for recall. The resulting model performed reasonably well on the testing dataset with AUC of 0.97. Out-of-sample results were also very promising as recall scores  were ranging from 0.76 to 1. 

### Access
*TODO*: Explain how you are accessing the data in your workspace.

Training data with respective CAMELS features is stored in CSV format `'data/camel_data_after2010Q3.csv'` and is uploaded to Azure storage to be accessed with `dataset = ws.datasets['camels']`. 

The data from last reports of failed banks is collected for 4 preceeding quarters as noted in `'AsOfDate'` column. All healthy or surviving banks are taken as of 2010Q3. This means that the failed banks are represented by a panel dataset with observations from diffirent quarters. For example, if failed Bank A submitted its last report in 2010Q1 and another failed Bank B submitted its last report in 2010Q2, both banks will appear in the training dataset. Healthy Bank C could have submitted reports in Q1 and Q2 as well, but only features as of 2010Q3 will be used for trianing, as it survived after 2010Q3 reporting cycle. This important choice was neccessary in order to partially mitigate imballanced classes.

Out-of-sample data covers 9 quarters starting in 2010Q4 and contains reports submitted by failing and surviving banks as of quarter end. Obviously, failed banks will not appear in the next quarterly reporting cycle, but not all missing banks are automatically failed banks. There are numerous reasons why reports can be missing, for example consolidation, change of charter or mergers and acquisitions. These out-of-sample datasets are stored in respective CSV files, for example `'oos/camel_data_after2010Q4_OOS.csv'` contains CAMELS features reported by all the banks in Q4 of 2010.

 

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
