
  - [Aster](http://www.asterdata.com)
    - for data warehouse **Adopt**

  - [DynamoDB](http://aws.amazon.com/dynamodb/) **Adopt**

  - [ElastiCache](http://aws.amazon.com/elasticache/) **Adopt**

  - [MongoDB](https://www.mongodb.org/) **Hold**
    Since our move to AWS we prefer to explore AWS-hosted solutions first to minimize maintenance overhead.
    For well structured data please consider RDS.
    For KV document lookups please consider DynamoDB.
    For caching layer please consider ElastiCache.

  - [Postgresql](http://www.postgresql.org/) (preference to [RDS](https://aws.amazon.com/rds/) **Adopt**
  - Schema management for postgresql: [Schema evolution manager](https://github.com/gilt/schema-evolution-manager) **Adopt**

  - [Redis](http://redis.io/)
    - for caching layer **Adopt** (please consider ElastiCache)
    - as a primary data storage **Hold** (Needs to be well understood and managed carefully when used as a primary data store, easier / less risky to try other solutions first)

  - [SOLR/Lucene](http://lucene.apache.org/solr/) **Adopt**


  - [Voldemort](http://www.project-voldemort.com/voldemort/) **Hold**
    Since our move to AWS we prefer to explore AWS-hosted solutions first to minimize maintenance overhead.
    For KV document lookups please consider DynamoDB.


