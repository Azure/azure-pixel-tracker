#  Azure Digital Marketing - Pixel Tracker 
![][pixel-1]

A marketer may want to be able to track their customers activities across a large medium of experiences. First, they would want to be able to understand customers activates on their main website. They would want to gather data to learn what pages customers were viewing, but more importantly what ads were customers being shown. And then where those ads successful, were their customer's clicking on them? This would be important to track and understand as they begin deploying machine learning models trained on this data and to understand the financial benefit. Then other parts of the business could be beginning sending data, such as if a customer answered a phone call, or clicked on an ad in an email. Examing all of this data in one location can help make deeper connections. The interface for sending data would have to simple, and be able to plug into many legacy systems written in a variety of languages. Some data feeds would be sent from internal sources, while others would originate from customer’s computers or browsers. These requirements lead to designing a pixel tracker that would be deployed on Azure. Azure enables such a system to quickly scale up as more business units plug into the services with a complete breakdown of cost. These are advantages over a typical on-prem based solution that a company would have had to build themselves to implement a custom solution previously. Companies do not have to try and keep up with their own hardware in case of increase customer base which would increase operating costs. 

So, what is a Tracking Pixel? 
A tracking pixel is a small unseen image embedded into web sites, emails or applications. It is essential a web request to a server running in Azure. The contents of the pixel are unimportant, instead the essential data is sent in query string arguments. A sample pixel call may look like…
```
https://pixeltracker.azurewebsites.net/pix?customerId=123&product=ac12&activity=purchase
```
The query string parameters are extracted from the request, and then advanced transformations can be performed. Some examples would be extracting information from the request from the user agent string, or using the IP address to perform a reverse lookup to estimated location. Design considerations ensure that additional custom transformations could be added quickly. Once transformed, the data is collected in Azure and persisted in a place that it can be queried and visualized by other services. 

The objective of this guide is to demonstrate how to configure the cortana intelligence solution for deploying a pixel tracker. Marketers can use this data to understand customer activity and can use it build better product selection algorithms.
## What's Under the Hood

The end-to-end solution is implemented in the cloud, using Microsoft Azure. The solution is composed of several Azure components, including data ingest, data storage, data movement, advanced analytics and visualization. The custom code is written in Java and executed using Spring Boot hosted on Azure API Apps. The java code includes the server implmentation, as well as unit and functional tests. 
## Solution Architecture
![Solution Diagram Picture](resources/architecture.png)

## Example use case
As an example, a website owner would like to know where people who visit the website live. Once the solution is deployed into the website owner's azure subscription, the tracking pixel must be added to the homepage of the webiste. Then, each user who vist's the webpage will generate a web request to the service running in Azure. The custom code will extract the user's IP address from the web request, and then use 3rd party data to convert the IP to an address. All of the request data will be stored in Azure Data Lake Store. Lastly, PowerBi can be used to create a simple dashboard visualizing the location data in the data lake store. 

## Getting Started

This solution package contains materials to help both technical and business audiences understand our Pixel Tracking Solution built on the [Cortana Intelligence Suite](https://azure.microsoft.com/en-us/suites/cortana-intelligence).

## Business Audiences

In this repository you will find a folder labeled [*Solution Overview for Business Audiences*](https://github.com/Azure/azure-pixel-tracker/tree/master/Solution%20Overview%20for%20Business%20Audiences) which contains a presentation covering this solution and benefits of using the Cortana Intelligence Suite.

For more information on how to tailor Cortana Intelligence to your needs [connect with one of our partners](http://aka.ms/CISFindPartner).

## Technical Audiences

### Manual Deployment
See the [Manual Deployment Guide](https://github.com/Azure/azure-pixel-tracker/tree/master/Manual%20Deployment) folder for a full set of instructions on how to put together and deploy the Pixel Tracker Solution using the Azure Portal. 

### Continuous Release 
See the [Continuous Release Guide](https://github.com/Azure/azure-pixel-tracker/tree/master/Continuous%20Release) folder for a full set of instructions on how to put together a contiousus build and release process for the Pixel Tracker Solution using Visual Studio Team Services.

For technical problems or questions about deploying this solution, please post in the issues tab of the repository.

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.


## Disclaimer
©2017 Microsoft Corporation. All rights reserved. This information is provided "as-is" and may change without notice. Microsoft makes no warranties, express or implied, with respect to the information provided here.
![][pixel-2]

[pixel-1]: (http://pixeltracker.azurewebsites.net/pixel.gif?page=main%2FREADME.MD&section=top)
[pixel-2]: (http://pixeltracker.azurewebsites.net/pixel.gif?page=main%2FREADME.MD&section=bottom)
