# Migrate Canvas Apps

Solution for migrating Canvas Apps and their related resources like Flows between environments. It includes simple Canvas App with supporting Cloud Flows and Custom Connector to do the migration by automating Power Platform legacy export/import functionality

As this solution uses **Custom Connector**, all the users need to have standalone Power Apps license Power Apps Premium or Power Apps per App

## Known issues
- Source and target environments are listed using "PowerAppsforMakers.GetEnvironments()" which returns might return environment where user of the tool does not have any permissions. Migration fails if user does not have Environment Maker permissions if the target environment

## Installation and configuration

### Create Entra ID application registration

Solution uses Power Platform BAP API to export and import packages, so we need to create Microsoft Entra ID application registration for the user authentication. Follow the steps below to register new Entra ID application. Depending of your tenant settings, you might need Global Administrator permissions to be able to register new application. 

1. Open the application blade in Microsoft Entra portal [Entra ID App Registrations](https://entra.microsoft.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
   
2. Click **New registration**

   <img width="200" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/07cf9acc-8210-452c-bc49-1612f69197bd">

<br>

3. Provide a name for the app and click **Register**

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/3121299f-ca64-4f3f-9ffc-b653aa2b0f60">
 
<br>

4. Copy **Application (client) ID** and **Directory (tenant) ID** from the Overview page to notepad for example
   
   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/28ae6806-a018-4a0c-a9a0-b8a7e7e1de23">
   
<br>

5. Browse to **Certificates & Secrets** blade anc click **+ New client secret**
   
   <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/f2bf4574-c3c7-4253-ae79-b6a20ad8e06a">

<br>

6. Define description for the secret and select expiration time

   <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c89eb9e2-dceb-47c7-a58a-b4b3329a956b">
   
<br>

7. Copy client secret value to notepad

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c826a0a4-98a3-4470-9050-eb528f62e504">
   
<br>

8. Browse to **API permissions** blade and click **+ Add a permission**

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/7b4fc919-2545-4152-988e-4e7519b45ccb">
   
<br>

9. Select **APIs my organization uses**, then search for **PowerApps** and select **PowerApps Service**

    <img width="250" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/1f0097bf-dc49-4a6d-b5a4-19d7a8c60659">

    
<br>

10. Select **Delegated permissions**, then select **User** permission and click **Add permissions**
    
    <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/0977ae9b-554b-459c-89d5-10312b97310c">
    
<br>

11. (optional) Grant admin consent for the application. If you do not do this then all users need to grant consent in the first use of the Canvas App migration tool

    <img width="250" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c3936ae3-88bc-4899-8aaa-971a9e6b6310">

<br>


### Import Migrate Canvas Apps - Prerequisites solution

This Power Platform solution installs Custom Connector

1. Open the environment to where you want to install the solution
  
2. Select **Solutions** from the menu and click **Import solution**
   
   <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/afd61089-f6b5-4f85-9452-9dd0bac21680">

<br>

3. Click **Browse** and select the **MigrateCanvasAppsPrerequisites_x_x_x_x_managed.zip** solution then click **Next**
   
   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c3902060-1380-4ff1-8b5c-70eeed7ecad0">

<br>

4. Fill-in **Client ID** and **Tenant ID** you copied to notepad and click **Import**. NOTE! Current version support now only plaintext client secret which need to be set after solution import
   
   <img width="200" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/297ea01d-3b3f-46db-b06e-cfdb55652aaa">

<br>

5. After solution is imported, select **Custom Connectors** from the menu and click the pencil icon of the **Migrate Canvas Apps** custom connector to open it in edit mode

   <img width="450" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/9e4eb7b7-c24f-439e-bca9-428aacdabbd1">

<br>

7. Select **2. Security** and click **Edit**

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/f8880165-396a-4998-a67e-f13c8892916f">

<br>

8. Copy-paste the **Client Secret** and click **Update connector**

   <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/3e8a667a-16f0-4937-b417-4a5eb5e579e6">

<br>

9. When connector is updated, scroll down to bottom of the page and copy **Redirect URL**

   <img width="400" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/e252f2be-5406-43a6-9d6d-3169c7d77b23">

<br>
   
10. Move back to Entra ID application registration we did in the first step and in the **Overview** page click **Add Redirect URI**

    <img width="450" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/777e55d2-f81d-4422-a6dc-cc3cce167ede">

<br>

11. Click **+Add a platform**

   <img width="200" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/94bc0714-16ee-44c8-92c0-71f683964589">

<br>
    
12. Select **Web**

    <img width="200" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/e205e102-fed6-4874-b9a1-1a93a25117a5">

<br>

13. Paste URL to the **Redirect URIs** field and click **Configure**

    <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/684701c1-37cf-4e62-bc33-4f21635e64bd">

<br>

14. Go back to custom connector configuration, select **5. Test** and click

    <img width="150" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/cbe2c8d4-c4ab-46bb-8b61-2b07d6a25b3b">

<br>

15. Create new connection by cliking **Create** and login with your account

    <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/4532948d-0257-45d6-8fba-ac145652700b">

<br>

16. (optional) You can test the connector by defining EnvironmentID and use the following body with your own application ID which you can find from the Canvas App details page. If you get response 200 and can see the application details in the response body, then custom connector is configured correctly

    ```
    {
       "baseResourceIds": [
          "/providers/Microsoft.PowerApps/apps/REPLACE-WITH-YOUR-APPID"
       ]
    }
    ```

    <img width="550" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/09428425-4061-4d3f-a56e-5fa963f07df4">

<br>

### Import Migrate Canvas Apps solution

1. Browse to **Solutions** of the environment and click **Import solution**

   <img width="250" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/08c1d5df-d45d-4539-899e-b6dad7f25597">

<br>

2. Click **Browse** and select the **MigrateCanvasApps_x_x_x_x_managed.zip** solution then click **Next**

   <img width="250" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/0f02ec45-27b4-4c85-b602-06b3d429e685">

<br>

3. Verify that all the connection are created. If not then click three dots next to connector to create new connection

   <img width="350" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/b3bfd212-e967-420f-97d3-d727398176d0">

<br>

4. With **Target Environment Types** environment variable we can control which type of environments are allowed as target environment for the migration

   <img width="600" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/b2ef3c48-c725-46bf-aaeb-7d7007011560">

<br>

4. Click **Import**

<br>
<br>

# How to use the tool

App migration status is saved to **Migration Job** Dataverse table and users need to have read-write permissions to this table. Solution contains own security role **Canvas Apps Migration Users** which grant Create, Read and Write permissions to the table as well as Read permissions to Process (workflows) system table, so add this security role to app app users
  
   <img width="500" alt="image" src="Images/securityrole.png" />
   
<br>
<br>

1. Run the **Migrate Canvas Apps** app

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/93d51143-59d8-4dc3-aa8e-aa4b936bbcc1">

<br>

2. Consent all connection by clicking **Allow**

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/d7fd1b7b-5aa7-4ce4-8888-5fc18bd22ec5">

 <br>
 
3. Tool support canvas apps migration and app permission migration between apps. Select **Migrate Canvas App**
   
   <img width="500" alt="image" src="Images/home.png" />
  
   <br>

4. Select source environment

   <img width="150" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c4633e20-37d5-44ad-884d-0a896e2d04ac">

 <br>
 
4. Select the app you want to migrate. This will run GetPacjageResources flow to fetch all related resources of the app like flows and connections

   <img width="300" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/c7e897f2-1fa5-44e2-9e39-72d5ace4a897">

   <br>

   **NOTE!** If app is using cloud flows then tool will notify that user has to reconnect (remove/add) all the flows to the app after the migration

   <img width="400" alt="image" src="Images/app-has-flows-warning.png">

   <br>
 
5. Select target environment

   <img width="200" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/0f203608-b3e4-45dd-adac-f540831bb7c6">

<br>

6. If there are flows included in the app having any connections, then you need to create all the connections before migration.

   You can select existing connection or create new connection by clicking the "**+**" icon. This will open connection creation to new browser tab for target environment. Remember to click **Refresh** to see the new connection(s) you created

   <img width="550" alt="image" src="Images/flow-conns.png">
   
   <br>

   <img width="450" alt="image" src="Images/sp-conn-example.png">

   <br>

   **NOTE!** Browser pop-up blocker might block opening new browser tab. If that is the case then one option is to open target environment in  make.powerapps.com and manually create connections

<br>

8. After connections are created, click **Refresh** to fetch connections. You can still select connection you want using the dropdown control if there are multiple connections

   <img width="500" alt="image" src="https://github.com/petepuu/Migrate-Canvas-Apps/assets/8307644/56446a13-9490-4b27-8d1d-c9de3e72835b">

<br>

9. Click **Migrate** to start the app migration. It takes few minutes depending how many related resources there are in the app


    <img width="450" alt="image" src="Images/app-migrating.png">

<br>

10. If migration was succefull then click **Next Step** to move to the next step of the process

    <img width="650" alt="image" src="Images/app-migrated-successfully.png">

<br>

11. In this step you can see all the permissions (if any) of the app in source environment and if you want you can share the migrated app in target environment with same users and groups. There is an option for you to select should the users be notified after app is shared with them. So, either click **Share App** with desired sharing notification setting or just click **Next Step** if you do not want to share the app for now

      <img width="600" alt="image" src="Images/share-app.png">

      <br>

      If you shared the app then when sharing is completed you should see the green success icon for each permission like below

      <img width="600" alt="image" src="Images/app-shared.png">

<br>

13. In the last step you have following options
    
      - Rename app in source environment by appending " - MIGRATED" to the end of the app display name
      - Remove all the other permissions except your own (if app is shared)
      - Send email notification to users and groups of the app about the change (if app is shared). You can change the recipients, subject and the email body but <u>tool will add a link to the migrated app in target environment to the end of the email</u>

         <br>
         <img width="600" alt="image" src="Images/last-step.png">

