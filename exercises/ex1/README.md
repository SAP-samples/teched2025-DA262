# Exercise 1 - Create a Full-Stack Project in SAP Build Code
1. Navigate to the <a href="https://sap-build-eu10-trial-4-ykabxtjj.authentication.eu10.hana.ondemand.com/login" target="_blank">SAP Build Lobby</a>🚀.

2. In the Lobby's All Projects view, _click_ Create, and from the dropdown list select __Create__ to create a new project from scratch.
<br>![](/exercises/ex1/images/1-create-from-lobby.png) 

>💡 __Insight Corner__: Alternatively to the Create (from scratch) option, one could copy existing projects to your SAP Build workspace in the following ways:
>- Clone from Git: to clone an existing project from a Git repository.
>- Add from Dev Space: to add a project from an existing SAP Business Application studio dev space to your SAP Build Code tenant.    

3. Then in the dialog Select __Application__ tile and Select __Next__ :
<br>![](/exercises/ex1/images/2-choose-application.png)  

4. Select the type of application you want to create.
   - Select __Full-Stack__ Application to create an application of type Full Stack with Productivity Tools & Select __Next__ . This will enable you to develop, extend, and deploy your app. 
<br>![](/exercises/ex1/images/3-choose-full-stack.png)  

5. Select __Full Stack Node-JS__ which is based on SAP Cloud Application Programming (CAP) for the backend and SAP Fiori and MDK for the frontend & Select __Next__ .
<br>![](/exercises/ex1/images/4-full-stack-node.png)


6. Enter the project details 
   - __Name__ : Risk_Management_U<##>
   - __Description__ : Risk Management Application & Services 
   - __Dev Space__ : Select the dev space where you want the project to reside, the one you created in the [previous exercise](../dev-space-config/README.md) and choose __TechEdDA262_U<##>__
   > In U<##>, ## where ## represents the unique number assigned to you at the start of the exercise (for example, 01 or 39)

   >💡 __Insight Corner__:
   SAP Build Code recommends the dev space it deems most suitable, and it will automatically create a new one for you if you don’t already have one. If you have other dev spaces of the same type (for example, Full-Stack), you can select between them. If you want to create a different dev space, or a dev space or another type, go to the Dev Space Manager. See Working in the Dev Space Manager.

<br>![](/exercises/ex1/images/5-enter-project-details.png)


7. Select __Review__ to review the project details. 

<br>![](/exercises/ex1/images/6-review-project-details.png)

8. Click __Create__.

- You can see the project being created in the project table view of the lobby. The creation of the project may take a few moments.

<br>![](/exercises/ex1/images/7-view-created-project.png)

9. Once project has been created successfully, click on the name of the project to open it. This will open in SAP Business Application Studio, the SAP Build Code development environment. 

> __ℹ️ NOTE__: The project will load in a new tab. You may see some pop-ups in the bottom corner, these can be ignored. You’ll also see a Cloud Foundry Login pop-up, which you can skip for now, as it will be covered in Exercise 6.


## SAP Build Code IDE Configurations

- Open the newly created project, then right-click on the Activity Bar (on the left side) to verify that the extensions shown below are enabled. If any are missing, right-click on an empty area of the Activity Bar and enable them.

<br>![](/exercises/ex1/images/12_activitybar.png)

<br>![](/exercises/ex1/images/8-activity-bar-list.png)

- Additionally, to display the menu bar on the top, Click on the settings icon.

<br>![](/exercises/ex1/images/9-settings.png)

- Search for Menu Bar Visibility and click on the drop down and choose Classic

<br>![](/exercises/ex1/images/10-menu-bar-visibility.png)

- Now your menu bar will look as shown below:

<br>![](/exercises/ex1/images/11-menu-bar.png)

Continue to - [Exercise 2 - Create Data Model & Service with Joule](../ex2/README.md)