## Output Components



### Textbox

Component creates a textbox to render output text or number.

Output type: **Union[str, float, int]**

```python
mlfoundry.webapp.outputs.Textbox(type="auto", label=None)
```

| Parameters                                         | Description                                                                                                                                                                                                                                                                                           |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**type**</mark>          | (str) - Type of value to be passed to component. "str" expects a string, "number" expects a float value, "auto" detects return type.                                                                                                                                                                  |
| <mark style="color:blue;">**label**</mark>         | (str) - component name in interface.                                                                                                                                                                                                                                                                  |
| <mark style="color:blue;">**invert_colors**</mark> | (str) - default value.                                                                                                                                                                                                                                                                                |
| <mark style="color:blue;">**source**</mark>        | (str) - component name in interface.                                                                                                                                                                                                                                                                  |
| <mark style="color:blue;">**tool**</mark>          | (str) - Tools used for editing. "editor" allows a full screen editor, "select" provides a cropping and zoom tool.                                                                                                                                                                                     |
| <mark style="color:blue;">**type**</mark>          | (str) - Type of value to be returned by component. "numpy" returns a numpy array with shape (width, height, 3) and values from 0 to 255, "pil" returns a PIL image object, "file" returns a temporary file object whose path can be retrieved by file_obj.name, "filepath" returns the path directly. |
| <mark style="color:blue;">**label**</mark>         | (str) - component name in interface.                                                                                                                                                                                                                                                                  |
| <mark style="color:blue;">**optional**</mark>      | (bool) - If True, the interface can be submitted with no uploaded image, in which case the input value is None.                                                                                                                                                                                       |

### Label

Component outputs a classification label, along with confidence scores of top categories if provided. Confidence scores are represented as a dictionary mapping labels to scores between 0 and 1.

Output type: **Union[Dict[str, float], str, int, float]**

```python
mlfoundry.webapp.outputs.Label(num_top_classes=None, type="auto", label=None)
```

| Parameters                                           | Description                                                                                                                                                                              |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**num_top_classes**</mark> | (int) - number of most confident classes to show.                                                                                                                                        |
| <mark style="color:blue;">**type**</mark>            | (str) - Type of value to be passed to component. "value" expects a single out label, "confidences" expects a dictionary mapping labels to confidence scores, "auto" detects return type. |
| <mark style="color:blue;">**label**</mark>           | (str) - component name in interface.                                                                                                                                                     |

### Image

Component displays an output image.

Output type: **Union[numpy.array, PIL.Image, str, matplotlib.pyplot, Tuple[Union[numpy.array, PIL.Image, str], List[Tuple[str, float, float, float, float]]]]**

```python
mlfoundry.webapp.outputs.Image(type="auto", label=None)
```

| Parameters                                 | Description                                                                                                                                                                                                                                                                           |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**type**</mark>  | (str) - Type of value to be passed to component. "numpy" expects a numpy array with shape (width, height, 3), "pil" expects a PIL image object, "file" expects a file path to the saved image or a remote URL, "plot" expects a matplotlib.pyplot object, "auto" detects return type. |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface.                                                                                                                                                                                                                                                  |

### Video

Used for video output.

Output type: **filepath**

```python
mlfoundry.webapp.outputs.Video(type=None, label=None)
```

| Parameters                                 | Description                                                                                                                                                               |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**type**</mark>  | (str) - Type of video format to be passed to component, such as 'avi' or 'mp4'. Use 'mp4' to ensure browser playability. If set to None, video will keep returned format. |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface.                                                                                                                                      |

### KeyValues

Component displays a table representing values for multiple fields.

Output type: **Union[Dict, List[Tuple[str, Union[str, int, float]]]]**

```python
mlfoundry.webapp.outputs.KeyValues(label=None)
```

| Parameters                                 | Description                          |
| ------------------------------------------ | ------------------------------------ |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface. |

### HighlightedText

Component creates text that contains spans that are highlighted by category or numerical value. Output is represent as a list of Tuple pairs, where the first element represents the span of text represented by the tuple, and the second element represents the category or value of the text.

Output type: **List[Tuple[str, Union[float, str]]]**

```python
mlfoundry.webapp.outputs.HighlightedText(color_map=None, label=None, show_legend=False)
```

| Parameters                                       | Description                                                              |
| ------------------------------------------------ | ------------------------------------------------------------------------ |
| <mark style="color:blue;">**color_map**</mark>   | (Dict[str, str]) - Map between category and respective colors            |
| <mark style="color:blue;">**label**</mark>       | (str) - component name in interface.                                     |
| <mark style="color:blue;">**show_legend**</mark> | (bool) - whether to show span categories in a separate legend or inline. |

### Audio

Creates an audio player that plays the output audio.

Output type: **Union[Tuple[int, numpy.array], str]**

```python
mlfoundry.webapp.outputs.Audio(type="auto", label=None)
```

| Parameters                                 | Description                                                                                                                                                                                                                                              |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**type**</mark>  | (str) - Type of value to be passed to component. "numpy" returns a 2-set tuple with an integer sample_rate and the data numpy.array of shape (samples, 2), "file" returns a temporary file path to the saved wav audio file, "auto" detects return type. |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface.                                                                                                                                                                                                                     |

### JSON

Used for JSON output. Expects a JSON string or a Python object that is JSON serializable.

Output type: **Union[str, Any]**

```python
mlfoundry.webapp.outputs.JSON(label=None)
```

| Parameters                                 | Description                          |
| ------------------------------------------ | ------------------------------------ |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface. |

### File

Used for file output.

Output type: **Union[file-like, str]**

```python
mlfoundry.webapp.outputs.File(label=None)
```

| Parameters                                 | Description                          |
| ------------------------------------------ | ------------------------------------ |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface. |

### Dataframe

Component displays 2D output through a spreadsheet interface.

Output type: **Union[pandas.DataFrame, numpy.array, List[Union[str, float]], List[List[Union[str, float]]]]**

```python
mlfoundry.webapp.outputs.Dataframe(headers=None, max_rows=20, max_cols=None, overflow_row_behaviour="paginate", type="auto", label=None)
```

| Parameters                                                  | Description                                                                                                                                                       |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**headers**</mark>                | (List[str]) - Header names to dataframe. Only applicable if type is "numpy" or "array".                                                                           |
| <mark style="color:blue;">**max_rows**</mark>               | (int) - Maximum number of rows to display at once. Set to None for infinite.                                                                                      |
| <mark style="color:blue;">**max_cols**</mark>               | (int) - Maximum number of columns to display at once. Set to None for infinite.                                                                                   |
| <mark style="color:blue;">**overflow_row_behaviour**</mark> | (str) - If set to "paginate", will create pages for overflow rows. If set to "show_ends", will show initial and final rows and truncate middle rows.              |
| <mark style="color:blue;">**type**</mark>                   | (str) - Type of value to be passed to component. "pandas" for pandas dataframe, "numpy" for numpy array, or "array" for Python array, "auto" detects return type. |
| <mark style="color:blue;">**label**</mark>                  | (str) - component name in interface.                                                                                                                              |

### Carousel

Component displays a set of output components that can be scrolled through.

Output type: **List[List[Any]]**

```python
mlfoundry.webapp.outputs.Carousel(components, label=None)
```

| Parameters                                      | Description                                                                                              |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**components**</mark> | (Union[List[OutputComponent], OutputComponent]) - Classes of component(s) that will be scrolled through. |
| <mark style="color:blue;">**label**</mark>      | (str) - component name in interface.                                                                     |

### Timeseries

Component accepts pandas.DataFrame.

Output type: **pandas.DataFrame**

```python
mlfoundry.webapp.outputs.Timeseries(x=None, y=None, label=None)
```

| Parameters                                 | Description                                                                                                                                                                      |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**x**</mark>     | (str) - Column name of x (time) series. None if csv has no headers, in which case first column is x series.                                                                      |
| <mark style="color:blue;">**y**</mark>     | (Union[str, List[str]]) - Column name of y series, or list of column names if multiple series. None if csv has no headers, in which case every column after first is a y series. |
| <mark style="color:blue;">**label**</mark> | (str) - component name in interface.                                                                                                                                             |
