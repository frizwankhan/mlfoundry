# log\_params

Logs the parameter given param\_dict that has param name as key and param value as value. If multiple param values are logged under the same param then an error is thrown out.

| Parameters                                       | Description                                                                                                                                                              |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <mark style="color:blue;">**param\_dict**</mark> | (dict) - params to be logged in a key value pair, key is the param name and value is the corresponding value. If a param with same key is logged an error is thrown out. |

#### Example

```python
params = {'classes': clf.classes_, 'features': clf.n_features_in_}
mlf_run.log_params(params)
```

We can also pass argparse Namespace objects directly

```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("-batch_size", type=int, required=True)
args = parser.parse_args()
mlf_run.log_params(args)
```
