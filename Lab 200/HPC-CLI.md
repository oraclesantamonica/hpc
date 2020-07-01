# ocihpc - Oracle Cloud Infrastructure HPC deployment tool

## Introduction 

`ocihpc` is a tool for simplifying deployments of HPC applications in Oracle Cloud Infrastructure (OCI).
The HPC Command Line Interface (CLI) enables you to use a single command to deploy HPC clusters of any size with our dedicated bare metal HPC compute instances and Mellanox RDMA networking. You don’t need to understand Terraform or Oracle Cloud Infrastructure Resource Manager to use the HPC CLI. It’s built on top of the core Oracle Cloud Infrastructure CLI, and includes prebuilt Terraform scripts.

Benefits of using HPC CLI include:

1. Fast Launch a complete compute cluster with RDMA over converged Ethernet (RoCE) v2 quickly through a single command.
2. Easy to Use No knowledge of Terraform or Oracle Cloud Infrastructure Resource Manager required to launch a basic cluster network.
3. Supports Message Passing Interface (MPI) Deployment includes a complete set of software packages for running parallel processing with RDMA, including Mellanox OFED with Open MPI, Intel MPI, and Platform MPI.
4. Customizable You can execute your own terraform scripts or add to the existing scripts provided with the tool to install your own applications.

## Required Artifacts

### Policies

The OCI user account you use in `ocihpc` should have the necessary policies configured for OCI Resource Manager. Please check [this link](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Tasks/managingstacksandjobs.htm) for information on required policies.

### Installing ocihpc

#### Installing ocihpc on macOS/Linux

1. Download the latest release with the following command and extract it:
```sh
curl -LO https://github.com/oracle-quickstart/oci-ocihpc/releases/download/v1.0.0/ocihpc_v1.0.0_darwin_x86_64.tar.gz
```

2. Make the ocihpc binary executable.
```sh
chmod +x ./ocihpc 
```

3. Move the ocihpc binary to your PATH.
```sh
sudo mv ./ocihpc /usr/local/bin/ocihpc 
```

4. Test that it works.
```sh
ocihpc version 
```

#### Installing ocihpc on Windows


1. Download the latest release from [this link](https://github.com/oracle-quickstart/oci-ocihpc/releases/download/v1.0.0/ocihpc_v1.0.0_windows_x86_64.zip) and extract it.

2. Add the ocihpc binary to your PATH.

3. Test that it works.
```sh
ocihpc.exe version 
```

### Creating an ssh keypair

#### Creating an SSH Key Pair on the Command Line
Please refer to [this link](https://docs.cloud.oracle.com/en-us/iaas/Content/GSG/Tasks/creatingkeys.htm) if you are using windows.

1. Open a shell or terminal for entering the commands.


2. Navigate to your .oci folder
```sh
cd Users/enjli/.ssh
```

3. If you haven't already created the folder, create a .ssh directory to store the credentials:
```sh
mkdir Users/enjli/.ssh
```

3. If you don't see any id_rsa key pairs in the folder, enter the following command and provide a name and passphrase when prompted. The keys will be created with the default values: RSA keys of 2048 bits.

```sh 
ssh-keygen
``` 



### Generate an API Signing Key
Your API requests will be signed with your private key, and Oracle will use the public key to verify the authenticity of the request.
Please refer to [this link](https://docs.cloud.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm#Other) if you are using windows.


1. If you haven't already, create a .oci directory to store the credentials:
```sh
mkdir Users/enjli/.oci
```

2. Generate the private key with  following commands.
```sh
openssl genrsa -out ~/.oci/oci_api_key.pem 2048
```
3. Ensure that only you can read the private key file:
```sh
chmod go-rwx ~/.oci/oci_api_key.pem
```

4. Generate the public key:
```sh
openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
```

### Add public key to Oracle Cloud Infrastructure

Now that you have a private / public key combo , you must add it to OCI Console under user setting:

1. Navigate to your .oci folder containing **oci_api_key_public.pem**. Copy the public key.

2. Login to your OCI console and click on Menu and select Identity and Users. Select a User and navigate to User Detail page.

3. Click on Add Public Key under API Keys section.

![](./ResourcesMenu.png " ")

![](./APIKeys.png " ")

- Paste Public key which you copied from CLI in Add Public Key

![](./AddPublicKey.png " ")


## Using ocihpc

### 1 - Configure
This step describes the required configuration for the CLI and includes optional configurations that enable you to extend CLI functionality.

Before using the CLI, you have to create a config file that contains the required credentials for working with your Oracle Cloud Infrastructure account. You can create this file using a setup dialog or manually, using a text editor and store it in your .oci folder.

An example config file would look like this:

![](./oci_config.png " ")


### 2 - List
You can get the list of available stacks by running `ocihpc list`.

Example:

```sh
$ ocihpc list

List of available stacks:

ClusterNetwork
Gromacs
OpenFOAM
```

### 3 - Initialize
Create a folder that you will use as the deployment source.

IMPORTANT: Use a different folder per stack. Do not initialize more than one stack in the same folder. Otherwise, the tool will overwrite the previous one.

Change to that folder and run `ocihpc init <stack name>`. `ocihpc` will download the necessary files to that folder.


```
$ mkdir ocihpc-test
$ cd ocihpc-test
$ ocihpc init --stack ClusterNetwork

Downloading stack: ClusterNetwork

ClusterNetwork downloaded to /Users/enjli/ocihpc-test/

IMPORTANT: Edit the contents of the /Users/enjli/ocihpc-test/config.json file before running ocihpc deploy command
```

### 4 - Deploy
Before deploying, you need to change the values in `config.json` file. The variables depend on the stack you deploy. An example `config.json` for Cluster Network would look like this:

```json
{
  "variables": {
    "ad": "kWVD:PHX-AD-1",
    "bastion_ad": "kWVD:PHX-AD-2",
    "bastion_shape": "VM.Standard2.1",
    "node_count": "2",
    "ssh_key": "ssh-rsa AAAAB3NzaC1yc2EAAAA......W6 enjli@enjli-mac"
  }
}
```


After you change the values in `config.json`, you can deploy the stack with `ocihpc deploy <arguments>`. This command will create a Stack on Oracle Cloud Resource Manager and deploy the stack using it.

For supported stacks, you can set the number of nodes you want to deploy by adding it to the `ocihpc deploy` command. If the stack does not support it or if you don't provide a value, the tool will deploy with the default numbers. 

For example, the following command will deploy a Cluster Network with 5 nodes:

```
$ ocihpc deploy --stack ClusterNetwork --node-count 5 --region us-ashburn-1 --compartment-id ocid1.compartment.oc1..6zvhnus3q
```

INFO: The tool will generate a deployment name that consists of `<stack name>-<current directory>-<random-number>`.

Example:

```
$ ocihpc deploy --stack ClusterNetwork --node-count 5 --region us-ashburn-1 --compartment-id ocid1.compartment.oc1..6zvhnus3q

Deploying ClusterNetwork-ocihpc-test-7355 [0min 0sec]
Deploying ClusterNetwork-ocihpc-test-7355 [0min 17sec]
Deploying ClusterNetwork-ocihpc-test-7355 [0min 35sec]
...
```

### 5 - Connect
When deployment is completed, you will see the the bastion/headnode IP that you can connect to:

```
Successfully deployed ClusterNetwork-ocihpc-test-7355

You can connect to your head node using the command: ssh opc@$123.221.10.8 -i <location of the private key you used>
```

You can also get the connection details by running `ocihpc get ip` command.

### 5 - Delete
When you are done with your deployment, you can delete it by changing to the stack folder and running `ocihpc delete --stack <stack name>`.

Example:
```
$ ocihpc delete --stack ClusterNetwork

Deleting ClusterNetwork-ocihpc-test-7355 [0min 0sec]
Deleting ClusterNetwork-ocihpc-test-7355 [0min 17sec]
Deleting ClusterNetwork-ocihpc-test-7355 [0min 35sec]
...

Succesfully deleted ClusterNetwork-ocihpc-test-7355
```
