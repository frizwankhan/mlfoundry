# log\_metrics

Logs a metric dictionary, If multiple metric value is logged under same metric then the value is appended.

| Parameters                                         | Description                                                                                                                             |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**metrics\_dict**</mark> | (dict) - dictionary of metrics, If metrics with same key is logged then the metric is appended to the already existing value in a list. |
| <mark style="color:blue;">**step**</mark> | (int) - step associated with the metrics present in metric_dict |
#### Example

```python
from sklearn.metrics import accuracy_score, f1_score

metrics_dict = {}

metrics_dict['accuracy_score'] = accuracy_score(y_test, y_hat_test)
metrics_dict['f1_score'] = f1_score(y_test, y_hat_test, average='micro')

mlf_run.log_metrics(metrics_dict)
```
