description: Self hosting Flagsmith.

# Self Hosting

You are free to self host the entire Flagsmith Platform. The easiest way to do this is via [Docker](/docker).

## Dependencies

Running the API has the following hard dependencies:

* Postgres database - the main data store

The API can also optionally make use of the following 3rd party services:

* Google Analytics - for API analytics
* InfluxDB - for API analytics
* SendGrid - for transactional email
* AWS S3 - to store Django Static Assets
* GitHub - oAuth provider
* Google - oAuth provider

## API Telemetry

Flagsmith collects information about self hosted installations. This helps us understand how the platform is being used. This data is *never* shared outside of the organisation, and is anonymous by design. You can opt out of sending this telemetry on startup by setting the `TELEMETRY_DISABLED` environment variable to any value.

We collect the following data on startup and then once every 8 hours per API server instance:

* Total number of Organisations
* Total number of Projects
* Total number of Environments
* Total number of Features
* Total number of Segments
* Total number of Users
* DEBUG django variable
* ENV django variable
* API server external IP address

## Running Flagsmith on Flagsmith

Flagsmith uses Flagsmith to control features on the front end dashboard. If you are self hosting the platform, you will sometimes see features greyed out, or you may want to disable specific features, e.g. logging in via Google and Github. If you are using your own Flagsmith environment then you will need to have a replica of our flags in order to control access to those features.

To do this,firstly create a new project within your self-hosted Flagsmith application. This is the project that we will use to control the features of the self-hosted Flagsmith instance. We will then point the self hosted front end dashboard at this Flagsmith project in order to control what features show for your self hosted Flagsmith instance.

Once you have created the project, you need to set the following [Front End](https://github.com/Flagsmith/flagsmith-frontend) environment variables in order to configure this:

* `FLAGSMITH`
  * The flagsmith environment key we use to manage features - Flagsmith runs on Flagsmith. This will be the API key for the project you created as instructed above.
* `FLAGSMITH_CLIENT_API`
  * The API URL which the Flagsmith front end dashboard should communicate with. This will most likely be the domain name of the Flagsmith API you are self hosting: Flagsmith runs on Flagsmith. E.g. For our SaaS hosted platform, the variable is `https://api.flagsmith.com/api/v1/`. For example, if you were running everything locally using the standard [docker-compose setup](https://github.com/Flagsmith/flagsmith-docker), you would use `http://localhost:8000/api/v1/`
  
Once you have set this up, you should see the Flagsmith front end requesting its own flags from the API (you can look in your browser developer console to see this). You can now start creating flags and overriding the default behaviours of the platform. For example, if you wanted to disable Google OAuth authentication, you would create a flag called `oauth_google` and disable it.

The list of the flags and remote config we're currently using in production is below:

```json
[
  {
    "description": "Whether to show the try it buttons",
    "name": "try_it",
    "type": "FLAG"
  },

  {
    "description": "Allows for new user overrides UX",
    "name": "improved_identity_overrides",
    "type": "FLAG"
  },

  {
    "description": "Configures integrations",
    "name": "integration_data",
    "value": "{\n  \"datadog\": {\n    \"perEnvironment\": false,\n    \"image\": \"https://www.vectorlogo.zone/logos/datadoghq/datadoghq-icon.svg\",\n    \"fields\": [\n      {\n        \"key\": \"base_url\",\n        \"label\": \"Base URL\"\n      },\n      {\n        \"key\": \"api_key\",\n        \"label\": \"API Key\"\n      }\n    ],\n    \"tags\": [\n      \"logging\"\n    ],\n    \"title\": \"Datadog\",\n    \"description\": \"Sends events to Datadog for when flags are created, updated and removed. Logs are tagged with the environment they came from e.g. production.\"\n  },\n  \"amplitude\": {\n    \"perEnvironment\": true,\n    \"image\": \"https://braze-marketing-assets.s3.amazonaws.com/images/partner_logos/amplitude-1.png\",\n    \"fields\": [\n      {\n        \"key\": \"api_key\",\n        \"label\": \"API Key\"\n      }\n    ],\n    \"tags\": [\n      \"analytics\"\n    ],\n    \"title\": \"Amplitude\",\n    \"description\": \"Sends data on what flags served to each identity.\"\n  }\n}"
  },

  {
    "description": "Determines what integrations show",
    "name": "integration_data",
    "value": "[\"datadog\"]"
  },

  {
    "description": "Controls rbac and 2f based on plans",
    "name": "plan_based_access",
    "type": "FLAG"
  },
  
  {
    "description": "Determines whether to show tags",
    "name": "tags",
    "type": "FLAG"
  },
  
  {
    "description": "Usage chart",
    "name": "usage_chart",
    "type": "FLAG"
  },

  {
    "name": "identity_segments",
    "description": "Lists segments users are in on the user page"
  },
  {
    "name": "segment_operators",
    "description": "Determines what rules are shown when creating a segment",
    "value": '[{"value":"EQUAL","label":"Exactly Matches (=)"},{"value":"NOT_EQUAL","label":"Does not match (!=)"},{"value":"PERCENTAGE_SPLIT","label":"% Split"},{"value":"GREATER_THAN","label":">"},{"value":"GREATER_THAN_INCLUSIVE","label":">="},{"value":"LESS_THAN","label":"<"},{"value":"LESS_THAN_INCLUSIVE","label":"<="},{"value":"CONTAINS","label":"Contains"},{"value":"NOT_CONTAINS","label":"Does not contain"},{"value":"REGEX","label":"Matches regex"}]'
  },
  {
    "name": "oauth_github",
    "description": "WIP oauth with github",
    "value": "oauth config"
  },
  {
    "name": "oauth_facebook",
    "description": "WIP oauth with facebook",
    "value": "oauth config"
  },
  
  {
    "name": "oauth_microsoft",
    "description": "WIP oauth with microsoft",
    "value": "oauth config"
  },
  
  {
    "name": "oauth_google",
    "description": "WIP oauth with google",
    "value": "oauth config"
  }
]
```

## Manual Installation

If you want a more configurable environment, you can manually install both the Front End and the API.

### Server Side API

The source code and installation instructions can be found at [the GitHub project](https://github.com/flagsmith/Bullet-Train-API). The API is written in Python and is based on Django and the Django Rest Framework. The Server side API relies on a Postgres SQL installation to store its data, and a Redis installation as a cache.

### Front End Website

The source code and installation instructions can be found at [the GitHub project](https://github.com/flagsmith/Bullet-Train-Frontend). The Front End Website is written in React/Javascript and requires NodeJS.
