# Regression

### Importing the libraries

```python
import shap
import pandas as pd 
from sklearn.datasets import load_boston
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split

import mlfoundry as mlf
```

### Loading the dataset

```python
boston = load_boston()

# Create a Pandas dataframe with all the features
X = pd.DataFrame(data = boston['data'], columns = boston['feature_names'])
y = boston['target']

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)
```

### Creating MLFoundry Run

```python
mlf_api = mlf.set_tracking_uri() # to save locally
mlf_run = mlf_api.create_run(project_name='boston-project')
```

### Logging the dataset

```python
mlf_run.log_dataset(X_train, data_slice=mlf.DataSlice.TRAIN)
mlf_run.log_dataset(X_test, data_slice=mlf.DataSlice.TEST)
```

### Training the model

```python
rf_reg = RandomForestRegressor(n_estimators=100)
rf_reg.fit(X_train, y_train)
```

### Logging parameters and predictions

```python
mlf_run.log_params(rf_reg.get_params())
mlf_run.log_model(rf_reg, mlf.ModelFramework.SKLEARN)

# logging predictions
y_hat_train = rf_reg.predict(X_train)
y_hat_test = rf_reg.predict(X_test)
mlf_run.log_predictions(pd.DataFrame(X_test), list(y_hat_test))
```

### Logging dataset stats

```python
import shap

X_test_df = X_test.copy()
X_test_df['targets'] = y_test
X_test_df['predictions'] = y_hat_test

# shap value computation
explainer = shap.TreeExplainer(rf_reg)
shap_values = explainer.shap_values(X_test)

mlf_run.log_dataset_stats(
    X_test_df, 
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=list(boston['feature_names']),
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    model_type=mlf.ModelType.REGRESSION,
    shap_values=shap_values
)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-22 19-27-54.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-22 19-45-02.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-24 02-16-53.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-22 19-46-22.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-22 19-47-22.png>)

### Logging metrics

```python
from sklearn.metrics import r2_score

metrics_dict = {}

metrics_dict['r2_score'] = r2_score(y_test, y_hat_test)

mlf_run.log_metrics(metrics_dict)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-22 19-26-23.png>)
