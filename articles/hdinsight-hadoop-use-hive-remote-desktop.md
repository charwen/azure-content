<properties
   pageTitle="Use Hadoop Hive in HDInsight | Azure"
   description="Learn how to use Hive with HDInsight through Remote Desktop."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang=""
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="02/18/2015"
   ms.author="larryfr"/>

# Use Hive with Hadoop on HDInsight with Remote Desktop

[AZURE.INCLUDE [hive-selector](../includes/hdinsight-selector-use-hive.md)]

In this article, you will learn how to connect to an HDInsight cluster using Remote Desktop and then run Hive queries using the Hive Command-Line Interface (CLI).

> [AZURE.NOTE] This document does not provide a detailed description of what the HiveQL statements used in the examples do. For information on the HiveQL used in this example, see <a href="../hdinsight-use-hive/" target="_blank">Use Hive with Hadoop on HDInsight</a>.

##<a id="prereq"></a>Prerequisites

To complete the steps in this article, you will need the following.

* A Windows-based HDInsight (Hadoop on HDInsight) cluster

* A Windows 7 or newer client OS

##<a id="connect"></a>Connect with remote desktop

Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at <a href="http://azure.microsoft.com/en-us/documentation/articles/hdinsight-administer-use-management-portal/#rdp" target="_blank">Connect to HDInsight clusters using RDP</a>.

##<a id="hive"></a>Use the Hive command

Once connected to the desktop for the HDInsight cluster, use the following steps to work with Hive.

1. From the HDInsight desktop, start the **Hadoop Command Line**.

2. Enter the following command to start the Hive CLI.

        %hive_home%\bin\hive

    Once the CLI has started, you will see the Hive CLI prompt - `hive>`.

3. Using the CLI, enter the following statements to create a new table named **log4jLogs** using the sample data.

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;

    These statements perform the following actions

    * **DROP TABLE** - deletes the table and the data file, in case the table already exists
    
    * **CREATE EXTERNAL TABLE** - creates a new 'external' table in Hive. External tables only store the table definition in Hive - the data is left in the original location

		> [AZURE.NOTE] External tables should be used when you expect the underlying data to be updated by an external source, such as an automated data upload process, or by another MapReduce operation, but always want Hive queries to use the latest data.
    	>
    	> Dropping an external table does **not** delete the data, only the table definition.
    
	* **ROW FORMAT** - tells Hive how the data is formatted. In this case, the fields in each log are separated by a space
	
    * **STORED AS TEXTFILE LOCATION** - tells Hive where the data is stored (the example/data directory,) and that it is stored as text
    
    * **SELECT** - select a count of all rows where column **t4** contain the value **[ERROR]**. This should return a value of **3** as there are three rows that contain this value


4. Use the following statements to create a new 'internal' table named **errorLogs**.

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

    These statements perform the following actions.

    * **CREATE TABLE IF NOT EXISTS** - creates a table, if it does not already exist. Since the **EXTERNAL** keyword is not used, this is an internal table, which is stored in the Hive data warehouse and is managed completely by Hive
    
		> [AZURE.NOTE] Unlike **EXTERNAL** tables, dropping an internal table will delete the underlying data as well.
		
    * **STORED AS ORC** - stores the data in Optimized Row Columnar (ORC) format. This is a highly optimized and efficient format for storing Hive data
    
    * **INSERT OVERWRITE ... SELECT** - selects rows from the **log4jLogs** table that contain **[ERROR]**, then insert the data into the **errorLogs** table

    To verify that only rows containing **[ERROR]** in column t4 were stored to the **errorLogs** table, use the following statement to return all the rows from **errorLogs**.

        SELECT * from errorLogs;

    Three rows of data should be returned, all containing **[ERROR]** in column t4.

##<a id="summary"></a>Summary

As you can see, the the Hive command provides an easy way to interactively run Hive queries on an HDInsight cluster, monitor the job status, and retrieve the output.

##<a id="nextsteps"></a>Next steps

For general information on Hive in HDInsight.

* [Use Hive with Hadoop on HDInsight](../hdinsight-use-hive/)

For information on other ways you can work with Hadoop on HDInsight.

* [Use Pig with Hadoop on HDInsight](../hdinsight-use-pig/)

* [Use MapReduce with Hadoop on HDInsight](../hdinsight-use-mapreduce/)


[1]: ../hdinsight-hadoop-visual-studio-tools-get-started/

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/en-us/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/en-us/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/en-us/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/en-us/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/en-us/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: ../hdinsight-use-oozie/
[hdinsight-analyze-flight-data]: ../hdinsight-analyze-flight-delay-data/



[hdinsight-storage]: ../hdinsight-use-blob-storage

[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-submit-jobs]: ../hdinsight-submit-hadoop-jobs-programmatically/
[hdinsight-upload-data]: ../hdinsight-upload-data/
[hdinsight-get-started]: ../hdinsight-get-started/

[Powershell-install-configure]: ../install-configure-powershell/
[powershell-here-strings]: http://technet.microsoft.com/en-us/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
