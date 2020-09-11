### 2. 
#### Course: Essential Google Cloud Infrastructure: Foundation
#### Module: Virtual Networks
#### Lab: Implement Private Google Access and Cloud NAT

#### Task 1: Create the VM instance

- First, we create a VPC network with the following properties
|Property|Value|
|---|--- |
|Name |privatenet-allow-ssh |
|Network |privatenet |
| Targets|	All instances in the network |
| Source filter	|IP ranges|
|Source IP ranges	 | 35.235.240.0/20|
|Protocols and ports | Specified: (tcp 22)|
|Private Google access | Disabled (for now)|

##### Gcloud translation:
Create VPC with custom-mode subnet creation and Private google access disabled

```
export PROJECT_ID=qwiklabs-gcp-00-1887033d498e
gcloud compute networks create privatenet --project=$PROJECT_ID --subnet-mode=custom --bgp-routing-mode=regional
```
Then we Create subnet called `privatenet-us` for VPC network `privatenet`
```
gcloud compute networks subnets create privatenet-us --project=qwiklabs-gcp-00-1887033d498e --range=10.130.0.0/20 --network=privatenet --region=us-central1
```

- Then, we create a VM instance with the following properties
|

****




#### Task 1: Create the VM

##### Gcloud translation for Task x:

#### Task 1: Create the VM 

##### Gcloud translation for Task x:

#### Task 1: 

##### G
