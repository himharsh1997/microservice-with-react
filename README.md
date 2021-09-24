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

## Communication strategies between services
- 1) Async, 2) Sync . These are not same as it mean in js world
- **Sync**: Services communicate with each other using direct requests(format could be any).eg; serviceD call serviceA then follow by call to serviceB then serviceC to get information of user with id 1.
   - Upsides of this approach:
     - Conceptually easy to understand.
     - Some service don't need database of their own.
   - Downside of this approach:
     - Dependency between services. One service go down can make other services down as well.
     - If any inter-service request fails, overall request fails. eg; if serviceD calling service A, B, C to get data and B,C dependent on A then on fail of serviceA other requests also fails.
     - Entire request is as fast as slowest inter-service request.
     - Can introduce webs of requests. Mean one service maybe calling other service to fullfull request and those services maybe calling other service. This create a graph of services call to fullfill one request from user. This sometime make debug+log issue difficult for finding service responsible for it.
 - **Async**: Services communicate with each other using events. Also known as event-based communication.
    - Setup we have in this:
      - Services
      - Event which user emit on event bus.
      - Event bus or broker to which any service has aceess+connect to read and write events.
    - Has same downside except also of not easy to understand.
