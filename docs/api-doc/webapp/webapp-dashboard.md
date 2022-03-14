# MLFoundry Dashboard

There are two ways to spin up the webapp or your model demo.

## Embedded with MLFoundry UI
This is the recommended route where you can spin up the webapp while leveraging the full power of mlfoundry based experiment tracking, use all the logged artifacts like model files, explainability solutions.
You should make sure to have all artifacts used in the predict function logged using mlfoundry APIs.

```
mlfoundry ui --path [MLF_PATH]
```

### Parameters

| Parameters | Description                                                                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| MLF\_PATH  | Optional. Specifies the path of the directory containing the mlf folder generated by mlfoundry. If not given, takes the current directory as input. |


## Independent Streamlit File
This is the option if you want to quickly spin up a Streamlit based model demo where you can still leverage the MLFoundry's APIs for creating model demo but not the other parts like experiment tracking and logging.

```
mlfoundry webapp --webapp_path [PYTHON_FILE_PATH]
```

### Parameters

| Parameters | Description                                                                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| WEBAPP\_PATH  | Specifies the path of python file which is independently executable streamlit like file. All artifacts like model files, datasets used in the file must be logged and loaded using mlfoundry APIs. |