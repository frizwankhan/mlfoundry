# get\_all\_projects

Returns name of all projects that has been created.

#### Output:

(List) - List of project names created already.

```python
import mlfoundry as mlf

mlf_api = mlf.set_tracking_uri()
mlf_projects = mlf_api.get_all_projects()
```
