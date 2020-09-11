#### Course: Essential Google Cloud Infrastructure: Foundation
#### Module: Virtual Machines
#### Lab: Working with Virtual Machines

#### Task 1: Create the VM 


- In the Cloud Console, on the Navigation menu (Navigation menu), click Compute Engine > VM instances.
- Click Create. `gcloud compute create`
- Specify the following and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Name	mc-server
Region	us-central1
Zone	us-central1-a
Identity and API access > Access scopes	Set access for each API
Storage	Read/Write
- Click Management, security, disks, networking, sole tenancy.
- Click Disks. You will add a disk to be used for game storage.
- Click Add new disk.
- Specify the following and leave the remaining settings as their defaults:

|Property	Value | (type value or select option as specified)|
|---|---|
|Name|	minecraft-disk|
|Disk| type	SSD Persistent Disk|
|Source type|	None (blank disk)|
|Size (GB)|	50|
|Encryption	|Google-managed key|
- Click Done. This creates the disk and automatically attaches it to the VM when the VM is created.
- Click Networking.
- Specify the following and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Network tags	minecraft-server
Network interfaces	Click default to edit the interface
External IP	Create IP Address
Name	mc-server-ip
- Click Reserve.

- Click Done.

- Click Create.

##### GCloud Translation for Task 1;

Set Global variable for convenience
```
export PROJECT_ID=qwiklabs-gcp-03-b9843bbcf1ff
```

Equivalent gcloud command, implementing given properties and console defaults:

```
gcloud beta compute --project=PROJECT_ID instances create mc-server --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --address=35.206.126.189 --network-tier=STANDARD --maintenance-policy=MIGRATE --service-account=759978245380-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/trace.append,https://www.googleapis.com/auth/devstorage.read_write --tags=minecraft-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mc-server --create-disk=mode=rw,size=50,type=projects/qwiklabs-gcp-03-b9843bbcf1ff/zones/us-central1-a/diskTypes/pd-ssd,name=minecraft-disk,device-name=minecraft-disk --reservation-affinity=any
```
*Tasks 2 and 3 are Gcloud-only tasks and so no translation needed.*


#### Task 4: Allow client traffic
- In the Cloud Console, on the Navigation menu (Navigation menu), click VPC network > Firewall rules.
- Click Create firewall rule.
- Specify the following and leave the remaining settings as their defaults:

|Property|	Value (type value or select option as specified)|
|---|---|
|Name|	minecraft-rule|
|Target|	Specified target tags|
|Target tags|	minecraft-server|
|Source filter|	IP ranges|
|Source IP ranges|	0.0.0.0/0|
|Protocols and ports|	Specified protocols and ports|
- For tcp, specify port 25565.
- Click Create. Users can now access your server from their Minecraft clients.

##### Gcloud translation for Task 4
```
gcloud compute --project=$PROJECT_ID firewall-rules create minecraft-rule --target-tags=minecraft-server --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:25565 --source-ranges=0.0.0.0/0 

```


****

Tasks 5 & 6 are console-only tasks, hence no translation needed.
Task 7 is review.
