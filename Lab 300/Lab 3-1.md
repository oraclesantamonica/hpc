# Run OpenFoam project in HPC cluster

## Introduction 
Setting up of the HPC hardware and deploying application software on top of it can take a long time and include various complex operations. With this demo all these tasks are automated within a OCI marketplace image and a fully working HPC environment along with OpenFoam simulation software is available right after the provisioning. All that is needed is to execute the workload on the fully functional HPC environment and run the visualisation application like Paraview to render the output. The total time to provision and running of workload can be completed in less than a hour.

## Reference architecture
![](images/29_Architecture.png " ")

## Objectives
- Provision a fully functional HPC enviorment using OCI marketplace image
 - Deployment of CFD based simulation application named OpenFoam
 - Execute Openfoam workload 
 - Render the output using Paraview visualization application.
 
 
## Required Artifacts
Before you begin using this lab, make sure to have access to the following:
1. Authorize the Compute service to manage tag namespaces on your behalf using the following policy:
   
   "Allow service compute_management to use tag-namespace in tenancy"
2. Bare Metal shape BM.HPC2.36 only allowed in this Demo. A minimum of 2 compute shapes are needed in the same Availability Domain.
3. VM compute image for bastion server. Does not have to be in the same Availability Domain as HPC nodes.
4. User credentials should have Quota to create or use existing VCN with Private and public subnet.
5. Access to marketplace image "CFD Ready Cluster". The version of marketplace image used in this DEMO is (Version: 20200625). 


###  What is OpenFoam:
   Computational Fluid Dynamics (CFD) is the simulation of fluid motion and heat transfer using numerical analysis. CFD software, such as OpenFOAM, can serve as a powerful tool for many engineering industries to improve designs, visualize impact, and detect inefficiencies in a system. However, the computational power required to process a simulation can be very intensive. Super-computers and HPC clusters are often used to execute large and complex models within a reasonable timeframe. 
   In this lab, you will compute and render the aerodynamic model of a motorcycle using an open-source CFD software, OpenFOAM, within an Oracle HPC cluster 

###  Infrastructure terminology:
   - Worker node: HPC compute instances providing the processing power to execute the workload of computational simulations or other engineering workload. In this Demo compute shape BM.HPC2.36 nodes are the worker nodes.
   - Head node: Compute instance from where all the computational jobs can be scheduled and submitted to be run on Worker nodes. Many times Head node and Bastion node can be the same machine. For this demo we are provisioning Bastion node.
   - GPU node: A specialized compute instance having GPU processors so as to render the output of computational workload. This demo is not provisioning a GPU machine.
   - Bastion node: A compute instance with Public IP, acts as entry point to connect to  worker nodes which are generally in Private network. 
   - RoCE network: RDMA over Converged Ethernet (RoCE) is a network protocol that allows remote direct memory access (RDMA) over an Ethernet network.



## Steps

### **STEP 1: Launch marketplace image**
- Before user launches the marketplace image, check for the Availability domain where 2 or more HPC nodes with shape BM.HPC2.36 are available. Goto Menu -> Governance -> Limits, Quotas and Usage

![](images/01-Service_Limits.png " ")


<p>&nbsp;</p>
<p>&nbsp;</p>

- Goto Menu -> Marketplace -> Applications

![](images/02-Marketplace.png " ")


<p>&nbsp;</p>
<p>&nbsp;</p>

- In the searchbox type "cfd"

![](images/03-Search_marketplace.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>

- Click on the Image, select it and then Click on "Launch Stack" button. Check that you are in the right compartment.

![](images/04-Launch_Stack.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>

- Once the Stack is Launched, fill in the details to create resources including HPC worker nodes, bastion server, Networking components, VCN server. The stack will also ask for inputs to deploy packages like openMPI and openFoam.

Provide name of the Stack below

![](images/05-Create_Stack01.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Select the Availability Domain where you have HPC nodes available for provisioning. Provide the ssh public key that will be needed to connect to Bastion server.

![](images/06-Create_Stack02.png " ")



- Select the Availability Domain to provision Bastion server. It can be differnt from the Availability Domain on HPC nodes. Select Bastion host shape of your choice. Enable the checkbox to install VNC server on Bastion server and provide the password that will be needed to make VNC connection later on.

![](images/07-Create_Stack03.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- For Worker nodes, the only option that is selected is BM.HPC2.36. You will need a minimum of 2 HPC compute shapes to complete the provisioning. Select 2 or more compute nodes in the below screen. Rest all of the options can be left to default.

![](images/08-Create_Stack04.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Under Network options, create a new VCN using the default values passed in the stack. This Demo creates new VCN created via this stack. But there is option to use existing VCN as well.

![](images/09-Create_Stack05.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- For File system section, use the below options that will create NFS mount and install gluster filesystem on top of it.

![](images/10-Create_Stack06.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Click on "INSTALL OPENFOAM" checkbox to install OpenFoam application in this HPC stack.

![](images/11-Create_Stack07.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- On the Last page of the stack verify the details that was entered and if everything is OK then click the "Create" button to start the provisioning of the infrastructure and software installation.

![](images/12-Create_Stack08.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Verify the progress of installation of the HPC stack 

![](images/13-StackJob.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Once the provisioning completes in around 20 minutes time, the logfile should contain the following message. Logs can be found at Menu -> Resource Manager -> Stacks(click your stack name) -> Jobs (Click on the job name link to view the complete log)

![](images/14-StackJobDetails.png " ")


<p>&nbsp;</p>
<p>&nbsp;</p>


### **STEP 2: Validate the setup**

- Click on "Associate Resources" link on the left of the screen to see the infrastructure resources that were provisioned by this stack. Associate resources can be found at Menu -> Resource Manager -> Stacks(click your stack name) -> Jobs (Click on the job name link) -> Associate Resources

![](images/15-Stack_Resources.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>



- Under Compute -> Instances, 3 compute instance will be provisioned, 2 HPC nodes in private network and 1 Bastion server with Public IP address.

![](images/16-Compute_Details.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>



- Connect to the Bastion server using public IP address as opc user using the ssh private key for which a public key was uploaded in the stack above. Use this method to connect from Linux/Mac Desktop. For Windows desktop use Putty and provide ppk private key to make connection.

![](images/17-Bastion_ssh.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>

- Verify the shared NFS storage that is mounted on 2 HPC nodes and the bastion server. This NFS storage contains the OpenFoam software which will be used to run the workload.

![](images/18-Bastion_storage.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- OpenFoam application is hosted under "/mnt/gluster-share/work" folder. Check for the hostfile that contains the priavte IP address of the 2 HPC nodes on which the workload will run.

![](images/19-Bastion_openfoam.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- The below output shows a running VNC server on port 5901. This port will be used to make VNC connection to bastion host.

![](images/20-Bastion_VNCdetails.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Check from the security list of Bastion server under Ingress rules that port 5901 is allowed. In case VNC is running on any other port and if that port is not allowed for ingress traffic in security list, create a new security rule for the same.

![](images/21-SecList_OpenVNC_Port.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>

- Verify the names of the HPC nodes

![](images/22-ListHPCNodes.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


### **STEP 3: Run simulation workload and Render the output**

- On Bastion server, run the following commands that will be needed to render the output using Paraview package.

```
sudo yum install -y mesa-libGLU
cd /mnt/gluster-share
curl -d submit="Download" -d version="v4.4" -d type="binary" -d os="Linux" -d downloadFile="ParaView-4.4.0-Qt4-Linux-64bit.tar.gz" https://www.paraview.org/paraview-downloads/download.php > file.tar.gz
tar -xf file.tar.gz
```

<p>&nbsp;</p>
<p>&nbsp;</p>

- Connect to one of the HPC nodes from bastion server and execute the workload

```
ssh 172.16.1.2
cd /mnt/gluster-share/work/
./Allrun 36
```

```
./Allrun 36
Cleaning /mnt/gluster-share/work case
Mesh Dimensions: (40 16 16)
Cores:36: 6, 6, 1
Running surfaceFeatures on /mnt/gluster-share/work
Running blockMesh on /mnt/gluster-share/work
Running decomposePar on /mnt/gluster-share/work
Running snappyHexMesh
Running patchsummary
Running potentialFoam
Running simpleFoam
Running reconstructParMesh on /mnt/gluster-share/work
Running reconstructPar on /mnt/gluster-share/work
219.95
[opc@inst-quqyz-accurate-swan work]$
```





<p>&nbsp;</p>
<p>&nbsp;</p>

- Once the workload completes successfully, configure VNC client on your machine like this. Provide Public IP of Bastion server and VNC port

![](images/24-VNC_Connection.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- OPTIONALLY, In case you are not allowed to open VNC port 5901 or due to security reason want to make ssh tunnel for this port, use the following command to make ssh tunnel on port 5901 without opening the port in the security list

Create tunnel from your laptop/desktop using the following command from terminal window. Here communication for port 5901 will be made on ssh port 22 and the IP address 150.136.41.3 is the public IP address of bastion server.

```
ssh -L 5901:localhost:5901 -i Dropbox/amar_priv_key -N -f -l opc 150.136.41.3
```

- Do not close the above ssh tunnel terminal window. Now initiate VNC session and this time instead of IP address use "localhost" on port 5901, even though this port is not opened in the security list of the subnet.

![](images/28-ssh_Tunnel.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- Start the Paraview application from within the bastion server

```
cd /mnt/gluster-share/ParaView-4.4.0-Qt4-Linux-64bit/bin/
./paraview
```


- In Paraview application window, File -> Open -> Path "/mnt/gluster-share/work" and select file name motorbike.foam. It will be zero byte file and that should be fine.

![](images/25-Paraview_Menu.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>

- On Left of the window, Under Properties tab, select Mesh Regions to select all the values and then unselect the top values which does not start with motorBike_ prefix. Make sure that all values starting with motorBike_ are selected. Click on the Apply button, some errors will pop up, ignore the error window that pops up to view the rendering of the image in VNC console.

![](images/26-Mesh_Regions.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


- An image like below will be rendered on the screen. Based on some display settings, the image on the screen might look a bit different. 

![](images/27-Image_Rendering.png " ")

<p>&nbsp;</p>
<p>&nbsp;</p>


#### All Done! This completes the demo for running OpenFoam application on HPC compute instances.

<p>&nbsp;</p>

