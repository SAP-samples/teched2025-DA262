# Exercise 6.3 - Accessing Cold Data for Risk Management: Historical Audit via Data Lake 💾

Data tiering in SAP HANA Cloud gives you a cost-effective storage solution, which allows you to assigning data to different storage and processing tiers so that you can fulfill your data management strategies.

__The Challenge:__ This historical data (the Cold Data) is too voluminous (gigabytes/terabytes) and too expensive to keep in the fast, in-memory Core SAP HANA DB. Storing it there would slow down the daily, mission-critical transactions.

Your task is to integrate these historical logs into your CAP application without copying data or modifying the core model.

## Pre-setup ( done by SAP )

1. The Data Lake has been configured, and historical data files have been uploaded. 

<br>![](/exercises/ex6/ex6.3/images/1_dlfiles.png) 

2. A database user named __AC230193U00__ and a role __HC_DL_FILES_ROLE__ have been created.
The necessary privileges have been granted to this role to enable the required operations for this exercise.

3. Virtual table access to the Data Lake files has been established, enabling seamless querying of the stored data.

<br>![](/exercises/ex6/ex6.3/images/2_vt.png) 

4. A User-Provided Service Instance named __UPS_DA262__ has been created.

>💡 __Insight Corner__: The User-Provided Service (UPS) holds the schema details that define how virtual tables connect to the Data Lake files.
It acts as a bridge, allowing users to easily access and query external data stored in the Data Lake directly from the database environment.


## Add Existing User Provided Service Instance into your Project

1. Go to your SAP HANA Projects, click on __'+'__ in database connections.

<br>![](/exercises/ex6/ex6.3/images/3_addups.png) 

2. Click on the drop down for 'Select connection type' and choose 'Existing user-provided service instance' & select __UPS_DA262__.

<br>![](/exercises/ex6/ex6.3/images/4_selectups.png) 

3. Your SAP HANA Projects view should look like this:

<br>![](/exercises/ex6/ex6.3/images/5_hanaprojview.png) 

4. In the __src__ folder, create a new subfolder named __Grants__, and inside it, create a new file called __UPS_DA262.hdbgrants__.

<br>![](/exercises/ex6/ex6.3/images/6_createhdbgrants.png) 

5. Once the file is created, right click on the file and select 'open with' and choose text editor.

<br>![](/exercises/ex6/ex6.3/images/7_opentexteditor.png) 

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

7. Now lets create a __synonym__ ,  go to src folder and create a new file called __RISK_MANAGEMENT_U00_VT_HISTORICAL_AUDIT_LOGS__

> __ℹ️ NOTE__: Replace the U00 with your U## number.

<br>![](/exercises/ex6/ex6.3/images/8_syn.png) 

8. Right click on the newly created file and open with text editor. Paste the following json.

```json
{
  "RISK_MANAGEMENT_U00_VT_HISTORICAL_AUDIT_LOGS": {
    "target": {
      "object": "VT_HISTORICAL_AUDIT_LOGS",
      "schema" : "AC230193U00"
    }
  }
}
```

9. Deploy your HDI container.

<br>![](/exercises/ex6/ex6.3/images/9_deploy.png) 

10. Open your HDI container.

<br>![](/exercises/ex6/ex6.3/images/10_openhdi.png) 

11. Click on Synonyms,  right click on the __RISK_MANAGEMENT_U00_VT_HISTORICAL_AUDIT_LOGS__ and click on open data to view the data.

<br>![](/exercises/ex6/ex6.3/images/11_opensyndata.png) 

12. Now, go back to your SAP Build Code & to your CAP Project, edit your schema.cds under src folder ( src>schema.cds). Add the following to the end of your current model. 

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

<br>![](/exercises/ex6/ex6.3/images/12_datamodel.png) 

13. go to srv folder > select service.cds and add the following.

```cds
 @readonly
    entity VT_HISTORICAL_AUDIT_LOGS as 
        projection on my.VT_HISTORICAL_AUDIT_LOGS;
```
<br>![](/exercises/ex6/ex6.3/images/13_addsrv.png) 

- service.cds file should look as follows:

<br>![](/exercises/ex6/ex6.3/images/14_srvfile.png) 


## Summary 📝

In this exercise, you extended an existing CAP-based Risk Management application to include historical data stored in SAP HANA Cloud Data Lake, showcasing how to enrich applications without violating clean core principles.

Continue to - [Exercise 7 - Run your application for testing](../ex7/README.md)






