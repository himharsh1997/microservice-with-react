# microservice-with-react

## Monolithic architecure
   Auth Middleware | Routing | Biz logic for all features | database(s)

## Microservice architecure
  - Auth Middleware | Routing | Biz logic for one feature | single database(if required)
  - ServiceA cannot directly interact with serviceB database.
  - Each service will get their own database if required(Pattern: Database-per-service)
  - Reason for above 2 points:
    - To make service run independently of other services.
    - Easy for scaling as that database will only serve one service.
    - As we remove dependency so in case ServiceA want to use serviceB database then if anything happen to database cause effect to 2 services(uptime reduce).
    - As we remove dependency so in case ServiceA want to use serviceB database but as in serviceB database some schema got changed but this updates are told with delay then serviceA feature will effect a lot or got crash(as serviceA is not configured to expect userName). eg {name: "Ram"} -> {userName: "Ram"}.
    - Some service may run more effecient on one database but not on other. sql(MySQL, postgres), nosql(mongodb, redis).

## Challenge with Microservice architecure
   Data management between services store in different services.
