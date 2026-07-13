# IBM Concert Operate Command Line Tool
aiopsctl is a tool for installing and managing IBM Concert Operate. By running aiopsctl on a set of Linux virtual machines, you can deploy IBM Concert Operate with just a few commands. For more information, visit the [IBM Documentation](https://www.ibm.com/docs/en/cloud-paks/cloud-pak-aiops/latest?topic=deploying-linux).

## Installation
aiopsctl is available for download from [GitHub releases](https://github.com/IBM/aiopsctl/releases). You can download the latest tar archive, extract the tool binary, and place it on the path of your Linux VMs.

### Download and install on Linux
If your Linux machine has access to the internet, you can use the following commands to download and install the CLI directly.
```sh
curl -LO "https://github.com/IBM/aiopsctl/releases/latest/download/aiopsctl-linux_amd64.tar.gz"
tar xf "aiopsctl-linux_amd64.tar.gz"
mv aiopsctl /usr/local/bin/aiopsctl
```

## Usage
For a complete guide to using aiopsctl along with helper scripts and instructions for air-gapped environments, please visit the [IBM Documentation](https://www.ibm.com/docs/en/cloud-paks/cloud-pak-aiops/latest?topic=deploying-linux).

```
$ aiopsctl --help
This tool allows you to install and manage IBM Concert Operate

Usage:
  aiopsctl [command]

Available Commands:
  bastion       Commands to manage a bastion host for air-gapped environments
  benchmark     Commands for running cluster benchmarks
  cluster       Commands to manage the cluster of nodes for Concert Operate on Linux
  completion    Generate the autocompletion script for the specified shell
  help          Help about any command
  license       View product license
  multi-cluster Commands to manage multi-cluster deployments of Concert Operate
  server        Manages the Concert Operate deployment in an existing cluster
  status        Get the Concert Operate on Linux status
  update        Update aiopsctl in internet-connected environments
  version       Print version information

Flags:
      --accept-license     Accept the license
  -c, --config string      Config file location (default "~/.aiopsctl.yaml")
  -h, --help               help for aiopsctl
  -l, --log-file string    Log filename, set to stdout to print to console
      --show-secrets       Allow the display of secrets in output
  -v, --verbose            Verbose logging
      --wait-timeout int   Wait timeout for services to come ready in minutes (default 180)
  -y, --yes                Assume "yes" as the answer to all prompts and run non-interactively

Use "aiopsctl [command] --help" for more information about a command.
```

---

## Commands

### `bastion`
Commands to manage a bastion host for air-gapped environments.

```
$ aiopsctl bastion --help
When deploying IBM Concert Operate in an air-gapped environment, these commands
can be used to configure the bastion host.

Available Commands:
  login         Login to a container registry
  logout        Logout of a container registry
  mirror-images Mirror Concert Operate on Linux images to a private registry
```

#### `bastion login`
Configure the bastion host with authentication for a container registry.

```
Usage:
  aiopsctl bastion login <registry URL> [flags]

Flags:
      --insecure-skip-tls-verify   Skip TLS verification when using the provided registry
  -p, --password string            The password for authentication with the registry
      --password-stdin             Take the password from stdin
  -u, --username string            The username for authentication with the registry
```

#### `bastion logout`
Remove authentication for a container registry from the bastion host.

```
Usage:
  aiopsctl bastion logout <registry URL> [flags]
```

#### `bastion mirror-images`
Mirror Concert Operate on Linux container images to a private registry for usage in air-gapped installations.

```
Usage:
  aiopsctl bastion mirror-images [flags]

Flags:
      --dry-run                 Initiate a dry-run mirror in which images will not be copied to the registry
      --prefer-bundled-bin oc   Use bundled oc and ibm-pak binaries even if pre-existing local copies are available
      --registry string         The registry target to which all images will be mirrored
```

---

### `benchmark`
Commands for running cluster benchmarks.

```
$ aiopsctl benchmark --help

Available Commands:
  storage     File I/O benchmark
```

#### `benchmark storage`
Run file I/O benchmark tests to evaluate a storage system.

```
Usage:
  aiopsctl benchmark storage [flags]

Flags:
      --job-name string        Name of the benchmark job (default "aiops-storage-benchmark")
      --job-size string        Size of the benchmark job file I/O (default "2Gi")
  -n, --namespace string       Namespace of the benchmark job (default "aiops")
      --node string            Node to evaluate, when empty the node will be selected by the scheduler
  -o, --output string          Output file path for the benchmark logs
      --raw                    Disable output parsing and print the raw log
      --remove                 Remove any benchmark artifacts without running a new evaluation
      --skip-cleanup           Skip artifact deletion after benchmark completion
      --storage-class string   Storage class to evaluate, when empty ephemeral storage will be evaluated (default "local-path")
      --volume-size string     Size of the benchmark volume (default "5Gi")
```

---

### `cluster`
Commands to manage the cluster of nodes for Concert Operate on Linux.

```
$ aiopsctl cluster --help

Available Commands:
  delete-node Forcefully remove a failed node from the Concert Operate on Linux cluster
  node        Manages a single node in the Concert Operate on Linux cluster
  status      Get status of the Concert Operate on Linux cluster
```

#### `cluster delete-node`
Forcefully remove a failed node from the Concert Operate on Linux cluster.

```
Usage:
  aiopsctl cluster delete-node <node name> [flags]
```

#### `cluster node`
Manages a single node in the Concert Operate on Linux cluster.

```
$ aiopsctl cluster node --help

Available Commands:
  down   Uninstall the current Concert Operate on Linux node
  info   Print Concert Operate on Linux node information
  up     Installs/upgrades and runs a new Concert Operate on Linux node on the current system
```

##### `cluster node down`
Uninstall the current Concert Operate on Linux node.

```
Usage:
  aiopsctl cluster node down [flags]

Flags:
      --app-storage string   The path used for application storage on the node
      --role rolesEnum       Specifies the role this node to confirm removal, one of: 'control-plane', 'worker'
```

##### `cluster node info`
Print Concert Operate on Linux node information.

```
Usage:
  aiopsctl cluster node info [flags]

Flags:
      --server-url-only   Only display server url (Can't be used with --token-only)
      --token-only        Only display cluster access token (Can't be used with --server-url-only)
```

##### `cluster node up`
Install, upgrade, and run a worker or control plane node on the current system, ready for use in Concert Operate on Linux.

```
Usage:
  aiopsctl cluster node up [flags]

Flags:
      --app-storage string          The path used for application storage on the node (default "/var/lib/aiops/storage")
      --cluster-cidr string         IPv4 network CIDR to use for pod IPs (default "10.42.0.0/16")
      --cluster-dns string          IPv4 Cluster IP for coredns service, must be within service-cidr range (default "10.43.0.10")
      --ephemeral-storage string    The path used for ephemeral storage on the node (default "<app-storage>/ephemeral")
      --http-proxy string           HTTP proxy URL (e.g., http://your-proxy.example.com:8888)
      --https-proxy string          HTTPS proxy URL (e.g., http://your-proxy.example.com:8888)
      --image-storage string        The path used for container image storage on the node
      --insecure-skip-tls-verify    Configure the node to skip TLS verification when using the provided registry
      --load-balancer-host string   The load balancer hostname to add to the cluster TLS certificate (control-plane only)
      --no-proxy string             Comma-separated list of hosts that should bypass the proxy (e.g., 127.0.0.0/8,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16)
      --offline                     Install and run this node in offline (air-gapped) mode. Requires images to be mirrored with mirror-images.
      --platform-storage string     The path used for platform storage on the node (control-plane only) (default "/var/lib/aiops/platform")
      --pod-log-storage string      The path used for pod log storage on the node
      --registry string             The URL of the registry from which images will be pulled (default "cp.icr.io")
      --registry-token string       The token used for authentication with the registry, typically your IBM Entitlement Key
      --registry-user string        The username for authentication with the registry (default "cp")
      --role rolesEnum              Specifies the role this node should take on, one of: 'control-plane', 'worker'
      --server-url string           The url of the control plane node the node should join to
      --service-cidr string         IPv4 network CIDR to use for service IPs (default "10.43.0.0/16")
      --token string                The token to use to join this node to the cluster
```

#### `cluster status`
Get status of the Concert Operate on Linux cluster.

```
Usage:
  aiopsctl cluster status [flags]
```

---

### `completion`
Generate the autocompletion script for aiopsctl for the specified shell.

```
$ aiopsctl completion --help

Available Commands:
  bash        Generate the autocompletion script for bash
  fish        Generate the autocompletion script for fish
  powershell  Generate the autocompletion script for powershell
  zsh         Generate the autocompletion script for zsh
```

---

### `license`
View product license.

```
Usage:
  aiopsctl license [flags]
```

---

### `multi-cluster`
Commands to manage multi-cluster deployments of Concert Operate, including geo-redundant high availability.

```
$ aiopsctl multi-cluster --help

Available Commands:
  add            Add clusters to a multi-cluster deployment of Concert Operate
  demote         Demote a cluster to standby state in a multi-cluster deployment
  exchange-ca    Exchange certificate authorities across clusters
  exchange-token Exchange access tokens across clusters
  link           Link two clusters for multi-cluster deployment
  list           List configured clusters for multi-cluster deployments
  promote        Promote a cluster to active state in a multi-cluster deployment
  remove         Remove a cluster namespace from a multi-cluster deployment
```

#### `multi-cluster add`
Add a cluster to a multi-cluster deployment of Concert Operate.

```
Usage:
  aiopsctl multi-cluster add <cluster name> <API server URL> [flags]

Flags:
      --insecure-skip-tls-verify   Skip TLS verification when connecting to the cluster
  -n, --namespace string           Namespace for Concert Operate installation (required)
  -p, --password string            Password for accessing the cluster API server
      --role string                Role of the cluster in multi-cluster deployment (Primary or Backup) (required)
      --token string               Bearer token for accessing the cluster API server
  -u, --username string            Username for accessing the cluster API server
```

#### `multi-cluster demote`
Demote a cluster to standby state in a multi-cluster deployment.

```
Usage:
  aiopsctl multi-cluster demote <cluster name> [flags]

Flags:
      --force                      Force the state change without a graceful failover
      --insecure-skip-tls-verify   Skip TLS verification when connecting to the Role Coordinator API
  -n, --namespace string           Namespace for Concert Operate installation (required)
```

#### `multi-cluster exchange-ca`
Exchange certificate authorities across clusters for a multi-cluster deployment. If a cluster has multiple namespaces configured, use `--cluster1-namespace` and `--cluster2-namespace` to specify which namespace to use for each cluster.

```
Usage:
  aiopsctl multi-cluster exchange-ca <cluster 1 name> <cluster 2 name> [flags]

Flags:
      --cluster1-namespace string   Namespace for cluster 1 (required if cluster has multiple namespaces)
      --cluster2-namespace string   Namespace for cluster 2 (required if cluster has multiple namespaces)
```

#### `multi-cluster exchange-token`
Exchange access tokens across clusters for a multi-cluster deployment. If a cluster has multiple namespaces configured, use `--cluster1-namespace` and `--cluster2-namespace` to specify which namespace to use for each cluster.

```
Usage:
  aiopsctl multi-cluster exchange-token <cluster 1 name> <cluster 2 name> [flags]

Flags:
      --cluster1-namespace string   Namespace for cluster 1 (required if cluster has multiple namespaces)
      --cluster2-namespace string   Namespace for cluster 2 (required if cluster has multiple namespaces)
      --lifetime int                Token lifetime in hours (default: 720 = 30 days) (default 720)
```

#### `multi-cluster link`
Link two clusters for multi-cluster deployment. This command performs three operations:
1. Configures the Installation CR in each cluster to enable multi-cluster
2. Exchanges CA certificates between clusters
3. Exchanges access tokens between clusters

If a cluster has multiple namespaces configured, use `--cluster1-namespace` and `--cluster2-namespace` to specify which namespace to use for each cluster.

```
Usage:
  aiopsctl multi-cluster link <cluster 1 name> <cluster 2 name> [flags]

Flags:
      --cluster1-namespace string   Namespace for cluster 1 (required if cluster has multiple namespaces)
      --cluster2-namespace string   Namespace for cluster 2 (required if cluster has multiple namespaces)
      --lifetime int                Token lifetime in hours (default: 720 = 30 days) (default 720)
```

#### `multi-cluster list`
List configured clusters for multi-cluster deployments.

```
Usage:
  aiopsctl multi-cluster list [flags]
```

#### `multi-cluster promote`
Promote a cluster to active state in a multi-cluster deployment.

```
Usage:
  aiopsctl multi-cluster promote <cluster name> [flags]

Flags:
      --force                      Force the state change without a graceful failover
      --insecure-skip-tls-verify   Skip TLS verification when connecting to the Role Coordinator API
  -n, --namespace string           Namespace for Concert Operate installation (required)
```

#### `multi-cluster remove`
Remove a cluster namespace from a multi-cluster deployment of Concert Operate.

```
Usage:
  aiopsctl multi-cluster remove <cluster name> [flags]

Flags:
  -n, --namespace string   Namespace for Concert Operate installation (required if multiple namespaces are configured)
```

---

### `server`
Manages the Concert Operate deployment in an existing cluster.

```
$ aiopsctl server --help

Available Commands:
  custom-certificate Configure Concert Operate on Linux to use provided custom certificate
  info               Display information about the Concert Operate deployment
  precheck           Runs the precheck tool to verify the cluster satisfies the application hardware requirements
  status             Display the status of the Concert Operate on Linux deployment
  up                 Installs the Concert Operate on Linux deployment
  version            Displays the current version of installed Concert Operate on Linux components
```

#### `server custom-certificate`
Configure Concert Operate on Linux to use a provided custom certificate.

```
Usage:
  aiopsctl server custom-certificate [flags]

Flags:
      --certificate-file string   PEM-encoded X.509 certificate chain file
      --key-file string           Key file used to sign certificate
```

#### `server info`
Display information about the Concert Operate deployment.

```
Usage:
  aiopsctl server info [flags]

Flags:
      --password-only   Only display the admin password (Can't be used with --url-only or --username-only)
      --url-only        Only display the console URL (Can't be used with --username-only or --password-only)
      --username-only   Only display the admin username (Can't be used with --url-only or --password-only)
```

#### `server precheck`
Run the precheck tool to verify the cluster satisfies the application hardware requirements.

```
Usage:
  aiopsctl server precheck [flags]

Flags:
      --hybrid-storage     Enable this if you are using hybrid storage to allow the prechecker to determine the appropriate amount of worker nodes
  -m, --multizone          Verify if zones meet the proper hardware requirements
  -n, --namespace string   Namespace where the precheck validation should be run
```

#### `server status`
Display the status of the Concert Operate on Linux deployment.

```
Usage:
  aiopsctl server status [flags]
```

#### `server up`
Install the Concert Operate on Linux deployment.

```
Usage:
  aiopsctl server up [flags]

Flags:
      --certificate-file string     PEM-encoded X.509 certificate chain file
      --key-file string             Key file used to sign certificate
      --load-balancer-host string   The load balancer hostname that will be used to access Concert Operate
      --mode deployMode             Specifies the deployment mode, one of: 'base', 'extended' (default base)
```

#### `server version`
Display the current version of installed Concert Operate on Linux components.

```
Usage:
  aiopsctl server version [flags]
```

---

### `status`
Get the Concert Operate on Linux status.

```
Usage:
  aiopsctl status [flags]
```

---

### `update`
Update aiopsctl in internet-connected environments to the latest available or a specific version.

```
Usage:
  aiopsctl update [flags]

Flags:
      --version string   aiopsctl version to install (vX.Y.Z)
```

---

### `version`
Print version and configuration information.

```
Usage:
  aiopsctl version [flags]
```

---

## Support
To report an issue or get help, please visit [IBM Support](https://www.ibm.com/mysupport/).
