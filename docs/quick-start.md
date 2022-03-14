# Quick Start

## Install the MLFoundry

{% tabs %}
{% tab title="Python" %}
```
# Install via pip
pip install mlfoundry mlfoundry-ui
```
{% endtab %}
{% endtabs %}

## Initialise MLFoundry

Create an project and use the project\_name to create a run:

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
 # create a run
mlf_run = mlf_api.create_run(project_name=<project-name>)
```

If you want to use a previously created run:

```python
mlf_api = mlf.get_client()
# printing all the run's available and get the run_id
mlf_runs = mlf_api.get_all_runs(project_name=<project-name>)
mlf_run = mlf_api.get_run(run_id=<run_id>)
```

## Start Dashboard
```bash
mlfoundry ui
```


## Start Logging

#### Log a model:

```python
mlf_run.log_model(sklearn_model, framework=mlf.ModelFramework.SKLEARN)
```

#### Log parameters

```python
mlf_run.log_params({'learning_rate':0.01,
                  'n_epochs:10'
                  })
```

#### Log metrics

```python
mlf_run.log_metrics({'accuracy':87,
                  'f1_score':0.84,
                  })
```
