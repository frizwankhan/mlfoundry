# get\_run

Given the run\_id _of a_ mlf\_run returns the corresponding MLFondryRun object.

| Parameters                                   | Description                                                    |
| -------------------------------------------- | -------------------------------------------------------------- |
| <mark style="color:blue;">**run\_id**</mark> | (str, optional) run\_id of MLFoundryRun object to be returned. |

#### Returns

<mark style="color:blue;">**MLFoundryRun**</mark> object - an MLFoundryRun object is returned if it exists.

#### Example

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
## Getting run by id
mlf_run = mlf_api.get_run(run_id=<run-id>)
```
