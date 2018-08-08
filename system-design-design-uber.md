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

![](/assets/solution uber.png)Trip Table

| UserId | FK |
| :--- | :--- |
| DriverId | FK |
| TripID | 这个要写到driver 的trip id里去 |
| Status | 询问，等待，开始，结束 |
| Start Lat |  |
| Stop Long |  |
| Dest Lat |  |
| Dest Long |  |



Driver Table

| UserId | FK |
| :--- | :--- |
| DriverId | FK |
| TripID | 这个是match到的时候从location table里写进来的 |
| Status | 询问中 （black list it, if rej\), 接单， 旅途中 |
| Start Lat |  |
| Stop Long |  |
| Dest Lat |  |
| Dest Long |  |



Location table

按照city进行sharding, city用geofence的办法来query

| Row Key | Driver 1, 2, 3, ,4, 5 |
| :--- | :--- |
| 9Q9H |  |
| 9Q9HA | \(driver id\) |
| 9Q9HAA |  |



