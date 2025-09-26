# Exercise 7 - Run your multi-target application for testing

In this exercise, we will configure the __run configuration__ and set the database as SAP HANA Cloud, in order to test to full multi-runtime target application stack
- Fiori UI
- Node.JS application runtime
- HANA Cloud database application runtime.


## Exercise 7.1  Create a run configuration

1. Open a run configuration view

- Click on __"Run configuration"-icon__ in the left-side activity bar
- click on the project __"launch.json"__-icon
- For Database, select __SAP HANA Cloud__. 
- Then select the __HDI container__ which was created in the [Exercise 6 - Bind your application to an HDI Container ](exercises/ex6/)

<br>![](/exercises/ex7/images/1_riskconfig.png)

<br>![](/exercises/ex7/images/2_riskconfig.png)


- Select the __OData__ option, and select __"Mock data"__. Live data will utilize the Business Partner-API from the Destination S/4HANA Cloud system thats configured in the SAP Business Technology Platform (BTP).
<br>![](/exercises/ex7/images/2_riskconfig.png)

<br>__2.__ __Run the application__ for testing by _clicking_ on the __"Run"-icon__ as shown below. 
<br>![](/exercises/ex7/images/3_livedata.png)

<br>__3.__ This will automatically open the application's Fiori UI in a new tab in the browser. 

<br>![](/exercises/ex7/images/4_showapp.png)

<br><br>
## Exercise 7.2  Check the service details and explore the application

<br>__4.__ You can now also explore the data that was generated from Business Partner API by clicking on service data details.   
<br>![](/exercises/ex7/images/5_showservice.png)
<br>![](/exercises/ex7/images/6_showodata.png)


<br>__5.__ Click on RiskApplication and click on go which shows the data.
- Note, the impact classification shown in the data view, is managed by the application logic we add in exercise 3.
<br>![](/exercises/ex7/images/7_riskapp.png)

<br>__6.__ you can edit the entry and assign a business partner id into a certain risk.
<br>![](/exercises/ex7/images/8_addbpid.png)

## Summary

You now have successfully built a full stack application with SAP HANA Cloud as database and Cloud Application Programming Model ( CAP ) with Fiori representation.




