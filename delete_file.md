### To delete a file present in a S3 bucket

#### Basic Syntax:
>`client_objectName.delete_object(Bucket = bucketName, Key=fileToDelete)`
    
1. **client_objectName**: this is the boto3 s3 object. (Replace *client_objectName* with the name of the s3 client object you create)
2. **delete_object**: the function name to delete objects in a s3 folder.
3. **Bucket**: the name of the bucket in s3. (Replace *bucketName* with the name of your s3 bucket)
4. **Key**: the input file name present in the bucket. (Replace *fileToDelete* with the name of the file to delete)



#### Example:
> `s3_object.delete_object(Bucket = 'test-bucket', Key = 'data1.csv')`

>
**s3_object** is the name of s3 client object created using boto3. 
**test-bucket** is the name of the bucket where the object is present.
**data1.csv**  is the file which needs to be deleted.

Above was a simple example to delete a file present in a bucket's folder.
But what if we want to delete object which is present in the subdirectories of an s3 bucket

#### Example:
> `s3_object.delete_object(Bucket = 'test-bucket', Key = 'folder1/folder2/data1.csv')`

>
**s3_object** is the name of s3 client object created using boto3. 
**test-bucket** is the name of the bucket where the object is present.
**data1.csv** is the file which is present in *folder1/folder2* buckets subdirectories.


**NOTE**: Whenever we have to use file inside sub directories of a bucket, we have to add those in *Key* argument and not in *Bucket* argument.