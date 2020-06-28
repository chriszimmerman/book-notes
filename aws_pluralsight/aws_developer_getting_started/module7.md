# Speeding Up with CloudFront and ElastiCache

## CloudFront

Improve latency - reduce distance between user and app

CloudFront - Global content delivery network designed to reduce latency and reduce application load

Edges "oobjects" and serves them directly

Distributions
- given their own domain
- distributes content from their own origins
- different ones have different behaviors for distributing content

CloudFront Popular Objects Report

## ElastiCache

In-memory cache - ex. Redis and memcached

Takes care of maintenance, upgrades, etc.
Automated read replicas

Cluster is unit - contains nodes

Redis is the industry leader for in-memory caching