# get\_model

get\_model returns the model object on the particular framework that was logged with mlfoundry.&#x20;

| Parameters | Description                                                                                                                         |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| dest\_path | (default: None) If specified by the user the model is saved to the path that is given in des\_path and the loaded model is returned |

#### Example:

```python
sk_model = mlf_run.get_model()

# use Pandas DataFrame to make predictions
pandas_df = ...
predictions = sk_model.predict(pandas_df)

```
