# Exercise 6.3 - Building and Integrating a HANA Stored Procedure into a CAP Service 🧩

In this step, we create an SAP HANA stored procedure that performs the calculation of risk scores based on key parameters within the Risk and Mitigation application. The stored procedure encapsulates the business logic required to evaluate risks efficiently at the database level, ensuring high performance and scalability.

Once the procedure is implemented, we expose it as a function in the CAP (Cloud Application Programming) service layer, allowing it to be invoked securely through the application’s API. This integration enables other services or UI components to dynamically retrieve computed risk scores in real time, promoting a clean separation of concerns between data processing and service exposure.

1. To create a stored procedure, first create a new folder named procedure inside the src directory.


- Go to View > Command Pallette, create a new database artifact named __calculateRiskScore__ of type hdbprocedure.

<br>![](/exercises/ex6/ex6.3/images/1_cmdpalette.png) 


<br>![](/exercises/ex6/ex6.3/images/2_createproc.png) 

<br>![](/exercises/ex6/ex6.3/images/14_procview.png) 


2. Create a stored procedure that computes the risk score for each risk. Paste the following script into your newly created .hdbprocedure file.

<br>![](/exercises/ex6/ex6.3/images/3_procedure.png) 

> __ℹ️ NOTE__:The RISK_MANAGEMENT_U00_RISKS & RISK_MANAGEMENT_U00_MITIGATIONS you see , __REPLACE__ ## where ## represents the unique number assigned to you at the start of the exercise (for example, 01 or 39)

```SQL
PROCEDURE "calculateRiskScore"( 
    OUT result TABLE (
        ID NVARCHAR(36),
        title NVARCHAR(100),
        impact INTEGER,
        criticality INTEGER,
        mitigations INTEGER,
        riskScore DECIMAL(15,2)
    )
)
   LANGUAGE SQLSCRIPT
   SQL SECURITY INVOKER
   --DEFAULT SCHEMA <default_schema_name>
   READS SQL DATA AS
BEGIN
   result = 
        SELECT 
            R.ID,
            R.title,
            R.impact,
            R.criticality,
            COUNT(M.ID) AS mitigations,
            CASE 
                WHEN COUNT(M.ID) = 0 THEN R.impact * R.criticality
                ELSE R.impact * R.criticality / (COUNT(M.ID) + 1)
            END AS riskScore
        FROM "RISK_MANAGEMENT_U00_RISKS" AS R
        LEFT JOIN "RISK_MANAGEMENT_U00_MITIGATIONS" AS M
        ON M.risk_ID = R.ID
        GROUP BY R.ID, R.title, R.impact, R.criticality;
END
```


3. Deploy the to the database again using the SAP HANA Projects view.

<br>![](/exercises/ex6/ex6.3/images/4_deploy.png) 

4. Once the deployment is successful, navigate to the Database Explorer and open your HDI container.

<br>![](/exercises/ex6/ex6.3/images/5_openhdi.png) 

5. The newly created procedure is now available and ready for testing.

<br>![](/exercises/ex6/ex6.3/images/6_checkproc.png) 

6.  Generate a CALL statement and verify the results.

<br>![](/exercises/ex6/ex6.3/images/7_gencall.png) 

7. The procedure now outputs all rows containing their corresponding risk scores.

<br>![](/exercises/ex6/ex6.3/images/8_proccall.png) 

8. Now we want to add this Procedure to the CAP service as a __action__. Edit __/srv/service.cds__.

- Add the below action snippet in to the service definition.

```cds
action calculateRiskScore() returns array of {
        ID          : UUID;
        title       : String(100);
        impact      : Integer;
        criticality : Integer;
        mitigations : Integer;
        riskScore   : Decimal(15, 2);
    };
```
This will expose an OData Function as part of the service interface. 

- Your service.cds file should now look as follows:

<br>![](/exercises/ex6/ex6.3/images/9_servicecds.png) 

9. Adding the function to the service definition alone doesn’t make it functional. While tables and views are managed by CAP’s built-in handlers, functions need to be explicitly implemented through a service handler exit.

- Open the service.js file in the /srv folder. CAP uses the matching name to identify this file as the place to implement custom exit handlers for the services defined in service.cds. Replace it with the following code.


```javascript
const cds = require('@sap/cds');
const risks_Logic = require('./code/risks-logic');

module.exports = class risk_Management_U00Srv extends cds.ApplicationService {
  async init() {
    const db = await cds.connect.to('db');

    // After READ hook for Risks
    this.after('READ', 'Risks', async (results, req) => {
      const risks_Logic = require(path.join(__dirname, 'code/risks-logic'));
      await risks_Logic(results, req);
    });

   // GET handler for calculateRiskScore
    this.on('calculateRiskScore', async (req) => {
        try {
            const result = await db.run(`
                <YOUR PROCEDURE CALL>
            `);
            return result;  // return the first table
        } catch (err) {
            console.error('Error calling HANA procedure:', err);
            req.error(500, 'Failed to calculate risk scores');
        }
    });

    return super.init();
  }
};

```
> __ℹ️ NOTE__: Use the CALL statement generated in Database Explorer to replace the one currently in your code.

- Navigate back to SAP HANA Database Explorer, copy the CALL statement it generated, and insert it into your script. 

<br>![](/exercises/ex6/ex6.3/images/11_copycall.png) 

- Remove the __RESULT => ?__ and insert __?__

- Final version of __service.js__ should look as follows:

<br>![](/exercises/ex6/ex6.3/images/10_servicejs.png) 

10. Lets test the service.

- create an HTTP file. In the project root folder, right click and create a folder __http-requests__ and add a new file __calculateRiskScore.http__

<br>![](/exercises/ex6/ex6.3/images/12_createfolder.png) 

- Write the post request inside the newly created file i.e __calculateRiskScore.http__

```http
POST http://localhost:4004/service/risk_Management_U00/calculateRiskScore
Content-Type: application/json
Accept: application/json
Body: {}
```

- You can run the application by the following command:

```
cds watch --profile hybrid
```

- Place the cursor on the first line of the request, click “Send Request” above the line (or right-click → Send Request), and the response will appear in a panel on right with the JSON output from your HANA procedure.
<br>![](/exercises/ex6/ex6.3/images/15_run.png) 

## Summary 📝

In this exercise, we created a calculateRiskScore stored procedure in SAP HANA to compute risk metrics for the Risks entity and exposed it via a CAP service as an OData function. By defining the function in service.cds and implementing the handler in service.js, we made the procedure accessible through HTTP requests, allowing it to be called directly from a browser or REST client while returning JSON results from HANA.

Continue to - [Exercise 6.4 - Accessing Cold Data for Risk Management: Historical Audit via Data Lake](../ex6.4/README.md)








 









