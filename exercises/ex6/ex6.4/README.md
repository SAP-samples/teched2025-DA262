# Exercise 6.4 - Accessing Cold Data for Risk Management: Historical Audit via Data Lake 💾

Data tiering in SAP HANA Cloud gives you a cost-effective storage solution, which allows you to assigning data to different storage and processing tiers so that you can fulfill your data management strategies.

__The Challenge:__ This historical data (the Cold Data) is too voluminous (gigabytes/terabytes) and too expensive to keep in the fast, in-memory Core SAP HANA DB. Storing it there would slow down the daily, mission-critical transactions.

Your task is to integrate these historical logs into your CAP application without copying data or modifying the core model.

## Pre-setup ( done by SAP )

1. The Data Lake has been configured, and historical data files have been uploaded. 

<br>![](/exercises/ex6/ex6.4/images/1_dlfiles.png) 

2. A database user named __AC230193U00__ and a role __HC_DL_FILES_ROLE__ have been created.
The necessary privileges have been granted to this role to enable the required operations for this exercise.

3. Virtual table access to the Data Lake files has been established, enabling seamless querying of the stored data.

<br>![](/exercises/ex6/ex6.4/images/2_vt.png) 

4. A User-Provided Service instance named UPS_DA262 has been created in the CloudFoundry space AC_HANACLOUD."

<br>![](/exercises/ex6/ex6.4/images/18_ups.png) 

>💡 __Insight Corner__: The User-Provided Service (UPS) holds the schema details that define how virtual tables connect to the Data Lake files.
It acts as a bridge, allowing users to easily access and query external data stored in the Data Lake directly from the database environment.


## Add Existing User Provided Service Instance into your Project

1. Go to your SAP HANA Projects, click on __'+'__ in database connections.

<br>![](/exercises/ex6/ex6.4/images/3_addups.png) 

2. Click on the drop down for 'Select connection type' and choose 'Existing user-provided service instance' & select __UPS_DA262__.

> __ℹ️ NOTE__: If you don’t see the ‘UPS_DA262’ result, please close the ‘Add Database Connection’ tab and reopen it.

<br>![](/exercises/ex6/ex6.4/images/4_selectups.png) 

3. Your SAP HANA Projects view should look like this:

<br>![](/exercises/ex6/ex6.4/images/5_hanaprojview.png) 

4. In the __src__ folder, create a new subfolder named __Grants__, and inside it, create a new file called __UPS_DA262.hdbgrants__.

<br>![](/exercises/ex6/ex6.4/images/6_createhdbgrants.png) 

5. Once the file is created, right click on the file and select 'open with' and choose text editor.

<br>![](/exercises/ex6/ex6.4/images/7_opentexteditor.png) 

6. Paste the following json. 

```json
{
	"ServiceName_1": {
		"application_user": {
			"global_roles": [
				{
					"roles": [
						"HC_DLFILES_ROLE"
					]
				}
			]
		},
		"object_owner": {
			"global_roles": [
				{
					"roles": [
						"HC_DLFILES_ROLE"
					]
				}
			]
		}
	}
}
```

7. Now lets create a __synonym__ ,  go to src folder and create a new folder called synonyms and create a new file called __VT_HISTORICAL_AUDIT_LOGS__ and file should end with hdbsynonym. 

- Alternatively, you can use view > commonad pallette > create 

<br>![](/exercises/ex6/ex6.4/images/8_syn.png) 

8. Right click on the newly created file and open with text editor. Paste the following json.

```json
{
  "VT_HISTORICAL_AUDIT_LOGS": {
    "target": {
      "object": "VT_HISTORICAL_AUDIT_LOGS",
      "schema" : "AC230193U00"
    }
  }
}
```

9. Deploy your HDI container.

<br>![](/exercises/ex6/ex6.4/images/9_deploy.png) 

10. Open your HDI container.

<br>![](/exercises/ex6/ex6.4/images/10_openhdi.png) 

11. Click on Synonyms,  right click on the __VT_HISTORICAL_AUDIT_LOGS__ and click on open data to view the data.

<br>![](/exercises/ex6/ex6.4/images/11_opensyndata.png) 

12. Now, go back to your CAP Project, edit your schema.cds under src folder ( src>schema.cds). Add the following to the end of your current model outside the context { }. 


```cds
@cds.persistence.exists
entity VT_HISTORICAL_AUDIT_LOGS {
    key BUSINESS_PARTNER_ID      : String(7);
    TRANSACTION_DATE        : Date;
    AUDIT_SCORE             : Double;
    LOG_DETAIL              : String(500);
}
```
- newly edited schema.cds should look like follows:

<br>![](/exercises/ex6/ex6.4/images/12_datamodel.png) 

13. go to srv folder > select service.cds and add the following.

```cds
using VT_HISTORICAL_AUDIT_LOGS from '../db/schema.cds';
```
```cds
 @readonly
    entity VT_Historical_Audit_Logs as 
        projection on VT_HISTORICAL_AUDIT_LOGS;
```
<br>![](/exercises/ex6/ex6.4/images/13_addsrv.png) 

14. Run the following command to build.

```
	cds build --production
```

15. Although we didn’t add any new database artifacts to the project, the addition of an entity to the service layer causes new views to be generated within SAP HANA. Therefore we need to deploy to the database using the SAP HANA Projects view before we can test.

<br>![](/exercises/ex6/ex6.4/images/17_deploy.png) 


16. Run the application by clicking on the 'Run' icon at right corner and access your CAP service layer to view the data that accessed from data lake files.

<br>![](/exercises/ex6/ex6.4/images/15_view.png) 
<br>![](/exercises/ex6/ex6.4/images/16_odata.png) 


## Summary 📝

In this exercise, you extended an existing CAP-based Risk Management application to include historical data stored in SAP HANA Cloud Data Lake, showcasing how to enrich applications without violating clean core principles.


----------------------------------------------------------
🎉 __Congratulations__! You’ve reached the end of this hands-on session.

You’ve successfully built and extended your CAP-based Risk Management application, integrated HANA Cloud artifacts, and explored how clean core principles come to life in practice.

👏 __Great work__ — this concludes the hands-on exercise!


<br>![](/exercises/ex6/ex6.4/images/teched.jpg) 


