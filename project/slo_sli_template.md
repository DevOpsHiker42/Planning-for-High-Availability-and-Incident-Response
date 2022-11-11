# API Service

| **Category**   | **SLI**                                                                              | **SLO**                                                                                                       |
|----------------|--------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Availability   | Proportion of successful requests (those that did not return HTTP 5xx error status)  | 99%                                                                                                           |
| Latency        | Proportion of requests with request duration less than specified threshold value     | 90% of requests below 100ms                                                                                   |
| Error Budget   | Proportion of unsuccessful requests (HTTP response code <> 200)                      | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget   |
| Throughput     | Successful requests (HTTP response code 2xx) processed per second                    | 5 RPS indicates the application is functioning                                                                |

