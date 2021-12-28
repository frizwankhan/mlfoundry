# Quick Start

## Install the MLFoundry

{% tabs %}
{% tab title="Python" %}
```
# Install via pip
pip install mlfoundry mlfoundry-ui --extra-index-url https://api.packagr.app/public
```
{% endtab %}
{% endtabs %}

## Initialise MLFoundry

Create an project and use the project\_name to create a run:

```python
import mlfoundry as mlf

mlf_api = mlf.set_tracking_uri()
 # create a run
mlf_run = mlf_api.create_run(project_name=<project-name>)
```

If you want to use a previously created run:

```python
mlf_api = mlf.set_tracking_uri()
# printing all the run's available and get the run_id
mlf_runs = mlf_api.get_all_runs()
mlf_run = mlf_api.get_run(run_id=<run_id>)
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

#### Log predictions

Log predictions synchronously:

```python
mlf_run.log_predictions(self, feature_df, predictions)
```

Log predictions asynchronously

```python
responses = mlf_run.log_predictions_async(self, feature_df, predictions)

#### To confirm that the log request completed successfully, await for futures to resolve: This is a blocking call
import concurrent.futures as cf
for response in cf.as_completed(responses):
  res = response.result()
```
