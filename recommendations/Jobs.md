# Jobs

## Adopt

  - Job System (Internal System)
    Legacy job system that can run ruby commands/java jars. Handles scheduling, error notification,

## Assess

  - Sundial (Internal System)
    Scheduled workflow engine leveraging ECS to run arbitrary commands in docker containers.

  - Akka (Cluster)
    Supports cluster singletons without zookeeper, supports cluster sharding.
    Can be a good fit where you already have Akka cluster running for other reasons.

## Hold

  - Curling a service on a schedule from cron to trigger tasks
  - Leader elected scheduled tasks on service nodes
    A few problems with these. For curling, we have the issue of concurrency control, unless you are also handling some global lock amongst your service nodes.
    For both, this means you are also adding load to your service at arbitrary points in time. This can degrade response times for regular traffic unexpectedly.

