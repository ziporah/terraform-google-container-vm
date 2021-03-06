# CoreDNS on Container Optimized OS

This module implements a DNS service running CoreDNS as a container on Container Optimized OS instances.

The following resources are created and managed by this module:

* `google_compute_address`, one reserved IP address per instance
* `google_compute_instance`, one or more COS instances
* `google_compute_firewall`, one firewall rule to allow traffic from specific IP ranges to the DNS port

Variables allow controlling several aspects of the created resurces, like number of instances (`instance_count`), container image used for CoreDNS (`container_image`), client IP ranges allowed to connect to the service(`client_cidrs`), and CoreDNS configuration file (`corefile`).

## Sample Usage

```hcl
module "dns-service" {
  source         = "terraform-google-modules/container-vm/google//modules/cos-coredns"
  instance_count = "1"
  project_id     = "my-project"
  region         = "europe-west3"
  zone           = "europe-west3-c"
  prefix         = "cloud"
  corefile       = "assets/Corefile.cloud"
  vm_tags        = ["ssh"]
  client_cidrs   = ["0.0.0.0/0"]
  network        = "my-network-name"
  subnetwork     = "https://www.googleapis.com/compute/v1/projects/my-project/regions/europe-west3/subnetworks/my-subnetwork"
}
```

[^]: (autogen_docs_start)

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| network | Self link of the VPC subnet to use for firewall rules. | string | n/a | yes |
| project\_id | Project id where the instances will be created. | string | n/a | yes |
| region | Region for external addresses. | string | n/a | yes |
| subnetwork | Self link of the VPC subnet to use for the internal interface. | string | n/a | yes |
| zone | Instance zone. | string | n/a | yes |
| boot\_disk\_size | Size of the boot disk. | string | `"10"` | no |
| client\_cidrs | Client IP CIDR ranges to set in the firewall rule. | list | `<list>` | no |
| container\_image | CoreDNS container version. | string | `"coredns/coredns"` | no |
| corefile | Path to the CoreDNS configuration file to use. | string | `""` | no |
| create\_firewall\_rule | Create tag-based firewall rule. | string | `"false"` | no |
| instance\_count | Number of instances to create. | string | `"1"` | no |
| instance\_type | Instance machine type. | string | `"g1-small"` | no |
| labels | Labels to be attached to the resources | map | `<map>` | no |
| log\_driver | Docker log driver to use for CoreDNS. | string | `"gcplogs"` | no |
| network\_tag | Network tag that identifies the instances. | string | `"coredns"` | no |
| prefix | Prefix to prepend to resource names. | string | `""` | no |
| scopes | Instance scopes. | list | `<list>` | no |
| service\_account | Instance service account. | string | `""` | no |
| stackdriver\_logging | Enable the Stackdriver logging agent. | string | `"true"` | no |
| stackdriver\_monitoring | Enable the Stackdriver monitoring agent. | string | `"true"` | no |
| vm\_tags | Additional network tags for the instances. | list | `<list>` | no |

## Outputs

| Name | Description |
|------|-------------|
| instances | Instance name => address map. |
| internal\_addresses | List of instance internal addresses. |
| names | List of instance names. |

[^]: (autogen_docs_end)
