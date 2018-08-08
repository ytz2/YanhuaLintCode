# Design Uber



## Scenario

| Use Cases | QPS | Storage |
| :--- | :--- | :--- |
| Driver report location | 1 hb/4s  200k/4 = 50k/s , peak 15k/s | 15 mb \* 1e5 = 1.5T /day |
| Customer request trip | 500 /s = 50k \* 0.1 / 1000 for peak |  |



Customer request trip

Match driver for customer

Driver AC/Deny request

Customer cancel trip

Driver pickup to start trip 

Driver dropoff customer to end trip



maintain below info : 

trip, driver and location map



## Service

![](/assets/ubserSvc.png)

How to match?

GEOHASH, google s2

![](/assets/solution uber.png)













