description: Manage your Feature Flags and Remote Config in your PHP applications.

# Flagsmith Client

The SDK client for Rust [https://flagsmith.com/](https://www.flagsmith.com/). Flagsmith allows you to manage feature flags and remote config across multiple projects, environments and organisations.

The source code for the client is available on [Github](https://github.com/flagsmith/flagsmith-rust-client).

## Usage

### Retrieving feature flags for your project

In your application initialise the BulletTrain client with your API key

```rust
let bt = bullettrain::Client::new("<Your API Key>");
```

To check if a feature flag exists and is enabled:

```rust
let bt = bullettrain::Client::new("<Your API Key>");
if bt.feature_enabled("cart_abundant_notification_ab_test_enabled")? {
    println!("Feature enabled");
}
```

To get the configuration value for feature flag value:

```rust
use bullettrain::{Client,Value};

let bt = Client::new("<Your API Key>");

if let Some(Value::String(s)) = bt.get_value("cart_abundant_notification_ab_test")? {
    println!("{}", s);
}
```

More examples can be found in the [Tests](https://github.com/Flagsmith/flagsmith-rust-client/blob/master/tests/integration_test.rs)

## Override default configuration

By default, client is using default configuration. You can override configuration as follows:

```rust
let bt = bullettrain::Client {
    api_key: String::from("secret key"),
    base_uri: String::from("https://features.on.my.own.server/api/v1/"),
};
```
