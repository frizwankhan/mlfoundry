# Text Classification

Link to the notebook: [**github**](https://github.com/truefoundry/mlfoundry-examples/blob/main/examples/tensorflow/sentiment_analysis.ipynb)****

### Importing the libraries

```python
import os
import re
import shutil
import string
import numpy as np
import mlfoundry as mlf

import pandas as pd
import tensorflow as tf
import tensorflow_datasets as tfds
from tensorflow.keras import layers
from tensorflow.keras import losses
from tensorflow.keras.layers.experimental.preprocessing import TextVectorization
from tensorflow.keras.callbacks import Callback
```

### Preparing the dataset

```python
# To remove <br/> from input text
def custom_standardization(input_data):
    lowercase = tf.strings.lower(input_data)
    stripped_html = tf.strings.regex_replace(lowercase, '<br />', ' ')
    return tf.strings.regex_replace(stripped_html, '[%s]' % re.escape(string.punctuation), '')


batch_size = 32
seed = 42
max_features = 10000
sequence_length = 250
embedding_dim = 16
epochs = 10
AUTOTUNE = tf.data.AUTOTUNE

vectorize_layer = TextVectorization(
    standardize=custom_standardization,
    max_tokens=max_features,
    output_mode='int',
    output_sequence_length=sequence_length)


def download_data():
    url = "https://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz"
    dataset = tf.keras.utils.get_file("aclImdb_v1", url, untar=True, cache_dir='.', cache_subdir='')
    dataset_dir = os.path.join(os.path.dirname(dataset), 'aclImdb')
    train_dir = os.path.join(dataset_dir, 'train')
    remove_dir = os.path.join(train_dir, 'unsup')
    shutil.rmtree(remove_dir)


def vectorize_text(text, label):
    text = tf.expand_dims(text, -1)
    return vectorize_layer(text), label

class MetricsLogCallback(Callback):
    def on_epoch_end(self, epoch, logs=None):
        mlf_run.log_metrics(logs)   # logging metrics using mlfoundry run


def get_raw_dataset():
    raw_train_ds = tf.keras.preprocessing.text_dataset_from_directory(
        'aclImdb/train',
        batch_size=batch_size,
        validation_split=0.2,
        subset='training',
        seed=seed)

    raw_val_ds = tf.keras.preprocessing.text_dataset_from_directory(
        'aclImdb/train',
        batch_size=batch_size,
        validation_split=0.2,
        subset='validation',
        seed=seed)

    raw_test_ds = tf.keras.preprocessing.text_dataset_from_directory(
        'aclImdb/test',
        batch_size=batch_size)
    return raw_train_ds, raw_val_ds, raw_test_ds


def prep_dataset(raw_train_ds, raw_val_ds, raw_test_ds):
    # Make a text-only dataset (without labels), then call adapt
    train_text = raw_train_ds.map(lambda x, y: x)
    vectorize_layer.adapt(train_text)

    # retrieve a batch (of 32 reviews and labels) from the dataset
    text_batch, label_batch = next(iter(raw_train_ds))

    train_ds = raw_train_ds.map(vectorize_text)
    val_ds = raw_val_ds.map(vectorize_text)
    test_ds = raw_test_ds.map(vectorize_text)

    train_ds = train_ds.cache().prefetch(buffer_size=AUTOTUNE)
    val_ds = val_ds.cache().prefetch(buffer_size=AUTOTUNE)
    test_ds = test_ds.cache().prefetch(buffer_size=AUTOTUNE)
    return train_ds, val_ds, test_ds


def build_model(train_ds, val_ds, test_ds):
    model = tf.keras.Sequential([
        layers.Embedding(max_features + 1, embedding_dim),
        layers.Dropout(0.2),
        layers.GlobalAveragePooling1D(),
        layers.Dropout(0.2),
        layers.Dense(1)])

    model.summary()

    model.compile(loss=losses.BinaryCrossentropy(from_logits=True),
                  optimizer='adam',
                  metrics=tf.metrics.BinaryAccuracy(threshold=0.0))

    model.fit(
        train_ds,
        validation_data=val_ds,
        epochs=epochs,
        callbacks=[MetricsLogCallback()])
    return model


def build_exportable_model(model):
    export_model = tf.keras.Sequential([
        vectorize_layer,
        model,
        layers.Activation('sigmoid')
    ])

    export_model.compile(
        loss=losses.BinaryCrossentropy(from_logits=False), optimizer="adam", metrics=['accuracy']
    )
    return export_model
```

### Creating MLFoundry Run

```python
mlf_api = mlf.get_client()
mlf_run = mlf_api.create_run(project_name='tensorflow-project')
```

### Building the model

```python
# Callback class to logg the metrics at the end of each epoch
class MetricsLogCallback(Callback):
    def on_epoch_end(self, epoch, logs=None):
        mlf_run.log_metrics(logs)   # logging metrics using mlfoundry run

# building the model
def build_model(train_ds, val_ds, test_ds):
    model = tf.keras.Sequential([
        layers.Embedding(max_features + 1, embedding_dim),
        layers.Dropout(0.2),
        layers.GlobalAveragePooling1D(),
        layers.Dropout(0.2),
        layers.Dense(1)])

    model.summary()

    model.compile(loss=losses.BinaryCrossentropy(from_logits=True),
                  optimizer='adam',
                  metrics=tf.metrics.BinaryAccuracy(threshold=0.0))

    model.fit(
        train_ds,
        validation_data=val_ds,
        epochs=epochs,
        callbacks=[MetricsLogCallback()])
    return model

# To combine the model trained earlier and vectorization layer
def build_exportable_model(model):
    export_model = tf.keras.Sequential([
        vectorize_layer,
        model,
        layers.Activation('sigmoid')
    ])

    export_model.compile(
        loss=losses.BinaryCrossentropy(from_logits=False), optimizer="adam", metrics=['accuracy']
    )
    return export_model
```

**Few things to note here:**

1. A callback class **MetricsLogCallback** has been implemented which uses the **log\_metrics** API to log all the metrics being calculated at the end of each epoch. ****&#x20;

### Training the model

```python
# download_data()
raw_train_ds, raw_val_ds, raw_test_ds = get_raw_dataset()
train_ds, val_ds, test_ds = prep_dataset(raw_train_ds, raw_val_ds, raw_test_ds)
model = build_model(train_ds, val_ds, test_ds)
export_model = build_exportable_model(model)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-23 20-26-46.png>)

### Logging Model

```python
model_loggable = {
    'model': export_model,
    'signatures': None,
    'options': None
}

mlf_run.log_model(model_loggable, mlf.ModelFramework.TENSORFLOW)
```

### Logging Predictions

```python
for text_batch, label_batch in raw_test_ds.take(1):
    X_test = text_batch.numpy()
    y_test = label_batch.numpy()

y_hat_test = export_model.predict(X_test)
y_hat_test = np.round(y_hat_test.reshape((batch_size))).astype('int32')

mlf_run.log_predictions(pd.DataFrame(X_test), list(y_hat_test))
```

### Logging dataset stats

```
X_test_df = pd.DataFrame(X_test, columns=['text'])
X_test_df['targets'] = y_test
X_test_df['predictions'] = y_hat_test

mlf_run.log_dataset_stats(
    X_test_df,
    data_slice=mlf.DataSlice.TEST,
    data_schema=mlf.Schema(
        feature_column_names=['text'],
        prediction_column_name="predictions",
        actual_column_name="targets"
    ),
    model_type=mlf.ModelType.BINARY_CLASSIFICATION,
)
```

![](<../../.gitbook/assets/Screenshot from 2021-12-23 18-38-44.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-23 20-26-57.png>)

![](<../../.gitbook/assets/Screenshot from 2021-12-23 20-30-37.png>)
