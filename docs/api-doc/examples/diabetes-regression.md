# Diabetes Regression

### Importing packages

```python
from sklearn import datasets
import pandas as pd
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split, cross_val_score

import mlfoundry as mlf
```

### Data preprocessing

```python
data = datasets.load_diabetes()

# Read the DataFrame, first using the feature data
df = pd.DataFrame(data.data, columns=data.feature_names)
# Add a target column, and fill it with the target data
df['target'] = data.target

# Create a Pandas dataframe with all the features
X = pd.DataFrame(data = data['data'], columns = data['feature_names'])
y = data['target']

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)
```

### Creating MLF Run

```python
mlf_api = mlf.set_tracking_uri()
mlf_run = mlf_api.create_run(project_name='diabetes-project')
mlf_run_2 = mlf_api.create_run(project_name='diabetes-project')
```

### Training and logging model

```python
tree_reg = RandomForestRegressor(n_estimators=150, max_depth=10)
tree_reg.fit(X_train, y_train)

tree_reg_1 = RandomForestRegressor(n_estimators=100, max_depth=15)
tree_reg_1.fit(X_train, y_train)

mlf_run.log_model(tree_reg, mlf.ModelFramework.SKLEARN)
mlf_run_2.log_model(tree_reg_1, mlf.ModelFramework.SKLEARN)
```

### Logging parameters and predictions

```python
mlf_run.log_params({"n_estimators":150, "max_depth":10})
mlf_run_2.log_params({"n_estimators":100, "max_depth":15})

# logging predictions
y_hat_train = tree_reg.predict(X_train)
y_hat_test = tree_reg.predict(X_test)
mlf_run.log_predictions(pd.DataFrame(X_test), list(y_hat_test))

y_hat_train_tree = tree_reg_1.predict(X_train)
y_hat_test_tree = tree_reg_1.predict(X_test)
mlf_run_2.log_predictions(pd.DataFrame(X_test), list(y_hat_test_tree))
```

### Logging metrics

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error

metrics_dict = {
    "mean absolute error":mean_absolute_error(y_test, y_hat_test),
    "mean squared error": mean_squared_error(y_test, y_hat_test)
}
mlf_run.log_metrics(metrics_dict)

metrics_dict = {
    "mean absolute error":mean_absolute_error(y_test, y_hat_test_tree),
    "mean squared error": mean_squared_error(y_test, y_hat_test_tree)
}
mlf_run_2.log_metrics(metrics_dict)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-34-15.png>)

### Logging dataset\_stats

```python
import shap

X_test_df = X_test.copy()
X_test_df['targets'] = list(y_test)
X_test_df['predictions'] = list(y_hat_test)

# shap value computation
explainer = shap.TreeExplainer(tree_reg)
shap_values = explainer.shap_values(X_test)

mlf_run.log_dataset_stats(
    X_test_df, 
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=list(data.feature_names),
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    shap_values=shap_values,
    model_type=mlf.ModelType.REGRESSION,
)

X_test_df = X_test.copy()
X_test_df['targets'] = list(y_test)
X_test_df['predictions'] = list(y_hat_test_tree)

# shap value computation
explainer = shap.TreeExplainer(tree_reg_1)
shap_values = explainer.shap_values(X_test)

mlf_run_2.log_dataset_stats(
    X_test_df, 
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=list(data.feature_names),
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    shap_values=shap_values,
    model_type=mlf.ModelType.REGRESSION,
)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-35-09.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-35-17.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-35-27.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-35-35.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-36-01.png>)
