### devmetrics-nodejs-core
NodeJS lib for metrics and logs.

Sends data to logstash over UDP.

Browse data in the Kibana or native interface at http://www.devmetrics.io/logs

####Installation

``` bash
  $ npm install devmetrics-core
```

####Library init

> Default way, sends data to devmetrics.io shared cluster (1M events/month for free) with autogenerated app_id:

``` js
var devmetrics = require('devmetrics-core')()
```

> Send data to devmertics shared cluster with private app_id:

``` js
var devmetrics = require('devmetrics-core')({'app_id': 'my-private-id'});
```

> Advanced init (for example, to send data to a private instance):

``` js
var devmetrics = require('devmetrics-core')({
  'host': 'service.devmetrics.io',
  'port': 5545,
  'app_id': 'my-private-id',
  'no_console': false,
  'software_version': '1.2'
});
```
All settings are optional:

- `host` - Set to your private instance of logstash, if required, defaults to `service.devmetrics.io`
- `port` - Logstash port, defaults to 5546 for devmetrics.io cluster
- `app_id` - The unique app_id to filter your data and access dashboards, defaults to `os.hostname()`
- `no_console` - Set to `true` for the silent mode (no stdout), defaults to `false`
- `software_version` - Your app's version. Useful for continuous integration, defaults to ``

####Simple Logs API

**Simple log**

``` js
devmetrics.info('hey info, not important');
devmetrics.warn('hey warn, probably important');
devmetrics.error('hey error, really important');
```

> Basic logging, for anything. Levels: trace, debug, info, warn, error

**Exception**

``` js
devmetrics.exception(e);
```

> Adds special sections to track application problems


####Event API

**User event**

``` js
/**
 * @param event_name Metric name, all events with same metric name are aggregated
 * @param user_id optional, User unique identifier, used only for log
 * @param event_tags optional, Additional info, metric dimensions, string or array
 */
devmetrics.userEvent('puchase_button_click', 123, ['android', 'US']);
devmetrics.userEvent('login', 89321);
```

> We are generally user oriented and track user actions. So let's separate business metrics from developer data.


**App event**

``` js
/**
 * @param event_name Metric name, all events with same metric name are aggregated
 * @param event_tags optional, Additional info, metric dimensions, string or array
 */
devmetrics.appEvent('app_started', 'server2');
devmetrics.appEvent('web-request', ['index-page', 'US_region']);
```

>  Almost the same API as for UserEvent, but userEvent is for business metrics and appEvent is for application and system metrics. Let's differentiate this data starting from the collect layer.

####Stay in touch

We are moving fast and appreciate feedback and feature requests.
See you @ https://gitter.im/devmetrics/dev#
