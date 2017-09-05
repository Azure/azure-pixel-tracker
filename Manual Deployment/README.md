
#Marketing Pixel Tracker Solution ![][pixel-1]

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Steps](#setup-steps)
   - [General](#setup-steps)
   - [Azure Event Hubs](#eventhub)
   - [Azure Stream Analytics](#asa)
   - [Azure Web App](#webapp)
- [PowerBI Dashboard](#powerbi-dashboard)
   - [Local setup](#pbilocal)
   - [Publishing](#pbipublish)

## Introduction

The objective of this Guide is to demonstrate... .  Retailers can use these predictions to... . This tutorial also shows how .

The end-to-end solution is implemented in the cloud, using Microsoft Azure. The solution is composed of several Azure components, including data ingest, data storage, data movement, advanced analytics and visualization.

This deployment guide will walk you through the steps of creating a pixel tracker solution, including:

- 

## Prerequisites

The steps described later in this guide require the following prerequisites:

1.  An [Azure subscription](https://azure.microsoft.com/en-us/) with login credentials

## Architecture

Figure 1 illustrates the Azure architecture that we will create.

![Figure 1: Architecture](media/architecture.png)
Figure 1: Architecture

## Setup Steps

The following are the steps to deploy the end-to-end solution for the predictive pipelines.

### Accessing Files in the Git Repository

This tutorial will refer to files available in the Technical Deployment Guide section of the [Cortana Intelligence Pixel Tracker solution git repository](). You can download all of these files at once by clicking the "Clone or download" button on the repository.

You can download or view individual files by navigating through the repository folders. If you choose this option, be sure to download the "raw" version of each file by clicking the filename to view it, then cliking Download.

### Choose a Unique String

You will need a unique string to identify your deployment because some Azure services, e.g. Azure Storage requires a unique name for each instance across the service. We suggest you use only letters and numbers in this string and the length should not be greater than 9.
 
We suggest you use "[UI]pixel[N]"  where [UI] is the user's initials,  N is a random integer that you choose and characters must be entered in in lowercase. Please open your memo file and write down "unique:[unique]" with "[unique]" replaced with your actual unique string.

### Create an Azure Resource Group

1. Log into the [Azure Management Portal](https://ms.portal.azure.com).
2. Click **Resource groups** button on upper left, and then click **+** button to add a resource group.
3. Enter your **unique string** for the resource group and choose your subscription.
4. For **Resource Group Location**, you should choose one of the following as they are the locations that offer all of the Azure services used in this guide:
  - East US 2

Please open your memo file and save the information in the form of the following table. Please replace the content in [] with its actual value.  

| **Azure Resource Group** |                     |
|------------------------|---------------------|
| resource group name    |[unique]|
| region              |[region]||

### Instruction for Finding Your Resource Group Overview

In this tutorial, all resources will be generated in the resource group you just created. You can easily access these resources from the resource group overview page, which can be accessed as follows:

1. Log into the [Azure Management Portal](https://ms.portal.azure.com).
2. Click the **Resource groups** button on the upper-left of the screen.
3. Choose the subscription your resource group resides in.
4. Search for (or directly select) your resource group in the list of resource groups.
 
Note that you may need to close the resource description page to add new resources.

In the following steps, if any entry or item is not mentioned in the instruction, please leave it as the default value.

<a name="eventhub"></a>
### Create an Azure Event Hub
1. Go to the [Azure Portal](https://ms.portal.azure.com) and navigate to your resource group.
2. In "Overview" panel, click **+ Add** to add a new resource. Type **Event Hubs** and hit "Enter" key to search.
3. Click on **Event Hubs** offered by Microsoft in the "Internet of Things" category.
4. Click **Create** at the bottom of the description panel.
5. In the new panel for creating a namespace, enter your **unique string** for "Name".
6. Leaving the default values for all other fields, click the **Create** button at the bottom of the panel.
7. Return to your resource group's overview page. When it has finished deploying, click on the resource of type "Event hubs".
9. Click the **+ Event Hub** button to add an event hub.
10. In the new panel:
    1. Enter **pixeltracker** for "Name".
    2. Enter **4** for "Partition Count".
    3. Enter **2** for "Message Retention".
    4. Click **Create** at the bottom.
11. Click on the ***Event Hubs*** option in the menu bar at left (under the "Entities" heading).
12. Click on the event hub named **pixeltracker** created through the previous steps. In the new panel:
    1. Click ***Shared access policies*** in the left-hand menu bar (under the ***SETTINGS*** heading).
    2. In the new panel,  click **+ Add** to add a new policy.
    3. In the new panel:
        1. Enter **sendreceive** for the "Policy name".
        2. Check **Send** and **Listen**.
        3. Click **Create** at the bottom of the panel.
        4. Wait until the new policy is created and shown in the listing of "Shared access policies".
        5. Click the policy **sendreceive**, and save the "PRIMARY KEY" to the memo table given below.

| **Azure Event Hub** |                        |
|---------------------|------------------------|
| EventHubServiceNamespace | [unique string]  |
| Event Hub           |  pixeltracker           |
| EventHubServicePolicy  |     sendreceive                 |
| EventHubServiceKey       |  [PrimaryKey]     ||

<a name="datalake"></a>
### Create an Azure Data Lake
1. Go to the [Azure Portal](https://ms.portal.azure.com) and navigate to your resource group.
2. In ***Overview*** panel, click **+ Add** to add a new resource. Enter **Data Lake Store** and hit "Enter" key to search.
3. Click on **Data Lake Store** offered by Microsoft in the "Storage" category.
4. Click **Create** at the bottom of the description panel.
5. Enter your **unique string** in the "Name" field.
6. Select "Use existing" for "Resource Group" and enter **unique string**.
7. Click **Create** at the bottom.

<a name="asa"></a>
### Create an Azure Stream Analytics Job
1. Go to the [Azure Portal](https://ms.portal.azure.com) and navigate to your resource group.
2. In ***Overview*** panel, click **+ Add** to add a new resource. Enter **Stream Analytics job** and hit "Enter" key to search.
3. Click on **Stream Analytics job** offered by Microsoft in the "Internet of Things" category.
4. Click **Create** at the bottom of the description panel.
5. Enter your **unique string** in the "Job name" field.
6. Click **Create** at the bottom.
7. Return to your resource group and refresh the listing until your Stream Analytics job appears, indicating that deployment has finished. Click on the Stream Analytics job's name.
9. In the Stream Analytics job overview panel, click **Inputs**.
10. In the new panel, click **+ Add** to add an input. In the "New input" panel:
    1. Enter  **eventHub** for the "Input alias".
    2. Choose **Data Stream** for the "Source type".
    3. Choose **Event hub** for the "Source".
    5. Choose your **unique string** for the "Service bus namespace".
    6. Choose **pixeltracker** for the "Event hub name".
    7. Choose **sendreceive** as the "Event hub policy name".
    8. Leaving other fields on their default values, click the **Create** button at the bottom.
10. Return to the Stream Analytics job overview panel and click **Outputs**.
:10. In the new panel, click **+ Add** to add an output. In the "New output" panel:
    1. Enter **datalake** for the "Output alias".
    2. Choose **Data Lake Store** for the "Sink".
    3. Click **Authorize**
    4. Enter "pixeldata/{date}/{time}/" for "Path prefix pattern".
    5. Click the **Create** button at the bottom.
11. Return to the Stream Analytics job overview panel and click **Query**.
11. In the new panel, click **Query** and click **+** in the new panel. Remove the default query content and enter:

    ```
    SELECT
    *
    INTO
        datalake
    FROM 
        eventHub;
    ```
    
    Click the **Save** icon to save the query.
    **[Note]: The input and output aliases are used in the query, and the selected column names must exactly match those in the Activities table.**
12. Return to the overview of the Stream Analytics job and click the "Start" button.
13. In the new panel, choose "Now" for the "Job output start time".
14. Click the **Start** button at the bottom.  

<a name="webapp"></a>
### Set up Azure Web Job

1. Go to the [Azure Portal](https://ms.portal.azure.com) and navigate to your resource group.
2. In ***Overview*** panel, click **+ Add** to add a new resource. Enter **Web App** and hit "Enter" key to search.
3. In the result list, click on **Web App** offered by Microsoft in the "Web + Mobile" category.
4. Click the **Create** button at the bottom of the description panel.
5. In the new panel:
    1. Enter your **unique string** for the "App name".
    3. Click **App Service Plan/Location**. In the new **App Service Plan** panel:
        1. Click **Create New** and enter your unique string for the **App Service Plan**.
        2. Choose the region where your resource group resides.
        3. Click **OK** at the bottom of the panel.
6. Turn On "Application Insights".
7. Click the **Create** button at the bottom.
8. Return to the resource group overview. Refresh the resource listing until the app service deployment completes (usually takes around two minutes).
9. Click on the App Service resource whose type is "App Service" (not "App Service Plan") in your resource group to load its overview panel.
10. In the left-side panel, search for "Application Settings" and click on the **Application Settings** result. In the new panel:
    1. Choose **Java 8** for the Java version.
    2. Choose "Newest Tomcat 8.5" for the Web container.
    3. Choose **64-bit** for the Platform.
    4. Toggle **On** for the "Always on" setting.
    5. Toogle **Off* ARR Affinity.
    5. In the **App settings** section, add the following key-value pairs (using values recorded in your Azure Event Hub memo table) and leave the default entry as it is:

      | **Azure App Service Settings** |        | 
      |------------------------|---------------------|
      | EventHubServiceNamespace |[unique string]          |
      | EventHub              |pixeltracker         |
      | EventHubServicePolicy              |sendreceive         |
      |    EventHubServiceKey           |[unique string]            ||

  Note that the value for "EventHubServiceNamespace" is the **unique string** only, without the extension "servicebus.windows.net." Click **Save** and return to the App Service overview panel.

11. Navigate to the Kudu Console for your Web App at **unique string**.scm.azurewebsites.net
12. Go to "Debug console", then select "CMD"
13. Navigate to "D:\home\site\wwwroot"
14. Upload "cortana-pixeltracker-server-1.0-SNAPSHOT.jar" and "web.config" from the resources folder to this location. 
15. Navigate to **uniquestring**.azurewebsites.net/pixel.jpg?var1=test&var2=test2 to test the service. 

[pixel-1]: (http://pixeltracker.azurewebsites.net/pixel.gif&page=Techincal+Deployment+Guide%2FREADME.MD&section=Summary)
