# Terraform AWS Elasticache

## Examples

Here are some examples of how you can use this module in your inventory structure:

### Redis

```hcl
    module "redis" {
    source                       = "clouddrove/elasticache/aws
    version                      = "0.15.0"
    name                         = "redis"
    environment                  = "test"
    label_order                  = ["environment", "name"]
    engine                       = "redis"
    engine_version               = "5.0.0"
    family                       = "redis5.0"
    port                         = 6379
    node_type                    = "cache.t2.micro"
    subnet_ids                   = ["subnet-xxxxxxx","subnet-xxxxxxx","subnet-xxxxxxx"]
    security_group_ids           = ["sg-xxxxxxxxx"]
    availability_zones           = ["eu-west-1a","eu-west-1b" ]
    auto_minor_version_upgrade   = true
    number_cache_clusters        = 2
   }

```

### Redis Cluster

```hcl
    module "redis-cluster" {
    source                      = "clouddrove/elasticache/aws
    version                     = "0.15.0"
    name                        = "cluster"
    environment                 = "test"
    label_order                 = ["environment","name"]
    cluster_replication_enabled = true
    engine                      = "redis"
    engine_version              = "5.0.0"
    family                      = "redis5.0"
    port                        = 6379
    node_type                   = "cache.t2.micro"
    subnet_ids                  = module.subnets.public_subnet_id
    security_group_ids          = [module.redis-sg.security_group_ids]
    availability_zones          = ["eu-west-1a","eu-west-1b" ]
    auto_minor_version_upgrade  = true
    replicas_per_node_group     = 2
    num_node_groups             = 1
    automatic_failover_enabled  = true
  }
```

### Memcache

```hcl
    module "memcached" {
    source                       = "clouddrove/elasticache/aws
    version                      = "0.15.0"
    name                         = "memcached"
    environment                  = "test"
    label_order                  = ["environment", "name"]
    cluster_enabled              = true
    engine                       = "memcached"
    engine_version               = "1.5.10"
    family                       = "memcached1.5"
    parameter_group_name         = "default.memcached1.5"
    az_mode                      = "cross-az"
    port                         = 11211
    node_type                    = "cache.t2.micro"
    num_cache_nodes              = 2
    subnet_ids                   = ["subnet-xxxxxxx","subnet-xxxxxxx","subnet-xxxxxxx"]
    security_group_ids           = ["sg-xxxxxxxxx"]
    availability_zones           = ["eu-west-1a","eu-west-1b" ]
    }
```

## Inputs

| Name                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Type           | Default                                                     | Required |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ----------------------------------------------------------- | :------: |
| apply_immediately           | Specifies whether any modifications are applied immediately, or during the next maintenance window. Default is false.                                                                                                                                                                                                                                                                                                                                                       | `bool`         | `false`                                                     |    no    |
| at_rest_encryption_enabled  | Enable encryption at rest.                                                                                                                                                                                                                                                                                                                                                                                                                                                  | `bool`         | `true`                                                      |    no    |
| attributes                  | Additional attributes (e.g. `1`).                                                                                                                                                                                                                                                                                                                                                                                                                                           | `list(any)`    | `[]`                                                        |    no    |
| auth_token                  | The password used to access a password protected server. Can be specified only if transit_encryption_enabled = true.                                                                                                                                                                                                                                                                                                                                                        | `string`       | `"gihweisdjhewiuei"`                                        |    no    |
| auto_minor_version_upgrade  | Specifies whether a minor engine upgrades will be applied automatically to the underlying Cache Cluster instances during the maintenance window. Defaults to true.                                                                                                                                                                                                                                                                                                          | `bool`         | `true`                                                      |    no    |
| automatic_failover_enabled  | Specifies whether a read-only replica will be automatically promoted to read/write primary if the existing primary fails. If true, Multi-AZ is enabled for this replication group. If false, Multi-AZ is disabled for this replication group. Must be enabled for Redis (cluster mode enabled) replication groups. Defaults to false.                                                                                                                                       | `bool`         | `false`                                                     |    no    |
| availability_zones          | A list of EC2 availability zones in which the replication group's cache clusters will be created. The order of the availability zones in the list is not important.                                                                                                                                                                                                                                                                                                         | `list(string)` | n/a                                                         |   yes    |
| az_mode                     | (Memcached only) Specifies whether the nodes in this Memcached node group are created in a single Availability Zone or created across multiple Availability Zones in the cluster's region. Valid values for this parameter are single-az or cross-az, default is single-az. If you want to choose cross-az, num_cache_nodes must be greater than 1.                                                                                                                         | `string`       | `"single-az"`                                               |    no    |
| cluster_enabled             | (Memcache only) Enabled or disabled cluster.                                                                                                                                                                                                                                                                                                                                                                                                                                | `bool`         | `false`                                                     |    no    |
| cluster_replication_enabled | (Redis only) Enabled or disabled replication_group for redis cluster.                                                                                                                                                                                                                                                                                                                                                                                                       | `bool`         | `false`                                                     |    no    |
| description                 | Description for the cache subnet group. Defaults to `Managed by Terraform`.                                                                                                                                                                                                                                                                                                                                                                                                 | `string`       | `"Managed by Terraform"`                                    |    no    |
| enable                      | Enable or disable of elasticache                                                                                                                                                                                                                                                                                                                                                                                                                                            | `bool`         | `true`                                                      |    no    |
| engine                      | The name of the cache engine to be used for the clusters in this replication group. e.g. redis.                                                                                                                                                                                                                                                                                                                                                                             | `string`       | `""`                                                        |    no    |
| engine_version              | The version number of the cache engine to be used for the cache clusters in this replication group.                                                                                                                                                                                                                                                                                                                                                                         | `string`       | `""`                                                        |    no    |
| environment                 | Environment (e.g. `prod`, `dev`, `staging`).                                                                                                                                                                                                                                                                                                                                                                                                                                | `string`       | `""`                                                        |    no    |
| extra_tags                  | Additional tags (e.g. map(`BusinessUnit`,`XYZ`).                                                                                                                                                                                                                                                                                                                                                                                                                            | `map(string)`  | `{}`                                                        |    no    |
| family                      | (Required) The family of the ElastiCache parameter group.                                                                                                                                                                                                                                                                                                                                                                                                                   | `string`       | `""`                                                        |    no    |
| kms_key_id                  | The ARN of the key that you wish to use if encrypting at rest. If not supplied, uses service managed encryption. Can be specified only if at_rest_encryption_enabled = true.                                                                                                                                                                                                                                                                                                | `string`       | `""`                                                        |    no    |
| label_order                 | Label order, e.g. `name`,`application`.                                                                                                                                                                                                                                                                                                                                                                                                                                     | `list(any)`    | `[]`                                                        |    no    |
| maintenance_window          | Maintenance window.                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `string`       | `"sun:05:00-sun:06:00"`                                     |    no    |
| managedby                   | ManagedBy, eg 'CloudDrove' or 'AnmolNagpal'.                                                                                                                                                                                                                                                                                                                                                                                                                                | `string`       | `"anmol@clouddrove.com"`                                    |    no    |
| name                        | Name (e.g. `app` or `cluster`).                                                                                                                                                                                                                                                                                                                                                                                                                                             | `string`       | `""`                                                        |    no    |
| node_type                   | The compute and memory capacity of the nodes in the node group.                                                                                                                                                                                                                                                                                                                                                                                                             | `string`       | `"cache.t2.small"`                                          |    no    |
| notification_topic_arn      | An Amazon Resource Name (ARN) of an SNS topic to send ElastiCache notifications to.                                                                                                                                                                                                                                                                                                                                                                                         | `string`       | `""`                                                        |    no    |
| num_cache_nodes             | (Required unless replication_group_id is provided) The initial number of cache nodes that the cache cluster will have. For Redis, this value must be 1. For Memcache, this value must be between 1 and 20. If this number is reduced on subsequent runs, the highest numbered nodes will be removed.                                                                                                                                                                        | `number`       | `1`                                                         |    no    |
| num_node_groups             | Number of Shards (nodes).                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `string`       | `""`                                                        |    no    |
| number_cache_clusters       | (Required for Cluster Mode Disabled) The number of cache clusters (primary and replicas) this replication group will have. If Multi-AZ is enabled, the value of this parameter must be at least 2. Updates will occur before other modifications.                                                                                                                                                                                                                           | `string`       | `""`                                                        |    no    |
| parameter_group_name        | The name of the parameter group to associate with this replication group. If this argument is omitted, the default cache parameter group for the specified engine is used.                                                                                                                                                                                                                                                                                                  | `string`       | `"default.redis5.0"`                                        |    no    |
| port                        | the port number on which each of the cache nodes will accept connections.                                                                                                                                                                                                                                                                                                                                                                                                   | `string`       | `""`                                                        |    no    |
| replicas_per_node_group     | Replicas per Shard.                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `string`       | `""`                                                        |    no    |
| replication_enabled         | (Redis only) Enabled or disabled replication_group for redis standalone instance.                                                                                                                                                                                                                                                                                                                                                                                           | `bool`         | `false`                                                     |    no    |
| replication_group_id        | The replication group identifier This parameter is stored as a lowercase string.                                                                                                                                                                                                                                                                                                                                                                                            | `string`       | `""`                                                        |    no    |
| repository                  | Terraform current module repo                                                                                                                                                                                                                                                                                                                                                                                                                                               | `string`       | `"https://github.com/clouddrove/terraform-aws-elasticache"` |    no    |
| security_group_ids          | One or more VPC security groups associated with the cache cluster.                                                                                                                                                                                                                                                                                                                                                                                                          | `list`         | `[]`                                                        |    no    |
| security_group_names        | A list of cache security group names to associate with this replication group.                                                                                                                                                                                                                                                                                                                                                                                              | `any`          | `null`                                                      |    no    |
| snapshot_arns               | A single-element string list containing an Amazon Resource Name (ARN) of a Redis RDB snapshot file stored in Amazon S3.                                                                                                                                                                                                                                                                                                                                                     | `any`          | `null`                                                      |    no    |
| snapshot_name               | The name of a snapshot from which to restore data into the new node group. Changing the snapshot_name forces a new resource.                                                                                                                                                                                                                                                                                                                                                | `string`       | `""`                                                        |    no    |
| snapshot_retention_limit    | (Redis only) The number of days for which ElastiCache will retain automatic cache cluster snapshots before deleting them. For example, if you set SnapshotRetentionLimit to 5, then a snapshot that was taken today will be retained for 5 days before being deleted. If the value of SnapshotRetentionLimit is set to zero (0), backups are turned off. Please note that setting a snapshot_retention_limit is not supported on cache.t1.micro or cache.t2.\* cache nodes. | `string`       | `"0"`                                                       |    no    |
| snapshot_window             | (Redis only) The daily time range (in UTC) during which ElastiCache will begin taking a daily snapshot of your cache cluster. The minimum snapshot window is a 60 minute period.                                                                                                                                                                                                                                                                                            | `any`          | `null`                                                      |    no    |
| subnet_ids                  | List of VPC Subnet IDs for the cache subnet group.                                                                                                                                                                                                                                                                                                                                                                                                                          | `list`         | `[]`                                                        |    no    |
| transit_encryption_enabled  | Whether to enable encryption in transit.                                                                                                                                                                                                                                                                                                                                                                                                                                    | `bool`         | `true`                                                      |    no    |

## Outputs

| Name               | Description                                  |
| ------------------ | -------------------------------------------- |
| id                 | Redis cluster id.                            |
| memcached_endpoint | Memcached endpoint address.                  |
| port               | Redis port.                                  |
| redis_endpoint     | Redis endpoint address.                      |
| tags               | A mapping of tags to assign to the resource. |
