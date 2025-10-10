# Exercise 4 - Consuming an external Business Partner Service

In this exercise, you will extend your application with the consumption of an external Business Partner service, to be connected to a S/4HANA Cloud Public Edition instance.
To facilitate that, we consume the API definition from the [SAP Business Accelerator Hub](https://api.sap.com/) and are able to connect to a SAP provided sandbox instance of S/4HANA Cloud Public Edition.  
By consuming the Business Partner API definition from SAP API Business Accerlator Hub, we ensure a system compliant API integration for the later runtime connection of our Risk Management Application with the real __clean core__ __"S/4HANA Cloud Public Edition"__-source system. 

<br>![](/exercises/ex4/images/1_apilogin.png)


 ## Exercise 4.1 Connect your application to the Business Partner API 
 
> __ℹ️ NOTE__: the Application-to-Cross Application (A2X) service category objects is targeted to facilitate the exchange of business information between a system and an unspecified client [service categories in APIs on SAP Business Accelerator Hub documentation](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/8308e6d301d54584a33cd04a9861bc52/1e60f14bdc224c2c975c8fa8bcfd7f3f.html?locale=en-US).

<br>__1.__ Go to the folder __business-partner__ and download the __API_BUSINESS_PARTNER.edmx__ file.

- Once downloaded, __upload__ the __.edmx-file__ to the root folder of your project.
![alt text](/exercises/ex4/images/2_upload.png)
![alt text](/exercises/ex4/images/3_upload_root.png)

<br>__2.__ Now, import the Business Partner API specification-file 
- Run the cds command in a project terminal. 
- To open Click on the icon as shown below, select terminal > new terminal
<br>![](/exercises/ex4/images/4_terminal.png)
```cds
cds import API_BUSINESS_PARTNER.edmx
```
<br>![](/exercises/ex4/images/5_cdsimport.png)  

<br>__3.__ The API_BUSINESS_PARTNER.edmx has been imported to the project folder __"srv/external"__
- In addition, it also generated the API_BUSINESS_PARTNER.csn file. CSN-files are used by the CDS framework, as a notation for compact representations of CDS models — tailored to serve as an optimized format to share and interpret models with minimal footprint and dependencies.

![alt text](/exercises/ex4/images/6_projstr.png)

<br>__4.__ Furthermore, the __./package.json__ file in your project root directory, is now updated in the __requires section__ with the external service.  
![alt text](/exercises/ex4/images/7_packagejson.png)

<br>__5.__ Now, let's link to the external Business Partner API in the data- and service-model definitions
- Add the below line in ./db/schema.cds and ./srv/service.cds

```cds 
using { API_BUSINESS_PARTNER } from '../srv/external/API_BUSINESS_PARTNER';
```
- the updated schema.cds and service.cds will be as follows :

![alt text](/exercises/ex4/images/9_schemacds.png)

![alt text](/exercises/ex4/images/10_servicecds.png)


## Exercise 4.2 - Edit the Data Model and Service Definition with Business Partner
In the following steps, we define the data model and service model relationship with the Business Partner API.

<br>__6.__ From your  __data model__ in the __Storyboard__  _select_ __"Open in Graphical Modeler"__ as shown below:
![](/exercises/ex4/images/11_opengm.png)

<br>__7.__  Select the __"Risks" entity__ 
- and __click__ on __"Add relationship", 
- then click into the empty space to open the next dialog
![alt text](/exercises/ex4/images/12_addrel.png)

<br>__8.__ Once the dialog box opens
- look for the __target entity-relationship__ shown in the image below 
  - Type: __Association__ and Cardinality: __To-One__
- and _rename_ the a_BusinessPartner to __BusinessPartner__.

![alt text](/exercises/ex4/images/13_dia.png)

<br>__9.__ Now the data model would look as follows :
![alt text](/exercises/ex4/images2/14_dm.png)

<br>__10.__ Go back to your __Storyboard__,  under __service__, select __"Open in Graphical Modeler"__ as shown below. 
- click on __Projection__ and add it to an empty space.

![alt text](/exercises/ex4/images/15_openservice.png)


![alt text](/exercises/ex4/images/17_addproj.png)

<br>__11.__ Under Projection definition on the right
- first select __"API_BUSINESS_PARTNER.A_BusinessPartner"__ as _key_ element of the associated entity
- then __"un-check" all properties__, 
- and __select__ on the following columns/properties:
  - __BusinessPartner, FirstName__ and __LastName__
- Save by clicking &#x2611;

![alt text](/exercises/ex4/images/18_projdets.png) 

<br>__12.__ Now, the __Graphical Service Model__ will look as follows: 

![alt text](/exercises/ex4/images/19_srvdef.png)

- and the __Storyboard__ will look as follows:

![alt text](/exercises/ex4/images/20_storyboard.png)



## Exercise 4.3 Connect your application to the Business Partner API Sandbox Environment

<br>__13.__ As a first step, a custom Node.Js service-handler is required to be able to read from external api.
- In the project folder, under ./srv __create__ a new (service-handler) file (*.js-file), name it __bp_api.js__. 
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
<br>__14.__ Now  __copy__ the __API Key__ and  the __SANDBOX_URL__ and add it to API_BUSINESS_PARTNER configuration in the __package.json__ file

```
"APIKey": "bevWzLBHsR00qBdYwhhbVttRIjhiA0rY"
```
```
https://sandbox.api.sap.com/s4hanacloud/sap/opu/odata/sap/API_BUSINESS_PARTNER
```

```json
      "API_BUSINESS_PARTNER": {
          "kind": "odata-v2",
          "model": "srv/external/API_BUSINESS_PARTNER",
          "credentials": {
            "url": "<URL>",
             "headers": {
                "APIKey": "<API_KEY>"
            }
          }
        }

```
<br>__15.__ Your __package.json__ should look as follows:

```json
      "API_BUSINESS_PARTNER": {
          "kind": "odata-v2",
          "model": "srv/external/API_BUSINESS_PARTNER",
          "credentials": {
            "url": "https://sandbox.api.sap.com/s4hanacloud/sap/opu/odata/sap/API_BUSINESS_PARTNER/",
             "headers": {
                "APIKey": "<API_KEY>"
            }
          }
        }

```
<br>__16.__ Save and close the files.

You've now created a custom handler for your service. This time it called on for the READ event.

The handler is invoked when your BusinessPartner service is called for a READ, so whenever there’s a request for business partner data, this handler is called. It ensures the request for the business partner is directed to the external business partner service. 

## Summary 📝

You have now successfully extended the CAP service with the consumption of an external Business Partner Service

Continue to  - [Exercise 5 - Add UI to your application ](../ex5/README.md)
