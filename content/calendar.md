+++
title = 'Calendar'
pager = false
+++

You can add all our (future) events to your calendar service. This way, each
time we publish a new event, you'll see it in your calendar, and potentially
receive a notification.

## Applications able to open `webcal://` URIs

If your have a calendar application installed on your device, and it is able to
open `webcal://` URIs (much like email clients can open `mailto://` URIs), just
click [this link here]({{< ref path="/events" outputFormat="calendar">}}).

If this does nothing, just use the next method instead.

## (Web) applications unable to open `webcal://` URIs

Copy the following link and paste it in whatever "Add by URL" text field your
calendar service provides you with.

```
{{< ref path="/events" outputFormat="calendar">}}
```
