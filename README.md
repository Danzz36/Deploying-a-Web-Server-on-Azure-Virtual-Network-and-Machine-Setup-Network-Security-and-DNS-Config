# Azure: Create a Virtual Machine and Deploy a Web-Server

## Objective

The objective of this projcet is to learn how to securely deploy and manage resources in Azure. The work will be focused on learning the fundamental networking architecture in Azure by creating Virtual Networks, Subnets, Network Security Groups, and Virtual Machines. I'll also be learning how to use Azure Bastion to securely connect to a Linux machine via SSH, install a web server, and configure it to be accessible via a public IP and custom DNS label. 

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

- Create a Resource Group
  - Log in to the Azure Portal and select "Create a Resource"
  - Search for Resource Group and once found, click create
  - Give it a name in order to find it later. I'll name it RG-USE-Nextcloud. RG-Resource Group, USE-US East Region, Nextcloud-for nextcloud server
  - Click on review+create and then create
  - Navigate to the resouce group
    

- Create a Virtual Network and a subnet
  - Once in the resource group, click on create and search for virtual network
  - Click on it and then click create
  - Make sure the correct resource group is selected under the project details and then give the virtual net a name under instance details. I'll name it VNET-USE-Nextcloud
  - When we create a virtual machine, it's important to remember that it needs to be in the same region ans the virtual network
  - Now click on next and then click the IP addresses tab at the top
  - It will give you a default address space of 10.0.0.0/16, but we want to change that. Replace that address with the address 172.10.0.0/16
  - Next, click add subnet. Once the pane opens, give it a name of SNET-USE-Nextcloud, make sure the proper IPv4 address range is selected (172.10.0.0/16), set the starting address as 172.10.0.0 and then select a size of /24
  - Then click add
  - Then click the review + create button
  - Click create. Once the deployment is complete, click your resource group and make sure it is there. It may take a minute and/or you might need to refresh the page

- Protect a subnet using a Network Security Group
  - Go back to your resource group
  - Click add or create and then search for network security group
  - Click on it and then click create
  - Make sure the resource group is correct and then give it a name under the instance details. I'll use NSG-USE-Nextcloud. Then click review + create and then click create obn the next page
  - Azure will have applied some inbound and outbound rules to it once its completely set up. Now we want to asign this netowrk security group to our subnet
  - Navigate back to the virtual network and then to the subnet
  - Once there, click the subnet and navigate to the network security group drop down in the pane that opens and select the network security group we just created. Now click save.

- Deploy Bastion to connect to a Virtual Machine
  - Since creating a bastion instance can take a little while to set up, I decided to create it now. In order to do this, we need to create a subnet for it first
  - To do this, navigate back to subnets
  - Click on add subnet
  - First, eslect Azure Bastion in the purpose dropdown menue. Then name it. The name of the subnet, which is a requirement, has to be AzureBastionSubnet
  - We'll use 172.10.1.0 for the strating address of the range and change the size to /24
  - We don't select any netowrk security group. Click on save
  - Now its time to create the bastion resource. Navigate back to the resource group and click on create
  - Search for Bastion. Click on it and then click create.
  - Make sure the correct resouce group has already been selected and then name it. We'll name it BASTION-USE-Nextcloud
  - Select the virtual network in the virtual network dropdown
  - Name the Public IP Adress Name as BASTIONIP-USE-Nextcloud and then click on review + create and then create
  - It will take a while to deploy, so we can work on other things until  Azure notifies us that it's done

- Create an Ubuntu Server Virtual Machine
  - Navigate back to the resource group and click on Create
  - Search for Ubuntu Server and use the most recent version. I used Ubuntu Server 22.04 LTS. Then click Create
  - Make sure the correct resource group has been selected
  - Give the virtual machine a name. I'll name it VM-USE-Nextcloud
  - We'll leave the default Availability zone as 1. This is becuase if we want to access the Nextcloud server, we'll need a public IP and the public IP needs to be deployed in the same availability zone as the virtual machine
  - For the size of the VM, click see all sizes. Ideally you would use a basic machine. Something like B1s which has 1 cpu, 1 gb ram, 2 data disks etc., but it isn't available in my region so I went with a DC1s_v2. Its optimized for confidentiality which is not necessary, but has 1 cpu, 4gb ram and 1 data disk and is the cheapest instance I could find.
  - SSH Public Key should be selected on the VM configuration. You can use anything you want for the username. Modify the key pair name to be more descriptive. We'll call it VM-USE-Nextcloud_SSHkey
  - Delect none for public inbound ports. We are going to be connecting to our virtual machine using bastion which is an internal resource
  - CLick on Next: Disks. We can leave the defualt of 30 GiB on the Premium SSD as is.
  - CLick on Next: Netoworking. Your virtual netowrk and subnet should aslready be selected. If not, make sure to choose the appropriate networks.
  - Selec None in the dropdown for Public IP. This is to allow use to learn more later in the process by doing it ourselves.
  - Click on review + create
  - Confirm all the settings are coorect and then click create
  - Click download private key and create resource. This will download the kep pair to your private machine.
  - Wait for the virtual machine to deploy. It will take a few minutes.

- Install Nextcloud by connecting via SSH using Bastion
  - Once your VM has been deployed, you can check on it by going back to your resource group. Click on it and take note of it's Private IP. We will need that later.
  - You will want to start your virtual maching which can be done at the top of the machines details page. Note that if you want to pause the project or end it, make sure to hit the stop button to avoid incurring additional charges.
  - Now its time to connect to it. Click the connect buton in the top tool bar of the machines details page and then click on bastion. Now click on the Use Bastion button
  - Now enter the username that you chose earlier and then select SSH private key from local file, and then find and select the SSH key that was downloaded before on your local machine. Then click on connect.
  - A new tab will open and you will now be connected to your virtual machine using SSH via Bastion
  - Now let's install Nextcloud by typing 'sudo snap install nextcloud'

- Publish an IP

- Create a DNS label



drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
