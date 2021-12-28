# Concepts

<mark style="color:blue;">**Project:**</mark> The space where you put all the MLFoundry runs and metadata generated while working on a single ML model.

<mark style="color:blue;">**Run:**</mark> You can create one Run per unique combination of your metrics, parameters, dataset and these run can be grouped together under an project which represents a logical task. For example, you might be building running an project to train a cat\_classifier model where you want to try two different combinations of kernel\_size- each of which can have a corresponding run.

<mark style="color:blue;">**project\_name:**</mark> A name that is given by the user to create an project.

<mark style="color:blue;">**run\_id**</mark>**:** A unique string that represents a Run and is custom created by MLFoundry.

<mark style="color:blue;">**run\_name:**</mark> we can assign each run a name.

<mark style="color:blue;">**Schema:**</mark> Schema is way of letting MLFoundry know about the dataset, so that it can compute metrics efficiently. Schema contains these list of variable:

1. feature\_column\_names (List(str)) - names of the features
2. numerical\_feature\_column\_names (List(str)) - names of the numerical feature column features
3. categorical\_feature\_column\_names (List(str)) - names of the categorical feature column features
4. timestamp\_column\_name (str) - name of timestamp column if any.
5. prediction\_label\_column\_name (str) - name of column that contains model prediction
6. actual\_label\_column\_name (str) - name of column that contains actual labels.

<mark style="color:blue;">**Metrics computed for different model types:**</mark>

1. **BINARYCLASSIFICATION**: accuracy, f1\_score, cohen\_kappa\_score, log\_loss, confusion matrix, roc\_curve, pr\_curve.
2. **MULTICLASS\_CLASSIFICATION**: accuracy, cohen\_kappa\_score, log\_loss, confusion matrix, roc\_curve, pr\_curve.
3. **REGRESSION**: mean\_squared\_error, mean\_absolute\_error, r2\_score, root\_mean\_squared\_error, explained\_variance\_score
