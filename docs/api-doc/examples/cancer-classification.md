# Cancer Classification

Link to the notebook: [**github**](https://github.com/truefoundry/mlfoundry-examples/blob/main/examples/sklearn/cancer_train.ipynb)****

### Importing packages

```python
from sklearn import datasets
import pandas as pd
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier

import mlfoundry as mlf
```

### Data preprocessing

```python
data = datasets.load_breast_cancer()
# Read the DataFrame, first using the feature data
df = pd.DataFrame(data.data, columns=data.feature_names)
# Add a target column, and fill it with the target data
df['target'] = data.target

# Store the feature data
X = data.data
X = pd.DataFrame(X, columns=data.feature_names)
# store the target data
y = data.target
# split the data using Scikit-Learn's train_test_split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y)
```

### Creating MLF Run

```python
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name='cancer-project')
mlf_run_2 = mlf_api.create_run(project_name='cancer-project')
```

### Training and logging model

```python
tree_clf = RandomForestClassifier(n_estimators=100, max_depth=15)
tree_clf.fit(X_train, y_train)

tree_clf_1 = RandomForestClassifier(n_estimators=150, max_depth=10)
tree_clf_1.fit(X_train, y_train)
## Logging model
mlf_run.log_model(tree_clf, mlf.ModelFramework.SKLEARN)
mlf_run_2.log_model(tree_clf_1, mlf.ModelFramework.SKLEARN)
```

### Logging parameters and predictions

```python
mlf_run.log_params({"n_estimators":100, "max_depth":15})
mlf_run_2.log_params({"n_estimators":150, "max_depth":10})

# logging predictions
y_hat_train = tree_clf.predict(X_train)
y_hat_test = tree_clf.predict(X_test)
mlf_run.log_predictions(pd.DataFrame(X_test), list(y_hat_test))

y_hat_train_tree = tree_clf_1.predict(X_train)
y_hat_test_tree = tree_clf_1.predict(X_test)
mlf_run_2.log_predictions(pd.DataFrame(X_test), list(y_hat_test_tree))
```

### Logging metrics

```python
from sklearn.metrics import accuracy_score, f1_score

f1 = f1_score(y_test, y_hat_test)
accuracy = accuracy_score(y_test, y_hat_test)

mlf_run.log_metrics({"f1":f1, "accuracy_score":accuracy})

f1 = f1_score(y_test, y_hat_test_tree)
accuracy = accuracy_score(y_test, y_hat_test_tree)
mlf_run_2.log_metrics({"f1":f1, "accuracy_score":accuracy})
```

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-27-02.png>)

### Logging dataset\_stats

```python
import shap

X_test_df = X_test.copy()
X_test_df['targets'] = list(y_test)
X_test_df['predictions'] = list(y_hat_test)
X_test_df['prediction_probabilities'] = list(tree_clf.predict_proba(X_test))

# shap value computation model 1 test set
explainer = shap.TreeExplainer(tree_clf_1)
shap_values = explainer.shap_values(X_test)

mlf_run.log_dataset_stats(
    X_test_df,
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=list(data.feature_names),
        prediction_column_name="predictions",
        actual_column_name="targets",
        prediction_probability_column_name="prediction_probabilities"
    ),
    shap_values=shap_values,
    model_type=mlf.ModelType.BINARY_CLASSIFICATION,
)

X_test_df = X_test.copy()
X_test_df['targets'] = list(y_test)
X_test_df['predictions'] = list(y_hat_test_tree)
X_test_df['prediction_probabilities'] = list(tree_clf_1.predict_proba(X_test))

# shap value computation Model 2 test set
explainer = shap.TreeExplainer(tree_clf)
shap_values = explainer.shap_values(X_test)


mlf_run_2.log_dataset_stats(
    X_test_df,
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=list(data.feature_names),
        prediction_column_name="predictions",
        actual_column_name="targets",
        prediction_probability_column_name="prediction_probabilities"
    ),
    shap_values=shap_values,
    model_type=mlf.ModelType.BINARY_CLASSIFICATION,
)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-27-39.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-27-46.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-27-52.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-28-01.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-28-09.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-28-14.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-30 00-28-23.png>)
