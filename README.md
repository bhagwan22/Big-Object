# Big Object:
> Standard:
    defined by Salesforce and are included in Salesforce products. 
    "FieldHistoryArchive" part of our Field Audit Trail product, is an example of a standard big object. FieldHistoryArchive allows you to store up to 10 years’ worth of archived field history data, helping you comply with industry regulations related to auditing and data retention.
	
> Custom big objects: 
      are defined and deployed in Setup. where we set its definition, fields, and index.
 
 ** Points **
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
				 - SOQL to query big object, we can query only on the fields that make up our index, in the order you defined them in.

	- big object support below field type: lookup, dateTime, text, email, phone, Number, longText
	- apex use "databse.insertImmediate()" to upsert big object record based on primary key

