# [Personalized Offers in Online Retail Guide][0]

Use the [manual][1] or [automated][2] deployment guide to launch the Personalized Offers solution into your Azure subscription. 

The Pixel Tracker and Personalized Offers will be linked together by connecting the Azure API App to the Azure Storage queues. This will be done by enabling the Personlized Offers module in the Pixel Tracker by setting the configuration property. 

### Add Queue Connection String to API App
1. Go to the [Azure Portal](https://ms.portal.azure.com) and navigate to your resource group.
2. In ***Overview*** panel, click **Resource groups**, and select the resource group used for the Personalized Offers solution. 
3. Click on the **Storage account** contained within the resource group. 
4. Click **Access Keys**, under **SETTINGS**. 
5. Copy the key1 Connection String. 
6. In ***Overview*** panel, click **Resource groups**, and select the resource group used for the Pixel Tracker solution. 
7. Click on the **App Service** contained within the resource group. 
8. Navitage to **Application Settings** under **Settings**.
9. Add a new **Connection strings**
    1. Name: PersonalizedOffers
    2. Value: Connection String from step 5.
    3. Type: Custom
10. Click **Save**, the service will reset.
11. Confirm the module was activated correctly by navitaged to your web app index page, {webappname}.azurewebsites.net and that "Personalized Offers" is set to true. 



[0]: https://gallery.cortanaintelligence.com/Solution/Personalized-Offers-2
[1]: https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Manual%20Deployment%20Guide
[2]: https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Automated%20Deployment%20Guid
