# Deployment

## Adopt

  - [CodeDeploy](http://aws.amazon.com/codedeploy/)
  - [Docker](https://www.docker.com/)

    Docker is largely in use as a replacement for RPMs, to wrap a program in some easily deployable format.
    We believe the ability to specify runtime requirements at development time, and not having to worry about the runtime capabilities of the server is extremely valuable.

## Assess

  - [CodePipeline ](http://aws.amazon.com/codepipeline/)
  - [EC2 Container Service](http://aws.amazon.com/ecs/)
  - [Ionroller](https://github.com/gilt/ionroller/)

    Ionroller is an exploration into 'immutable deploys'. It trades off speed of initial deploy (which involves new machines/elbs/etc being created) with the ability to:
      - test independently in production
      - quickly roll over to a different stack, enabling [Blue Green Deploys](http://martinfowler.com/bliki/BlueGreenDeployment.html)

  - [Mobile Service Tools](https://github.com/gilt/mobile-service-tools) (Closed Source)
  - [sbt-codedeploy](https://github.com/gilt/sbt-codedeploy)

  - [AWS Lambda](http://aws.amazon.com/lambda/)

    A nice alternative to deploying/managing service clusters.
    Consider using in places where functionality is very simple, especially if the traffic is low and it's hard to justify the cost of running EC2 nodes all the time.

## Hold

  - Ioncannon (Closed Source)

    The initial premise of Ioncannon, to deploy to a staging environment, run a bunch of functional tests, and promote to production automatically has had some difficulties.
    Particularly, maintaining said stage environment has proven challenging and not particularly efficient. Other methodologies are being explored in tooling above.
