# Remote Tracking Server

Apart from using a local file path, we can also use a remote-tracking server to log experiments along with artifacts.
The tracking server stores the [metrics](api-doc/mlfoundry/mlfoundryrun/get\_metrics.md), [parameters](api-doc/mlfoundry/mlfoundryrun/log\_params.md) in Postgres or any other SQLAlchemy compatible database. [Datasets](api-doc/mlfoundry/mlfoundryrun/log\_dataset.md), [models](api-doc/mlfoundry/mlfoundryrun/log\_model.md), [dataset statistics](api-doc/mlfoundry/mlfoundryrun/log\_dataset\_stats.md), and other artifacts are stored in an S3 bucket.

To upload the artifacts to the configures S3 bucket, mlfoundry does not require AWS credentials and relies on the tracking server to provide Singed URI.

```
     ┌────────────────────┐ 1. Asks for signed URI┌────────────────────┐
     │                    ├───────────────────────►                    │
     │                    │                       │                    │
     │     mlfoundry      │                       │   Tracking Server  │
     │                    ◄───────────────────────┤                    │
     │                    │ 2. Returns Signed URI │                    │
     └─────────┬──────────┘                       └────────────────────┘
               │3. Direct upload
               │
     ┌─────────▼──────────┐
     │                    │
     │                    │
     │      S3 Bucket     │
     │                    │
     │                    │
     └────────────────────┘
```

## Starting the tracking server

First, we can use Docker Compose to locally bring up the tracking server with Postgres database using the below `docker-compose.yml`.

```yaml
version: '3.9'
services:
  db:
    image: postgres:alpine
    container_name: tracking-server-db
    environment:
      # This environment variables can be defined in a .env file
      # https://docs.docker.com/compose/environment-variables/#the-env-file
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
    restart: on-failure
  adminer:
    image: adminer
    restart: on-failure
    ports:
      - 8080:8080
    depends_on:
      - db
    restart: on-failure
  tracking-server:
    image: public.ecr.aws/z8k2w0e1/mlflow-server:latest
    container_name: tracking-server
    ports:
      - 5000:5000
    # We can pass the AWS credentials without mounting the
    # ${HOME}/.aws/credentials too
    # environment:
      # - AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
      # - AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}

    # Define DEFAULT_ARTIFACT_ROOT in a .env file
    # Example: s3://my-bucket/path/to/artifact-root/
    command: >
      "mlflow server \
        --host=0.0.0.0 \
        --default-artifact-root=${DEFAULT_ARTIFACT_ROOT} \
        --backend-store-uri=postgresql+psycopg2://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${POSTGRES_DB}"
    depends_on:
      - db
    restart: on-failure
    volumes:
      - ${HOME}/.aws/credentials:/root/.aws/credentials:ro
```

Example `.env` file,
```
POSTGRES_PASSWORD=test
POSTGRES_USER=test
POSTGRES_DB=test
DEFAULT_ARTIFACT_ROOT=s3://my-bucket/path/to/artifact-root/
```

To start the tracking server, execute `docker-compose up`,
```bash
docker-compose -f <docker-compose-file-name> up --build
```

We can access the tracking server UI at [localhost:5000](localhost:5000).

## Using tracking server along with mlfoundry

After the tracking server is up and running, we can start using mlfoundry.

#### Create a Run
```python
import mlfoundry as mlf

mlf_api = mlf.get_client(tracking_uri="http://localhost:5000")
mlf_run = mlf_api.create_run(project_name="boston")
```

#### Prepare dataset
```python
import pandas as pd
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split

boston = load_boston()
dataset = pd.DataFrame(data=boston["data"], columns=boston["feature_names"])
dataset["target"] = boston["target"]
dataset_train, dataset_test = train_test_split(dataset, random_state=42)
```

#### Log dataset with mlfoundry
```python
mlf_run.log_dataset(dataset_train, data_slice=mlf.DataSlice.TRAIN)
mlf_run.log_dataset(dataset_test, data_slice=mlf.DataSlice.TEST)
```

mlfoundry will save the datasets in the S3 bucket (`DEFAULT_ARTIFACT_ROOT`) configured while starting the tracking server. We can find all the artifacts saved in a run below.
`s3://my-bucket/path/to/artifact-root/<experiment-id>/<run-id>/artifacts/`

We can load the dataset back using the snippet below,
```python
dataset_train = mlf_run.get_dataset(mlf.DataSlice.TRAIN)
```

#### Train and log model
```python
from sklearn.ensemble import RandomForestRegressor

rf_reg = RandomForestRegressor(n_estimators=100)
rf_reg.fit(dataset_train[boston["feature_names"]], dataset_train["target"])

mlf_run.log_params(rf_reg.get_params())
mlf_run.log_model(rf_reg, mlf.ModelFramework.SKLEARN)
```
