# log\_model

Logs a model, given a model object along with the framework. We can only log one model per run. If attempt to log model more than once an error is thrown out.

| Parameters                                     | Description                                                                                                                                                   |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**framework**</mark> | <p>(mlf.ModelFrameWork) Framework of the model. example: sklearn, pytorch, tensorflow.</p><p>List of available frameworks: <a href="../enums.md">Link</a></p> |
| <mark style="color:blue;">**model**</mark>     | model object in particular                                                                                                                                    |

#### Example

```python
mlf_run.log_model(clf, mlf.ModelFramework.SKLEARN)
```
