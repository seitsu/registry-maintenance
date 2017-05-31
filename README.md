## Machine-readable Domain Name Registry Maintenance Notifications

### The Goal
The goal is to create a machine-readable domain name registry maintenance notification format to make things easier for domain name registrars to keep up with maintenance notices.

### Using it

#### Intro
  * RESTful webservice on port 443 with JSON formatted response according [RFC 7159](https://tools.ietf.org/html/rfc7159)
  * HTTP Response Code according [RFC 7231](https://tools.ietf.org/html/rfc7231) (200 successful, 204 no content or 400 bad request)
  * HTTP Response Header according [RFC 2616](https://tools.ietf.org/html/rfc2616#section-14.17) (content-type: application/json)

#### Calling the service
GET https://status.registry.tld/maintenances?environment=production&start=2017-04-01&end=2017-06-30

Available arguments to call the service:
```
  environment        [optional] (string)   'production', 'test', 'staging' or 'all' (default: production)
  start              [optional] (date)     according ISO 8601 YYYY-MM-DD in UTC (default: today)
  end                [optional] (date)     according ISO 8601 YYYY-MM-DD in UTC (default: today + 3 months)
```

#### Successful response according [maintenance.json] and [maintenance-schema.json]

Explanation of successful response values - HTTP Response Code 200:
```
  notifications      [optional] (array)    contains a single maintenance notification
    id               [required] (string)   unique id for each maintenance, shouldn't be changed if maintenance gets postponed
    systems          [required] (array)    contains name, host and impact
      name           [required] (string)   name of affected system
      host           [required] (string)   affected maintained systems (host or ip)
      impact         [required] (string)   impact level per affected system; values are 'partial' or 'blackout'
    environment      [required] (string)   environment of affected maintained systems
    start            [required] (datetime) according ISO 8601 YYYY-MM-DDThh:mm:ssTZD
    end              [required] (datetime) according ISO 8601 YYYY-MM-DDThh:mm:ssTZD
    reason           [required] (string)   free text why this maintenance is necessary, could be empty
    remark           [required] (string)   URL to detailed maintenance description or empty
    tlds             [required] (array)    affected top-level domains
    intervention     [required] (array)    contains connection and implementation
      connection     [required] (boolean)  true or false - if registrar needs to do something connection related
      implementation [required] (boolean)  true or false - if registrar needs to change their implementation
```

#### Error response according [error.json] and [maintenance-schema.json]

Explanation of error response values:
```
  error              [required] (string)   'NO_RESULT', 'WRONG_VALUE_ENVIRONMENT', 'WRONG_VALUE_START_DATE' or 'WRONG_VALUE_END_DATE'
```

Error messages - HTTP Response Code 204 no content or 400 bad request:
```
  204   NO_RESULT                 No maintenance notifications found in this timeframe
  400   WRONG_VALUE_ENVIRONMENT   Given environment does not exist
  400   WRONG_VALUE_START_DATE    Given start date does not exist or before end date
  400   WRONG_VALUE_END_DATE      Given end date does not exist or before start date
```
### Contributing
If you'd like to contribute, please go ahead.

### License
This is free and unencumbered software released into the public domain. For more info, please read the [LICENSE] file distributed.

[license]: /LICENSE
[maintenance.json]: /maintenance.json
[maintenance-schema.json]: /maintenance-schema.json
[error.json]: /error.json
