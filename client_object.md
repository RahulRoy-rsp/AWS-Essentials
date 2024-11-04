### To create a boto3 client object

#### Basic Syntax:

> `import boto3`
`client_objectName = boto3.client("clientName")`
    
1. **client_objectName**: this is the boto3 s3 object. (Replace *client_objectName* with the name of the s3 client object you create)
2. Replace clientName with the client name, i.e the AWs service you want to use.

#### Example:
> `s3_object = boto3.client("s3")`

**s3_object** is the name of s3 client object created using boto3. 
As we want to use s3 services, we passed the argument as *s3*