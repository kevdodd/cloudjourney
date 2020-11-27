
# S3

* **S3 Object locking** - uses write one read many (WORM) model to protect objects from being deleted or modified
	* **Governance Mode** with this mode enabled it allows permissions to be applied to specific users to alter retention settings and delete objects
	* **Compliance Mode** - in this mode rention settings for all objects cannot be altered and object cannot be deleted, this applies to the root user too
	* Retention period - protects an object from being deleted for a fixed amount of time, timestamp placed in metadata file for object
	* Legalhold - same as retention period except no end date; set indefinitely
	
* **S3 Glacier Vault Lock** - very similar to S3 object locking, allows you to enforce compliance controls for Glacier vaults in S3

* **S3 Performance**
	* **Prefixes** - this is the folder structure between the bucketname and the object name bucketname **/folder1/folder2/**/object.tiff
		* 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix
		* To increase performance customers can spread their data across prefixes - e.g if using x2 prefixes you can get 11,000 GET requests per second
	* **KMS limits** - There is a KMS quota using for uploading(encryption) and downloading(decrypting) objects in S3, each upload / download will increase the quota
		* Quota cannot be increased
		* Quota is region specific and is either (depending on region) 5,500, 10,000 or 30,000 requests per second
	* **Multi-part uploads** - Allows you to split your file into parts and upload each part individually, parallel uploading
		* Recommended for files over 100MB
		* Required for all files over 5GB
	* **S3 byte range (downloads)** - file to be downloaded can be split into parts and downloaded individually
		* If the download fails then the byte range that has failed can be simply redownloaded
		* Speeds up the download process of the object
	* **S3 Select** - use SQL expressions to subset of data from a object stored in S3
		* This increases performance because the customer does not have to download the whole object
		* Example - imagine a CSV in a zip file, with S3 select you can run a SQL expression to extract data from the CSV file without downloading and unzipping the object
	* **S3 Glacier selecet** - Similar to S3 select, allows customer to run SQL expressions against Glacier objects stored in S3

* **Sharing S3 buckets across accounts** - AWS gives you the ability to share buckets with another account in your organization or a 3rd party via the following methods
	* Using bucket policies & IAM access - this is restricted to programmatic access only
	* Using bucket ACLs & IAM access - also restricted to programmatic access only
	* Using cross-account IAM roles - programmatic & console access
	
* **Cross region replication** - allows you to replicate to another bucket in another region
	* versioning must be enabled on both the source and destination for CRR to be enabled
	* can be replaced to a bucket in the same AWS account or a separate account
	* this will not replicate objects that exist in the bucket but will replicate objects uploaded thereafter
* **Transfer acceleration** - Uses AWS' edge locations to improve upload times. Rather than uploading directly to the bucket is is uploaded to an edge location and then transferred to the bucket using AWS' high speed prviate network

* **AWS DataSync** - Service used to sync large amount of data from on-prem datacentre to and from AWS
	* Datasyc agent needs to be installed source servers
	* Supports NFS and SMB shares
	* Replication can be done hourly, weekly or daily
	
* **Cloudfront** - AWS content delivery network(CDN) is a distributed system that improves performance by caching data to edge locations located geographically closer to the end user
	* The end user will connect to the edge location located closest to them and request some content, the edge location then retrieves the content from AWS (if not already cached), the content is then cached for the period of the TTL timer so if another user requests the same data they can source it straight from the edge location
	* Possible to read OR write data to the edge locations
	* Can clear cached objects (or invalid) but the customer will be charged to do so
	* **Origin** - this is the source data that you are caching at the edge locations
	* **Signed URL** - allows you to provide restricted access to content for a set period of time.
		* 1 URL per file
		* Example: Imagine you have users who subscribe to Netflix, Netflix will share the signed URL with the subscription account which allows access to the content on Netflix
		* **Signed Cookie** - this is similar to a signed URL with the exception that 1 signed cookie = multiple files

* **Snowball** - Device used to transfer large amounts of data and is available as either 50TB or 80TB. Tamper-resistant, 256bit encryption and contains TPM Module.
	* Data on the snowball is erased by using software in such a way that previous removed data cannot be restored
	
* **Snowball Edge** - Similar to Snowball but with the ability of compute power as well as storage. Has 100TB of storage capacity
	* Can be used offline and connect to on-prem storage systems
	
* **Snowmobile** - Allows the customer to transfer upto 100PB of data into AWS. Arrives on a truck!

* **Storage Gateway** - Physical or virtual appliance that will replace your on-premise data to AWS
	* 3 different variants of Storage Gateway Types
		* **File Gateway** - Store files in S3 using a NFS/SMB mountpoint. Files are stored directly in S3
		* **Volume gateway**
			* **Stored Volumes** - Data is stored on site and presents the volume is presented to servers using iSCSI. Data is asyncronously backed up to S3
			* **Cached Volumes** - This option allows the customer to use a volume stored in S3 as their primary data store but cache frequently access data locally to achieve low-latency access to data. Volumes are attached to on-premise servers using iSCSI and have capacity of upto 32TB
		* **Tape gateway** - Customer can replace their existing on-premise tape infrastructure with virtual tapes provided by the Tape gateway. Stores virtual tapes in S3

* **Athena** - Interactive query service which allows you to query data stored in S3 using SQL statements
	* Serverless
	* Works directly with data stored in S3
	
* **Macie** - Service that uses AI to recognize if PII data is stored in S3, can also analyze CloudTrail logs
	* Includes dashboards, reports and alerting
	* Can be used in PCI-DSS compliance

# IAM (Identity and Access Management)

* **AWS Organizataions** - Allows you to consilidate AWS accounts into an AWS Organization which allows centralised billing and management
	* Structure is Root account > OU accounts (could be departments such as HR / Finance etc) > AWS accounts
	* Policies are applied to OUs and is trickled down to all accounts under the OU
	* **Consolidated billing** - Allows you to consolidate billing from all accounts into one account - remember back to "the more you use the less you pay"


# EC2

* **Pricing Models**
	* **On-Demand** - Allows the customer to pay a fixed-rate per hour/second with no commitment
		* Good for applications with short-time, spikey or unpredictable workloads that cannot be interrupted
		* Also good for newly developed applications that are being tested
	* **Reserved** - Provides the ability for the customer to reserve capactiy for a fixed amount of time of either 1 or 3 years, significantly cheaper than on-demand
		*  **Types of Reserved Instances**
			* **Standard Reserved Instances** - offer up to 75% off the total price, the more you pay upfront or the longer the contract term then the cheaper it will be
			* **Convertible Reserved Instances** - offer up to 54% off, allows the customer to increase the resources of the RI (RAM, CPU etc) but does not allow the resources to be lowered
			* **Scheduled Reserved Instances** - Customers can reserved EC2 instances which run scheduled workloads within a set time frame. Workloads must be predictible
		
	* **Spot instances** - Allows customer to bid for unused EC2 capacity for very very cheap prices however if the capacity is needed then the EC2 instance is shutdown within minutes. Spot block can be used to block the instance from being terminated between 1 - 6 hours.
	* **Dedicated hosts** - Reserve a physical EC2 server dedicated for customers use only, can be beneficial when needing transfer server-bound software licenses
		* Can be reserved on-demand (hourly)
		* Can purchase a reservation and get up to 70% discount
		* Good for regulatory requirements which don't support multi-tenant configurations
		
* **EC2 Misc**
	* Termination protection is off by default - prevents you from accidently terminating your EC2 instance
	* The root volume of an EC2 instance will terminal by default, EBS volumes must be manually terminated
	* EBS root volumes on default AMIs can be encrypted off the bat. Also can use a third party tool to encrypt EBS volumes
	* Instance store provides temporary block storage for EC2 instances
		* All data is lost when the EC2 instance is rebooted, terminated or if the underlying physical disk becomes faulty
		* Underlying storage is physically attached to the host computer
		* Instance store storage can only be created when the EC2 instance is initially created, cannot be configured thereafter
	* Spot Fleet - a collection of Spot instances with the option of on-demand instances. Aims to create EC2 instances to meet target capacity
	
* **Security Groups**
	* Act as a firewall for EC2 instances
	* Statefull firewall (if we have a rule to allow HTTP inbound then it will permit outband connections for these)
	* More than 1 Security Group can be applied to an EC2 instance
	* All inbound traffic is blocked by default
* **EBS**
	* Elastic Block Storage - provides virtual hard disk for your EC2 instances that uses block storage
	* EBS Volumes are automatically replicated wihtin the same AZ
	* Types of EBS Volumes
		* **General Purpose (SSD)**
			* Balances price and performance, good for most work loads
			* Max IOPS is 16,000
			* Capacity between 1GB - 16TB
			* API name - gp2
		* **Provisioned IOPS (SSD)**
			* Highest performant EBS volume, designed for mission critical workloads such as databases
			* Max IOPS is 64,000
			* Capacity between 4GB - 16TB
			* API name is io1
		* **Throughput Optimized HDD**
			* Designed for frequently accessed data with a requirement of high throughput
			* Low cost HDD
			* Most suitable for big data and data warehouses
			* Max IOPS is 500
			* Capacity between 500GB - 16TB
			* API name is st1
		* **Cold HDD**
			* Low cost HDD designed for less frequently accessed data such as File Servers
			* Capacity between 500GB and 16TB
			* API name is sc1
			* Maximum IOPS is 250
		* **EBS Magnetic**
			* Previous generation HDD
			* Suitable for non-frequently access data
			* API name - Standard
			* Capacity between 1GB - 1TB
			* Max IOPS is 40-200
	* Volume types / capacity can be changed on the fly
	* Snapshots are incremental
	* Snapshots are automatically created in the same AZ as the EC2 instance its attached to
	* To move a EC2 volume to another AZ take a snapshot of it then create an AMI using the snapshot and use the AMI to spin up a new EC2 instance in the other AZ
	
* **EC2 NIC**
	* **Elastic Network Interface (ENI)** - A standard virtual NIC that is low-cost. Good for a standard network or management network
	* **Enhance Networking (EN)** - uses SR-IOV (allows a single PCIe device such as a NIC to appear as multiple) to enhance network I/O performance, reduce inter-instance latency improve bandwidth and PPS (packets per/s). There is no additional charge
		* Elastic Network Adapter - ENA should be chosen for enhanced networking option which allows network speeds of upto 100Gbps
	* **Elasic Fabric Adapter (EFA)** - Networking device that can be attached to EC2 instances for High-Performance Computing (HPC) which allows much lower latency and higher throughput. EFA OS Bypass function allows EFA devices to communicate directly with each other and bypass the OS kernal, this reduces latency
	
		* 
