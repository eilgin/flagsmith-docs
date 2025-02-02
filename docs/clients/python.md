description: Manage your Feature Flags and Remote Config in your Python applications.

This library can be used with server-side Python projects. The source code for the client is available on [Github](https://github.com/flagsmith/flagsmith-python-client).

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See running in production for notes on how to deploy the project on a live system.

## Installing

### VIA pip

```bash
pip install bullet-train
```

## Usage

### Retrieving feature flags for your project

```python
from bullet_train import BulletTrain;

bt = BulletTrain(environment_id="<YOUR_ENVIRONMENT_KEY>")

if bt.has_feature("header"):
  if bt.feature_enabled("header"):
    # Show my awesome cool new feature to the world

value = bt.get_value("header", '<My User Id>')

value = bt.get_value("header")

bt.set_trait("accept-cookies", "true", "ben@bullet-train.io"))
bt.get_trait("accept-cookies", "ben@bullet-train.io"))
```

### Available Options

| Property        | Description           | Required  | Default Value  |
| ----- |:-------------| -----:| -----:|
| ```environment_id```     | Defines which project environment you wish to get flags for. *example ACME Project - Staging.* | **YES** | None
| ```api```     | Use this property to define where you're getting feature flags from, e.g. if you're self hosting. |  **NO** | https://api.bullet-train.io/api/

### Available Functions

| Function        | Description |
| ------------- |:-------------:|
| ```has_feature(key)```     | Determine if given feature exists for an environment. ```bt.has_feature("powerUserFeature") // true```
| ```feature_enabled(key)```     | Get the value of a particular *feature flag* e.g. ```bt.feature_enabled("powerUserFeature") // true```
| ```feature_enabled(key, userId)```     | Get the value of a particular *feature flag* e.g. ```bt.feature_enabled("powerUserFeature", 1234) // true```
| ```get_value(key)```     | Get the value of a particular *remote config* e.g. ```bt.get_value("font_size") // 10```
| ```get_value(key, userId)```     | Get the value of a particular feature for a specified user e.g. ```bt.get_value("font_size", 1234) // 15```
| ```set_trait(trait_key, trait_value, userId)```     | Set the value of a particular trait for a specified user e.g. ```bt.set_trait("font_size", 12, 1234) // 15```
| ```get_trait(trait_key, userId)```     | Get the value of a particular trait for a specified user e.g. ```bt.get_trait("font_size", 1234) // 12```
| ```get_flags()```     | Trigger a manual fetch of the environment features, returns a list of flag objects, see below for returned data
| ```get_flags_for_user(1234)```     | Trigger a manual fetch of the environment features with a given user id, returns a list of flag objects, see below for returned data

### Identifying users

Identifying users allows you to target specific users from the [Bullet Train dashboard](https://www.bullet-train.io/).
You can include an optional user identifier as part of the `has_feature` and `get_value` methods to retrieve unique user flags and variables.

### Flags data structure

| Field | Description | Type |
| ---- | ------------ | ---- |
| id | Internal id of feature state | Integer |
| enabled | Whether feature is enabled or not | Boolean |
| environment | Internal ID of environment | Integer | 
| feature_state_value | Value of the feature | Any - determined based on data input on [bullet-train.io](https://bullet-train.io). |
| feature | Feature object - see below for details | Object |

### Feature data structure

| Field | Description | Type |
| ---- | --------------- | --- |
| id | Internal id of feature | Integer |
| name | Name of the feature (sometimes referred to as key or ID) | String |
| description | Description of the feature | String |
| type | Feature Type. Can be FLAG or CONFIG | String |
| created_date | Date feature was created | Datetime |
| inital_value | The initial / default value set for all feature states on creation | String |
| project | Internal ID of the associated project | Integer |  
