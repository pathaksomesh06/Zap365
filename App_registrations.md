# Azure AD Configuration

## Register an Application with the Microsoft Identity Platform

## Prerequisites

- **Azure Account**: You must have an Azure account with an active subscription.  
- **Permissions**: Your Azure account must be at least a **Cloud Application Administrator**.
- **Tenant Setup**:

## Register an Application

Follow these steps to create the app registration:

1. **Sign in** to the Microsoft Entra admin center as at least a **Cloud Application Administrator**.
2. **Switch Tenants** (if needed):  
   If you have access to multiple tenants, use the **Settings icon** in the top menu to switch to the tenant in which you want to register the application via the **Directories + subscriptions** menu.
3. **Navigate to App Registrations**:  
   Browse to **Identity > Applications > App registrations** and select **New registration**.

   **The app has to be registered as Multi-Tenant App**:
  

5. **Enter a Display Name**:  
   - Provide a display name for your application.  
   - This name might be seen by users during sign-in.  
   - You can change the display name at any time, and multiple app registrations can share the same name.  
   - **Note**: The automatically generated **Application (client) ID** uniquely identifies your app, not its display name. Copy the App ID, we will need it later during Intune configuration.
6. **Specify the Sign-In Audience**:  
   Choose who can use the application (sometimes called its sign-in audience).
7. **Configure Redirect URI**:  
   Leave **Redirect URI (optional)** alone for now; you'll configure it in the next section.
8. **Complete Registration**:  
   Select **Register** to complete the initial app registration.
9. **Review the Overview Pane**:  
   Once registration is complete, the Microsoft Entra admin center displays the app registration's **Overview** pane, which shows the **Application (client) ID** (client ID) that uniquely identifies your application.


  
## Add a Redirect URI

A redirect URI is where the Microsoft identity platform sends security tokens after authentication.

`Bundle ID` = `com.irl.zap365`
`Redirect URI` = `msauth.com.irl.Zap365://auth`



## Add a Application ID URI
Add the application ID URI as below:
- Click Application ID URI from the right hand pane
- Add App ID URI
- Verify that application ID URI is automatatically updated 

<img width="1055" alt="Screenshot 2025-05-02 at 21 31 17" src="https://github.com/user-attachments/assets/a902b92e-d88b-4d0e-91ce-7e6fbfe4e7e9" />


## In the Manage section, click API permissions

- Click Add a permission.
- The Request API permissions page appears.
- In the Microsoft APIs section, click Microsoft Graph.
- Select Delegated permissions as the type of permissions and below permissions
<img width="836" alt="Screenshot 2025-05-02 at 21 31 55" src="https://github.com/user-attachments/assets/3eb49084-f9fb-4417-99f2-b4637199cf60" />
- Navigate back to Manage & click Grant Admin Consent


# Intune Configuration
Use app configuration policies in Microsoft Intune to provide custom configuration settings for the app so that it can read the App ID you just created in previous step.

Follow these steps to create a managed devices configuration profile:

- **Sign in** to the Microsoft Intune admin center.
- Navigate to **Apps > Configuration > Create > Managed devices**.
- On the **Basics** page, set the following details:
   - **Name**: The name of the profile that appears in the Microsoft Intune admin center.
   - **Description**: The description of the profile that appears in the Microsoft Intune admin center.
   - **Device enrollment type**: This should be set to **Managed devices**.
- **Select iOS/iPadOS as the Platform**.
- Click **Select app** next to **Targeted app**.  
   The **Associated app** pane is displayed.
- In the **Targeted app** pane, choose the **Zap365** app to associate with the configuration policy, and click **OK**.
- Click **Next** to display the **Settings** page.
-  In the dropdown box, select the **Configuration settings format** and choose the **Enter XML data** option.
-  Paste the following XML data:
- Assign the profile to all users

```xml
<dict>
    <key>PayloadDescription</key>
    <string>Zap365 Configuration Profile</string>
    <key>PayloadDisplayName</key>
    <string>Zap365 Configuration</string>
    <key>PayloadIdentifier</key>
    <string>com.irl.Zap365.config1</string>
    <key>PayloadRemovalDisallowed</key>
    <false/>
    <key>PayloadType</key>
    <string>Configuration</string>
    <key>PayloadUUID</key>
    <string>3C03929C-D6CA-41F9-9E4E-3C2227868E11</string>
    <key>PayloadVersion</key>
    <integer>1</integer>
    <key>PayloadContent</key>
    <array>
        <dict>
            <key>PayloadType</key>
            <string>com.apple.app.managed</string>
            <key>PayloadVersion</key>
            <integer>1</integer>
            <key>PayloadIdentifier</key>
            <string>com.irl.Zap365.managedapp</string>
            <key>PayloadUUID</key>
            <string>CFD75594-390E-47F7-A7E1-F992DEBFF45A</string>
            <key>PayloadEnabled</key>
            <true/>
            <key>PayloadDisplayName</key>
            <string>Zap365 Managed App Config</string>
            <key>BundleIdentifier</key>
            <string>com.irl.Zap365</string>
            <key>Configuration</key>
            <dict>
                <key>AzureADClientID</key>
                <string>replace with your app id</string>
            </dict>
        </dict>
    </array>
</dict>
<img width="729" alt="Screenshot 2025-05-02 at 21 33 17" src="https://github.com/user-attachments/assets/82061a97-542c-47a9-9b36-a9e19e6d13ca" />
