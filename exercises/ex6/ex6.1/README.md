# Exercise 6.1 - Run your application for testing

In this exercise, we will configure the __run configuration__ and set the database as SAP HANA Cloud, in order to test to full multi-runtime target application stack
- Fiori UI
- Node.JS application runtime
- HANA Cloud database application runtime.

1. Open a run configuration view

- Click on __"Run configuration"-icon__ in the left-side activity bar

<br>![](/exercises/ex6/ex6.1/images/14_runicon.png)

- click on the project __'Run Risk_Management_U##'__ which will open the __"launch.json"__ on the right.

<br>![](/exercises/ex6/ex6.1/images/15_click.png)

- For Database, select __SAP HANA Cloud__. 
- Then select the __HDI container__ which was created in the [Exercise 6 - Bind your application to an HDI Container ](exercises/ex6/)

<br>![](/exercises/ex6/ex6.1/images/1_riskconfig.png)


<br>__2.__ __Run the application__ for testing by _clicking_ on the either of the __"Run"-icon__ as shown below. 

<br>![](/exercises/ex6/ex6.1/images/3_runoptions.png)


<br>__3.__ This will automatically open the application's Fiori UI in a new tab in the browser. 

<br>![](/exercises/ex6/ex6.1/images/4_showapp.png)

<br><br>
## Exercise 6.1.1  Check the service details and explore the application

<br>__4.__ You can now also explore the data that was generated from Business Partner API by clicking on service data details.   
<br>![](/exercises/ex6/ex6.1/images/5_showservice.png)
<br>![](/exercises/ex6/ex6.1/images/6_showodata.png)


<br>__5.__ Click on RiskApplication and click on go which shows the data.

<br>![](/exercises/ex6/ex6.1/images/11_clickriskapp.png)

- Note, the impact classification shown in the data view, is managed by the application logic we add in exercise 3.
<br>![](/exercises/ex6/ex6.1/images/7_riskapp.png)

<br>__6.__ you can edit the entry and assign a business partner id into a certain risk.
<br>![](/exercises/ex6/ex6.1/images/8_addbpid.png)

<br>__7.__ Go back to your project & stop the application by clicking on the icon shown below.

<br>![](/exercises/ex6/ex6.1/images/16_stop.png)

- Close all open tabs before proceeding to the next exercise.

## Summary 📝

You now have successfully built a full stack application with SAP HANA Cloud as database and Cloud Application Programming Model ( CAP ) with Fiori representation adhering to clean core principles.

Continue to - [Exercise 6.2 - Add SAP HANA Cloud Native Artifacts to a CAP Application](../ex6.2/README.md)



