# Test in Production

## Adopt

  - Three node pools for services
    * Production: handles most of the user traffic
    * Production Canary: gets a percentage of the production user traffic, new code versions can be tried here before full rollout.
    * "Dark" Canary (developer access only): has access to production data and services, requests can be routed to it with *X-Gilt-Dev* HTTP header or *dev* cookie. Can be used to test drive and load test versions of the code that pass unit tests or iterate on data-heavy UI that needs many production services / data. Test cookie can be set by following this [link](http://www.gilt.com/z/dev). Developer builds of mobile clients can be configured to run in 'dev' mode to set *X-Gilt-Dev* header on every request.

  - Test one thing at a time. While it's possible to e.g. propagate X-Gilt-Dev header down through the chain of service dependencies, it leads to a brittle test setup where it's hard to be sure these services are *independently* deployable.
    * Compatible API changes can be tested by driving API end points.
    * Incompatible changes require new end points anyway, can be tested, released to production and then production URLs to evolve other services that depend on them.

  - Test accounts. E.g. "anyemail@gilttest.com" is already set up to not send orders to warehouse.

  - Targeting. E.g. discounts can be issued to a test user alone.
