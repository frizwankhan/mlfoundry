# log\_dataset\_stats

log\_dataset\_stats computes whylogs profile and different metrics for a dataframe and logs it. If shap\_values\_column\_names exists in the schema input then shap value is also logged for the dataframe.

| Parameters                                        | Description                                                                                                                                                               |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**df**</mark>           | (pandas.DataFrame) Dataframe to compute the metrics and whylogs profiles                                                                                                  |
| <mark style="color:blue;">**data\_slice**</mark>  | <p>(mlf.DataSlice) dataslice that the dataframe belongs to.</p><p>List of available dataslice: <a href="broken-reference"><mark style="color:purple;">Link</mark></a></p> |
| <mark style="color:blue;">**data\_schema**</mark> | (mlf\_.Schema\_) schema of the dataframe that contains the feature names and target labels.                                                                               |
| <mark style="color:blue;">**model\_type**</mark>  | (mlf.ModelType) List of available ModelType: [<mark style="color:purple;">Link</mark>](broken-reference)                                                                  |

#### Examples

```python
import mlfloundry as mlf
import shap        

X_train_df1 = pd.DataFrame(X_train, columns=iris.feature_names)
X_test_df1 = pd.DataFrame(X_test, columns=iris.feature_names)

# Schema for the dataset
schema = mlf.Schema(
        feature_column_names=iris.feature_names,
        prediction_column_name="predictions",
        actual_column_name="targets"
        )

# compute and log stats for train data
mlf_run.log_dataset_stats(
    X_train_df, 
    data_slice=mlf.DataSlice.TRAIN,
    data_schema=schema,
    model_type=mlf.ModelType.MULTICLASS_CLASSIFICATION
)
```
