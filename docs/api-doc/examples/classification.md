# Classification

### Importing the libraries

```python
import pandas as pd
import numpy as np
from sklearn import datasets
from sklearn import svm
from sklearn.model_selection import train_test_split

# importing mlfoundry
import mlfoundry as mlf
```

### Loading the dataset

In this example we will use Iris dataset to show the use case of mlfoundry-ui.&#x20;

```python
iris = datasets.load_iris()
iris_frame = pd.DataFrame(iris.data, columns = iris.feature_names)
```



### Creating MLFoundry Run

```python
mlf_api = mlf.set_tracking_uri() # to save locally
mlf_run = mlf_api.create_run(project_name='iris-project')
```

**Few things to note here:**

1. When we pass no argument to mlf.set\_tracking\_uri then all the run artifacts and metrics will be stored locally in mlf directory that is created in the same directory as the notebook.
2. <mark style="color:blue;">mlf\_api.create\_run(project\_name="iris-project")</mark> will create a run under the project named iris-project. If the cell is executed again a new run will be created under the project named iris-project.

### Logging the dataset

```python
mlf_run.log_dataset(iris_frame, data_slice=mlf.DataSlice.TRAIN)  # saves in parquet format
```

Few things to note here:

1. Please check here for the available list of [#dataslice](../mlfoundry/enums.md#dataslice "mention")
2. fileformat can be either CSV or PARQUET: [#fileformat](../mlfoundry/enums.md#fileformat "mention")

### Training the model

```python
X, y = iris.data, iris.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Model Training
clf = svm.SVC(gamma='scale', kernel='rbf', probability=True)
clf.fit(X, y)
```

### Logging predictions

```python
y_hat_train = clf.predict(X_train)
y_hat_test = clf.predict(X_test)
mlf_run.log_predictions(pd.DataFrame(X_test), list(y_hat_test))
```

**Few thing to note here:**

* To log predictions asynchronously we can use log\_predictions\_async instead of log\_predictions. To know more about log\_predictions\_async refer to __ [Broken link](broken-reference "mention")__

### Logging dataset stats

```python
import shap

y_train_prob = clf.predict_proba(X_train)

X_train_df = pd.DataFrame(X_train, columns=iris.feature_names)
## By logging targets and prediction users will have access to auto generated metrics,
## prediction and truth label distributions and confusion matrices
X_train_df['targets'] = y_train
X_train_df['predictions'] = y_hat_train

## By logging prediction probabilities users will have access to ROC, PR curve
X_train_df['prediction_probabilities'] = list(y_train_prob)

X_test_df = pd.DataFrame(X_test, columns=iris.feature_names)
## By logging targets and prediction users will have access to auto generated metrics,
## prediction and truth label distributions and confusion matrices
X_test_df['targets'] = y_test
X_test_df['predictions'] = y_hat_test

# shap value computation
# By logging shap values users will have access to feature importance in the mlfoundry-ui
X_train_df1 = pd.DataFrame(X_train, columns=iris.feature_names)
X_test_df1 = pd.DataFrame(X_test, columns=iris.feature_names)
explainer = shap.KernelExplainer(clf.predict_proba, X_train_df1)
shap_values = explainer.shap_values(X_test_df1)

mlf_run.log_dataset_stats(
    X_test_df, 
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=iris.feature_names,
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    model_type=mlf.ModelType.MULTICLASS_CLASSIFICATION,
    shap_values=shap_values
)

```

#### Opening mlfoundry-ui:

To open mlfoundry dashboard run in command line:&#x20;

```
mlfoundry ui --path [MLF_PATH]
```

Where \[MLF\_PATH] is the path to mlf directory. \
To know more about mlfoundry-ui, please check [mlfoundry-dashboard.md](../../mlfoundry-dashboard.md "mention")



#### MLFoundry Dashboard:

* For classification problem, if prediction probabilities for each class is passed, then mlfoundry-ui shows PR and ROC curves.

![](<../../.gitbook/assets/Screenshot from 2021-12-22 17-30-31.png>)

* For classification problem confusion matrix is generated for different data slice.

![](<../../.gitbook/assets/Screenshot from 2021-12-24 02-03-49.png>)

* We also auto generate lots of metrics using mlfoundry and can be viewed under **auto genertated metrics** in mlfoundry-ui. To check the list of auto generated metrics refer to&#x20;
* Using mlfoundry-ui, users can also view the summary of the data under data health tab in mlfoundry-ui. The summary of the data includes different statistical measures like mean, median, mode, interquartile range and more for each feature in dataset. They also have access to unique numbers and maximum and minimum in a feature.

![](<../../.gitbook/assets/Screenshot from 2021-12-22 17-41-20.png>)

* If the users have logged shap values of the features in log\_dataset\_stats, we also show the feature importance of each feature.

![](<../../.gitbook/assets/Screenshot from 2021-12-24 02-12-54.png>)

* Using mlfoundry-ui users can also compare prediction and truth distribution for different data slices. Here in the plot columns represent the truth and predictions while rows represent different data slices.

![](<../../.gitbook/assets/Screenshot from 2021-12-24 01-49-31.png>)

* Using mlfoundry-ui users can also view histograms of different numerical and categorical features.

![](<../../.gitbook/assets/Screenshot from 2021-12-22 17-45-31.png>)

### Logging parameters

```python
params = {'classes': clf.classes_, 'features': clf.n_features_in_}
mlf_run.log_params(params)
```

**Few things to note here:**

1. We can't log params multiple times with the same param name. Example: "classes" cant be logged twice.
2. If the user wants to change a parameter or log a parameter multiple times, they can do so by creating a new run.

### Logging the model

```python
mlf_run.log_model(clf, mlf.ModelFramework.SKLEARN)
```

To check the available list of frameworks please check [Broken link](broken-reference "mention")

### Logging metrics

```python
from sklearn.metrics import accuracy_score, f1_score

metrics_dict = {}

metrics_dict['accuracy_score'] = accuracy_score(y_test, y_hat_test)
metrics_dict['f1_score'] = f1_score(y_test, y_hat_test, average='micro')

mlf_run.log_metrics(metrics_dict)
```

**Few things to note here:**

1. After logging metrics we can use mlfoundry ui to visualise the metrics. All the metrics that logged under log\_metrics can be found under **user generated metrics** in **mlfoundry-ui.**

![](<../../.gitbook/assets/Screenshot from 2021-12-22 17-16-21.png>)
