# Quality Assurance

Gilt iterated over many different approaches to quality assurance over time. There's no clear "one size fits all" winner. Below is the list of recommendations that can be combined to, collectively, achieve the required level of correctness / quality.

A nice, in depth, look at microservice testing: [link](http://martinfowler.com/articles/microservice-testing/).


## Adopt

  - [Monitoring systems](Monitoring.md). While they don't prevent defects from going out into production, they catch what "falls through" other testing methods. Should be considered a "must have".
    In particular, collecting business-specific metrics and alerting on those can be a great way to limit all sorts of damage.

  - Canary deploys, see [Test in production](Quality-TIP.md). Should be considered a standard practice for all highly trafficed systems.

  - Strongly typed systems. Type system is the closest thing to the code and, unlike conventional tests that only exercise small sets of parameters, can be used to assert that certain invariants hold true in *all* cases.

  - Separate effectful (state changing, e.g. "writes" to data store, service calls) computations from pure ones (referentially transparent function calls). Pure computations are easy to test with unit tests and easy to reason about in the context of error handling and retry logic.

  - Immutable data structures, especially for concurrent / distributed systems. Simplifies reasoning about the code and leads to fewer defects.

  - Idempotent systems. Much easier to address fail/retry and other corner cases.

  - Unit tests. Close to code, tend to evolve with the code and not be abandoned. Can be hard to add after the fact if the system is composed of tightly coupled components and singletons. Programming against interface and dependency injection help. In scala [Type Classes](http://danielwestheide.com/blog/2013/02/06/the-neophytes-guide-to-scala-part-12-type-classes.html) help as well.

  - [Test in production](Quality-TIP.md) Works well for "mostly read only" systems, mobile, UI.

  - Functional tests with mock dependencies. Can be used to test "glue" code that binds components already tested by unit tests.

  - Functional "sandbox" tests (hit "sandbox" service dependencies). Can be useful to test 3rd party integration.

  - Deploy service into a "sandbox" environment and run functional tests. Can be a good approach for services with relatively few dependencies that can, in turn, be sandboxed or mocked. Can be used to test "read only" services but "test in prod" is probably easier.

  - UI drivers, e.g. Selenium testing. Can work well in some limited set of use cases where data dependencies are simple and flow is mostly linear, e.g. login/registration. Tend to be really brittle in the scenarios that deal with complex or time-sensitive models.
  - Generators. Useful to build up more complex testing scenarios by providing a base for constructing a test scenario.

  - Load test individual service nodes (perhaps only selected URL patterns there that matter to the core experience if a service is not 'micro'). Our core services should be horizontally-scalable by design. Having reviewed design, it should be sufficient for us to load test on a small scale (1 to a few nodes, e.g. dark canary) to catch performance regressions. This has the advantages of being a focused (on a single service) test with a clear ownership and not needing a full peak capacity downstream of the service under test.


## Hold

  - Legacy "stage" environment. A per-team 'stage' full-stack service environment system has been deprecated. It doesn't scale to 'microservice' architecture. We've considered alternatives, like a formal "pre prod" environment but were never able to pull in enough resources to make that a reality. While we want to avoid adding more dependencies on it, we have no choice but to continue to use it for legacy systems tightly coupled with Gilt DB.

  - Daily "full stack" load tests. In theory it's nice to be hitting the site the way users do and checking that all front end apps/services and their transitive dependencies all work. In practice there are a few problems with this: tests need to be coordinated across all teams or they'll interfere with normal capacity planning and auto scaling activities; ownership is a bit fuzzy (people tend to assume the problem is elsewhere in the stack, not in the code that they own); what do results even mean in the presence of auto-scaling?
