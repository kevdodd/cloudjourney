
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
		* Example: Imagine you have users who subscribe to Netflix, Netflix will share the signed URL with the subscription account which allows access to the content on Netflix
	
	

# IAM (Identity and Access Management)

* **AWS Organizataions** - Allows you to consilidate AWS accounts into an AWS Organization which allows centralised billing and management
	* Structure is Root account > OU accounts (could be departments such as HR / Finance etc) > AWS accounts
	* Policies are applied to OUs and is trickled down to all accounts under the OU
	* **Consolidated billing** - Allows you to consolidate billing from all accounts into one account - remember back to "the more you use the less you pay"
	
	* 
