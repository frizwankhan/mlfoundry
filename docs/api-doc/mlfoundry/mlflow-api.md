# MlFlow API

We can use any mlflow api on top of our mlf\_run.

#### Example

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name=<project-name>)

with mlf.mlflow.start_run(run_id=mlf_run.run_id, experiment_id=mlf_run.experiment_id) as run:
    mlf.mlflow.set_tag("release.version", "0.1.0")
```
