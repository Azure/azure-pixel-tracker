# [Personalized Offers in Online Retail Guide.MD][0]

Use the [manual][1] or [automated][2] deployment guide to launch the Personalized Offers solution into your Azure subscription. 

The Pixel Tracker and Personalized Offers will be linked together using Azure Stream Analytics. Stream Analytics will be used to filter the Pixel Tracker Event Hub to only the events of intereste for the personalized offers solution. It will then output that data into the Event Hub deployed in the personalized offers resource group, where it will be consumed by that solution. 

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
    7. Create a new CG, **pixel2po** as the "Event hub policy name".
    8. Leaving other fields on their default values, click the **Create** button at the bottom.
10. Return to the Stream Analytics job overview panel and click **Outputs**.
:10. In the new panel, click **+ Add** to add an output. In the "New output" panel:
    1. Enter **poEventHub** for the "Output alias".
    2. Choose **Data Stream** for the "Source type".
    3. Choose **Event hub** for the "Source".
    5. Choose your **personalized offers unique string** for the "Service bus namespace".
    6. Choose **personalized offers** for the "Event hub name".    
    7. Leaving other fields on their default values, click the **Create** button at the bottom.
11. Return to the Stream Analytics job overview panel and click **Query**.
11. In the new panel, click **Query** and click **+** in the new panel. Remove the default query content and enter:

    ```
    SELECT
    *
    INTO
        poEventHub
    FROM 
        eventHub;
    ```
    
    Click the **Save** icon to save the query.
    **[Note]: The input and output aliases are used in the query, and the selected column names must exactly match those in the Activities table.**
12. Return to the overview of the Stream Analytics job and click the "Start" button.
13. In the new panel, choose "Now" for the "Job output start time".
14. Click the **Start** button at the bottom.  

[0]: https://gallery.cortanaintelligence.com/Solution/Personalized-Offers-2
[1]: https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Manual%20Deployment%20Guide
[2]: https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Automated%20Deployment%20Guid
