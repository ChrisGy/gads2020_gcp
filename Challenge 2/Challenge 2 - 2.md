### 2. 
#### Course: Essential Google Cloud Infrastructure: Foundation
#### Module: Virtual Networks
#### Lab: Implement Private Google Access and Cloud NAT

#### Task 1: Create the VM instance

Set global variable `Project_ID` for convenience
`export PROJECT_ID=qwiklabs-gcp-00-1887033d498e`

1. Create VPC with custom-mode subnet creation and Private google access disabled, with following properties
|Property|Value|
|VPC Name |privatenet|
|VPC Subnet mode|Custom|
|New Subnet Name|privatenet-us|
|New Subnet Region|us-central1|
|New subnet IP address range|10.130.0.0/20|
|New subnet Private Google access|Disabled|

We create the VPC network:
```
gcloud compute networks create privatenet --project=$PROJECT_ID --subnet-mode=custom --bgp-routing-mode=regional
```
Then we Create subnet called `privatenet-us` for VPC network `privatenet`
```
gcloud compute networks subnets create privatenet-us --project=$PROJECT_ID --range=10.130.0.0/20 --network=privatenet --region=us-central1
```

2. We create a firewall rule for the VPC network in step (1) with following properties:
|Property|Value|
|---|---|
|Name |privatenet-allow-ssh |
|Network |privatenet |
| Targets|	All instances in the network |
| Source filter	|IP ranges|
|Source IP ranges	 | 35.235.240.0/20|
|Protocols and ports | Specified: (tcp 22)|

```
gcloud compute --project=$PROJECT_ID firewall-rules create privatenet-allow-ssh --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=tcp:22 --source-ranges=35.235.240.0/20
```


3. Then, we create a VM instance with the following properties

|Property|	Value (type value or select option as specified)|
|---|---|
|Name	|vm-internal|
|Region|	us-central1|
|Zone|	us-central1-c|
|Series|	N1|
|Machine type|	n1-standard-1 (1vCPU, 3.75 GB memory)|
|Boot disk|	Debian GNU/Linux 10 (buster)|
|Network|privatenet|
|Subnet|privatenet-us|
|External IP|None|

****




#### Task 1: Create the VM

##### Gcloud translation for Task x:

#### Task 1: Create the VM 

##### Gcloud translation for Task x:

#### Task 1: 

##### G
