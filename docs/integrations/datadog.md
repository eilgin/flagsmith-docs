description: Integrate Flagsmith with Datadog

# Datadog Integration

<img src="/images/integrations/datadog/datadog-logo.svg"/>

You can integrate Flagsmith with Datadog. Send flag change events from Flagsmith into your Datadog event stream.

## Integration Setup

<img src="/images/integrations/datadog/datadog-3.png"/>

1. Log into Datadog and go to Integrations > API
2. Generate an API key
3. Add the Datadog API key into Flagsmith (Integrations > Add Datadog Integration)
4. Add the Datadog URL into Flagsmith - (This is either `https://api.datadoghq.com/` or `https://api.datadoghq.eu/`)

<img src="/images/integrations/datadog/datadog-1.png"/>

<img src="/images/integrations/datadog/datadog-2.png"/>

Flag change events will now be sent to Datadog.
