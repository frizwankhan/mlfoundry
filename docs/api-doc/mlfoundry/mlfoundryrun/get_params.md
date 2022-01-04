# get\_params

get\_params returns all the params that are logged using MLFoundry.

#### Example:

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name=<project-name>)

params = {'classes': clf.classes_, 'features': clf.n_features_in_}
mlf_run.log_params(params)

params = mlf_run.get_params()
```
