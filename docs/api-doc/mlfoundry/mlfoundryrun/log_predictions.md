# log\_predictions

Logs prediction to synchronously. Predictions are saved in the local system temporarily, if multiple predictions are made and the time difference between the predictions exceeds the time threshold then the predictions are logged, or if the prediction size increases beyond the threshold file size limit of 10 MB then the predictions are logged.

| Parameters                                          | Description                                                                                                                                                                                                                                 |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**feature\_df**</mark>    | (pandas.DataFrame) Dataframe of the processed data                                                                                                                                                                                          |
| <mark style="color:blue;">**predictions**</mark>    | (pandas.Series or List) : list of prediction values to be logged. Validation is done to check if the size of feature df and prediction matches fileformat.                                                                                  |
| <mark style="color:blue;">**fileformat**</mark>     | <p>(mlf.FileFormat, optional, default=PARQUET) Format of the file for the dataset to be saved.</p><p>List of available fileformat:<a href="../enums.md"> <mark style="color:purple;">Link</mark></a><mark style="color:purple;"></mark></p> |
| <mark style="color:blue;">**feature\_names**</mark> | (List optional) - list of feature names                                                                                                                                                                                                     |

#### Examples

```
mlf_run.log_predictions(X_test, list(y_hat_test))
```
