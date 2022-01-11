# get\_dataset

get\_dataset returns the already logged dataset as a pandas dataframe.

| Parameters  | Description                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| data\_slice | <p>(mlf.DataSlice) data slice type of ‘df’ argument.</p><p>List of available data_slice: <a data-mention href="../enums.md#dataslice">#dataslice</a></p> |

#### Returns

<mark style="color:blue;">**Pandas Dataframe**</mark>: Returns the logged data as a pandas dataframe for a particular data\_slice. If dataset is not logged, MLFoundryException is thrown out.
