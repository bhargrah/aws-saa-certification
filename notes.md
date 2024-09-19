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

#### ELB (Elastic Load Balancing)
- vertical scaling : increase instance size (scale up/dowm) (typically done for databases)
- horizontal scaling : increse number of instances (scala in/out)
- LB , server forwards to mupltiple servers
- Benifits : single point of acces (DNS) , handle failure of downstream , regular heath check , provides SSL termination , enforce stikiness with cookie, HA
- ELB , managed load balancer , cost less to setup ,integrate with AWS offerings
- Types : Classic Load Balancer(old) , Application Load Balancer(ALB) , Network Load Balancer(NLB) , Gateway Load Balancer(GLB)
- ALB : Layer 7 (HTTP) , supports HTTP/2 & WebSocket , redirects HTTP & HTTPs , routing based on URL , hostname , query string n headers (/id=123 etc)
- ALB (target group) :  EC2 , ECS tasks , Lambda fucntions , IP address (must be private) , great fit for microservice architecture
- ALB (target group ( EC1 , EC2) )
- NLB : Layer 4 (TCP & UDP) , million of request/sec , ultra-low latency , one static IP per AZ , can assign elastic IP , extream performance
- NLB (target group) :  EC2 , IP address (must be private) , ALB , health checks configured using TCP , HTTP , HTTPS
- GLB : Layer 3 (IP Packet) :  deploy , scale and manage a fleet of 3rd party network virtual appliances
- GLB - Firewalls , Intrusion detection and prevention system , deep packet inspection , payload manipulation
- GLB - GENEVE protocol on port 6081
- GLB (target group) :  EC2 , IP address (must be private) only
- Sticky Session (session affinity) CLB . ALB and NLB (not use cookie) 
- Type : application-based cookie (custome n application) , duration based cookie
- Cross-Zone Load Balancing(CZLB) - LB distribute evenly across all registered instances in all AZ [ 50/50 , all nodes gets 10 each , without CZLB  50/50 , all nodes gets disproportionate]
- ALB (default , no extra charges) , NLB & GLB (disable by default , extra for inter AZ data if enabled)
- SSL/TLS - allows to encrypt traffic b/w client and server
- SNI - Server Name Indication , supported by ALB , NLB n CloudFront
- ELB Connection draining or Deregistration delay (ALB & NLB) , time to complete in-flight requests , 1 t0 3600 seconds range (default : 300s)

#### ASG (Auto Scalaing Group)
- Scalate out to match incresed load
- Scale in to match decresed load
- Recreate in case desired number is terminated
- Min , max , desired capacity (think)
- Launch template (terraform)
- CloudWatch alarm & scaling (monitor metrics (CPU utilization) trigger instances)
- Scaling Policies :
   * Dynamaic Scaling (Traget tracking (CPU = 40% unitlization) , Simple/Step ( CPU > 70% , add 2 instance) )
   * Scheduling Scaling (scaling on known usage pattern , 10 - 5 PM on Friday , scale up )
   * Predective Scaling (forcast and scale up/down)
- Metrics : CPU Utilization , RequestCountPerTarget , Average Network In / Out , Custom metrics too
- Tip : use ready to use AMI to reduce bootup time

#### RDS + Aurora + ElasticCache
- RDS (Relational Databse Server) : Postgres , MySQL , Maria DB , Oracle , Microsoft SQL Server , IBM DB2 , Aurora
- Managed service : provisioning , OS patching , backup , monitoring , Multi AZ for DR , scaling capabiliteis , cannot SSH on host
- Storage Auto scaling - RDS DB instance dynamically [set Maximum Storage Threshold]
- Read Replicas - reads scaling (selects only) , max 15 read replicas , can be in (with in AZ , cross AZ or cross region (cost , west -> east)) , replication is ASYNC (evental consistent) , prmote to their own DB
- Multi AZ (Disater Recovery) : SYNC replication , one DNS name , increse availability , automatic failover to standby , no read/write on standby
- Single to Multi AZ - no down time , click modify on database [snapshot , upload to new database , start replication]
- RDS Custom - Oracle & Microsoft SQL only, self manage instance patching , install patches , access EC2 instance
- Aurora : proprietary tech from AWS , Postres and MySQL supported
- Aurora , cloud optimized , 5X perf over MySQL , #X perf over Postgres
- Auotmatically grows in 10GB increments , upto 128 TB 15 read replicas , sub 10ms latency 
- HA and failover , 20% expecnsive cost
- Self healing with peer-to-peer replication , storage striped across 100s of volumes
- master , 15 read replication , 30 sec failover
- Writer Endpoint  ( Master , transparet to client) only for writing
- Reader Endpoint  ( Read Replica , transparet to client) , read replica can autoscale
- Custom endpoint (Analytical queries)
- Auror Serverless : Automated databases , auto scaling , (infrequent , intermittent or unpredectible) workfloads , pay per second , can be cost effective
- Global Aurora :
   * Primary region(read/write) , 5 secondary , 1 sec replication lag
   * Typical corss-region replication takes less than 1 second
- RDS Backup :
   * Automated - Daily backup (during work window), Trnasaction logs backedup every 5 min , 1 to 35 retrention , set 0 to disable backup
   * Manual - Triggered by user , can retain as long as you want 
- Aurora Backup :
   * Automated - 1 to 35 retrention cannot be disaible , point in time recovery in that timeframe
   * Manual - Triggered by user , can retain as long as you want
- Restore : from snapshot , S3 , Percona XtraBackup (on premise to aurora)
- Database Cloning : copy-on-write protocol , faster
- Security : At rest encryption - AWS KMS (master & replicas) , in-flight encryption - TLS ready by default , IAM authenticate , security group
- RDS Proxy : allow apps to pool and share DB connections , improve DB efficiency by reducing stress on databases resource , minimize open connection
- RDS proxy - serverless , autoscaling , HA , reduce failover time by 66% , no code change in apps , enforce IAM authenticate by leveraging AWS secret manager
- ElasticCache : managed Redis or Memcached , help with read intensive workloads , cache hit/miss scenarios

#### Route 53
- DNS (Domain Name System) , translate hostname to machine IP address
   * Domain Registrar (eg GoDaddy) ,
   * DNS Records - A,AAAA,CNAME
   * Zone File - contains DNS records
   * Name Server - resolvs DNS queries 
   * Top Level Doamin - .com, .us, .in 
   * Second level domain - amazon.com , google.com
- Route 53 - HA , scalablae , fully managed and authoritative DNS (customer can update DNS records)
- Its also a domain registrara , can check helth of resources , only service with 100% availability SLA
- Records - (Domain/subdomain , Record Type , Value , Routing Policy , TTL)
    * Record Type (A - maps a hostname to IPv4 , AAAA - maps to IPv6 , CNAME - maps hostname to another hostname ,  NS - name server )
    * Hosted Zones - Public (open to public) & Private (company internal network)
- TTL (Time to live , clients will cache the results) , think of high and low TTL value
- Alias Records - Maps hostname to an AWS resource , can be used for zone apex , always of type A /AAAA
- Routing Policies (put on the records)
   * Simple - routes traffic to single resource , if multiple IPs retured , client have to choose one , no health check 
   * Weighted - control the % of requests that go to each specific resource , multiple records each has weight associated
   * Latency - Redirects to resource that has the least latecy to us 
   * Failover - Primary-Secondary records , moment health is red then DNS route the request to secondary (actual 2 records created)
   * Geolocation - routing is based on user location
   * Geoproximity - to shidt more traffic to resource based on the defined bias [expand (1 to 99) , shrink(-1 to -99)] , think US , control east cost and west cost region
   * IP Based - Routing is based on client IP address 
   * Multi-Value Answer - return multiple values /resorces , can return 8 recods , remove unhealthy resource

 #### S3 (simple storage service)
 - Use : Backup & storage , disaster recovery , archive , hybrid cloud storage , application hosting , media hoisting , data lakes , software delivery , static website
 - S3 : store objects (files) in buckets (directories) , globally unique name , defined on regional level
 - Objects : files have key , key is FULL path , prefix + object name (s3://my-buket/job/jp/resume.doc)
 - Max size 5TB (5000GB) , use multi-part upload
 - Metadata , Tags , version ID
 - Security : Bucket Policy (gets attached to bucket only)
    * User Based (IAM Policies - control which API calls allowed)
    * Resource Based (Bucket Policies - bucket wide rules from S3 console, Object Access control list (finer grain) , Bucket Access Control list (less common))
    * Encryption
    * Bucket Policies (JSON based , resorce - bucket & object , effect - allow/deny , actions : det API to allow/deny)
- Versioning : Version files , enabled at bucket level , protect against unintented deletes , easy roll back , null version (if versioning enabeld in b/w)
- Replication (CRR (Cross region replication) & SRR (same region replicatoin)) , must give proper IAM permission to S3 , asyncronus replication
- Replication only enables for new items , S3 Batch replication (replicate existing objects) , can replicate delete markers too , no chaining of buckets
- Storage Class
    * Genereal Purpose (99.99% Av , used fr frequently acceessed data, Low latency n high throughput , sustain 2 concurrent faciclity failuers)
    * Standard-Inferquent Access ( 99.9% availability , less frequently accessed , requird rapid access , lower cost than S3 standard , UC - disaster recovery , backups)
    * One Zone-Infequent acess (99.9(11)% durability in single AZ , data lost if AZ destroyed , 99.5% availability , UC - secondary copy of data)
      [Glacier : Low cost object storage for archival/backup , pricing : price for storage + object retrival cost]
    * Glacier Instance retrival (milli second retrival , great for data access one a quater , min storage duration 90 days)
    * Glaciuer flexible retrival (Mode - Expediated (1 to 5 min) | Standard (3 to 5 hr) | Bulk (5 to 12 hr) , bulk is free ) , min storage duration 90 days
    * Glacier deep archive (long term storage ( Standard (12 hr) | Bulk (48 hr) )) , min storage duration of 180 days
    * Intelligent tiering (small monthly fees , move object automaticallt b/w access tiers based on usage , no retrival charges)
        * Frequest Access (automatic) : default tier
        * Infrequent Access (automatic): object not access for 30 days
        * Archive Instant Access (automatic): object not access for 90 days
        * Archive Access (optional): configurable from 90 days to 700+ days
        * Deep Archive Access (optional): confi from 180 days to 700+ days
- Durability : (high , 11 9's) | Availability : measures redily avaiable a service is , depends upon storage class
- Lifecycle rules controls the transition b/w storage classes
- Lifecycle Rule - Transition actions , Expriration actions , certain prefix , can use tags also
- S3 analytics , run on S3 bucket , updated daily
- S3 requestor pays - client or user pays for the downloading objetcs for S3 bucket
- S3 events notification - S3:ObjectCreate (S3 events --> SNS , SQS n Lambda Function notification targets) + EventBridge too (new)
- S3 event - IAM permissoin , SNS , SQS & lambda resource policy needed ( no IAM roles )
- S3 performance - automatically scales to request rates , latency 100-200 ms [3500 PUT/COPY/POST/DELETE or 5500 GET/HEAD request /sec / prefix ]
- No limit to the mumber of prefixes in a bucket , if reads are spread across prefixes then we can achive more GET
- Multi part upload (> 100MB , must use > 5 GB) , help praallelize speed
- S3 transfer acceleration (increase transfer speed by transferring file to AWS edge location then forward data to S3 traget bucket)
- Fetches - S3 byte-Range fetches (parallelize GETs by requesting specific byte ranges) , think for header fetch question
- S3 batch operation - perform bulk operations on existing S3 objects (e.g encrypt un-encrypt objects)
- Prcoess : use S3 inventory to get object list and use S3 select to filter you objetcs , gives to S3 batch operation
- S3 Storage lense
    * general insigts about your S3 storage
    * StorageByte , object count
      
#### S3 : Encryption (SSE - Server Side Encryption)
- SSE - S3
   *  Encryption using keys handled , maanged and oned by AWS
   *  Object is encryptrd server-side , type AES-256
   *  Set header "x-amz-side-encryption":"AES256"
   *  Default for new buckets & objects
- SSE - KMS
   *  Encryption using keys handled , managed by AWS KMS (key management service)
   *  Advandatge : user control + audit key usage using CloudTrail
   *  Set header "x-amz-side-encryption":"aws:kms"
   *  Limitation - GenerateDataKey KMS API , calls Decrypt KMS API , KMS quota per sec 5500 , 10000 , 30000 req/sec (region based) , can ask for increase
- SSE - C
   * Encryption keys fully managed by cutomer outside AWS
   * S3 does NOT store encryption key you provided
   * HTTPS must be used , key to be provided every time by client 
- Client Side Encryption
   * Use client libraries such as Amazon S3 Client-Side Encryption Library
   * Client must encrypt data themselves before sending to S3
   * Client must decrypt data themselves when retriving from Amazon S3
   * Customer fully managed the keys and encryption cycle
- Encryption in transit (SSL/TLS)
   * S3 exposed two endpoints - HTTP (non encrypted) & HTTPS (encrypted)
   * HTTPS is recommended 
- Enforce Encryption in transit (aws:SecureTransport) , attach policy
- S3 Access logs - request to S3 bucket can be logged into another S3 bucket for auditing.
- S3 glacier Vault Lock
    * Adpot WORM (Write Once Read Many) model
    * Vault Lock Policy , Locks the policy for future edits , helps for compliance and data retention
- S3 Object Lock (Versining must be enabled)
    * Adpot WORM (Write Once Read Many) model
    * Block object version delettion for a specific amount of time
    * Retention Mode - Compliance (super super strict mode)
       * Object version can't be overwritten or dleted by any user , including root user
       * Object retention modes can't be chnaged and retention periods can't be shortened
    * Retention Mode - Governance 
       * Most users can'toverwrite or dleted an object or alter lock settings
       * Some user have special permissoin to change retention
    * Retention Period : protect the obejct for fixed period , can be extended
    * Legal Hold : protect the object indefinnately , indepednet from retention period ( can be place/removed by s3:PutObjectLegalHold IAM permission)
- S3 Access Points : Policy , R/W to specicfic prefix , each access has its own permission (fianance , sales , traders etc), security management , own DNS (private/public)
- S3 Object Lambda : perfrom operation on objects via lambda (S3 access lambda access points)

#### CloudFront & AWS Global Accelerator (global)
- Content Delivery Network (CDN) , improves read perfromace , contect is cache at edge
- Improved user experice , 216 points , DDoS protection
- Origins
   * S3 bucket
       * distributuibng files and caching them at edge
       * enhance security with cloudfront Origin Access Control (OAC)
       * OAC is replacing Origin Access Identity (OAI)
       * CloudFront can be used as an ingress (to upload files to S3)
   * Custome Origin (HTTP)
       * Application Load balancer
       * E3 instance
       * S3 website (bucket should be static S3 website) 
- All the content needs to be publicly avaibale , Geo restrictions (allow/block list)
- Pricing , cost of data out per edge locations varies (india most expecnsive)
- Price classes
    * Price class All - all regions - best performance 
    * Proce class 200 - most regions , but excluide the most expensive regions 
    * Price class 100 - only the least expecnsivbe regions 
