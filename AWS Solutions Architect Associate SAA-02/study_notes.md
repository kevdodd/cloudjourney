
# S3

* **S3 Object locking** - uses write one read many (WORM) model to protect objects from being deleted or modified
	* **Governance Mode** with this mode enabled it allows permissions to be applied to specific users to alter retention settings and delete objects
	* **Compliance Mode** - in this mode rention settings for all objects cannot be altered and object cannot be deleted, this applies to the root user too
	* Retention period - protects an object from being deleted for a fixed amount of time, timestamp placed in metadata file for object
	* Legalhold - same as retention period except no end date; set indefinitely
	
* **S3 Glacier Vault Lock** - very similar to S3 object locking, allows you to enforce compliance controls for Glacier vaults in S3

* **S3 Performance** - 
