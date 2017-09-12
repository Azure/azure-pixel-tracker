Use the [manual](https://github.com/Azure/cortana-intelligence-personalized-offers/tree/master/Manual%20Deployment%20Guide) or automated deployment guide to launch the Personalized Offers solution into your Azure subscription. 

Once completed follow the steps here to tie together the two solutions. 

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
	- ***Event hub consumer group***: Add New, **personalofferscg**
	- ***Event Serialization Format***: JSON
	- ***Encoding***: UTF-8
- Click ***Create***

### Output configuration for the Stream Analytics jobs:  
    1. At the top of the *Inputs* page click __+ADD OUTPUT__
    2. In the new panel:
    	
    	a. *Input Alias* : **ClickActivity**
    	
    	b. *Source Type* : **Data stream**
    2. *Source* : **Event hub**
    3. *Subscription* : Use event hub from current subscription
    4. *Service bus namespace* : **unique string for personaloffers deployment**
    5. *Event hub name* : **personalizedofferseh**
    6. *Event hub policy name* : **RootManageSharedAccessKey**
    7. *Event serialization format* : JSON
    8. *Encoding* : UTF-8
    9. Click __Create__

