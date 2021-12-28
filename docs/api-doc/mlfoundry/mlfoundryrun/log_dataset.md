# log\_dataset

Logs a dataset, The dataset can be ‘train’, ‘test’, ‘validate’, ‘prediction’. We can only log one dataset for a slice per run. If attempt to log dataset more than once an error is thrown out.

| Parameters                                       | Description                                                                                                                                                                                                   |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**df**</mark>          | (pandas.DataFrame) Dataset to upload                                                                                                                                                                          |
| <mark style="color:blue;">**data\_slice**</mark> | <p>(mlf.DataSlice) data slice type of ‘df’ argument.</p><p>List of available data_slice: <a href="broken-reference"><mark style="color:purple;">Link</mark></a></p>                                           |
| <mark style="color:blue;">**fileformat**</mark>  | <p>(mlf.FileFormat, optional, default=PARQUET) format of the file for the dataset to be saved.</p><p>List of available fileformat: <a href="broken-reference"><mark style="color:purple;">Link</mark></a></p> |

#### Example

```python
# saves in parquet format
mlf_run.log_dataset(iris_frame, data_slice=mlf.DataSlice.TRAIN)

# saves in csv format
mlf_run.log_dataset(iris_frame,
                   data_slice=mlf.DataSlice.TRAIN,
                   fileformat=mlf.FileFormat.CSV)
```
