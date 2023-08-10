# Be a Subnet Validator

This page will guide you to start your own Avalanche Validator through Avalanche-CLI with only
1 command!

:::warning

ALPHA WARNING: This command is currently in experimental mode. Proceed at your own risk.

:::

## Prerequisites

Before we begin, you will need to:

- Create an AWS account and have an AWS `credentials` file in home directory, More info can be
  found [here](https://docs.aws.amazon.com/sdkref/latest/guide/file-format.html#file-format-creds)
- Install [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- Install [Ansible](https://crunchify.com/how-to-install-ansible-on-macos/)


Currently, we are only supporting AWS cloud service, but we plan to add more cloud services in the
near future.

## Starting a Validator

To start an Avalanche validator, run:

```shell
avalanche node create <clusterName>
```

The created node will be part of cluster `clusterName` and all avalanche node commands applied to
cluster `clusterName` will apply to all nodes in the cluster.

:::note

Please note that running a validator on AWS will incur cost.

Ava Labs is not responsible for the cost incurred from starting up Avalanche validator through
Avalanche-CLI

:::

Currently, we have set the following specs of the AWS cloud server to a fixed value, but we are
planning to implement customization feature in the near future:

- Cloud Region: `us-east-2`
- OS Image: `Ubuntu 20.04 LTS (HVM), SSD Volume Type`
- Instance Type: `c5.2xlarge`
- Storage: `1 TB`

The command will ask which Avalanche Go version that you would like to install in the cloud server.
You can choose `default` (which will install the latest available Avalanche Go version) or you can
enter the name of a deployed Subnet in your local machine that you plan to be validated by this
node (we will get the latest Avalanche Go version that is still compatible with the deployed
Subnet's RPC version)

Once the command has successfully completed, Avalanche-CLI outputs the cloud server node ID as well
as the public IP that the node can be reached at.

Avalanche-CLI also outputs the command that you can use to ssh into the cloud server node.

By the end of successful run of `create` command, Avalanche-CLI would have:

- Installed Avalanche Go in cloud server
- Installed Avalanche CLI in cloud server
- Downloaded the `.pem` private key file to access the cloud server into your local `.ssh` 
directory.
  Back up this private key file as you will not be able to ssh into the cloud server node without it
- Downloaded `staker.crt` and `staker.key` files to your local `.avalanche-cli` directory so that
  you can back up your node. More info about node backup can be found [here](/node/maintain/node-backup-and-restore.md)
- Started the process of bootstrapping your new Avalanche node to the Primary Network


Please note that you will have to wait until the node has finished bootstrapping before the node can be a
Primary Network or Subnet Validator. To check whether the node has finished bootstrapping, run
`avalanche node status --bootstrapped`. Once the node is finished bootstrapping, the response will be:

```text
{
    "jsonrpc": "2.0",
    "result": {
        "isBootstrapped": true
    },
    "id": 1
}
```