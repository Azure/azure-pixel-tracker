This guide is based on the README provided [here](https://github.com/Azure/cortana-intelligence-personalized-offers-retail/blob/master/Technical%20Deployment%20Guide/README.md).

Particular sections that are different will be detailed here. Otherwise that guide should be followed. 

# 3. Azure Event Hub
We will instead use the Event Hub created when we created the pixel tracker. Navitage to that event hub and to ensure processing of the hub is successful 
we need to create [consumer groups](https://azure.microsoft.com/en-us/documentation/articles/event-hubs-programming-guide/#event-consumers) on the hub. 
For this solution we will need to create two different groups, *blobcg* and *pbicg*. 

-	Log into ***manage.windowsazure.com***
-	In the left panel, click ***SERVICE BUS***
-	In the list, choose the namespace we created above - [UI]pixeltracker[N]
-	Click ***EVENT HUBS*** at the top of the right pane
-	The event hub we have created above ([UI]pixeltracker[N]) should be highlighted. 
-   In the bottom frame, click the ***CREATE CONSUMER GROUP*** and create the *blobcg* consumer group. 
Repeat this step to create the *pbicg* consumer group

Finally, we need to record the connection information for this event hub so that we can feed in
 event data from our data generator. While on the page for your service bus do the following: 

- 	Click the blue back arrow in the top left of the page to return to the ***SERVICE BUS*** page.
-	Highlight the namespace we created above (personaloffers[UI][N]-ns) by clicking on the row but do not open the namespace page.
-	At the bottom of the page click ***CONNECTION INFORMATION***
-	Copy the *CONNECTION STRING* information into the appropriate section in the ConnectionInformation.txt file.

# 4. Azure Stream Analytics Jobs
Use the following section in place of the one provided to use the event hub deployed for the pixel tracker. 

### Input configuration for the Stream Analytics jobs:
-	Navigate to ***portal.azure.com*** and login in to your account.
-	On the left tab click ***Resource Groups***
-	Click on the resource group we created earlier ***personaloffers_resourcegroup*** 
-	Click on the one of the Stream Analytics jobs that was created in the earlier steps.
-	Under ***Job Topology*** click ***INPUTS***
-	In the ***Inputs*** frame, Click ***ADD*** at the top
-	Enter the following inputs:
	- ***Input Alias***: inputeh
	- ***Source Type***: Data Stream
	- ***Source***: Event Hub
	- ***Service bus namespace***: Select the namespace you created above, [UI]pixeltracker[N]
	- ***Event Hub Name***: Select the event hub you created above, [UI]pixeltracker[N]
	- ***Event hub consumer group***:
		- personaloffersasastorage uses *blobcg*
		- personaloffersasapbi1 uses *pbicg*
		- personaloffersasapbi2 uses *pbicg*
	- ***Event Serialization Format***: JSON
	- ***Encoding***: UTF-8
- Click ***Create***
