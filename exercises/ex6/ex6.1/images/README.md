# Exercise 6.1 - Add SAP HANA Cloud Native Artifacts to a CAP Application

Add SAP HANA Cloud native artifacts, such as calculation views and stored procedures, to an SAP Cloud Application Programming Model (CAP) application.

## Exercise 6.1.1 -  Create Calculation View and Expose via CAP (SAP HANA Cloud) 

>💡 __Insight corner__: Calculation Views and other HANA native artifacts allow you to leverage HANA specific features and optimizations that might not otherwise be available at the abstraction layers within the SAP Cloud Application Programming Model. Calculation Views are especially good at aggregation and filtering of large datasets.

1. Create a new Calculation View via View > Command Pallette and then SAP HANA: Create SAP HANA Database Artifact command pallet entry

<br>![](/exercises/ex6/ex6.1/images/1_cmdpalette.png) 

<br>![](/exercises/ex6/ex6.1/images/2_viewcreate.png) 

2. Create a calculation view called __V_RISKS__ of Data Category DIMENSION and Dimension Type of STANDARD. Press Create.

<br>![](/exercises/ex6/ex6.1/images/3_cvdetails.png) 

3. The new artifact is created in the /db/src folder. This way you can have a single HANA database model that contains both HANA native content and CAP generated content.

<br>![](/exercises/ex6/ex6.1/images/4_projstr.png) 

4. Click on the __V_RISKS.hdbcalculationview__ to load the graphical calculation view editor.

<br>![](/exercises/ex6/ex6.1/images/5_viewcv.png) 

5. Now lets model the join relationship. Drop a join node into the modeling space.Click on the join and drop it at the end of the projection.

<br>![](/exercises/ex6/ex6.1/images/6_dropjoin.png) 

6. Use the __'+'__ sign sign to add tables to the node. On clicking, it will open a new dialog box to add the data source.

<br>![](/exercises/ex6/ex6.1/images/7_addds.png) 

7. Type in RISK and then select the table you created earlier via CDS called __RISK_MANAGEMENT_U00_RISKS & RISK_MANAGEMENT_U00_MITIGATIONS__ and press Finish.

>__ℹ️ NOTE:__ Choose in the same order. 

<br>![](/exercises/ex6/ex6.1/images/8_addtables.png) 

8. You should see both artifacts in the join node.

<br>![](/exercises/ex6/ex6.1/images/9_artifacts.png) 

9. Double-click on the join node. A panel will open on the right.

>__ℹ️ NOTE:__ The two tables might not be visible initially. Please check your zoom level and try moving your cursor to locate them.

<br>![](/exercises/ex6/ex6.1/images/10_joinpanel.png) 

10. Drag and drop the ID field to the ID ( RISK_MANAGEMENT_U<##>_MITIGATIONS) field and Set the cardinality to 1..n.

<br>![](/exercises/ex6/ex6.1/images/11_setcard.png) 

11. In the Mapping tab, add the columns as shown below as the output by dragging from left to right.
- Rename the Mitigation title & description to __MITI_TITLE__ & __MITI_DESC__.

<br>![](/exercises/ex6/ex6.1/images/12_mapping.png) 
<br>![](/exercises/ex6/ex6.1/images/13_mapping.png) 

12. Connect the join node with the Projection node using ↗️

<br>![](/exercises/ex6/ex6.1/images/14_arrow.png) 
<br>![](/exercises/ex6/ex6.1/images/15_connect.png) 

13. Click on the Projection node and double-click on the join parent to add all the columns to the output.

<br>![](/exercises/ex6/ex6.1/images/16_joinmap.png) 

14. From the SAP HANA Projects view, press the Deploy button & Check the deployment log to make sure everything was successfully created in the database.

<br>![](/exercises/ex6/ex6.1/images/17_deploy.png) 
<br>![](/exercises/ex6/ex6.1/images/18_deploylogs.png) 

15. Open the HDI Container in the Database Explorer. Choose your assigned user.

<br>![](/exercises/ex6/ex6.1/images/19_openhdi.png) 

16. Under Column Views you will find your Calculation View. Choose __Open Data__.

<br>![](/exercises/ex6/ex6.1/images/20_opendata.png) 

## Exercise 6.1.2 - Create calculation view proxy entity

We now want to expose our Calculation View to the Cloud Application Programming model by creating a “proxy” entity for the view in the CDS data model.

17. Return to the __SAP Build Code__ and open _schema.cds_.

<br>![](/exercises/ex6/ex6.1/images/21_schemadef.png) 

18. We need to add a matching entity definition for the Calculation View. This means redefining all the column names and data types / lengths. Doing so manually would be error prone, but the __hana-cli__ has a utility that will help. Open a terminal and change to the db folder with the command cd db. 

- Install the __hana-cli__ with the following command:
```
npm install -g hana-cli
```
- Run the command cd db to navigate to the database directory
```
cd db
```
- Now issue the below command. With this command you are looking up the definition of the view but asking for the output (-o) in the CDS format
```
hana-cli inspectView -v V_RISKS -o cds
```
<br>![](/exercises/ex6/ex6.1/images/22_hanacli.png) 

>💡 __Insight corner__: You can build a developer-centric SAP HANA command line tool called __hana-cli__, particularly designed to be used when performing SAP HANA development.

- Run the below command to return to orginal app directory.
```
cd ..
```

19. Copy this block from the terminal and paste it into the interactions.cds file at the end outside the context block.

<br>![](/exercises/ex6/ex6.1/images/23_copy.png) 

>💡 __Insight corner__: CDS does have an annotation called @cds.persistence.exists. This annotation allows you to re-define an existing DB object and CDS won’t attempt to create or alter it. It will just assume it already exists in the matching state.There is also the annotation @cds.persistence.calcview. This will further tell the Cloud Application Programming Model that this target entity is also a Calculation View.

20. Now open the service.cds file from the /srv folder. Add this new Calculation View based entity to the CAP service as read-only.

<br>![](/exercises/ex6/ex6.1/images/24_servicemod.png) 

21. From the terminal, issue the below comamnd.
```
cds build --production
```
<br>![](/exercises/ex6/ex6.1/images/25_cdsbuild.png) 

22. Although we didn’t add any new database artifacts to the project, the addition of an entity to the service layer causes new views to be generated within SAP HANA. Therefore we need to deploy to the database using the SAP HANA Projects view before we can test.

<br>![](/exercises/ex6/ex6.1/images/17_deploy.png) 

23. You can test your view via the service layer by adding /odata/v4/catalog/V_RISKS to the path.

<br>![](/exercises/ex6/ex6.1/images/26_viewodata.png) 

