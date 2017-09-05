1. # Azure Digitial Marketing - Pixel Tracker Continuous Release Guide

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
1. Import Pixel Tracker ARM from Github to VSTS
  a. Create Build to validate ARM template
  b. Create Release
    i. Create Env to execute and clean up Functional Test
    ii. Create Env for deployment
2. Import Pixel Tracker Java from Github to VSTS
  a. Create Build to gate master branch
  b. Create Build to compile master branch
  c. Create Release
    i. Create Env to execute and clean up Functuional Test
    ii. Create Env for deployment
