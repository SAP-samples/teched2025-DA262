# Exercise 5 - Add a UI to your application

In this exercise, let's add a simple Fiori UI to your risk management application.

## Exercise 5.1 - Create the basic UI from a template 🖼️
<br>__1.__ Go to your __Storyboard__, _click_ on __'+'__ icon under __UI application__.
<br>![](/exercises/ex5/images/1_createui.png)

<br>__2.__ Enter the details for the UI application, 
- Display name: __RiskApplication__
- Application name: __riskapplication__
- select your project's __service-object name__ ( example : __'risk_Management_U00Srv'__ ) as a data source
- then click __"Next"__
<br>![](/exercises/ex5/images/2_ui_config.png)

<br>__3.__ Select __"UI application Type"__
- Click select __"Template-Based, Responsive Application"__
- then _click_ __"Next"__
<br>![](/exercises/ex5/images/3_ui_template.png)

<br>__4.__ Select the specific __"UI application Template"__
- Click select __"List Report Page"__
- then click __"Next"__
<br>![](/exercises/ex5/images/4_ui_template.png)

<br>__5.__ Select the __"Data Objects"__
- Choose __"Risks"__ as the main entity 
<br>![](/exercises/ex5/images/5_ui_template.png)

<br>__6.__ Click on __Finish__. 
- This step will add all the necessary libraries to your project which will take a couple of seconds.
- You've now created the List report UI to your application.
<br><br>  
## Exercise 5.2 - Now modify the UI  ✏️🖼️

In this section, We will customise the Fiori app by removing the Criticality field and instead deriving it from the Impact using the logic implemented in Exercise 3. Additionally, we will represent the calculated criticality with an icon and add a Business Partner field to assign the appropriate stakeholder to each risk.

<br>__7.__ Once all the necessary libraries have been added to your project, the page map will open.

>💡 __Insight Corner__: Page Map is a visual tool within the SAP Fiori Tools that provides a visual representation of an application’s pages, navigations, and service entities that it uses.

- [__Optional__]: Incase the page map doesnt open on its own, you can navigate to it by right clicking on the newly created UI module which is under the UI applications.
<br>![](/exercises/ex5/images/6_pagemap.png)

<br>__8.__  Click on the icon __Configure Page__ to edit the page's properties and settings. 
<br>![](/exercises/ex5/images/7_editpagemap.png)

<br>__9.__ In the columns list, delete the <strong>criticality</strong> column. SAP Fiori has the UI annotation of criticality which allows you to visually indicate if a value is negative, critical or positive via color-coding. Therefore we dont need an additional column. Click on delete icon to delete the column
<br>![](/exercises/ex5/images/8_delcrit.png)

<br>__10.__ Select the column <strong>impact</strong> and on the right pane, scroll down to find the criticality property and set the value to criticality.Therefore __impact__ will now show the criticality threshold based on the risks.

<br>![](/exercises/ex5/images/9_selcrit.png)

<br>__11.__ Wait a few seconds until the new option labeled __Criticality Representation__ appears. Then, add the criticality representation with an icon, this will display an icon next to the value, visually indicating the risk level.
<br>![](/exercises/ex5/images/10_criticon.png)

<br>__12.__ Go back to the page map and Select the configure icon of object page section.
<br>![](/exercises/ex5/images/11_gobackPM.png)

<br>![](/exercises/ex5/images/12_objpage.png)

<br>__13.__ Go to General Information > Form > Fields > Add Basic field. Here we can add the Business Partner to the UI of your application.

<br>![](/exercises/ex5/images/13_add.png)

<br>__14.__ Add a new column BusinessPartner. 
<br>![](/exercises/ex5/images/14_addbp.png)

<br>__15.__ Rename the BusinessPartner_BusinessPartner label to BusinessPartner. 
<br>![](/exercises/ex5/images/15_renamebp.png)

<br>__16.__ Close the pagemap, return to your __storyboard__ & it should now look like below. 

<br>![](/exercises/ex5/images/16_storyboardfinal.png)



## Summary  📝

Now the UI of your application is ready
Continue to - [Bind your application to an HDI Container & View the data in SAP HANA Database Explorer](../ex6/README.md)

