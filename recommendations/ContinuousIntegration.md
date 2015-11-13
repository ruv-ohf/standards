# Continous Integration

Gilt has adopted a number of continuous integration technologies; most notably Jenkins which is used to power ION-Cannon deployment, Gerrit and the Daily Load Tests. A centralized Jenkins system will continue to be maintained for historical reasons, to support ION Cannon, daily performance tests, and Gerrit. This will owned by INFRA. Engineering teams should NOT add new projects into this centralized repository, but should prefer to adopt per-department CI tech nologies as appropriate. While there continues to be innovation in the CI space, there is a general consensus that we do not wish to invest or innovate in this area; teams should be free to use whatever tool is most appropriate to their needs, with a preference to open-source and low-cost options.

## Adopt
  - Per-department Jenkins instances for closed-source with dependencies on private code within Gilt.

  - Per-department SaaS services (e.g. [Travis](https://travis-ci.org), [CircleCI](https://circleci.com)) for open-source.

## Assess

  - Per-department SaaS for closed-source IF there are no internal dependencies.

## Hold

  - A centralized Jenkins system will continue to be maintained for historical reasons, to support ION Cannon, daily performance tests, and Gerrit. This will owned by INFRA. Engineering teams should NOT add new projects into this centralized repository, but should prefer to adopt per-department CI technologies as appropriate.

  - SaaS CI products should not bused for closed-source with internal / private dependencies.
