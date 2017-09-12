Use the [manual](https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Manual%20Deployment%20Guide) or automated deployment guide to launch the Personalized Offers solution into your Azure subscription. 

Particular sections that are different will be detailed here. Otherwise that guide should be followed. 

# 1. Azure Stream Analytics Jobs


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
