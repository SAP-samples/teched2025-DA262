# Exercise 4 - Add the destination to your project

>💡 Insight Corner: Destinations are key building blocks on SAP BTP, as they are used to define connections for outbound communication from your application to remote systems. These remote systems can be on-premises or in the cloud.
>- The destination you will create in this tutorial connects to a set of OData services known as the "Business Partner".

> ℹ️ NOTE: For this tutorial, we've already set up the destination. If you'd like to learn more about it, feel free to refer to this [Create Destinations in SAP BTP Cockpit](https://developers.sap.com/tutorials/cp-cf-create-destination..html)

1.  Go to your "Storyboard",
- Click on the '+'

<br>![](/exercises/ex4/images/1_add_extresource.png)

2. This brings up the service center tab, allowing you to view the pre-configured SAP BTP destinations.

<br>![](/exercises/ex4/images/2_servicecenter_dest.png)

3. Choose "S4HANA_ODATA_BusinessPartner", which opens the details section. 
- Choose "Add to Project".

<br>![](/exercises/ex4/images/3_add_bp.png)

4. Now, your storyboard should look as shown below:

<br>![](/exercises/ex4/images/4_storyboard.png)

## Exercise 4.1 - Edit the Data Model and Service Definition with Business Partner
In the following steps, we define the data model and service model relationship with the Business Partner API.

5. From your  data model in the Storyboard  _select_ "Open in Graphical Modeler" as shown below:
![](/exercises/ex4/images/5_open_graphicmodeller.png)

6.  Select the "Risks" entity 
- and click on "Add relationship", 
- then click into the empty space to open the next dialog
![alt text](/exercises/ex4/images/6_add_reltn.png)

7. Once the dialog box opens
- look for the target entity-relationship shown in the image below 
  - Type: Association and Cardinality: To-One
- and _rename_ the a_BusinessPartner to BusinessPartner.

![alt text](/exercises/ex4/images/7_relationship_details.png)

8. Now the data model would look as follows :
![alt text](/exercises/ex4/images/8_storyboard_w_bp.png)

9. Go back to your Storyboard,  under service, select "Add Service Entity" as shown below. 

![alt text](/exercises/ex4/images/9_add_serviceentity.png)

10. This loads the graphical modeller view, click on __Add Projection__. Under Projection definition on the right
- first select "API_BUSINESS_PARTNER.A_BusinessPartner" as _key_ element of the associated entity
- then "un-check" all properties, 
- and select on the following columns/properties:
  - BusinessPartner, FirstName and LastName
- Save by clicking &#x2611;

![alt text](/exercises/ex4/images/10_add_projection.png) 

11. Now, the __Graphical Service Model__ and the __Storyboard__ will look as follows: 

![alt text](/exercises/ex4/images/11_graphic_modeller.png)

![alt text](/exercises/ex4/images/12_storyboard_final.png)



## Exercise 4.2 Connect your application to the Business Partner API Sandbox Environment

12. As a first step, a custom Node.Js service-handler is required to be able to read from external api.
- In the project folder, under ./srv create a new (service-handler) file (*.js-file), name it bp_api.js. 
- Then copy the below code into the file:

```cds 
const cds = require('@sap/cds');
module.exports = cds.service.impl(async function() {
    const bp = await cds.connect.to('API_BUSINESS_PARTNER');    
    this.on('READ', 'A_BusinessPartner', async req => {        
        return bp.run(req.query);       
    });
});
```
13. Now, go back to the SAP Business Accerlator Hub and copy the API Key for the selected Business Partner (A2X) and add it to API_BUSINESS_PARTNER configuration in the package.json file

![alt text](/exercises/ex4/images/image-20.png)  

```json
      "API_BUSINESS_PARTNER": {
          "kind": "odata-v2",
          "model": "srv/external/API_BUSINESS_PARTNER",
          "credentials": {
            "url": "<URL>",
             "headers": {
                "APIKey": <API_KEY>
            }
          }
        },

```

14. Moreover, copy the SANDBOX_URL from the SAP Business Accerlator Hub and apply it also to the package.json-file

![alt text](/exercises/ex4/images/image-11.png)


```json
      "API_BUSINESS_PARTNER": {
          "kind": "odata-v2",
          "model": "srv/external/API_BUSINESS_PARTNER",
          "credentials": {
            "url": "https://sandbox.api.sap.com/s4hanacloud/sap/opu/odata/sap/API_BUSINESS_PARTNER/",
             "headers": {
                "APIKey": <API_KEY>
            }
          }
        },

```
15. Save and close the files.

You've now created a custom handler for your service. This time it called on for the READ event.

The handler is invoked when your BusinessPartner service is called for a READ, so whenever there’s a request for business partner data, this handler is called. It ensures the request for the business partner is directed to the external business partner service. 

## Summary

You have now successfully extended the CAP service with the consumption of an external Business Partner Service

Additionally if you want to try the same with S4 system, follow this - [ Add Business Partner Data ](../ex_optional/README.md)

Continue to  - [Exercise 5 - Add UI to your application ](../ex5/README.md)






