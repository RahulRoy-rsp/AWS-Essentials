### To upload a file present in a S3 bucket

#### Basic Syntax:
>`client_objectName.put_object(Bucket = bucketName, Key=fileToUpload, Body = name_of_the_file_after_uploading)`
    
1. **client_objectName**: this is the boto3 s3 object. (Replace *client_objectName* with the name of the s3 client object you create)
2. **put_object**: the function name to put objects in a s3 folder.
3. **Bucket**: the name of the bucket in s3. (Replace *bucketName* with the name of your s3 bucket)
4. **Key**: the input file name present in the bucket. (Replace *fileToUpload* with the name of the input file)
5. **Body**: the output file name. (Replace *name_of_the_file_after_uploading* with the name of your s3 bucket)


#### Example:
> `s3_object.put_object(Bucket = 'test-bucket', Key = 'data1.csv', Body = 'output.csv')`

>
**s3_object** is the name of s3 client object created using boto3. **test-bucket** is the name of the bucket where the object will be stored.
**data1.csv** (source) is the input file that will be put into the destination.
**output.csv** (destination) will be the name of the file that will be given once uploaded.

Above was a simple example to put the file present in a bucket's folder.
But what if we want to put object which is present in the subdirectories of an s3 bucket

#### Example:
> `s3_object.put_object(Bucket = 'test-bucket', Key = 'folder1/folder2/data1.csv', Body = 'output.csv')`

>
**s3_object** is the name of s3 client object created using boto3. **test-bucket** is the name of the bucket where the object will be stored.
**data1.csv** (source) is the input file which is present in *folder1/folder2* buckets subdirectories that will be put into the destination.
**output.csv** (destination) will be the name of the file that will be given once uploaded.


**NOTE**: Whenever we have to use file inside sub directories of a bucket, we have to add those in *Key* argument and not in *Bucket* argument.