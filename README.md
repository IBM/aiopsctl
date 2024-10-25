# IBM Cloud Pak for AIOps Command Line Tool
aiopsctl is a tool for installing and managing IBM Cloud Pak for AIOps. By running aiopsctl on a set of Linux virtual machines, you can deploy IBM Cloud Pak for AIOps with just a few commands. For more information, visit the [IBM Documentation](https://www.ibm.com/docs/en/cloud-paks/cloud-pak-aiops/latest?topic=deploying-linux).

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

```sh
$ aiopsctl --help
This tool allows you to install and manage IBM Cloud Pak for AIOps

Usage:
  aiopsctl [command]

Available Commands:
  cluster     Commands to manage the cluster of nodes
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  license     View product license
  mustgather  Run the AIOps MustGather script to collect server information for support
  server      Manages the AIOps deployment in an existing cluster
  status      Get cluster and AIOps installation status
  update      Update aiopsctl in internet-connected environments
  version     Print version information

Flags:
      --accept-license     Accept the license
  -c, --config string      Config file location (default "~/.aiopsctl.yaml")
  -h, --help               help for aiopsctl
  -l, --log-file string    Log filename, set to stdout to print to console
      --show-secrets       Allow the display of secrets in output
  -v, --verbose            Verbose logging
      --wait-timeout int   Wait timeout for services to come ready in minutes (default 180)

Use "aiopsctl [command] --help" for more information about a command.
```

## Support
To report an issue or get help, please visit [IBM Support](https://www.ibm.com/mysupport/).
