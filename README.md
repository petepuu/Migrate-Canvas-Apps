# Power Platform Tools

This is a repo of Power Platform helper tools

## Migrate Canvas Apps

Solution for migrating Canvas Apps and their related resources like Flows between environments. Solution includes simple Canvas App with supporting Cloud Flows and Custom Connector to do the migration. Migration  automated way to export/import apps. 

### Installation and configuration

#### Create Entra ID application registration

Solution uses Power Platform BAP API to export and import packages, so we need to create Microsoft Entra ID application registration for the user authentication. Follow the steps below to register new Entra ID application. Depending of your tenant settings, you might need Global Administrator permissions to be able to register new application. 

1. Open the application blade in Microsoft Entra portal [Entra ID App Registrations](https://entra.microsoft.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
   
2. Click **New registration**

   <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/71a52f05-ea43-42f4-8f66-944138a82aa8">
   
<br>

3. Provide a name for the app and click **Register**

   <img width="400" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/ca42f7b5-b137-4ffe-8878-609008e84be0">
   
<br>

4. Copy **Application (client) ID** and **Directory (tenant) ID** from the Overview page to notepad for example
   
   <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/b4e27866-a855-4331-8c09-c560c4bed5f5">
   
<br>

5. Browse to **Certificates & Secrets** blade anc click **+ New client secret**
   
   <img width="400" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/f158b353-5ee2-4060-8f14-48b88f57c1a0">
   
<br>

6. Define description for the secret and select expiration time

   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/f753024c-c490-48ce-845a-223456e23bf1">
   
<br>

7. Copy client secret value to notepad

   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/90937c80-d2dc-446a-8341-c374852f69fe">
   
<br>

8. Browse to **API permissions** blade and click **+ Add a permission**

   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/1af2ccc0-54d0-41e7-885b-15084c818162">
   
<br>

9. Select **APIs my organization uses**, then search for **PowerApps** and select **PowerApps Service**

    <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/ee634259-b162-4a93-b64a-c6b294251a59">
    
<br>

10. Select **Delegated permissions**, then select **User** permission and click **Add permissions**
    
    <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/35480805-2176-4e65-9f5b-64e62ca89190">
    
<br>

11. (optional) Grant admin consent for the application. If you do not do this then all users need to grant consent in the first use of the Canvas App migration tool

    <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/a4bc2033-ac07-4308-b450-1658a60f4c0b">

<br>


#### Import Migrate Canvas Apps - Prerequisites solution

This Power Platform solution installs Custom Connector

1. Open the environment to where you want to install the solution
  
2. Select **Solutions** from the menu and click **Import solution**
   
   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/76f547bc-03c1-45fc-ac91-0a339e833b8a">

<br>

3. Click **Browse** and select the **MigrateCanvasAppsPrerequisites_x_x_x_x_managed.zip** solution then click **Next**
   
   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/ee6c4749-54c7-4d5b-a97e-a4247e695ad1">

<br>

4. Fill-in **Client ID** and **Tenant ID** you copied to notepad and click **Import**. NOTE! Current version support now only plaintext client secret which need to be set after solution import
   
   <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/cd386a87-8009-49ac-8039-aefae0ae761b">

<br>

5. After solution is imported, select **Custom Connectors** from the menu and click the pencil icon of the **Migrate Canvas Apps** custom connector to open it in edit mode

   <img width="450" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/39fc9ccb-0065-458d-8177-d81e94884b69">

<br>

7. Select **2. Security** and click **Edit**

   <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/e9e9f212-6955-4e2c-b71d-8fcf40d4fa7a">

<br>

8. Copy-paste the **Client Secret** and click **Update connector**

   <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/c1248819-29d0-4f0f-8af4-fa1a231f3b64">

<br>

9. When connector is updated, scroll down to bottom of the page and copy **Redirect URL**

   <img width="400" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/8106b29e-fbcf-4201-a731-ed12576f1442">

<br>
   
10. Move back to Entra ID application registration we did in the first step and in the **Overview** page click **Add Redirect URI**

    <img width="450" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/3d10f7c6-4138-411e-8f98-6e497fa25e01">

<br>

11. Click **+Add a platform**

   <img width="200" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/7db241f5-83cc-4047-95fe-5c73bbb01e33">

<br>
    
12. Select **Web**

    <img width="200" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/c8372736-dc58-4e29-93d2-e791ab24af1d">

<br>

13. Paste URL to the **Redirect URIs** field and click **Configure**

    <img width="350" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/61d199de-54a8-42e0-b4f0-13b0b96f3527">

<br>

14. Go back to custom connector configuration, select **5. Test** and click

    <img width="150" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/b84e62f7-ee40-4787-9234-d2f2ab02e3be">

<br>

15. Create new connection by cliking **Create** and login with your account

    <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/c8fd1433-7413-4b8f-b90d-e9e63ca30c54">

<br>

16. (optional) You can test the connector by defining EnvironmentID and use the following body with your own application ID which you can find from the Canvas App details page. If you get response 200 and can see the application details in the response body, then custom connector is configured correctly

    ```
    {
       "baseResourceIds": [
          "/providers/Microsoft.PowerApps/apps/REPLACE-WITH-YOUR-APPID"
       ]
    }
    ```

    <img width="600" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/19ec245c-9160-472a-90fb-ab895bfa7c93">

<br>

#### Import Migrate Canvas Apps solution

1. Browse to **Solutions** of the environment and click **Import solution**

   <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/fae84d17-3e4a-4cc6-95b4-c20b08225ddc">

<br>

2. Click **Browse** and select the **MigrateCanvasApps_x_x_x_x_managed.zip** solution then click **Next**

   <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/57c9b62a-6de5-470c-8483-99a82c27c55e">

<br>

3. Verify that all the connection are created. If not then click three dots next to connector to create new connection

   <img width="400" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/bddfc6e9-249d-479a-b06e-c7031dd6f9df">

<br>

4. Solution import takes couple minutes

<br>

## How to use the tool

Migration status is saved to **Migration Job** Dataverse table and users of the app needs to have read-write permissions to this table. Solution contains own security role **Canvas Apps Migration Users** which grant Create, Read and Write permissions to the table, so add this security role to app app users

1. Run the **Migrate Canvas Apps** app

   <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/379b47fd-2836-4b71-8fef-ff74815d4794">

<br>

2. Consent all connection by clicking **Allow**

   <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/983d8f1b-9471-48fc-9306-4b5d78eba7f6">
 
 <br>
 
3. Select source environment

   <img width="200" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/80df4716-d919-41c8-81df-a9db736555d6">

 <br>
 
4. Select the app you want to migrate. This will run GetPacjageResources flow to fetch all related resources of the app like flows and connections

   <img width="300" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/a7fcfed6-39fd-4f5d-88ac-a6e9924f7fcb">

 <br>
 
5. Select target environment

   <img width="250" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/a04d02bd-1b35-4f7e-aaf6-b0c00dd76fe4">

<br>

6. If there are flows included in the app having any connections, then you need to create all the connections before migration.

   You can create new connection(s) or select existing connection(s). Connection creation opens to new browser tab for target environment. If you want to create all connections click **Create All** which opens own browser tab for each connection.

   <img width="600" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/2581209d-df95-4de4-ac46-e424251c0a83"> 
   
   <img width="650" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/a4236efb-4ef0-4012-82ce-179273a49b25">

   **NOTE!** Browser pop-up blocker might block opening new browser tab. If that is the case then one option is to open target environment in  make.powerapps.com and manually create connections

<br>

8. After connections are created, click Refresh to fetch connections. You can still select connection you want using the dropdown control if there are multiple connections

   <img width="500" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/f4bb4b69-6abf-4d8d-afa6-5435572111a9">

<br>

9. Click **Migrate** to start the app migration. It takes few minutes depending how many related resources there are in the app

    <img width="450" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/cefc829e-c2f6-4fd7-9f4d-ae5266ec632d">


11. If migration was succefull then click **Next Step** to move to the next step of the process

    <img width="550" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/4982836b-5a4a-40ef-b663-6d17486cdec7">

<br>

11. Select the migrated app from the list. If the app is not listed then click refresh icon to fetch your apps from target environment.

    **NOTE!** sometimes it can take little while for the migrated app to be visible here

    <img width="550" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/039a4817-ad5f-4964-a757-d983e851ed72">
   
<br>

12. Now you should see all the permissions (if any) of the app in source environment. You can remove permissions if needed or do not share the app in target environment at all. Make the necessary changes and click **Share App** or click **Next Step** if you do not want to share the app for now

    <img width="550" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/3d2ef5c2-f552-43a9-bc62-05a717277cf3">

    If you shared the app then when sharing is completed you should see the green success icon for each permission like below

    <img width="550" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/9636fdf4-72fd-4ed2-bcb4-043ef7fbe41a">
      
<br>

13. In this step you have following options
    
      - Rename app in source environment by appending "(MIGRATED)" to the end of the app display name
      - Remove all the other permissions except your own
      - Remove the old version from source environment

         <br>
         <img width="400" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/454c8a28-7205-49e8-bda2-7ea1b470abe5">

<br>

14. In last step you can send an email notification to all the users and groups about the migration (group members are not extracted, so email is send to group address). Email contains app link to the new app in target environment

    <img width="600" alt="image" src="https://github.com/petepuu/power-platform-tools/assets/8307644/4a679d9d-f529-4196-9279-53b6bbb3c27c">


  
