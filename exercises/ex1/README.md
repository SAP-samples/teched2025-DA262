# Exercise 1 - Create a Full-Stack Project in SAP Build Code
1. Navigate to the [SAP Build Lobby](https://sap-build-us10-trial.us10.build.cloud.sap/lobby)🚀.

2. In the Lobby's All Projects view, _click_ Create, and from the dropdown list _select_ __Create__ to create a new project from scratch.
<br>![](/exercises/ex1/images/1-create-from-lobby.png) 

>💡 __Insight Corner__: Alternatively to the Create (from scratch) option, one could copy existing projects to your SAP Build workspace in the following ways:
>- Clone from Git: to clone an existing project from a Git repository.
>- Add from Dev Space: to add a project from an existing SAP Business Application studio dev space to your SAP Build Code tenant.    

3. Then in the dialog _Select_ __Application__ tile and _Select_ __Next__ :
<br>![](/exercises/ex1/images/2-choose-application.png)  

4. Select the type of application you want to create.
   - Select __Full-Stack__ Application to create an application of type Full Stack with Productivity Tools. This will enable you to develop, extend, and deploy your app. 
<br>![](/exercises/ex1/images/3-choose-full-stack.png)  

5. Select __Full Stack Node-JS__ which is based on SAP Cloud Application Programming (CAP) for the backend and SAP Fiori and MDK for the frontend.
<br>![](/exercises/ex1/images/4-full-stack-node.png)


6. Enter the project details 
   - __Name__ : Risk_Management_U<##>
   - __Description__ : Risk Management Application & Services 
   - __Dev Space__ : Select the dev space where you want the project to reside, the one you created in the [previous exercise](exercises/dev-space-config/) and choose __TechEdDA262_U<##>__
   > In U<##>, ## represents the number given to you in the beginning of the exercise. eg : 01 or 39

   >💡 __Insight Corner__:
   SAP Build Code recommends the dev space it deems most suitable, and it will automatically create a new one for you if you don’t already have one. If you have other dev spaces of the same type (for example, Full-Stack), you can select between them. If you want to create a different dev space, or a dev space or another type, go to the Dev Space Manager. See Working in the Dev Space Manager.

<br>![](/exercises/ex1/images/5-enter-project-details.png)


7. Select __Review__ to review the project details. 

<br>![](/exercises/ex1/images/6-review-project-details.png)

8. Click __Create__.

- You can see the project being created in the project table view of the lobby. The creation of the project may take a few moments.
<br>![](/exercises/ex1/images/7-view-created-project.png)

9. Once created you see a message stating that the project has been created successfully, click the project to open it.  

Success, the project is now prepared.
You can now open in SAP Business Application Studio, the SAP Build Code development environment. 

## SAP Build Code IDE Configurations

Open the newly created project, right click on the activity bar and check if the below extensions are enabled.

<br>![](/exercises/ex1/images/8-activity-bar-list.png)

Additionally, to display the menu bar on the top. Click on the settings icon.

<br>![](/exercises/ex1/images/9-settings.png)

Search for Menu Bar Visibility and click on the drop down and choose Classic

<br>![](/exercises/ex1/images/10-menu-bar-visibility.png)

Now your menu bar will look as shown below:

<br>![](/exercises/ex1/images/11-menu-bar.png)

Continue to - [Exercise 2 - Create Data Model & Service with Joule](../ex2/README.md)