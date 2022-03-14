# Iris Webapp

### Importing packages

```python
import pickle
import random
import pandas as pd
import mlfoundry as mlf
from sklearn.svm import SVC
from sklearn import datasets
from sklearn.model_selection import train_test_split

```

### Train Model

The function can be in a different file but the model should be logged via mlfoundry so it can be easily picked up by the inference function.

```python
def train_and_log_iris_model():

    mlf_api = mlf.get_client()
    mlf_run = mlf_api.create_run(
        project_name="iris_webapp_testing", run_name="mlf_logged_model"
    )

    iris = datasets.load_iris()
    X, y = iris.data, iris.target

    X_train, X_test, Y_train, Y_test = train_test_split(
        X, y, test_size=0.2, random_state=0
    )
    mlf_run.log_dataset(
        pd.DataFrame(X_train),
        data_slice=mlf.DataSlice.TRAIN,
        fileformat=mlf.FileFormat.CSV,
    )  # saves in parquet format
    svm = SVC(kernel="rbf", probability=True)
    svm.fit(X_train, Y_train)
    mlf_run.log_model(svm, mlf.ModelFramework.SKLEARN)
    return mlf_run.run_id

```

### Creating Inference Function for Demo

```python
def predict_fn(i1: float, i2: float, i3: float, i4: float) -> str:
    try:
        class_name_map = dict(
            zip([0, 1, 2], ["Iris Setosa", "Iris Versicolour", "Iris Virginica"])
        )
        features = pd.DataFrame([[i1, i2, i3, i4]])
        with open('model.pickle', 'wb') as f:
            model = pickle.load(f)
        model_output = model.predict(features)[0]
        return class_name_map[model_output]
    except Exception:
        return random.choice(["Iris Setosa", "Iris Versicolour", "Iris Virginica"])
```

### Creating Webapp

```python
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name="iris_with_mlf_webapp_api")
raw_in, raw_out = mlf_run.webapp(
    fn=predict_fn, inputs=["number", "number", "number", "number"], outputs="text"
)
```
