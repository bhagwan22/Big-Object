# Big Object:
> Standard:
  - defined by Salesforce and are included in Salesforce products. 
  - "FieldHistoryArchive" part of our Field Audit Trail product, is an example of a standard big object. FieldHistoryArchive allows you to store up to 10 years’ worth of archived field history data, helping you comply with industry regulations related to auditing and data retention.
	
> Custom big objects: 
  - are defined and deployed in Setup. where we set its definition, fields, and index.
 
 ## Points
 ```
	- sufix __b
	- can use in SOQL, Asyc SOQL 
	- Async SOQL most efficient way to process the large amount of data in a big object.
	- once deployed can not edit or delete index, to change index have to create new big object
	- big object not allow subquery
	- 100 big object per org(field limit same as standard object)
	- can not do trigger, flow, process
	- can define a custom big object from Setup or with Metadata API
	- The index defines the composite primary key for a big object and is used for querying and filtering the big object data.
	- we can't deploy a big object until it includes an index that contains at least one custom field.
	
	-  "index" (primary key) : index determine the big object’s identity and ability to be queried.
				 - Only required custom fields(upto 5) are allowed in an index.
				 - 100 max char limit
				 - long text area field can not be used in index
				 - Once you’ve created an index, you can’t edit or delete it. To change the index, create another big object with a new index.
				

	- big object support below field type: lookup, dateTime, text, email, phone, Number, longText
	- apex use "databse.insertImmediate()" to upsert big object record based on primary key
```

## SOQL
 - SOQL to query big object, we can query only on the fields that make up our index, in the order you defined them in without missing any field.
 - We can use these comparison operators =, <, >, <=, >=, or IN on the last field in your query. Any prior fields in your query can only use the = operator. 
 - Example: **the below SOQL query will work because it inclued all field in soql and filter with correct sequence**
 	 ```
 	 - The following queries assume that you have a big object in which the index is defined by Account__c, Game_Platform__c, and Play_Date__c.
 	  SELECT Account__c, Game_Platform__c, Play_Date__c
	  FROM Customer_Interaction__b
	  WHERE Account__c='001R000000302D3' AND Game_Platform__c='PC' AND Play_Date__c=2017-09-06T00:00:00Z 	
	```
	
## Async SOQL:
  - With Async SOQL we can use filtering to extract a small subset of your big object data into a custom object. with correct mapping
  - in Async SOQL we can skip including all index and also can apply filter to index field
  - Async SOQL also allow Aggrigate query in big Object
  - Note: *** Async query included only with licensing of additional big object capability ***
  - https://MyDomainName.my.salesforce.com/services/data/v41.0/async-queries/
```
	{ 
	   "query": "SELECT Account__c, In_Game_Purchase__c FROM Customer_Interaction__b WHERE Play_Date__c=2017-09-06T00:00:00Z",

	   "operation": "insert",

	   "targetObject": "Customer_Interaction_Analysis__c", 

	   "targetFieldMap": {"Account__c":"Account__c",
			      "In_Game_Purchase__c":"Purchase__c"
			      },
	   "targetValueMap": {"$JOB_ID":"BackgroundOperationLookup__c",
			      "Copy fields from source to target":"BackgroundOperationDescription__c"
			     }
	}
```
  - return JobId and Status
  
  __ Note: can only insert virtual objects(big, ext) with Database.InsertImmidiate(obj) and insertAsync(obj); __
