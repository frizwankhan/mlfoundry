# create\_run

Creates a run for the given project(project\_name). Each run will have a unique run\_id. We can associate each run with a run\_name. Many runs can be grouped together under a single project(project\_name)

|                                                    | Description                                                                                                                                                                                 |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**project\_name**</mark> | (str) If the project\_name already exists then the run is grouped under that project. If the project\_name does not exist, a new project is created and the run is assigned to the project. |
| <mark style="color:blue;">**run\_name**</mark>     | (str, optional, default=None) While creating a run we can pass in a run\_name to identify the run easily.                                                                                   |
| <mark style="color:blue;">**tags**</mark>     | (Dict[str, Any], optional) Tags to associate with this run |

#### Returns

<mark style="color:blue;">**MLFoundryRun**</mark> object - an MLFoundryRun object is created with the specified project\_name , uniquely created run\_id and optional run\_name.

#### _Example_

```python
import mlfoundry as mlf

mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name=<project-name>)
```
