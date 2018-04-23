--------

This guide is for the Snowball \(50 TB or 80 TB of storage space\)\. If you are looking for documentation for the Snowball Edge, see the [AWS Snowball Edge Developer Guide](http://docs.aws.amazon.com/snowball/latest/developer-guide/whatisedge.html)\.

--------

# Importing Data from HDFS<a name="importing-hdfs"></a>

You can import data into Amazon S3 from your on\-premises Hadoop Distributed File System \(HDFS\) through a Snowball\. You perform this import process by using the Snowball client\. Importing from HDFS is not supported with the Amazon S3 Adapter for Snowball\. Following, you can find information about how to prepare for and perform HDFS data transfer\.

Although you can write HDFS data to a Snowball, you can't write Hadoop data from a Snowball to your local HDFS\. As a result, export jobs are not supported for HDFS\.

If you have a large number of small files, say under a megabyte each in size, then transferring them all at once has a negative impact on your performance\. This performance degradation is due to per\-file overhead when you transfer data from HDFS clusters\.

**Important**  
The `batch` option for the Snowball client's copy command is not supported for HDFS data transfers\. If you must transfer a large number of small files from an HDFS cluster, we recommend that you find a method of collecting them into larger archive files, and then transferring those\. However, these archives are what is imported into Amazon S3\. If you want the files in their original state, take them out of the archives after importing the archives\.

## Preparing for Transferring Your HDFS Data with the Snowball Client<a name="preparing-hdfs"></a>

Before you transfer your HDFS \(version 2\.x\) data, do the following:
+ **Confirm the Kerberos authentication settings for your HDFS cluster** – The Snowball client supports Kerberos authentication for communicating with your HDFS in two ways: with the Kerberos login already on the host system and with authentication through specifying a principal and keytab in the `snowball cp` command\. The following HDFS/Kerberos encryption types are known to work with Snowball:
  + des3\-cbc\-sha1\-kd
  + aes\-128\-cts\-hmac\-sha1\-96
  + 256\-cts\-hmac\-sha1\-96
  + rc4\-hmac \(arcfour\-hmac\)

  Alternatively, you can copy from an unsecured HDFS cluster\.
+ **Confirm that your workstation has the Hadoop client 2\.x version installed on it** – To use the Snowball client, your workstation needs to have the Hadoop client 2\.x installed, running, and able to communicate with your HDFS 2\.x cluster\.
+ **Confirm the location of your site\-specific configuration files** – If you are using site\-specific configuration files, you need to use the `--hdfsconfig` parameter to pass the location of each XML file\.
+ **Confirm your Namenode URI** – Each HDFS 2\.x cluster has a Namenode\.core\-site\.xml file\. This file includes a `property` element with the name of `fs.defaultFS` and a value of `IP Address:port`, for example `hdfs://192.0.2.0:9000`\. You use this value, the Namenode URI, as a part of the source schema when you run Snowball client commands to perform operations on your HDFS cluster\. For more information, see [Sources for the Snowball Client Commands](using-client.md#client-source-schemas)\.

**Note**  
Currently, only HDFS 2\.X clusters are supported with Snowball\. You can still transfer data from an HDFS 1\.x cluster by staging the data that you want to transfer on a workstation, and then copying that data to the Snowball with the standard `snowball cp` commands and options\.

When you have confirmed the information listed previously, identify the Amazon S3 bucket that you want your HDFS data imported into\.

After your preparations for the HDFS import are complete, you can begin\. If you haven't created your job yet, see [Importing Data into Amazon S3 with AWS Snowball](create-import-job-steps.md) until you reach [Use the AWS Snowball Client](transfer-data.md#transferthroughclient)\. At that point, return to this topic\.

## Before Transferring Data from HDFS<a name="beforehdfs"></a>

Before using the Snowball client to copy HDFS \(version 2\.x\) data, take the following steps:

1. To transfer data from an HDFS cluster, get the latest version of the Snowball client\. You can download and install the Snowball client from the [AWS Snowball Tools Download](http://aws.amazon.com/snowball/tools) page\. There you can find the installation package for your operating system\. Follow the instructions to install the Snowball client\.

1. Ensure that your HDFS cluster is running, and accessible from the workstation that you've installed the Snowball client on\.

## Transferring Data from HDFS<a name="transferhdfs"></a>

Now you're ready to transfer data from your HDFS \(version 2\.x\) cluster\. For more information on all the Snowball client copy command options, including those specific to HDFS, see [Options for the snowball cp Command](copy-command-reference.md)\.

If you encounter performance issues while transferring data from your HDFS 2\.x cluster to a Snowball, see [Performance Considerations for HDFS Data Transfers](performance.md#hdfs-performance)\.

## After Transferring Data from HDFS<a name="afterhdfs"></a>

Once you've finished transferring data from your HDFS \(version 2\.x\) cluster, you can validate the data on the Snowball with the following steps:

1. Use the `snowball validate` command to verify the number of uploaded files and confirm that they were uploaded correctly\. 

1. List all the files at the destination path or paths to confirm that the HDFS file or files were copied\. For example, you can use the following command:

   ```
   snowball ls s3://bucket-name/destination-path
   ```