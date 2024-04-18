### Statement of Purpose

The goal of this project is to review Time-Series Analysis using python. The data is some make-up data of sales in Macau. On weekdays, it would be relatively low, but on weekends, when significantly more tourists come, it would be considerably high. This pattern should repeat every week until some "external breaks" like holidays or new year vacations. During summer in Macau, there might be typhoons. When a level 8+ typhoon is on, most of the shops, restaurants, banks, and supermarkets (almost everything) would be mandatorily closed. I want to predict future sales based on recent historical sales by building a SARIMA model. Although it's not very difficult to draw "correct" the future predictions out without building a model, I still want my "python TS machine" to do the work and see if it generates the expected result. More details will be stated in later sections.

### Brief Explanation of SARIMA model

A Seasonal Autoregressive Integrated Moving Average (SARIMA) model has 7 paramters: (p,d,q,P,D,Q,s). In my model, I would use s=7 for seasonality because of weekly repeated patterns in my data. There might exist non-zero (P,D,Q) due to the weekly behavior. Usually, people would decide these parameters based on observations of the ACF plot and the PACF plot. My model would automatically generate those values given some MAX values of inputs. It will interate from zero to MAX for all parameters and automatically compare all those models based on AIC, test MSE, BG test result, and the number of insignificant variables.

### Explanation of Python Script

The first thing to do is to read data from excel and make it to be workable time-series data. This could be done using "read_excel" and "set_index" functions. Then I pull out ACF and PACF plots to see what the model parameters would look like. The ACF plot shows very significant values in period 1,7, and 14. This suggests parameters "q" and "Q" to be 1 and 2. The PACF plot shows very significant values in period 1, 2 (-), 6, 7, 8 (-). This suggests parameters "p" and "P" to be 1 and 1, and it is likely the data is non-stationary. After I split training sest and test set, the Augmented Dickey-Fuller (ADF) test shows that d(1) and (d,D)(1,1) are stationary. I would examine d(1) data by setting d=1. 

Now I set max values before iterations. I would set max_range_list = [2,1,2,1,0,2] with seasonality = 7. 
