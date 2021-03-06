<properties 
	pageTitle="Connect Excel to Hadoop with Power Query | Azure" 
	description="Learn how to take advantage of business intelligence components and use Excel to access data stored in Azure HDInsight using Power Query." 
	services="hdinsight" 
	documentationCenter="" 
	authors="bradsev" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/20/2015" 
	ms.author="bradsev"/>




#Connect Excel to Hadoop with Power Query

One key feature of Microsoft's big data solution is the integration of  Microsoft Business Intelligence (BI) components with Hadoop clusters in HDInsight. A primary example of this integration is the ability to connect Excel to the Azure storage account containing the data associated with your Hadoop cluster by using Microsoft Power Query for Excel. This article walks you through how to set up and use Power Query from Excel to query data associated with an Hadoop cluster managed with HDInsight. 

**Prerequisites**

Before you begin this article, you must have the following:

- A HDInsight cluster. To configure one, see [Get started with Azure HDInsight][hdinsight-get-started].
- A computer that is running Windows 7, Windows Server 2008 R2, or above.
- Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.

## In this article

- [Install Microsoft Power Query for Excel](#InstallPowerQuery)
- [Import data into Excel](#ImportData)
- [Next steps](#NextSteps)


## <a id="InstallPowerQuery"></a>Install Microsoft Power Query for Excel

Power Query can be used to import data from a variety of sources into Microsoft Excel, where it can power Business Intelligence (BI) tools like PowerPivot and Power View. In particular, Power Query can import data that has been output or that has been generated by a Hadoop job running on an HDInsight cluster. 

Download Microsoft Power Query for Excel from the [Microsoft Download Center][powerquery-download] and install it.

## <a id="ImportData"></a>Import HDInsight data into Excel

The Power Query add-in for Excel makes it easy to import data from your HDInsight cluster into Excel where business intelligence tools such as PowerPivot and Power Map may be used to inspect, analyze, and present the data.

**To import data from an HDInsight cluster**

1. Open Excel.

2. Create a new blank workbook.

3. Click the **Power Query** menu, click **From Other Sources**, and then click **From Azure HDInsight**. 

	![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

	Note: If you don't see the **Power Query** menu, go to **File** > **Options** > **Add-Ins**, and select **COM Addins** from the drop-down **Manager** box at the bottom of the page. Select the **Go...** button and verify that the box for the Microsoft Office Power Query for Excel Add-In has been checked.

3. Enter the **Account Name** of the Azure Blob storage account associated with your cluster, and then click **OK**. 

4. Enter the **Account Key** for the Blob storage account, and then click **Save**. (You need to do this only the first time you access this store.)	

5. In the **Navigator** pane on the left of the **Query Editor**, double-click the Blob storage container name. By default the container name is the same name as the cluster name. 

6. Locate **HiveSampleData.txt** in the **Name** column (the folder path is **../hive/warehouse/hivesampletable/**), and then click **Binary** on the left of HiveSampleData.txt.

	![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. If you want, you can rename the column names. When you are ready, click **Apply & Close**.	

	![HDI.PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a id="NextSteps"></a>Next steps

In this article you learned how to use Power Query to retrieve data from HDInsight into Excel. Similarly, you can retrieve data from HDInsight into SQL Azure. It is also possible to upload data into HDInsight. To learn more, see the following articles:

* [Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-ODBC]
* [Upload Data to HDInsight][hdinsight-upload-data].

[hdinsight-ODBC]: ../hdinsight-connect-excel-hive-ODBC-driver/
[hdinsight-get-started]: ../hdinsight-get-started/
[hdinsight-upload-data]: ../hdinsight-upload-data/

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png 
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG 

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689 
