--------

This guide is for the Snowball \(50 TB or 80 TB of storage space\)\. If you are looking for documentation for the Snowball Edge, see the [AWS Snowball Edge Developer Guide](http://docs.aws.amazon.com/snowball/latest/developer-guide/whatisedge.html)\.

--------

# Transferring Data with the Amazon S3 Adapter for Snowball<a name="snowball-transfer-adapter"></a>

The Amazon S3 Adapter for Snowball is a programmatic tool that you can use to transfer data between your on\-premises data center and a Snowball\. It replaces the functionality of the Snowball client\. As with the Snowball client, you need to download the adapter's executable file from the [AWS Snowball Resources](http://aws.amazon.com/snowball/resources/) page, install it, and run it from your computer workstation\. When programmatically transferring data to a Snowball, all data goes through the Amazon S3 Adapter for Snowball, without exception\.

We highly recommend that your workstation be a powerful computer\. It should be able to meet high demands in terms of processing, memory, and networking\. For more information, see [Workstation Specifications](specifications.md#workstationspecs)\.

## Downloading and Installing the Amazon S3 Adapter for Snowball<a name="adapter-install"></a>

You can download and install the Amazon S3 Adapter for Snowball from the [AWS Snowball Resources](http://aws.amazon.com/snowball/resources/) page\. Once there, find the installation package for your operating system, and follow the instructions to install the Amazon S3 Adapter for Snowball\. Running the adapter from a terminal in your workstation might require using a specific path, depending on your operating system\.

To install the adapter, first download the snowball\-adapter\-*operating\_system*\.zip file from the [AWS Snowball Resources](http://aws.amazon.com/snowball/resources/) page\. Unzip the file, and navigate the extracted folder\. For the Mac and Linux versions of the adapter, to make the snowball\-adapter file executable, change the permissions on this file within the bin directory with the following commands\.

`chmod +x snowball-adapter`

**To confirm the adapter was installed successfully**

1. Open a terminal window on the workstation with the installed adapter\.

1. Navigate to the directory where you installed the snowball\-adapter\-*operating\_system* subdirectory\.

1. Navigate to snowball\-adapter\-*operating\_system*/bin\.

1. Type the following command to confirm that the adapter was installed correctly: `./snowball-adapter --help`\. 

   If the adapter was successfully installed, its usage information appears in the terminal\.

Installing the adapter also adds the snowball subdirectory to your \.aws directory\. Within this snowball directory, you can find the logs and config subdirectories\. Their contents are as follows:
+ The logs directory is where you find the log files for your data transfers to the Snowball through the Amazon S3 Adapter for Snowball\.
+ The config directory contains two files:
  + The snowball\-adapter\-logging\.config file contains the configuration settings for the log files written to the \~/\.aws/snowball/logs directory\.
  + The snowball\-adapter\.config file contains the configuration settings for the adapter itself\.

**Note**  
The \.aws directory is located at \~/\.aws in Linux, OS X, or Unix, or at C:\\User\\*USERNAME*\\\.aws on Windows\.