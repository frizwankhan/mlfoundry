# log\_predictions\_async

Similar to log\_predictions API, but logs the predictions asynchronously. And returns a python Future object, which we can await and check if the predictions are logged successfully.

| Parameters                                          | Description                                                                                                                                      |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| <mark style="color:blue;">**feature\_df**</mark>    | (pandas.DataFrame) Dataframe of the processed data                                                                                               |
| <mark style="color:blue;">**predictions**</mark>    | (pandas.Series or List) : list of prediction values to be logged. Validation is done to check if the size of feature df and prediction matches . |
| <mark style="color:blue;">**fileformat**</mark>     | (mlf.FileFormat, optional, default=PARQUET) Format of the file for the dataset to be saved.                                                      |
| <mark style="color:blue;">**feature\_names**</mark> | (List optional) - list of feature names                                                                                                          |

#### Examples

```python
responses = mlf_run.log_predictions_async(X_test, list(y_hat_test))

#### To confirm that the log request completed successfully, await for futures to resolve: This is a blocking call
import concurrent.futures as cf
for response in cf.as_completed(responses):
  res = response.result()
```
