## About

Terraform module to run ElasticSearch cluster. 

### Params 

* `name` - A unique name of ElasticSearch domain.
* `project` - A project tag.
* `subnets`- A list of VPC subnet IDs for a cluster placement.
* `trusted_cidr_blocks` - A list of trusted external IP. You may provide consumer subnets.

### Optional params with default values

* `node_disk_size` - The allocated storage in gibibytes for each cluster node. (Default is `10`)
* `node_instance_type` - Instance type of cluster node. (Default is `t2.small.elasticsearch`)
* `node_amount` - Amount of cluster nodes. **NB!** the number of servers in a cluster must be a multiple of the number of networks. (Default is `2`)
* `elasticsearch_version` - ElasticSearch version (Default is `6.5`)
* `multi_az` - Availability zones awareness. You should provide only one subnet id in case of disabling the zone awareness. (Default is `true`)

## Usage

Default ElasticSearch cluster (3 `t2.small.elasticsearch` nodes):

```hcl
module "example_es" {
  source              = "github.com/jetbrains-infra/terraform-aws-elasticsearch"
  project             = "FooBar"
  name                = "example"
  db_subnets          = [aws_subnet.private_subnet_1.id, aws_subnet.private_subnet_2.id]
  trusted_cidr_blocks = [
    aws_subnet.public_subnet_1.cidr_block,
    aws_subnet.public_subnet_2.cidr_block
  ]
}
```

## Outputs

* `endpoint` -"Domain-specific endpoint used to submit index, search, and data upload requests."
* `kibana_endpoint` - Domain-specific endpoint for kibana without https scheme."
* `domain_id` - Unique identifier for the domain."
* `domain_name` - The name of the Elasticsearch domain."
