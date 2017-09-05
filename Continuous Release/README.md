# Azure Digitial Marketing - Pixel Tracker Continuous Release Guide

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Steps](#setup-steps)

## Introduction

The objective of this Guide is to demonstrate how to configure a continuous release enviromenet in Visual Studio Team services to build and publish the pixel tracker solution.

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

You can download or view individual files by navigating through the repository folders. If you choose this option, be sure to download the "raw" version of each file by clicking the filename to view it, then cliking Download.

1. Pixel Tracker ARM

    1.1 Import Pixel Tracker ARM from Github to VSTS

    1.2 Create Build to validate ARM template
        
    1.3.1 Create Release Env to execute and clean up Functional Test
        
    1.3.2 Create Release Env for deployment
        
2. Pixel Tracker Java

    2.1 Import Pixel Tracker Java from Github to VSTS
      
    2.2 Create Build to gate master branch
    
    2.3 Create Build to compile master branch    

    2.4.1 Create Release Env to execute and clean up Functuional Test
          
    2.4.2 Create Release Env for deployment
