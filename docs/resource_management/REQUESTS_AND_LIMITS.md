# Resource requests and limits
To ensure that Kubernetes appropriately schedules resources, the respective product `values.yaml` is configured with default `cpu` and `memory` [resource request values](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

### Resource requests
The default resource requests that are used for each product are defined below. Take note that these values are geared toward small data sets. For larger enterprise deployments refer to the data center infrastructure recommendations [here](https://confluence.atlassian.com/enterprise/data-center-infrastructure-recommendations-972333478.html). Using the [formula](#Memory-request-sizing) below, the `memory` specific values are derived from the default `JVM` requirements defined for each product's Docker container.

| Product  | CPU   |  Memory |
|----------|:-----:|------:|
| [Jira](https://bitbucket.org/atlassian-docker/docker-atlassian-jira/src/master/#markdown-header-memory-heap-size)                    | `2`   | `2G`  |
| [Confluence](https://bitbucket.org/atlassian-docker/docker-atlassian-confluence-server/src/master/#markdown-header-memory-heap-size)   | `2`   | `2G`  |
| [Bitbucket](https://bitbucket.org/atlassian-docker/docker-atlassian-bitbucket-server/src/master/)                                    | `2`   | `2G`  |
| [Crowd](https://bitbucket.org/atlassian-docker/docker-atlassian-crowd/src/master/)                                                   | `2`   | `1G`  |

### Memory request sizing
Request sizing must allow for the size of the product `JVM`. That means the `maximum heap size`, `minumum heap size` and the `reserved code cache size` (if applicable) plus other JVM overheads, must be considered when defining the request `memory` size. As a rule of thumb the formula below can be used to deduce the appropriate request memory size.
```shell
(maxHeap + codeCache) * 1.5
```

### Resource limits
Environmental and hardware constraints are different for each deployment, therefore the product `values.yaml` do not provide a resource [`limit`](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-requests-and-limits-of-pod-and-container) definition. Resource usage limits can be defined by updating the commented out `resources.container.limits` stanza within the appropriate product `values.yaml`.


***
* Go back to [README.md](../../README.md)
* Go back to the [operation guide](../OPERATION.md)
* Go back to [prerequisites](../PREREQUISITES.md)
