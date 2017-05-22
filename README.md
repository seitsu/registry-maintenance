Registry Maintenance Notifications with JSON
=================

## The Goal
The goal is to create a machine-readable registry maintenance notification format to make things easier for domain name registrars to keep up with maintenance notices.

## Using

#### Intro
  * RESTful webservice on port 443 with JSON formatted response according [RFC 4627](https://www.ietf.org/rfc/rfc4627.txt)
  * HTTP Response Code according [RFC 7231](https://tools.ietf.org/html/rfc7231) (200 succesful and 404 error)
  * HTTP Header content-type: application/json

#### Calling the service

GET https://status.registry.tld/maintenance

```
GET https://status.registry.tld/maintenance?environment=production&start=2017-04-01&end=2017-06-30

  environment     [optional] (string) 'production', 'test', 'staging' or 'all' (default: production)
  start           [optional] (date)   according ISO 8601 YYYY-MM-DD (default: today)
  end             [optional] (date)   according ISO 8601 YYYY-MM-DD (default: today + 3 months)
```

#### Successful response according [maintenance.json] and [maintenance-schema.json]

Explanation of succesful response values:
```
  systems         [required] (array)    contains name, hostname and impact
    name          [required] (string)   name of affected system
    hostname      [required] (string)   affected maintained systems
    impact        [required] (string)   impact level per affected system; values are 'partial' or 'blackout'
  environment     [required] (string)   environment of affected maintained systems
  start           [required] (datetime) according ISO 8601 YYYY-MM-DDThh:mm:ssTZD
  end             [required] (datetime) according ISO 8601 YYYY-MM-DDThh:mm:ssTZD
  reason          [required] (string)   'planned maintenance' or 'emergency maintenance'
  remark          [required] (string)   URL to detailed maintenance description or empty
  tlds            [required] (array)    affected top-level domains
  intervention    [required] (string)   'yes' or 'no' - if yes, please specify on the website where remarks link to 
```

#### Error response according [error.json] and [error-schema.json]

Explanation of error response values:
```
  code            [required] (string)  'NO_RESULT', 'WRONG_VALUE_ENVIRONMENT', 'WRONG_VALUE_START_DATE' or 'WRONG_VALUE_END_DATE'
  error           [required] (string)  explanation of the error code
```

Error messages by code
```
  NO_RESULT                 No maintenance notifications found in this timeframe
  WRONG_VALUE_ENVIRONMENT   Given environment does not exist
  WRONG_VALUE_START_DATE    Given start date does not exist or before end date
  WRONG_VALUE_END_DATE      Given end date does not exist or before start date
```
## Contributing
If you'd like to contribute, please go ahead.

## License
This is free and unencumbered software released into the public domain. For more info, read the [LICENSE] file distributed

[license]: /LICENSE
[maintenance.json]: /maintenance.json
[maintenance-schema.json]: /maintenance-schema.json
[error.json]: /error.json
[error-schema.json]: /error-schema.json
