# Tensorflow Model Serving

There are many ways to serve a model. 

## Introduction
TensorFlow Serving is a flexible, high-performance serving system for machine learning models, designed for production environments. TensorFlow Serving makes it easy to deploy new algorithms and experiments, while keeping the same `erver architecture and APIs`. TensorFlow Serving provides out-of-the-box integration with TensorFlow models, but can be easily extended to serve other types of models and data.


## I. Key Concepts


### 1. Servables
Servables are the central abstraction in TensorFlow Serving. Servables are the underlying objects that clients use to perform computation (for example, a lookup or inference).


A single Servable might include anything from a single shard of a lookup table to a single model to a tuple of inference models

Typical servables include the following:

* a TensorFlow SavedModelBundle (tensorflow::Session)
* a lookup table for embedding or vocabulary lookups


### 2. Servable Versions
TensorFlow Serving can handle one or more versions of a servable over the lifetime of a single server instance. This enables fresh algorithm configurations, weights, and other data to be loaded over time. Versions enable more than one version of a servable to be loaded concurrently, supporting gradual rollout and experimentation. At serving time, clients may request either the latest version or a specific version id, for a particular model.


### 3. Servable Streams
A servable stream is the sequence of versions of a servable, sorted by increasing version numbers.

### 4. Models
TensorFlow Serving represents a model as one or more servables. A machine-learned model may include one or more algorithms (including learned weights) and lookup or embedding tables.


### 5. Loaders
Loaders manage a servable's life cycle. The Loader API enables common infrastructure independent from specific learning algorithms, data or product use-cases involved. 


### 6. Sources
Sources are plugin modules that find and provide servables. Each Source provides zero or more servable streams. For each servable stream, a Source supplies one Loader instance for each version it makes available to be loaded
`Aspired Versions`
Aspired versions represent the set of servable versions that should be loaded and ready. Sources communicate this set of servable versions for a single servable stream at a time.


### 7. Managers
Managers handle the full lifecycle of Servables, including:

* loading Servables
* serving Servables
* unloading Servables


### 8. Core

TensorFlow Serving Core treats servables and loaders as opaque objects:

* Sources create Loaders for Servable Versions.
* Loaders are sent as Aspired Versions to the Manager, which loads and serves them to client requests.


## II. Server API or RESTful API

The request and response is a JSON object. The composition of this object depends on the request type or verb. See the API specific sections below for details.

### 1. Model status API
This API closely follows the ModelService.GetModelStatus gRPC API. It returns the status of a model in the ModelServer

```bash
GET http://host:port/v1/models/${MODEL_NAME}[/versions/${VERSION}|/labels/${LABEL}]
```

Including /versions/${VERSION} or /labels/${LABEL} is optional. If omitted status for all versions is returned in the response.

- `Response format`: if successful, returns a JSON representation of GetModelStatusResponse protobuf.

### 2. Model Metadata API
This API closely follows the PredictionService.GetModelMetadata gRPC API. It returns the metadata of a model in the ModelServer

```bash
GET http://host:port/v1/models/${MODEL_NAME}[/versions/${VERSION}|/labels/${LABEL}]/metadata
```
Including `/versions/${VERSION} or /labels/${LABEL}` is optional. If omitted the model metadata for the latest version is returned in the response.

- `Response format`: if successful, returns a JSON representation of GetModelMetadataResponse protobuf.

### 3. Classify and Regress API
This API closely follows the Classify and Regress methods of PredictionService gRPC API.

```bash
POST http://host:port/v1/models/${MODEL_NAME}[/versions/${VERSION}|/labels/${LABEL}]:(classify|regress)
```
Including /versions/${VERSION} or /labels/${LABEL} is optional. If omitted the latest version is used.

* Request format: the request body for the classify and regress APIs must be a JSON object formatted as follows:

```json
{
  // Optional: serving signature to use.
  // If unspecifed default serving signature is used.
  "signature_name": <string>,

  // Optional: Common context shared by all examples.
  // Features that appear here MUST NOT appear in examples (below).
  "context": {
    "<feature_name3>": <value>|<list>
    "<feature_name4>": <value>|<list>
  },

  // List of Example objects
  "examples": [
    {
      // Example 1
      "<feature_name1>": <value>|<list>,
      "<feature_name2>": <value>|<list>,
      ...
    },
    {
      // Example 2
      "<feature_name1>": <value>|<list>,
      "<feature_name2>": <value>|<list>,
      ...
    }
    ...
  ]
}
```



### 5. Predict API
This API closely follows the PredictionService.Predict gRPC API.

```bash
POST http://host:port/v1/models/${MODEL_NAME}[/versions/${VERSION}|/labels/${LABEL}]:predict
```
Including /versions/${VERSION} or /labels/${LABEL} is optional. If omitted the latest version is used.

* Request format: the request body for predict API must be JSON object formatted as follows:

```json
{
  // (Optional) Serving signature to use.
  // If unspecifed default serving signature is used.
  "signature_name": <string>,

  // Input Tensors in row ("instances") or columnar ("inputs") format.
  // A request can have either of them but NOT both.
  "instances": <value>|<(nested)list>|<list-of-objects>
  "inputs": <value>|<(nested)list>|<object>
}
```

Specifying input tensors in row format.
 Use the ``inputs`` format if all named input tensors have the same 0-th dimension. If they don't, use the columnar format described later below.

- In the row format, inputs are keyed to instances key in the JSON request.

- When there is only one named input, specify the value of instances key to be the value of the input:

```json
{
  // List of 3 scalar tensors.
  "instances": [ "foo", "bar", "baz" ]
}

{
  // List of 2 tensors each of [1, 2] shape
  "instances": [ [[1, 2]], [[3, 4]] ]
}
```

Tensors are expressed naturally in nested notation since there is no need to manually flatten the list.

For multiple named inputs, each item is expected to be an object containing input name/tensor value pair, one for each named input. As an example, the following is a request with two instances, each with a set of three named input tensors


```json
{
 "inputs": {
   "tag": ["foo", "bar"],
   "signal": [[1, 2, 3, 4, 5], [3, 4, 1, 2, 5]],
   "sensor": [[[1, 2], [3, 4]], [[4, 5], [6, 8]]]
 }
}
```

Note, inputs is a JSON object and not a list like instances (used in the row representation). Also, all the named inputs are specified together, as opposed to unrolling them into individual rows done in the row format described previously. This makes the representation compact (but maybe less readable).


Reference : https://www.tensorflow.org/tfx/guide/serving








