--------

This guide is for the Snowball \(50 TB or 80 TB of storage space\)\. If you are looking for documentation for the Snowball Edge, see the [AWS Snowball Edge Developer Guide](http://docs.aws.amazon.com/snowball/latest/developer-guide/whatisedge.html)\.

--------

# Troubleshooting for a Standard Snowball<a name="troubleshooting"></a>

The following can help you troubleshoot problems that you might have with an AWS Snowball \(Snowball\)\. If you're having trouble establishing a connection to a Snowball, see [Why can’t my AWS Snowball appliance establish a connection with the network?](https://aws.amazon.com/premiumsupport/knowledge-center/troubleshoot-connect-snowball/) in the *AWS Knowledge Center*\.

## Troubleshooting Connection Problems<a name="connection-troubleshooting"></a>

The following can help you troubleshoot issues you might have with connecting to your Snowball\. 
+ Routers and switches that work at a rate of 100 megabytes per second won't work with a Snowball\. We recommend that you use a switch that works at a rate of 1 GB per second \(or faster\)\. 

## Troubleshooting Data Transfer Problems<a name="transfer-troubleshooting"></a>

If you encounter performance issues while transferring data to or from a Snowball, see [Performance for AWS Snowball](performance.md) for recommendations and guidance on improving transfer performance\. The following can help you troubleshoot issues you might have with your data transfer to or from a Snowball\.
+ Data can't be transferred into the root folder of the Snowball\. If you're having trouble transferring data into the Snowball, make sure that you're transferring data into a folder on the Snowball that is not the root folder\.
+ For security purposes, data transfers must be completed within 90 days of the Snowball being prepared\. After 90 days, the Snowball becomes locked to additional on\-premises data transfers\. If the Snowball becomes locked during a data transfer, return the Snowball and create a new job to transfer the rest of your data\. If the Snowball becomes locked during an import job, we can still transfer the existing data on the Snowball into Amazon S3\.
+ Objects transferred onto Snowballs have a maximum key length of 933 bytes\. Key names that include characters that take up more than one byte each still have a maximum key length of 933 bytes\. When determining key length, you include the file or object name and also its path or prefixes\. Thus, files with short file names within a heavily nested path can have keys longer than 933 bytes\. The bucket name is not factored into the path when determining the key length\. Some examples follow\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/snowball/latest/ug/troubleshooting.html)

  If a key's length is larger than 933 bytes, you see the following error message when you try to copy the object to a Snowball:

  ```
  Failed to copy the following file: <Name of object with a keylength over 933 bytes>
            PARENT_NOT_FOUND:
  ```

  If you receive this error message, you can resolve the issue by reducing the object's key length\.
+ If you're using Linux and you can't upload files with UTF\-8 characters to a Snowball, it might be because your Linux workstation doesn't recognize UTF\-8 character encoding\. You can correct this issue by installing the `locales` package on your Linux workstation and configuring it to use one of the UTF\-8 locales like `en_US.UTF-8`\. You can configure the `locales` package by exporting the environment variable `LC_ALL`, for example: `export LC_ALL=en_US.UTF-8`
+ If you encounter unexpected errors during data transfer to the Snowball, we want to hear about it\. Make a copy of your logs and include them along with a brief description of the issues that you encountered in a message to AWS Support\. For more information about logs, see [Snowball Logs](using-client.md#snowballlogs)\.

## Troubleshooting Client Problems<a name="client-troubleshooting"></a>

The following can help you troubleshoot issues with the Snowball client\.
+ If you're having trouble using the Snowball client, type the command `snowball help` for a list of all available actions for that tool\.
+ Although you can run multiple instances of the Snowball client at the same time, each instance of the client requires up to 7 GB of dedicated RAM for memory\-intensive tasks, such as performing the `snowball cp` command\. If your workstation runs out of memory as it runs the Snowball client, you see a Java `OutOfMemoryError` exception returned in the terminal window\. You can resolve this issue by freeing up resources on the workstation or increasing the amount of memory for your workstation, and then performing your Snowball client task again\.
+ If you encounter issues while transferring data to a Snowball using the client on a PC running Microsoft Windows Server, it might be due to the Data Deduplication feature in Windows\. If you have the Data Deduplication feature turned on, we recommend that you use the Amazon S3 Adapter for Snowball with the AWS CLI to transfer data instead\. For more information, see [Transferring Data with the Amazon S3 Adapter for Snowball](snowball-transfer-adapter.md)\.

### Troubleshooting Snowball Client Validation Problems<a name="client-validation"></a>

When you transfer data, the copy operation first performs a precheck on the metadata for each file to copy\. If any of the following attributes are true about a file's metadata, then the copy operation stops before it transfers any files:
+ **The size of the file is greater than 5 TB** – Objects in Amazon S3 must be 5 TB or less in size, so files that are larger 5 TB in size can't be transferred to the Snowball\. If you encounter this problem, separate the file into parts smaller than 5 TB, compress the file so that it's within the 5 TB limit, or otherwise reduce the size of the file, and try again\.
+ **The file is a symbolic link, and only contains a reference to another file or directory** – Symbolic links \(or junctions\) can't be transferred into Amazon S3\.
+ **There are permissions issues for access to the file** – For example, a user might be trying to read a file on the Snowball client when that user doesn't have read permissions for that file\. Permissions issues result in precheck failures\.
+ **Object key length too large** –If an object's key length is larger than 933 bytes, it fails the precheck\.

For a list of files that can't be transferred, check the terminal before data copying starts\. You can also find this list in the `<temp directory>/snowball-<random-character-string>/failed-files` file, which is saved to your Snowball client folder on the workstation\. For Windows, this temp directory would be located in `C:/Users/<username>/AppData/Local/Temp`\. For Linux and Mac, the temp directory would be located in `/tmp`\.

If you discover errors when you run the `snowball validate` command, identify the files that failed the transfer, resolve the issues that the error messages report, and then transfer those files again\. If your validation command fails with the same error message, then you can use the `–f` option with the `snowball cp` command to force the copy operation and overwrite the invalid files\.

### HDFS Troubleshooting<a name="hdfs-troubleshooting"></a>

When setting up a data transfer from your HDFS \(version 2\.x\) cluster to a Snowball device, you may encounter Kerberos authentication errors\. This can happen if you're not using one of the verified encryption types known to work with Snowball:
+ des3\-cbc\-sha1\-kd
+ aes\-128\-cts\-hmac\-sha1\-96
+ 256\-cts\-hmac\-sha1\-96
+ rc4\-hmac \(arcfour\-hmac\)

If you've encountered a Kerberos authentication issue, you can attempt to resolve it with one of the following workarounds:
+ **Temporarily disable Kerberos** – If you disable Kerberos on your HDFS cluster, you should also disconnect any non\-essential active connections to the cluster while transferring data\. Once your transfer is complete, reactivate your Kerberos authentication\.
+ **Use a Snowball Edge with the file interface** – The Snowball Edge provides an NFS mount point through it's file interface feature\. You could mount the Snowball Edge, and copy the files from your HDFS cluster\. For more information on using the file interface, see [Using the File Interface for the AWS Snowball Edge](http://docs.aws.amazon.com/snowball/latest/developer-guide/using-fileinterface.html) in the AWS Snowball Edge Developer Guide\. 

## Troubleshooting Adapter Problems<a name="adapter-troubleshooting"></a>

If you're communicating with the Snowball through the Amazon S3 Adapter for Snowball using the AWS CLI, and you encounter an error that says `Unable to locate credentials. You can configure credentials by running "aws configure".` you need to configure your AWS credentials used by the CLI to run commands\. For more information, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

## Troubleshooting Import Job Problems<a name="import-troubleshooting"></a>

Sometimes files fail to import into Amazon S3\. If the following issue occurs, try the actions specified to resolve your issue\. If a file fails import, you might need to try importing it again\. Importing it again might require a new job for Snowball Edge\.

**Files failed import into Amazon S3 due to invalid characters in object names**  
This problem occurs if a file or folder name has characters that aren't supported by Amazon S3\. Amazon S3 has rules about what characters can be in object names\. For more information, see [Object Key Naming Guidelines](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-key-guidelines)\.

**Action to take**  
If you encounter this issue, you see the list of files and folders that failed import in your job completion report\.

In some cases, the list is prohibitively large, or the files in the list are too large to transfer over the internet\. In these cases, you should create a new Snowball import job, change the file and folder names to comply with Amazon S3 rules, and transfer the files again\.

If the files are small and there isn't a large number of them, you can copy them to Amazon S3 through the AWS CLI or the AWS Management Console\. For more information, see [How Do I Upload Files and Folders to an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the *Amazon Simple Storage Service Console User Guide\.*

## Troubleshooting Export Job Problems<a name="export-troubleshooting"></a>

Sometimes files fail to export into your workstation\. If the following issue occurs, try the actions specified to resolve your issue\. If a file fails export, you might need to try exporting it again\. Exporting it again might require a new job for Snowball Edge\.

**Files failed export to a Microsoft Windows Server**  
A file can fail export to a Microsoft Windows Server if it or a related folder is named in a format not supported by Windows\. For example, if your file or folder name has a colon \(`:`\) in it, the export fails because Windows doesn't allow that character in file or folder names\.

**Action to take**

1. Make a list of the names that are causing the error\. You can find the names of the files and folders that failed export in your logs\. For more information, see [Getting Your Job Completion Report and Logs in the Console](report.md)\.

1. Change the names of the objects in Amazon S3 that are causing the issue to remove or replace the unsupported characters\.

1. If the list of names is prohibitively large, or if the files in the list are too large to transfer over the internet, create a new export job specifically for those objects\.

   If the files are small and there isn't a large number of them, copy the renamed objects from Amazon S3 through the AWS CLI or the AWS Management Console\. For more information, see [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) in the* Amazon Simple Storage Service Console User Guide\.*