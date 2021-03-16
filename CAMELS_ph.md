# CAMELS risk profiles
Note: this research presents preliminary findings and is being distributed to interested readers solely to stimulate discussion and elicit comments. 

## "Report of Condition and Income" (CALL report)
Approximately 2,000 original  features were obtained for every bank instance from "Report of Condition and Income" (CALL report, example report is stored here `'data/CALL_175458.PDF'`) using publicly available SOAP APIs on https://banks.data.fdic.gov/docs/. Eventually, only 14 financial metrics were used for the actual classification:

![]()

### Surviving banks
Distribution of selected financial ratios of surviving banks:

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/strong_financials.png)

### Failed banks
Distribution of selected financial ratios of failed banks:

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/weak_financials.png)

## CAMELS framework
Although there was a lot of intra-class variability, these ratios were successfully used to produce comparable risk profiles according to CAMELS valuation framework. 

### Featurization 
![]()

The framework aims to assess performance along 6 risk dimensions, namely Capital, Assets, Management, Earnings, Liquidity, and Sensitivity to market risk. 

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/single_CAMELS.png)

### Weak profile
It was assumed that a failed bank will exceed its risk capacity along several dimensions and eventually would face a liquidity crises. For example, first (1) deteriorating asset quality will lead to (2) negative earnings. This will pierce capacity on (3) capital dimension and the bank may face (4) liquidity crisis. Below is multi-period evolution of a model failed bank.

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/weak_CAMELS.png)

### Strong profile
Healthy banks manage to maintain their risk profile within the contour of their risk capacity at all times. Below is multi-period evolution of a model stressed but surviving bank.

![](https://github.com/allaccountstaken/automl_v_hyperdrive/blob/main/plots/strong_CAMELS.png)


For more information about CAMELS framework please visit the regulator's resource here https://www.fdic.gov/deposit/insurance/assessments/risk.html or an unofficial explanation here https://en.wikipedia.org/wiki/CAMELS_rating_system.
