## Revision Notes

#### IAM (global)
- User and Groups , groups have many users ,  users can belong to mupltiple groups , users can exist alone without group
- Permissions : JSON document that outlines permission for users or group aka policies , policies attached to user or group
- Policies : (Version , Statement , Effect - [Allow, Deny] , Action [ec2:describe, iam:Get*] , Principal [which account/user policies applied to] ,  Resource [AWS resource , S3 etc])
- Custome policies can be created and applied
- Password , force pwd reset can be enabled , Multi Factor authentication (MFA) , Google Authnticator , Authy
- AWS CLI - direct access to API of AWS services , access keys needed to connect (ID & PASSWORD)
- AWS SDK - software develoment kit (java , python etc) , embedded within applicatoin
- Roles (for services not user) , permission to AWS services with IAM Roles (EC2 instance , lambda function roles)
- Roles have policies attached to control the access
- IAM Credentials Report (account level) - list all users and credentials status , audit
- IAM Access Advisor (user level) - service permission granted to users nn last accessed details , used to revise policy etc , audit

#### AWS CloudShell (not global)
- Terminal on AWS portal

#### EC2 (Elastic Compute Cloud) (global)
-  OS : Linux , Windows or Mac OS
-  CPU , RAM , Network attached (EBS & EFS) , Hardware (EC2 instance stoire)
-  Security group , bootstrapping scripts (runs once at boot time)
-  AMI (Amazon Machine Images) package OS , additional installations as reusable template .
-  Instance types
    * t-type and m-type for general purpose (web servers , code repositories -> balance compute , memeory n networking)
    * c-type for compute optimized (compute-intensive , batch processing , media trnascoding, high perf web server , high perf comuting , ML , gaming server)
    * r-type, x-type, and z-type for memory-optimized (high perf , relatinal/non db's , distributed cache , in-memeory database for BI , real time processing of unstrutured data)
    * d-type, h-type, and i-type for storage optimized (high sequential r/w . OLTP system , relatinal/non db's , redis (in-memery) , DFS)
    * f-type, g-type, and p-type for accelerated computing
-  Security Groups -> firewalls , allow rules only , can be refrence by IP or security group , inbound and outbound rules
-  Ports : 22 (SSH) , 21 (FTP) , 22 (SFTP , secure) , 80 (HTTP) , 443 (HTTPS) , 3389 (RDP)
-  Purchasing Options
   * On Demand 
   * Reserved
   * Saving Plans
   * Spot Instance (90% discount)
   * Dedicated Hosts
   * Capcaity Reservations
- Spot Fleets (Startegies : lowestPrice , diversified , capacityOptimized , priceCapacityOptimized)
- EC2 Hibernate - faster boot time (RAM retained -> EBS volume) 
   * Stop : data on disk kept intact in next restart 
   * Terminate : data or ant EBS volume(root) setup to be destroyed
   * Use Case - Long running processing , services that takes time to initialize
   * RAM size < 150 GB
   * Root volume must be EBS
   * Avaialble on all instance type
   * 60 days retention 

#### EC2 Placement Groups (Elastic Compute Cloud) (global)
- Private Vs Public IP (think)
- Elastic IPs : public IPv4 , fixed , only 5 elastic IP , try avoiding elastic IP
- EC2 placement stategy : Cluster ,  Spread n Partition
   * Cluster - Same AZ , Low latency , 10GBPS network , drawback - AZ fails , all instances fails if AZ goes down
   * Spread  - span across multiple AZ , reduce risk of fails , different physical hardware  | drawback - 7 instance/AZ per placement group [maximize avaialbility , critical workload]
   * Partition - 7 partition per AZ , 100s of EC2 , each parition share diffrent physical server [HDFS , HBase , Cassandra , Kafka]

#### ENI (Elastic Network Interfaces)
- Logical component in a VPC that represents a virtual network card
- Attribtes : IPv4 per private IPv4 , one or more SG , a MAC address
- Bound to specific availibility zone

#### EC2 : Storage
- EBS (Elastic Block Store) volume is a network drive , gets attached to instance , latency impact
- Think of hard disk which retains data after instance teminates , attach / detach 
- Mounte to one instance at a time , bound to specififc AZ , instance (eu-west-1a) --> volume (eu-west-1a) --X--> volume (eu-west-1b) not work
- Delete on Termination attribute , once set , will delete EBS too.
- Snapshots (backup) , can be taken any point in time , used to migrate data from one AZ to another
- EBS Snapshot Archive --> Archive tiew , 75% cheaper , takes 24 to 72 hours for restoration
- Recycle Bin for EBS snapshots - rules to retain deleted snapshot , to recover from accidental deletion , 1 day to 1 year retention period
- Fast Snapshot Restore (FSR) - force full initialization of snapshot to avoid lartency , expecsive
- EC2 instance store : local storage comes with insatnce , vanish when instance restarts (use case cache , buffer data etc)
- EBS Volume Types
   * gp2/gp3 (SSD) - general purpose SSD , uses as boot volumes (1 GB - 16 TB)
   * io1 / io2 (SSD) Block express - highest performance SSD , mission critical , low latency or high throughput worklods , uses as boot volumes [database workloads]
   * st1 (HHD) , low cost , designed for frequencyt accessed , throughput intensive workload , cannot be used for boot volume
   * sc1 (HHD) , lowest cost , cannot be used for boot volume
- EBS Volume Multi Attach (only possible with io1/io2 family)
   * possible , EBS volume to multiple EC2 in same AZ
   * Full read and write permission , upto 16 instance at a time#
   * file system must be cluster aware  
- EBS Volume Encryption
   * data a rest , in flight between inctances and volume
   * snapshot are alos encrypted
   * also volume create dfrom snapshot is encrypted
   * minimal impact on latency {KMS (AES-256)}
   * snapshot(unencrypt) --> snapshot (encrypt , using copy) --> create new EBS volume from snapshot --> attach to original instance

#### AMI (Amazon Machine Image)
- custome EC2 instance
- add own software , configuration , OS , monitoring
- faster boot time , as software are pre-pakaged
- AMI for specific region , can be copoied across regions
- Start EC2 , stop it , create AMI (EBS also gets created)

#### EFS : Elastic File System
- Managed NFS(network file system) that can be mounted on many EC2
- EFS works with EC2 in multi-AZ
- Higly available , scalable , expensive (3x gp3) , pay per use , no capacity planning
- Use : web serving , data sharing , wordpress
- Security group for control access , encryption at rest
- Linux AMI only not with windows
- Scale : 1000s concurrent NFS clients , 10GB+ /s throughput , can grow to petabyte
- Mode
   * Performance Mode : 1) genereal purpose(default) & 2) max I/O (high latency , throughput)
   * Throughput Mode : 1) Bursting 2) Provisioned 3) Elastic  
- Storage Tiers
   * standard : 
   * infrequent access : cost to retrieve
   * archive : few time a year , 50% cheaper 
