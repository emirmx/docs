---
title: "Association storage migration time"
url: /refguide/association-storage-migration-time/
weight: 15
---

## Introduction

Mendix allows you to change [association storage type](/refguide/association-storage/) for many-to-one and one-to-one associations and switch between Association Tables (legacy option) or Direct Associations (recommended option). Changing the association storage type in an already deployed app leads to data synchronization which can take significant time depending on the amount of data in the database.

This page contains time measurements for association storage migration that can help you estimate data migration time for your app.

## Scenarios

We performed association storage type migration for associations in the model shown below

{{< figure src="/attachments/refguide/runtime/data-storage/association-storage-migration-time/association-storage-model.png" alt="Test model for association storage migration - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

 The model was used to measure migration time for the following cases:

- **Many-to-one association:** Each of N objects of entity `Many` is associated to one of 10 objects of entity `ToOne`
- **One-to-one association:** Each of N objects of entity `One` is associated to one of N objects of entity `ToOne`
- **10 many-to-one associations in one entity:** Each of N objects of entity `Multiple` has 10 associations to objects of entities `Target01` to `Target10`. This means that there are 10*N association being migrated.

For every database vendor, we ran migration for 3 different values of N. For every such configuration, we executed the migration 10 times and measured the average migration time over those 10 runs.

## Measurements

### PostgreSQL

|Associations per table (N)|Many to one|One to one|Many to one, 10 associations in one entity|
|-|-|-|-|
|20K|0.176 seconds|0.194 seconds|1.08 seconds|
|1M|8.3 seconds|9.2 seconds|38.1 seconds|
|20M|199 seconds|222 seconds|850 seconds|

{{< figure src="/attachments/refguide/runtime/data-storage/association-storage-migration-time/storage-migration-postgres.png" alt="Migration time measurements for PostgreSQL - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

### SQL Server

|Associations per table (N)|Many to one|One to one|Many to one, 10 associations in one entity|
|-|-|-|-|
|20K|0.177 seconds|0.177 seconds|0.94 seconds|
|1M|8.3 seconds|8 seconds|42 seconds|
|20M|183 seconds|184 seconds|1013 seconds|

{{< figure src="/attachments/refguide/runtime/data-storage/association-storage-migration-time/storage-migration-mssql.png" alt="Migration time measurements for SQL Server - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

### MySQL

|Associations per table (N)|Many to one|One to one|Many to one, 10 associations in one entity|
|-|-|-|-|
|20K|0.568 seconds|0.567 seconds|6.492 seconds|
|1M|19.9 seconds|22.8 seconds|295.1 seconds|
|20M|473 seconds|605 seconds|7318 seconds|

{{< figure src="/attachments/refguide/runtime/data-storage/association-storage-migration-time/storage-migration-mysql.png" alt="Migration time measurements for MySQL - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

### SAP HANA

|Associations per table (N)|Many to one|One to one|Many to one, 10 associations in one entity|
|-|-|-|-|
|20K|0.132 seconds|0.148 seconds|1.17 seconds|
|1M|3.2 seconds|3 seconds|18.9 seconds|
|20M|23.1 seconds|27.9 seconds|184.7 seconds|

{{< figure src="/attachments/refguide/runtime/data-storage/association-storage-migration-time/storage-migration-saphana.png" alt="Migration time measurements for SAP HANA - click to enlarge" class="no-border" >}}
(*Click the image to enlarge*)

