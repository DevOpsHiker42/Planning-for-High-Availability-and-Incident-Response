| # API Service  |                                                                                      |                                                                                                               |
|----------------|--------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Category       | SLI                                                                                  | SLO                                                                                                           |
| -------------- | -----                                                                                | ------------------------------------------------------------------------------------------------------------- |
| Availability   | Percentage of successful requests (requests that are not an HTTP 5xx error)          | 99%                                                                                                           |
| Latency        | Request duration histogram (all requests)                                            | 90% of requests below 100ms                                                                                   |
| Error Budget   | Percentage of unsuccessful requests (HTTP response code <> 200) in compliance period | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget   |
| Throughput     | Rate of successful requests (HTTP response code 2xx) processed                       | 5 RPS indicates the application is functioning                                                                |

