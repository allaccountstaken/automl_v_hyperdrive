# Banks Classification 

*TODO:* Write a short introduction to your project.

The primary objective is to develop an early warning system, i.e. binary classification failed (1) vs. healthy (0), for regulated banks using their quarterly filings with the FDIC. There was a significant increase in the number of failing banks during from 2009 and 2012. Historical data from the first 8 quarters ending 2010Q3 (`./data`) is used to train the model and out-of-sample testing is performed on quarterly data ending 2012Q4 (`./oos`). Overall, 137 failed banks and 6,877 surviving banks were used for then modeling. For more information on methodology please view `CAMELS.md` file included in the repository.

## Project Set Up and Installation
*OPTIONAL:* If your project has any special installation steps, this is where you should put it. To turn this project into a professional portfolio project, you are encouraged to explain how to set up this project in AzureML.

## Dataset

### Overview
*TODO*: Explain about the data you are using and where you got it from.

Approximately 2,000 preliminary features were obtained for every bank instance from "Report of Condition and Income" (CALL report) using publicly available SOAP APIs. Eventually, only 14 features were used for classification. For more information please visit https://www.investopedia.com/terms/c/callreport.asp


### Task
*TODO*: Explain the task you are going to be solving with this dataset and the features you will be using for it.

Selected financial ratios listed above were used to produce unique risk profiles according to CAMELS valuation framework. This framework is used to assess performance along 6 risk dimensions, namely Capital, Assets, Management, Earnings, Liquidity, and Sensitivity to market risk, hence the abbreviation. For more information please visit https://en.wikipedia.org/wiki/CAMELS_rating_system

### Access
*TODO*: Explain how you are accessing the data in your workspace.

Training data with respective CAMELS features is stored in CSV format `'data/camel_data_after2010Q3.csv'`. The data from last reports of failed banks is collected for 8 preceeding quarters as noted in `'AsOfDate'` column. Healthy or surviving banks are taken as of 2010Q3. This means that failed banks are represented by panel data from diffirent quarters. For example, if failed Bank A submitted its last report in 2010Q1 and another failed Bank B submitted its last report in 2010Q2, both banks will appear in the training dataset. Healthy Bank C could have submitted reports in Q1 and Q2 as well, but only features as of 2010Q3 will be used for trianing. This important choice was neccessary in order to mitigate imballanced classes problem.

Out-of-sample data covers 9 quarters starting in 2010Q4 and contains reports submitted by failing and surviving bankc as of quarter end. Obviously, failed banks will not appear in the next quarter of reports, but not all missing banks are failed. There are numerous reasons why reports can be missing, for example consolidation, charter change and acqusitions. These quarterly data is stored in respective CSV files, for example `'camel_data_after2010Q4_OOS.csv'` contains CAMELS features reported by all the banks in Q4 of 2010.

 

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
