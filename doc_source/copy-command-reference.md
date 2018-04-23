--------

This guide is for the Snowball \(50 TB or 80 TB of storage space\)\. If you are looking for documentation for the Snowball Edge, see the [AWS Snowball Edge Developer Guide](http://docs.aws.amazon.com/snowball/latest/developer-guide/whatisedge.html)\.

--------

# Options for the snowball cp Command<a name="copy-command-reference"></a>

Following, you can find information about `snowball cp` command options and also syntax guidelines for using this command\. You use this command to transfer data from your workstation to a Snowball\.


| Command Option | Description | 
| --- | --- | 
| \-b, \-\-batch | String\. Significantly improves the transfer performance for small files by batching them into larger `.snowballarchives` files\. Batching is on by default\. You can change the following defaults to specify when a file is included in a batch: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/snowball/latest/ug/copy-command-reference.html) During import into Amazon S3, batches are extracted and the original files are imported into Amazon S3\. Only `.snowballarchives` files that were created during the copy command with this option are extracted automatically during import\.   | 
| \-\-checksum | On and set to false by default\. Calculates a checksum for any source and destination files with the same name, and then compares the checksums\. This command option is used when a copy operation is resumed\. Using this option adds computational overhead during your copy operation\.  When this option isn't used, a faster comparison of just file names and dates occurs when you resume as copy operation\.   | 
| \-f, \-\-force | On and set to false by default\. This command option has two uses: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/snowball/latest/ug/copy-command-reference.html)  The preceding use cases are not mutually exclusive\. We recommend that you use `-f` with care to prevent delays in data transfer\.   | 
| \-h, \-\-help | On and set to false by default\. Displays the usage information for the `snowball cp` command in the terminal\. | 
| \-\-noBatch | String\. Disables automatic batching of small files\. If you're copying a directory, and you use this option, you must also use the `--recursive` option\. This option is hidden\. For performance reasons, we don't recommend that you use it unless your use case requires it\.  | 
| \-r, \-\-recursive | On and set to false by default\. Recursively traverses directories during the `snowball cp` command's operation\. | 
| \-s, \-\-stopOnError | On and set to false by default\. Stops the `snowball cp` command's operation if it encounters an error\. | 

In addition to the previously defined Snowball client copy command options, there are some options specific to transferring data from an HDFS cluster\. The following table describes those options\. For more information on transferring from an HDFS cluster, see [Importing Data from HDFS](importing-hdfs.md)\.

**Important**  
The `--batch` option for the Snowball client's copy command is not supported for HDFS data transfers\. If you must transfer a large number of small files from an HDFS cluster, we recommend that you find a method of collecting them into larger archive files, and then transferring those\. However, these archives are what is imported into Amazon S3\. If you want the files in their original state, take them out of the archives after importing the archives\.


| HDFS\-Specific Command Option | Description | 
| --- | --- | 
| \-\-hdfsconfig |  Used with the `hdfs://` import schema, this option sets the path to a custom XML configuration file on the server running your HDFS cluster\. This option must be repeated if you have multiple configuration files\. For example, the following specifies two configuration files\. `--hdfsconfig src/core/Namenode-site.xml --hdfsconfig /hdfs/corp/conf/hdfs-site.xml`  | 
| \-k | On and set to false by default\. Used with the `hdfs://` import schema and the `-p` option, this option sets the path to the keytab file used to authenticate the Snowball client's connection to the HDFS cluster before copying data to a Snowball\.  You must have both the principal and the keytab registered with the Kerberos authentication server used to authenticate the HDFS cluster\. If you recently ran the `kinit` command on your terminal, then you don't need to specify this option\.   | 
| \-n | On and set to false by default\. Used with the `hdfs://` import schema, this option copies data from a nonsecure HDFS cluster\. | 
| \-p | On and set to false by default\. Used with the `hdfs://` import schema and the `-k` option, this option sets the principal used to authenticate the Snowball client's connection to the HDFS cluster before then copying data to a Snowball\.  You must have both the principal and the keytab registered with the Kerberos authentication server used to authenticate the HDFS cluster\. If you recently ran the `kinit` command on your terminal, then you don't need to specify this option\.  | 