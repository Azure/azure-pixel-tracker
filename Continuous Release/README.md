# Azure Digitial Marketing - Pixel Tracker Continuous Release Guide

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Steps](#setup-steps)

## Introduction

Tche objective of this Guide is to demonstrate how to configure a continuous release enviromenet in Visual Studio Team services to build and publish the pixel tracker solution.

## Prerequisites

The steps described later in this guide require the following prerequisites:

1.  An [Azure subscription](https://azure.microsoft.com/en-us/) with login credentials
2. A [Visual Studo Team Services](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) account

## Architecture

Figure 1 illustrates the Azure architecture that we will create.

 ![Figure 1: Architecture](../resources/architecture.png)
Figure 1: Architecture

## Setup Steps

The following are the steps to deploy the end-to-end solution for the predictive pipelines.

### Accessing Files in the Git Repository

This tutorial will refer to files available in the [Java](https://github.com/Azure/azure-pixel-tracker-java) and [ARM](https://github.com/Azure/azure-pixel-tracker-arm) submodules of the [Azure Digitial Marketing Pixel Tracker git repository](https://github.com/Azure/azure-pixel-tracker). You can download all of these files at once by clicking the "Clone or download" button on the repository.

You can download or view individual files by navigating through the repository folders. If you choose this option, be sure to download the "raw" version of each file by clicking the filename to view it, then clicking Download.

### Choose a Unique String

You will need a unique string to identify your deployment because some Azure services, e.g. Azure Storage requires a unique name for each instance across the service. We suggest you use only letters and numbers in this string and the length should not be greater than 9.
 
We suggest you use "[UI]pixeltracker[N]"  where [UI] is the user's initials,  N is a random integer that you choose and characters must be entered in in lowercase. Please open your memo file and write down "unique:[unique]" with "[unique]" replaced with your actual unique string.

### Configure the Azure Resource Management template gating build
1. Log into the [Visual Studio Team Services Portal](visualstudio.com/) and navitage to the desired account.
1. Navitage to the desired team's *Code* section.
1. Select *Import repository* from the repository drop down menu.
    1. Enter **https://github.com/Azure/azure-pixel-tracker-arm** for "Clone URL".
     1. During PREVIEW use credentials, and enter your github username and github personal access token
    1. Choose a "Name" for your new repository.
    1. Complete the task by clicking "Import"
1. Create a gating build to validate the master branch before commits can be made.
    1. Select "Set up build" in the "Files" view of your new repository.
    1. Select "Empty process" in the "Select a template" page.
    1. Provide a "Name" for your build, such as **[UI]'s Pixel Tracker ARM Gating Build**
    1. Choose a default agent queue, such as **Hosted**
        1. Advanced users may configure their own build agent.
    1. Select "Add Task", and choose the **Azure Resource Group Deployment** task.
        1. Choose an "Azure subscription" from the drop down menu. 
        1. Provide a name for the Resource Group, such as **[UI]pixeltracker[N]**.
        1. Choose a "Location" for deploying the resource group.
        1. Provide a "Template" location, which by default will be **azuredeploy.json**.
        1. Provide a "Template paramters" location, which by default will be **azuredeploy.parameters.json**.
        1. Change "Deployment mode" from **Incremental** to **Validation only**.
    1. Select "Save & queue".
 1. Configure the gating build to execute when a pull request is created by enabling protection on the master branch.
    1. Return to the "Files" tab of your repository. 
    1. Select "Manage repositories" from the repositories drop down menu. 
    1. Select your repository from the list. 
    1. Select "Master" from the drop down below your repository name. 
    1. Select "Branch Policies" 
        1. Check "Protect this branch", note this will force all code changes to be submitted via pull request for this branch. 
        1. Select "+Add build policy"
            1. Choose the "Build definition" that you created in the previous step.
            1. Select "Save".

### Configure the Java gating build
1. Select *Import repository* from the repository drop down menu.
    1. Enter **https://github.com/Azure/azure-pixel-tracker-java** for "Clone URL".
     1. During PREVIEW use credentials, and enter your github username and github personal access token
    1. Choose a "Name" for your new repository.
    1. Complete the task by clicking "Import"
1. Create a gating build to validate the master branch before commits can be made.
    1. Select "Set up build" in the "Files" view of your new repository.
    1. Select "Empty process" in the "Select a template" page.
    1. Provide a "Name" for your build, such as **[UI]'s Pixel Tracker Java Gating Build**
    1. Choose a default agent queue, such as **Hosted**
        1. Advanced users may configure their own build agent.
    1. Select "Add Task", and choose the **Gradle** task.
        1. Enter *test* for "Tasks"
        1. Change "Test Results Files" to **\*\*/build/test-results/test/TEST-*.xml**
        1. Choose a "Location" for deploying the resource group.
        1. Provide a "Template" location, which by default will be **azuredeploy.json**.
        1. Provide a "Template paramters" location, which by default will be **azuredeploy.parameters.json**.
        1. Change "Deployment mode" from **Incremental** to **Validation only**.
    1. Select "Save & queue".
 1. Configure the gating build to execute when a pull request is created by enabling protection on the master branch.
    1. Return to the "Files" tab of your repository. 
    1. Select "Manage repositories" from the repositories drop down menu. 
    1. Select your repository from the list. 
    1. Select "Master" from the drop down below your repository name. 
    1. Select "Branch Policies" 
        1. Check "Protect this branch", note this will force all code changes to be submitted via pull request for this branch. 
        1. Select "+Add build policy"
            1. Choose the "Build definition" that you created in the previous step.
            1. Select "Save".   
            
### Configure the Java artifacts build
1. From the "Build" tab, select "+New"
1. Select "Empty process" in the "Select a template" page.
1. Provide a "Name" for your build, such as **[UI]'s Pixel Tracker Java Artifact Build**
1. Choose a default agent queue, such as **Hosted**
1. Select "Add Task", and choose the **Gradle** task.
	1. Enter *build* for "Tasks"
	1. Change "Test Results Files" to **\*\*/build/test-results/test/TEST-*.xml**
	1. Select **Cobertura** for the "Code Coverage Tool"
	1. Select "Run FindBugs"
1. Select "Add Task", and choose the **Copy Files** task.
	1. Enter **$(build.sourcesdirectory)** for "Source Folder"
	1. Enter **\*\*/\*.jar** for "Contents"
	1. Enter **$(build.artifactstagingdirectory)** for "Target Folder"
1. Select "Add Task", and choose the **Copy Files** task.
	1. Enter **server/src/main/resources** for "Source Folder"
	1. Enter **\*.config** for "Contents"
	1. Enter **$(build.artifactstagingdirectory)** for "Target Folder"
1. Select "Add Task", and choose the **Publish Build Artifacts** task.
	1. Enter **$(build.artifactstagingdirectory)** for "Path to Publish"
	1. Enter **drop** for "Artifact Name"
1. Enable "Continuous Integration" from the "Triggers" tab.
	1. Select "Enable"
	1. Select "Batch changes while a build is in progress". 
1. Select "Save & queue".

### Configure the release
1. From the "Releases" tab, select "Create release definition" from the "+" dropdown.
1. Select **Empty proces** when asked to "Select a Template".
1. Provide a name for this Environment, such as **Integration**
1. Select "+ Add artifact" to link to the Java Artifcat built in the previous step.
	1. Select the "Source (Build definition)" which you named **[UI]'s Pixel Tracker Java Artifact Build**
	1. Select "Add".
1. Select "+ Add" to include the ARM template Git.
	1. For "Source Type", select **GIT**.
	1. For "Source (repository)", select the git repository you created for the ARM template. 
	1. Provide an alais for the repo, **arm-pixel-tracker-git**. 
	1. Select "Add"
1. Select "+ Add" to include the Java template Git, in order to run our integrated tests. 
	1. For "Source Type", select **GIT**.
	1. For "Source (repository)", select the git repository you created for the Java project. 
	1. Provide an alais for the repo, **java-pixel-tracker-git**. 
	1. Select "Add"
1. Select the thunderbolt on the "Build" circle to enable "Continuous Integration".
	1. Click the toggle to "Enable" 
	1. Close with the "X"
1. Configure the steps in the "Tasks" tab. 
	1. Create a task to deploy the Resource Group into your Azure Subscription. 
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select "Azure Resource Group Deployment"
		1. Select your "Azure Subscription" from the drop down menu. 
		1. Enter the "Resource group" as **$(rg)**
		1. Select the "Location" **East US 2**.
		1. Provide the following location for the "Template", **$(System.DefaultWorkingDirectory)/arm-pixel-tracker-git/azuredeploy.json**
		1. Provide the following location for the "Template parameters", **$(System.DefaultWorkingDirectory)/arm-pixel-tracker-git/azuredeploy.paramters.json**
		1. Provide the following for the "Override template parameters", **-environment $(env)**
		1. Confirm the deployment mode is set to **Incremental**
	1. Create a task to copy the Web.config file from VSTS to the Azure Web App using the provided PowerShell script.
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select "Azure PowerShell"
		1. Select your "Azure Subscription" from the drop down menu. 
		1. Confirm "Script Type" is set to **Sccript File Path**
		1. Enter the following for the "Script Path", **$(System.DefaultWorkingDirectory)/arm-pixel-tracker-git/deploySpring.ps1**
		1. Enter the following for Script Arguments, **-appdirectory '$(System.DefaultWorkingDirectory)\[UI]'s Pixel Tracker Java Artifact Build\drop\' -resourceGroup $(rg) -webappname $(webAppName)**
	1. Create a task to copy the Java jar from VSTS to the Azure Web App using the provided PowerShell script.
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select "Azure PowerShell"
		1. Select your "Azure Subscription" from the drop down menu. 
		1. Confirm "Script Type" is set to **Sccript File Path**
		1. Enter the following for the "Script Path", **$(System.DefaultWorkingDirectory)/arm-pixel-tracker-git/deploySpring.ps1**
		1. Enter the following for Script Arguments, **-appdirectory '$(System.DefaultWorkingDirectory)\[UI]'s Pixel Tracker Java Artifact Build\drop\server\build\libs\' -resourceGroup $(rg) -webappname $(webAppName)**
	1. Create a task to perform a ping test to confirm the server is up before performing integrated testing. 
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select "Cloud-based Web Performance Test"
		1. Leave the "VS Team Services Connection" empty in order to use your current VSTS account. 
		1. Enter the "Website Url" as **http://$(webAppName).azurewebsites.net**
		1. Enter the "test Name" as **Ping Test**
		1. Set the "User Load" to **25**
		1. Set the "Run Duration (sec)" to **60**
		1. Confirm "Run load test using" is selected as **Automatically provisioned agents**.
		1. Under the "Control Options" select "Continue on error". 
	1. Create a task to perform the functional tests once the service is online. 
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select the "Gradle" task. 
		1. For the "Gradle Wrapper location provide the following, **$(System.DefaultWorkingDirectory)/java-pixel-tracking-git/gradlew**
		1. For the "Options" provide the hostname as **-Phostname=http://$(webAppName).azurewebsites.net/**
		1. For "Tasks" enter **integrationTest*. 
		1. Set the "Test Reults Files" to **\*\*/build/test-results/integrationTest/TEST-*.xml**
	1. Create a task to perform the functional tests once the service is online. 
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select the "Cloud-based Web Performance Test" task. 
		1. Leave the "VS Team Services Connection" empty in order to use your current VSTS account. 
		1. Enter the "Website Url" as **http://$(webAppName).azurewebsites.net**
		1. Enter the "test Name" as **Load Test**
		1. Set the "User Load" to **25**
		1. Set the "Run Duration (sec)" to **60**
		1. Confirm "Run load test using" is selected as **Automatically provisioned agents**.
		1. Confirm "Fail test if Avg.Repsonse Time(ms exceeeds)" is selected as **2500**.
	1. Create a task to clean up the Resource Group in your Azure Subscription. 
		1. Add a task to the phase by clicking "+" in the "Agent Phase" frame.
		1. Select "Azure Resource Group Deployment"
		1. Select your "Azure Subscription" from the drop down menu. 
		1. Change the "Action" to **Delete resource group**
		1. Enter the "Resource group" as **$(rg)**
		1. Under "Control Options", change "Run this task" to **Even if a previous task has filed, unless the deployment was cancelled**. 
1. Configure the inputs in the "Variables" tab. 
	1. Add a variable
		1. With "Name" **env**
		1. With "Value" **int**
		1. With "Scope" **Integration**
	1. Add a variable
		1. With "Name" **rg**
		1. With "Value" **[UI]int[N]**
		1. With "Scope" **Integration**
	1. Add a variable
		1. With "Name" **webAppName**
		1. With "Value" **[UI]-pixeltracker-[N]**
		1. With "Scope" **Integration**	
