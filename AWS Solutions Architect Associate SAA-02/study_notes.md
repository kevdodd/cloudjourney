
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
	
