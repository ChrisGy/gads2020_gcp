### 2. 
#### Course: Essential Google Cloud Infrastructure: Foundation
#### Module: Virtual Networks
#### Lab: Implement Private Google Access and Cloud NAT

#### Task 1: Create the VM instance

Set global variable `Project_ID` for convenience, using command: `export PROJECT_ID=qwiklabs-gcp-00-1887033d498e`

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

We create the above VM using the following gcloud command:
```
gcloud beta compute --project=qwiklabs-gcp-00-1887033d498e instances create vm-internal --zone=us-central1-c --machine-type=n1-standard-1 --subnet=privatenet-us --no-address --maintenance-policy=MIGRATE --service-account=827262531687-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-internal --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any
```

****




#### Task 2: Enable Private Google Access

1. First we create a Cloud Storage bucket with the following properties:

|Property|Value|
|---|---|
|Name|*(Any globally unique name)* $PROJECT_ID-bucket|
|Location type|Multi-region|

We use the `US` multi-region for our bucket:
```
gsutil mb -c standard -l US gs://$PROJECT_ID-bucket
```

2. Next, we enable Private Google Access
```
gcloud compute networks subnets update privatenet-us --enable-private-ip-google-access --region=us-central1
```



#### Task 3: Configure Cloud NAT gateway

We first create our Cloud Router using the command:
```
gcloud compute routers create nat-router --network=privatenet --region=us-central1
```

We create a Cloud NAT gateway with the following properties:
|Property|Value|
|---|---|
|Gateway name|	nat-config|
|VPC network|	privatenet|
|Region	|us-central1|

```
gcloud compute routers nats create nat-config --router=nat-router --region=us-central1 --auto-allocate-nat-external-ips --nat-all-subnet-ip-ranges
```


#### Task 4: Configure and view logs with Cloud Nat Logging

We can Enable Logging using the command:

```
gcloud compute routers nats update nat-config --router=nat-router --region=us-central1 --enable-logging
```
