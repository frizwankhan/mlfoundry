# webapp

The interface of our webapp is inspired by [gradio](https://gradio.app/docs/).

`webapp` allows you to define a model demo interface on the ui.

| Parameters                                   | Description                                                    |
| -------------------------------------------- | -------------------------------------------------------------- |
| <mark style="color:blue;">**fn**</mark> | (Union[Callable, List[Callable]]) function to be executed in the demo. |
| <mark style="color:blue;">**inputs**</mark> | (Union[str, InputComponent, List[Union[str, InputComponent]]]) a single input component, or list of input components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of input components should match the number of parameters in fn. |
| <mark style="color:blue;">**outputs**</mark> | (Union[str, OutputComponent, List[Union[str, OutputComponent]]]) a single output component, or list of output components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of output components should match the number of values returned by fn. |

#### Returns

| Return Object                                   | Description                                                    |
| -------------------------------------------- | -------------------------------------------------------------- |
| <mark style="color:blue;">**raw_in**</mark> | (Union[Any, List[Any]]) A single value or a list of values as input by the user. The number of elements in raw\_in will match the number of parameters in fn.|
| <mark style="color:blue;">**raw_out**</mark> | (Union[Any, List[Any]]) A single value or a list of values as returned by the function. The number of elements in raw\_out will match the number of values returned by fn.|


#### Example

```python
import mlfoundry as mlf
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name='demo')

def greet(name):
    return "Hello " + name + "!!"

raw_in, raw_out = mlf_run.webapp(fn=greet, inputs="text", outputs="text")
```
