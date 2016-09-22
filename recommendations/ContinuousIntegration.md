# Continuous Integration

Gilt has adopted a number of continuous integration technologies; most notably Jenkins which is used to power legacy deployment, Gerrit and load tests. To facilitate a smooth transition, a centralized Jenkins system will continue to be maintained for historical reasons. Engineering teams should NOT add new projects into this centralized repository, but should prefer to adopt per-department CI technologies as appropriate. While there continues to be innovation in the CI space, there is a general consensus that we do not wish to invest or innovate in this area; teams should be free to use whatever tool is most appropriate to their needs.

## Adopt
  - Per-department Jenkins instances for closed-source with dependencies on private code within Gilt.

  - Per-department SaaS services (e.g. [Travis](https://travis-ci.org), [CircleCI](https://circleci.com)) for open-source projects or private repos on github that do not have internal dependencies.

## Hold

  - A centralized Jenkins system will continue to be maintained for historical reasons, to support legacy deployment, daily performance tests, and Gerrit. Engineering teams should NOT add new projects into this centralized repository, but should prefer to adopt per-department CI technologies as appropriate.
