# webapp

`log_webapp_file` allows you to register your full webapp file with mlfoundry. This is a python script which is a fully functional streamlit file.

| Parameters                                   | Description                                                    |
| -------------------------------------------- | -------------------------------------------------------------- |
| <mark style="color:blue;">**file_path**</mark> | (str) Path to your streamlit compatible python file which must have an implementation of demo function and a call to [webapp](docs/api-doc/webapp/webapp.md) api.|


#### API Example

```python
import mlfoundry as mlf
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name='demo-webapp')
mlf_run.log_webapp_file('classfication/webapp/streamlit_file.py')
```
