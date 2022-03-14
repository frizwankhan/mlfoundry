## Input Components

### Textbox

Component creates a textbox for user to enter input. Provides a string as an argument to the wrapped function.

Input type: **str**

```python
mlfoundry.webapp.inputs.Textbox(lines=1, placeholder=None, default="", label=None)
```

| Parameters      | Description                                          |
| --------------- | ---------------------------------------------------- |
| **lines**       | (int) - number of line rows to provide in textarea.  |
| **placeholder** | (str) - placeholder hint to provide behind textarea. |
| **default**     | (str) - default text to provide in textarea.         |
| **label**       | (str) - component name in interface.                 |

### Number

Component creates a field for user to enter numeric input. Provides a number as an argument to the wrapped function.

Input type: **float**

```python
mlfoundry.webapp.inputs.Number(default=None, label=None)
```

| Parameters  | Description                          |
| ----------- | ------------------------------------ |
| **default** | (float) - default value.             |
| **label**   | (str) - component name in interface. |

### Slider

Component creates a slider that ranges from `minimum` to `maximum`. Provides a number as an argument to the wrapped function.

Input type: **float**

```python
mlfoundry.webapp.inputs.Slider(minimum=0, maximum=100, step=None, default=None, label=None)
```

| Parameters  | Description                                |
| ----------- | ------------------------------------------ |
| **minimum** | (float) - minimum value for slider.        |
| **maximum** | (float) - maximum value for slider.        |
| **step**    | (float) - increment between slider values. |
| **default** | (float) - default value.                   |
| **label**   | (str) - component name in interface.       |

### Checkbox

Component creates a checkbox that can be set to `True` or `False`. Provides a boolean as an argument to the wrapped function.

Input type: **bool**

```python
mlfoundry.webapp.inputs.Checkbox(default=False, label=None)
```

| Parameters  | Description                          |
| ----------- | ------------------------------------ |
| **label**   | (str) - component name in interface. |
| **default** | (bool) - default value.              |

### CheckboxGroup

Component creates a set of checkboxes of which a subset can be selected. Provides a list of strings representing the selected choices as an argument to the wrapped function.

Input type: **Union[List[str], List[int]]**

```python
mlfoundry.webapp.inputs.CheckboxGroup(choices, default=[], type="value", label=None)
```

| Parameters  | Description                                                                                                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **choices** | (List[str]) - list of options to select from.                                                                                                                                 |
| **default** | (List[str]) - default selected list of options.                                                                                                                               |
| **type**    | (str) - Type of value to be returned by component. "value" returns the list of strings of the choices selected, "index" returns the list of indicies of the choices selected. |
| **label**   | (str) - component name in interface.                                                                                                                                          |

### Radio

Component creates a set of radio buttons of which only one can be selected. Provides string representing selected choice as an argument to the wrapped function.

Input type: **Union[str, int]**

```python
mlfoundry.webapp.inputs.Radio(choices, type="value", default=None, label=None)
```

| Parameters  | Description                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **choices** | (List[str]) - list of options to select from.                                                                                                           |
| **type**    | (str) - Type of value to be returned by component. "value" returns the string of the choice selected, "index" returns the index of the choice selected. |
| **default** | (str) - default value.                                                                                                                                  |
| **label**   | (str) - component name in interface.                                                                                                                    |

### Dropdown

Component creates a dropdown of which only one can be selected. Provides string representing selected choice as an argument to the wrapped function.

Input type: **Union[str, int]**

```python
mlfoundry.webapp.inputs.Dropdown(choices, type="value", default=None, label=None)
```

| Parameters  | Description                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **choices** | (List[str]) - list of options to select from.                                                                                                           |
| **type**    | (str) - Type of value to be returned by component. "value" returns the string of the choice selected, "index" returns the index of the choice selected. |
| **default** | (str) - default value.                                                                                                                                  |
| **label**   | (str) - component name in interface.                                                                                                                    |

### Image

Component creates an image upload box with editing capabilities.

Input type: **Union[numpy.array, PIL.Image, file-object]**

```python
mlfoundry.webapp.inputs.Image(shape=None, image_mode="RGB", invert_colors=False, source="upload", tool="editor", type="numpy", label=None, optional=False)
```

| Parameters        | Description                                                                                                                                                                                                                                                                                           |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **shape**         | (List[str]) - list of options to select from.                                                                                                                                                                                                                                                         |
| **image_mode**    | (str) - Type of value to be returned by component. "value" returns the string of the choice selected, "index" returns the index of the choice selected.                                                                                                                                               |
| **invert_colors** | (str) - default value.                                                                                                                                                                                                                                                                                |
| **source**        | (str) - component name in interface.                                                                                                                                                                                                                                                                  |
| **tool**          | (str) - Tools used for editing. "editor" allows a full screen editor, "select" provides a cropping and zoom tool.                                                                                                                                                                                     |
| **type**          | (str) - Type of value to be returned by component. "numpy" returns a numpy array with shape (width, height, 3) and values from 0 to 255, "pil" returns a PIL image object, "file" returns a temporary file object whose path can be retrieved by file_obj.name, "filepath" returns the path directly. |
| **label**         | (str) - component name in interface.                                                                                                                                                                                                                                                                  |
| **optional**      | (bool) - If True, the interface can be submitted with no uploaded image, in which case the input value is None.                                                                                                                                                                                       |

### Video

Component creates a video file upload that is converted to a file path.

Input type: **filepath**

```python
mlfoundry.webapp.inputs.Video(type=None, source="upload", label=None, optional=False)
```

| Parameters   | Description                                                                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **type**     | (str) - Type of video format to be returned by component, such as 'avi' or 'mp4'. If set to None, video will keep uploaded format.           |
| **source**   | (str) - Source of video. "upload" creates a box where user can drop an video file, "webcam" allows user to record a video from their webcam. |
| **label**    | (str) - component name in interface.                                                                                                         |
| **optional** | (bool) - If True, the interface can be submitted with no uploaded video, in which case the input value is None.                              |

### Audio

Component accepts audio input files.

Input type: **Union[Tuple[int, numpy.array], file-object, numpy.array]**

```python
mlfoundry.webapp.inputs.Audio(source="upload", type="numpy", label=None, optional=False)
```

| Parameters   | Description                                                                                                                                                                                                                                                                             |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **source**   | (str) - Source of audio. "upload" creates a box where user can drop an audio file, "microphone" creates a microphone input.                                                                                                                                                             |
| **type**     | (str) - Type of value to be returned by component. "numpy" returns a 2-set tuple with an integer sample_rate and the data numpy.array of shape (samples, 2), "file" returns a temporary file object whose path can be retrieved by file_obj.name, "filepath" returns the path directly. |
| **label**    | (str) - component name in interface.                                                                                                                                                                                                                                                    |
| **optional** | (bool) - If True, the interface can be submitted with no uploaded audio, in which case the input value is None.                                                                                                                                                                         |

### File

Component accepts generic file uploads.

Input type: **Union[file-object, bytes, List[Union[file-object, bytes]]]**

| Parameters     | Description                                                                                                                                                                                                                               |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **file_count** | (str) - if single, allows user to upload one file. If "multiple", user uploads multiple files. If "directory", user uploads all files in selected directory. Return type will be list for each file in case of "multiple" or "directory". |
| **type**       | (str) - Type of value to be returned by component. "file" returns a temporary file object whose path can be retrieved by file_obj.name, "binary" returns an bytes object.                                                                 |
| **label**      | (str) - component name in interface.                                                                                                                                                                                                      |
| **optional**   | (bool) - If True, the interface can be submitted with no uploaded image, in which case the input value is None.                                                                                                                           |

### Dataframe

Component accepts 2D input through a spreadsheet interface.

Input type: **Union[pandas.DataFrame, numpy.array, List[Union[str, float]], List[List[Union[str, float]]]]**

```python
mlfoundry.webapp.inputs.Dataframe(headers=None, row_count=3, col_count=3, datatype="str", col_width=None, default=None, type="pandas", label=None)
```

| Parameters    | Description                                                                                                                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **headers**   | ()List[str]) - Header names to dataframe.                                                                                                                                                                    |
| **row_count** | (int) - Limit number of rows for input.                                                                                                                                                                      |
| **col_count** | (int) - Limit number of columns for input. If equal to 1, return data will be one-dimensional. Ignored if `headers` is provided.                                                                             |
| **datatype**  | (Union[str, List[str]]) - Datatype of values in sheet. Can be provided per column as a list of strings, or for the entire sheet as a single string. Valid datatypes are "str", "number", "bool", and "date". |
| **col_width** | (Union[int, List[int]]) - Width of columns in pixels. Can be provided as single value or list of values per column.                                                                                          |
| **default**   | (List[List[Any]]) - Default value                                                                                                                                                                            |
| **type**      | (str) - Type of value to be returned by component. "pandas" for pandas dataframe, "numpy" for numpy array, or "array" for a Python array.                                                                    |
| **label**     | (str) - component name in interface.                                                                                                                                                                         |

### Timeseries

Component accepts pandas.DataFrame uploaded as a timeseries csv file.

Input type: pandas.DataFrame

```python
mlfoundry.webapp.inputs.Timeseries(x=None, y=None, label=None, optional=False)
```

| Parameters   | Description                                                                                                                                                                      |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **x**        | (str) - Column name of x (time) series. None if csv has no headers, in which case first column is x series.                                                                      |
| **y**        | (Union[str, List[str]]) - Column name of y series, or list of column names if multiple series. None if csv has no headers, in which case every column after first is a y series. |
| **label**    | (str) - component name in interface.                                                                                                                                             |
| **optional** | (bool) - If True, the interface can be submitted with no uploaded csv file, in which case the input value is None.                                                               |
