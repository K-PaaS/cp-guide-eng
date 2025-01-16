### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](https://github.com/K-PaaS/cp-guide-eng/blob/master/use-guide/Readme.md) > [Terraman Use Guide](../cp-terraman-guide.md) > Terraman Deployment Guide

<br>

## Table of Contents

1. [Documentation overview](#1)    
   1.1. [Purpose](#1.1)   
   1.2. [Scope](#1.2)  
   1.3. [References](#1.3)  
2. [Prerequisite](#2)   
   2.1. [About firewalls](#2.1)  
3. [Cluster deployment process](#3)  
4. [Cloud Accounts](#4)   
   4.1. [Register Cloud Accounts](#4.1)  
　 4.1.1. [OpenStack](#4.1.1)     
　 4.1.2. [AWS](#4.1.2)    
　 4.1.3. [NHN](#4.1.3)  
5. [SSH Keys](#5)  
   5.1. [Register SSH Keys](#5.1)  
　 5.1.1. [Register an existing SSH key](#5.1.1)  
　 5.1.2. [Register a new SSH key](#5.1.2)  
6. [Instance Code Template](#6)  
  6.1. [Instance Code Template Example](#6.1)  
　 6.1.1. [OpenStack](#6.1.1)     
　 6.1.2. [AWS](#6.1.2)     
　 6.1.3. [NHN](#6.1.3)    
  6.2. [Register an instance code template](#6.2)  
7. [Clusters](#7)   
  7.1. [Create Clusters](#7.1)  

<br>

## <div id='1'> 1. Documentation overview

### <div id='1.1'> 1.1. Purpose
This document (Terraman Deployment Guide) is intended to describe the Hashicorp Configuration Language (HCL) syntax for each IaaS to create subclusters using **OpenTofu**, and to help users write Infrastructure as Code (IaC) code to deploy subclusters.

|Clusters|Explained|
|---|---|
|Host Cluster|The Kubernetes main cluster on a container platform|
|Sub Cluster |A newly created or registered managed cluster through the Container Platform portal|.


### <div id='1.2'> 1.2. Scope
It is based on deploying a Kubernetes Cluster.


### <div id='1.3'> 1.3. References
**OpenStack** 
> [OpenStack Provider](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs) <br>

**AWS**
> [AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) <br>

**NHN**
> [NHN Terraform Use Guide](https://docs.nhncloud.com/en/Compute/Instance/en/terraform-guide/)<br>
> [OpenStack Provider](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs) <br>


<br>

## <div id='2'> 2. Prerequisite
K-PaaS container platform cluster, portal deployment should be done in advance.
> [[K-PaaS Container Platform Cluster Installation Guide]](../../../install-guide/standalone/cp-cluster-install-single.md) <br>
> [[K-PaaS Container Platform Portal Deployment Guide]](../../../install-guide/portal/cp-portal-standalone-guide.md)

### <div id='2.1'> 2.1 About firewalls
- Master Node

|Protocols|Ports|Remarks|
|---|------|---|
|tcp|22|ssh|
|TCP|6443|Kubernetes API Server|
- Open IaaS-specific API firewalls
  (Example: OpenStack API - 8000, 8774, 5000, 9292, 9876, 9696, 8004, 8780, 8776)
- Instances created on each IaaS must have ports open for remote access.

<br>

## <div id='3'> 3. Cluster deployment process
The `SSH Key` method of cluster deployment via Terraman utilizes an Instance Code Template to deploy sub-clusters through the setup of Cloud Accounts and SSH Keys.
1. [Register Cloud Accounts](#4)
2. [Register SSH Keys](#5)
3. [Register Instance Code Template or utilize default IaC code](#6)
4. [Deploy Clusters](#7)


<img src="../../images/terraman/IMG_3.png">


<br>
<br>

## <div id='4'> 4. Cloud Accounts
### <div id='4.1'> 4.1 Cloud Account Registration
Register your cloud account information in the Container Platform portal menu Global > Cloud Accounts.

#### <div id='4.1.1'> 4.1.1 OpenStack
Fill in the OpenStack Cloud information in the fields below.
- OpenStack supports client environment scripts called RC files, which can be downloaded by clicking Account in the top right corner of the OpenStack dashboard.

|Topic|About OpenStack Cloud| Where to Find Information|
|:------:|:------:|:------:|
|auth_url field|OS_AUTH_URL|OpenStack RC file reference|
|password field|Account password|Account password|
|user_name field|Account username|Account username|
|project field|OS_PROJECT_ID|see OpenStack RC file|
|region field|OS_REGION_NAME|see OpenStack RC file|


<img src="../../images/terraman/IMG_4_1_1.png">


#### <div id='4.1.2'> 4.1.2 AWS
Enter your AWS Cloud information in the fields below.
- You can get your AWS credentials from the IAM dashboard. (If you lose it, you'll need to re-issue it)

|Topics|AWS Cloud Information|Information Locator|
|:------:|:------:|:------:|
|accessKey field|Access Key ID|Issued from the IAM Dashboard|
|secretKey field|Secret Access Key|Issued from IAM Dashboard|
|region field|Enabled regions|Verify your account information[(Refer to the region name guide)](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions)|


<img src="../../images/terraman/IMG_4_1_2.png">

#### <div id='4.1.3'> 4.1.3 NHN
Enter your NHN Cloud information in the fields below.
- You can check the authentication information of NHN Cloud on the Compute > Instance page.

|Topic|About NHN Cloud|Where to find information|
|:------:|:------:|:------:|
|auth_url field|Identity service (identity)|Instance page > API endpoint settings > Identity service (identity)|
|password field|Account password|Account password|Account password|
|user_name field|Account name|Account name|
|project field|Tenant ID|Instance page > API endpoint settings > Tenant ID|  
|region field|Used regions|Check account information[(Refer to the region name guide)](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/s3-api-guide/#signature)| 


<img src="../../images/terraman/IMG_4_1_3.png">


<br>
<br>


## <div id='5'> 5. SSH Keys
### <div id='5.1'>  5.1. Register SSH Keys
SSH key is required to access newly created instances through the container platform Terraman. <br>
Register an SSH key in the Container Platform portal menu Global > SSH Keys.

#### <div id='5.1.1'> 5.1.1. Register an existing SSH key
If you own a private key issued by that cloud service, register it in the SSH Keys menu.
- Enter the SSH Key name as the name you want to use.

<img src="../../images/terraman/IMG_5_1_1_1.png">



#### <div id='5.1.2'> 5.1.2. Register a new SSH key
When you issue a new SSH key, register the public key with the cloud service and the private key in the SSH Keys menu of the Container Platform portal.

```bash
# Generate an rsa key pair with ssh-keygen
$ ssh-keygen -t rsa -m PEM -N '' -f $HOME/.ssh/id_rsa
Generating public/private rsa key pair.
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:odWdv3PDIEpkPuoS53yM0hrsEQZL4mHvM0KwLK2uC57 ubuntu@cp-master
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|         . . .   |
|.+ o    = . o    |
|++= o  * .   .   |
|oo+o .. S . . .  |
|.+..o  o o . + . |
|o +o O. .     *  |
|=o.o=o*        o |
|E++o ++          |
+----[SHA256]-----+
```
```bash
# Confirm RSA key pair generation
$ ls .ssh/
id_rsa  id_rsa.pub

# Lookup public key: id_rsa.pub
$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCjlIHPM3Nnh+6CK3klnD0l4epC2RG...

# Private key: ID_RSA lookup
$ cat .ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAo5SBzzNzZ4fugit5JZw9JeHqQtkRhTJ8Zdl3CCVKdIWfOI79...
-----END RSA PRIVATE KEY-----
```


##### Example) Register a Cloud Platform (OpenStack) public key (id_rsa.pub)
<img src="../../images/terraman/IMG_5_1_1_2.png">


##### Example) Register Container Platform Portal SSH Keys private key (id_rsa)
<img src="../../images/terraman/IMG_5_1_1_1.png">

<br>

## <div id='6'> 6. Instance Code Template
> By default, there is one Code Template registered for each CSP that is required to create an instance. If you need additional code templates, you can register new ones and use them when creating a cluster.

### <div id='6.1'> 6.1 Instance Code Template Example
#### <div id='6.1.1'> 6.1.1 OpenStack
This section describes how to create an instance in OpenStack using Terraman. The underlying IaC code is geared toward creating instances, and the *network, keyfair, security groups, etc.* utilize information already created in the IaaS. Therefore, other resources other than instances must be pre-created.
> [Note variables when writing OpenStack IaC code](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs#configuration-reference)
- When creating an instance, you must specify the names "master" and "worker".  
*Example*
  + When creating a single instance
    - resource "openstack_compute_instance_v2" "master" {...}
  + When creating n instances
    - resource "openstack_compute_instance_v2" "master" {...}
    - resource "openstack_compute_instance_v2" "worker1" {...}
    - resource "openstack_compute_instance_v2" "worker2" {...}
    - resource "openstack_compute_instance_v2" "worker3" {...} ...

<details>
<summary> :page_facing_up: <b>OpenStack Instance Code Template</b> </summary>
<div markdown="1">

```
# Available OpenStack images
data "openstack_images_image_v2" "ubuntu" {
  name = "ubuntu-20.04"                                                                 # Image name
}

# Available OpenStack keyPairs
data "openstack_compute_keypair_v2" "cp-keypair" {
  name = "passta-cp-opentofu-keypair"	                                                # keyPair name
}

# Available OpenStack floating_ip addresses (you'll need one for each instance)
data "openstack_networking_floatingip_v2" "cp-floatingip-master" {
  address = "x.x.x.x"        			                                        # Floating IP addresses
}

# Available OpenStack floating_ip addresses (you'll need one for each instance)
data "openstack_networking_floatingip_v2" "cp-floatingip-worker" {
  address = "x.x.x.x"        			                                        # Floating IP addresses
}

# Available OpenStack networks
data "openstack_networking_network_v2" "cp-network" {
  name = "cp-network"			                                                # network name
}

# Available OpenStack subnets
data "openstack_networking_subnet_v2" "cp-subnet" {
  name = "cp-subnet"				                                        # subnet name
}

# Available OpenStack routers
data "openstack_networking_router_v2" "ext_route" {
  name = "ext_route"					                                # router name
}

# Available OpenStack security_groups
data "openstack_networking_secgroup_v2" "cp-secgroup" {
  name = "cp-secgroup"			                                                # security group name
}

# Manage V2 router interface resources within OpenStack.
resource "openstack_networking_router_interface_v2" "cp-router-interface" {
  router_id = data.openstack_networking_router_v2.ext_route.id				# The ID of the router to which this interface belongs
  subnet_id = data.openstack_networking_subnet_v2.cp-subnet.id		                # The ID of the subnet to which this interface is connected
}

# Managing V2 VM instance resources within OpenStack
resource "openstack_compute_instance_v2" "opentofu-master-node" {                       # When creating instances, be sure to separate them into "master" and "worker" names
  name              = "opentofu-master-node"				                # Unique names for resources
  flavor_id         = "m1.large"							# Flavor ID of the desired flavor for the server
  key_pair          = data.openstack_compute_keypair_v2.cp-keypair.id		        # The name of the key pair to put on the server
  security_groups   = [data.openstack_networking_secgroup_v2.cp-secgroup.id]   	        # An array of one or more security group names to associate with the server.
  availability_zone = "octavia"								# Availability zone to create the server in
  region            = "RegionOne"							# Region to create server instances in

  # Block area
  block_device {
    uuid                  = data.openstack_images_image_v2.ubuntu.id		        # UUID of the image, volume, or snapshot
    source_type           = "image"					                # The source type of the device. One of "", "image", "volume", or "snapshot".
    volume_size           = 80							        # Size of the volume to create (GB)
    boot_index            = 0								# Boot index of the volume
    destination_type      = "volume"				                        # Types created
    delete_on_termination = true						        # Whether to delete volume/block devices on instance shutdown, default false
  }

  # network zone
  network {
    uuid = data.openstack_networking_network_v2.cp-network.id			        # The network UUID to connect to the server
  }
}

resource "openstack_compute_instance_v2" "opentofu-worker-node" {                       # When creating two or more instances, be sure to separate them into "master" and "worker" names
  name              = "opentofu-worker-node"
  flavor_id         = "m1.large"
  key_pair          = data.openstack_compute_keypair_v2.cp-keypair.id
  security_groups   = [data.openstack_networking_secgroup_v2.cp-secgroup.id]
  availability_zone = "octavia"
  region            = "RegionOne"

  block_device {
    uuid                  = data.openstack_images_image_v2.ubuntu.id
    source_type           = "image"
    volume_size           = 80
    boot_index            = 0
    destination_type      = "volume"
    delete_on_termination = true
  }

  network {
    uuid = data.openstack_networking_network_v2.cp-network.id
  }
}

# Floating IP zones
resource "openstack_compute_floatingip_associate_v2" "fip_1" {
  floating_ip = data.openstack_networking_floatingip_v2.cp-floatingip-master.address	# Floating IP to connect to
  instance_id = "${openstack_compute_instance_v2.opentofu-master-node.id}"		# Instances to connect floating IPs to
  wait_until_associated = true					                        # Set this option to have OpenTofu poll instances until a floating IP is connected if the OpenStack environment does not automatically wait for the connection to complete, default false
}

resource "openstack_compute_floatingip_associate_v2" "fip_2" {
  floating_ip = data.openstack_networking_floatingip_v2.cp-floatingip-worker.address
  instance_id = "${openstack_compute_instance_v2.opentofu-worker-node.id}"
  wait_until_associated = true
}
```

</div>
</details>

#### <div id='6.1.2'> 6.1.2 AWS
This section describes how to create an instance on AWS using Terraman. The basic IaC code is tailored to a template that utilizes an already registered keyfair and OS image. Therefore, the keypair and OS image must be pre-created.
> [Variable notes when writing AWS IaC code](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#aws-configuration-reference)
- When creating an instance, you must specify the names "master" and "worker". 
*Example*
  + When creating a single instance
    - resource "aws_instance" "master" {...}
  + When creating n instances
    - resource "aws_instance" "master" {...}
    - resource "aws_instance" "worker1" {...}
    - resource "aws_instance" "worker2" {...}
    - resource "aws_instance" "worker3" {...} ...

<details>
<summary> :page_facing_up: <b>AWS Instance Code Template</b></summary>
<div markdown="2">

```
# Provide information about key pairs
data "aws_key_pair" "default_key" {
  key_name = "cluster-name-key"		                                                # Key pair name
}

# Provide the ID of the registered AMI to use in the resource
data "aws_ami" "ubuntu" {
	most_recent = true							        # Use the most recent AMI if more than one result is returned
	filter {									# One or more name/value pairs to filter
		name = "name"								# The name of the filter by field you defined in the native AWS API
		values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]	# The set of allowed values for a given field
	}
	filter {
		name = "virtualization-type"
		values = ["hvm"]
	}
	owners = ["099720109477"]				                        # List of AMI owners to limit searches to
}

# VPC resource area
resource "aws_vpc" "cp-opentofu-vpc" {
	cidr_block = "172.10.0.0/20"					                # IPv4 CIDR blocks for VPCs
	tags = { Name = "cp-opentofu-vpc" }		                                # Map of tags to assign to resources
}

# VPC Subnet Resource Zones
resource "aws_subnet" "cp-opentofu-subnet01" {
  vpc_id = "${aws_vpc.aws-vpc.id}"					                # VPC ID
  cidr_block = "172.10.0.0/24"						                # IPv4 CIDR block for a subnet
  availability_zone = "ap-northeast-2a"				                        # AZs on a subnet
  tags = { Name = "cp-opentofu-subnet01" }	                                        # Map of tags to assign to resources
}
resource "aws_subnet" "cp-opentofu-subnet02" {
  vpc_id = "${aws_vpc.aws-vpc.id}"
  cidr_block = "172.10.1.0/24"
  availability_zone = "ap-northeast-2a"
  tags = {
    Name = "cp-opentofu-subnet02"
  }
}

# Security Groups resource area
resource "aws_security_group" "cp-opentofu-sg-all" {
  name = "cp-opentofu-sg-all"				                                # Name of the security group
  description = "Allow all inbound traffic"			                        # Security group descriptions
  vpc_id = "${aws_vpc.aws-vpc.id}"	                                                # VPC ID

  ingress {							                        # Building blocks for incoming rules
	from_port = 0					                                # Startup port
	to_port = 0						                        # End range port
	protocol = "-1"					                                # Protocols
	cidr_blocks = ["0.0.0.0/0"]		                                        # CIDR Block Sets
  }

  egress {							                        # Building blocks for egress rules
	from_port = 0
	to_port = 0
	protocol = "-1"
	cidr_blocks = ["0.0.0.0/0"]
  }
}

# Instance Resource Area
resource "aws_instance" "master" {                                                      # When creating two or more instances, be sure to separate them into "master" and "worker" names
  ami = "${data.aws_ami.ubuntu.id}"							# The AMI to use for the instance
  instance_type = "t3.medium"								# Instance type to use for instances
  key_name = data.aws_key_pair.default_key.key_name					# Key name of the key pair to use for the instance
  subnet_id = "${aws_subnet.cp-opentofu-subnet01.id}"		                        # VPC subnet ID to start
  vpc_security_group_ids = [								# Security group ID to connect to
	"${aws_security_group.cp-opentofu-sg-all.id}"
  ]
  associate_public_ip_address = true			                                # Whether to associate a public IP address with an instance in a VPC
  tags = {										# Map of tags to assign to resources
	Name = "cp-opentofu-m"
  }
  provisioner "remote-exec" {				                                # Ability to run scripts locally or remotely when creating or removing resources with OpenTofu
	connection {							                # Connection Block
	  type = "ssh"						                        # Connection types
	  host = "${self.public_ip}"			                                # Address of the resource to connect to
	  user = "ubuntu"								# The user to use for the connection
	  private_key = file("~/.ssh/cluster-name-key.pem")		                # Contents of the SSH key to use for the connection
	  timeout = "1m"								# Timeout waiting for a connection to become available
	}
	inline = [
	  "cat .ssh/authorized_keys"							# Run commands
	]
  }
}

resource "aws_instance" "worker" {                                                      # When creating two or more instances, be sure to separate them into "master" and "worker" names                
  ami = "${data.aws_ami.ubuntu.id}"
  instance_type = "t3.medium"
  key_name = data.aws_key_pair.default_key.key_name
  subnet_id = "${aws_subnet.cp-opentofu-subnet01.id}"
  vpc_security_group_ids = [
    "${aws_security_group.cp-opentofu-sg-all.id}"
    #data.aws_security_group.default.id
  ]
  associate_public_ip_address = true
  tags = {
    Name = "cp-opentofu-w"
  }
  provisioner "remote-exec" {
    connection {
        type = "ssh"
        host = "${self.public_ip}"
        user = "ubuntu"
        private_key = file("~/.ssh/cluster-name-key.pem")
        timeout = "1m"
    }
    inline = [
      "cat .ssh/authorized_keys"
    ]
  }
}

# Resource area for managing the default routing table for a VPC
resource "aws_default_route_table" "nc-public" {
  default_route_table_id = "${aws_vpc.aws-vpc.default_route_table_id}"	                # ID of the default routing table
  tags = { Name = "New Public Route Table" }						# Map of tags to assign to resources
}

# Resource Area for Creating VPC Routing Tables
resource "aws_route_table" "nc-private" {
  vpc_id = "${aws_vpc.aws-vpc.id}"				                        # VPC ID
  tags = { Name = "New Route Private Table" }	                                        # Map of tags to assign to resources
}

# A resource area for creating associations between routing tables and subnets or routing tables and Internet gateways or virtual private gateways.
resource "aws_route_table_association" "aws_public_2a" {
  subnet_id = "${aws_subnet.cp-opentofu-subnet01.id}"		                        # Subnet ID to create a connection
  route_table_id = "${aws_vpc.aws-vpc.default_route_table_id}"		                # ID of the routing table to connect to
}

resource "aws_route_table_association" "aws_private_2a" {
  subnet_id = "${aws_subnet.cp-opentofu-subnet02.id}"
  route_table_id = "${aws_vpc.aws-vpc.default_route_table_id}"
}

# Resources for creating a VPC Internet Gateway
resource "aws_internet_gateway" "aws-igw" {
  vpc_id = "${aws_vpc.aws-vpc.id}"					                # VPC ID to create
  tags = { Name = "aws-vpc Internet Gateway" }		                                # Map of tags to assign to resources
}

# Resources for creating routing table entries (routes) in the VPC routing table
resource "aws_route" "aws_public" {
  route_table_id         = "${aws_vpc.aws-vpc.default_route_table_id}"	                # IDs in the routing table
  destination_cidr_block = "0.0.0.0/0"							# Target CIDR block
  gateway_id             = "${aws_internet_gateway.aws-igw.id}"			        # The identifier of the VPC Internet gateway or virtual private gateway.
}

resource "aws_route" "aws_private" {
  route_table_id         = "${aws_route_table.nc-private.id}"
  destination_cidr_block = "0.0.0.0/0"
  nat_gateway_id         = "${aws_nat_gateway.aws-nat.id}"
}

# Provide elastic IP resources
resource "aws_eip" "aws_nat" {
  vpc = true		                                                                # If the EIP is in a VPC, the default is true unless the region supports EC2-Classic.
}

# Resource Area for Creating a VPC NAT Gateway
resource "aws_nat_gateway" "aws-nat" {
  allocation_id = "${aws_eip.aws_nat.id}"					        # Assigned ID of the elastic IP address for the gateway
  subnet_id     = "${aws_subnet.cp-opentofu-subnet01.id}"		                # Subnet ID of the subnet where you want to place the gateway
}
```

</div>
</details>

#### <div id='6.1.3'> 6.1.3 NHN
This section describes how to create an instance on NHN using Terraman. The basic IaC code is geared towards creating instances, and the *network, keyfair, security groups, etc.* utilize information already created in the IaaS. Therefore, other resources other than the instance must be pre-created.
> [Variable notes when writing NHN IaC code](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs#configuration-reference)
- When creating an instance, you must specify the names "master" and "worker".
*Example*
  + When creating a single instance
    - resource "openstack_compute_instance_v2" "master" {...}
  + When creating n instances 
    - resource "openstack_compute_instance_v2" "master" {...}
    - resource "openstack_compute_instance_v2" "worker1" {...}
    - resource "openstack_compute_instance_v2" "worker2" {...}
    - resource "openstack_compute_instance_v2" "worker3" {...} ...

<details>
<summary> :page_facing_up: <b>NHN Instance Code Template</b></summary>
<div markdown="3">

```
## Use this data source to get the ID of an available OpenStack network.
data "openstack_networking_network_v2" "cp-network" {
  name = "Default Network"
}

## Use this data source to get the ID of an available OpenStack subnet.
data "openstack_networking_subnet_v2" "cp-subnet" {
  network_id = data.openstack_networking_network_v2.cp-network.id
  name       = "Default Network"
}

## Use this data source to get the ID of an available OpenStack security group.
data "openstack_networking_secgroup_v2" "cp-sg" {
  name = "default"
}

## Use this data source to get the ID of an available OpenStack image.
data "openstack_images_image_v2" "ubuntu_focal" {
  name        = "Ubuntu Server 22.04.3 LTS (2023.11.21)"
  most_recent = true
}

## Use this data source to get the ID of an available OpenStack network.
data "openstack_networking_network_v2" "ext_network" {
  name = "Public Network"
}

## Manages a V2 port resource within OpenStack.
resource "openstack_networking_port_v2" "nic" {

  name = "cp-nic01"
  network_id = data.openstack_networking_network_v2.cp-network.id
  fixed_ip {
    subnet_id = data.openstack_networking_subnet_v2.cp-subnet.id
  }
  security_group_ids = [
    data.openstack_networking_secgroup_v2.cp-sg.id
  ]
}

resource "openstack_networking_port_v2" "nic2" {

  name = "cp-nic02"
  network_id = data.openstack_networking_network_v2.cp-network.id
  fixed_ip {
    subnet_id = data.openstack_networking_subnet_v2.cp-subnet.id
  }
  security_group_ids = [
    data.openstack_networking_secgroup_v2.cp-sg.id
  ]
}

## Manages a V2 VM instance resource within OpenStack.
resource "openstack_compute_instance_v2" "vm-cp-master" {                               # When creating two or more instances, be sure to separate them into "master" and "worker" names

  name              = "cp-cluster-master"
  availability_zone = "kr-pub-a"
  flavor_name       = "m2.c4m8"
  key_pair          = "nhn-cluster-key"

  block_device {                                                                        # Configuration of block devices.
    uuid                  = data.openstack_images_image_v2.ubuntu_focal.id              # The UUID of the image, volume, or snapshot. Changing this creates a new server.
    source_type           = "image"                                                     # The source type of the device. Must be one of "blank", "image", "volume", or "snapshot". Changing this creates a new server.
    destination_type      = "volume"                                                    # The type that gets created. Possible values are "volume" and "local". Changing this creates a new server.
    delete_on_termination = true                                                        # Delete the volume / block device upon termination of the instance. Defaults to false. Changing this creates a new server.
    volume_size           = 40                                                          # The size of the volume to create (in gigabytes).
    volume_type           = "General HDD"                                               # The volume type that will be used, for example SSD or HDD storage.
  }

  network {
    port = openstack_networking_port_v2.nic.id                                          # The port UUID of a network to attach to the server.
  }
}

## Manages a V2 floating IP resource within OpenStack Neutron (networking) that can be used for load balancers. These are similar to Nova (compute) floating IP resources, but only compute floating IPs can be used with compute instances.
resource "openstack_networking_floatingip_v2" "fip_1" {
  pool       = data.openstack_networking_network_v2.ext_network.name                    # The name of the pool from which to obtain the floating IP. Changing this creates a new floating IP.
}

## Associate a floating IP to an instance.
resource "openstack_compute_floatingip_associate_v2" "fip_1" {
  floating_ip = openstack_networking_floatingip_v2.fip_1.address                        # The floating IP to associate.
  instance_id = openstack_compute_instance_v2.vm-cp-master.id                           # The instance to associte the floating IP with.
  wait_until_associated = true                                                          # In cases where the OpenStack environment does not automatically wait until the association has finished, set this option to have OpenTofu poll the instance until the floating IP has been associated. Defaults to false.
}

resource "openstack_compute_instance_v2" "vm-cp-worker" {
  name              = "cp-cluster-worker"
  availability_zone = "kr-pub-a"
  flavor_name       = "m2.c4m8"
  key_pair          = "nhn-cluster-key"

  block_device {
    uuid                  = data.openstack_images_image_v2.ubuntu_focal.id
    source_type           = "image"
    destination_type      = "volume"
    delete_on_termination = true
    volume_size           = 40
    volume_type           = "General HDD"
  }

  network {
    port = openstack_networking_port_v2.nic2.id
  }
}

resource "openstack_networking_floatingip_v2" "fip_2" {
  pool       = data.openstack_networking_network_v2.ext_network.name
}

resource "openstack_compute_floatingip_associate_v2" "fip_2" {
  floating_ip = openstack_networking_floatingip_v2.fip_2.address
  instance_id = openstack_compute_instance_v2.vm-cp-worker.id
  wait_until_associated = true
}
```

</div>
</details>

<br>

### <div id='6.2'> 6.2 Register an instance code template
Register the IaC code in the Container Platform portal menu Global > Instance Code Template.

<img src="../../images/terraman/IMG_6_2.png">

<br>

## <div id='7'> 7. Clusters
### <div id='7.1'> 7.1 Create a cluster
In the Container Platform portal menu Global > Clusters, enter the following items to create a cluster.

|Item|Description|
|---|---|
|Cluster Name|Name of the cluster to create|
|Provider|Select a CSP (AWS, OpenStack, NHN Cloud, Naver Cloud)|
|Cloud Account|Select the registered account information for each CSP|
|Template|Select a registered code template|
|Description|Fill in the details of the cluster to be created (Option)|
|Template Detail|Selected Code Template is displayed and can be modified|


<img src="../../images/terraman/IMG_7_1_1.png">


##### When creating a cluster, sub-clusters are created by the Terraman API, and you can check the cluster status through the status item on the right.
<img src="../../images/terraman/IMG_7_1_2.png">

##### You can check the progress of the cluster creation as a log on the Cluster Logs list page (click the status icon).
<img src="../../images/terraman/IMG_7_1_3.png">

##### When the cluster deployment is complete, the status icon in status changes to green, as shown in the screen shot.
<img src="../../images/terraman/IMG_7_1_4.png">

##### Once the cluster deployment is complete, you can see that it is registered as a subcluster on the Overview page, as shown below.
<img src="../../images/terraman/IMG_7_1_5.png">

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](https://github.com/K-PaaS/cp-guide-eng/blob/master/use-guide/Readme.md) > [Terraman User Guide](../cp-terraman-guide.md) > Terraman Deployment Guide
