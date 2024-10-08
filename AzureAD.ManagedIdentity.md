To set up authentication in a Blazor Server application using Azure AD and MSAL, you can use Managed Identity and Azure App Service’s built-in authentication with the following steps:

### Step 1: Configure Azure AD and Managed Identity
1. **Create an Azure AD App Registration** for your Blazor Server application:
   - Go to **Azure Active Directory** > **App registrations** > **New registration**.
   - Enter the name, and under **Redirect URI**, select **Web** and specify your app's URL (e.g., `https://yourapp.azurewebsites.net/signin-oidc` for Azure).
   - Enable ID tokens in **Authentication** settings.

2. **Enable Managed Identity for the App Service**:
   - In your Azure App Service, go to **Identity** and enable **System Assigned** or **User Assigned** Managed Identity.
   - Note the **Client ID** of this Managed Identity for later steps.

3. **Assign API permissions** to the App Registration:
   - In **API Permissions**, add Microsoft Graph API permissions, such as **User.Read** for basic user profile data.
   - Grant admin consent for these permissions.

### Step 2: Update `appsettings.json`
In your Blazor Server project’s `appsettings.json`, add the following configuration:
```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "yourdomain.onmicrosoft.com",
    "TenantId": "your-tenant-id",
    "ClientId": "your-app-registration-client-id",
    "CallbackPath": "/signin-oidc"
  }
}
```

### Step 3: Configure MSAL Authentication in `Program.cs`
Modify your `Program.cs` to add MSAL authentication:
```csharp
builder.Services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApp(builder.Configuration.GetSection("AzureAd"));

builder.Services.AddAuthorization(options =>
{
    options.FallbackPolicy = options.DefaultPolicy;
});
```

### Step 4: Update Login and Logout Links in Layout
In the main layout (e.g., `MainLayout.razor`), add login and logout links:
```razor
@if (!User.Identity.IsAuthenticated)
{
    <a href="MicrosoftIdentity/Account/SignIn">Login</a>
}
else
{
    <a href="MicrosoftIdentity/Account/SignOut">Logout</a>
    <p>Hello, @User.Identity.Name!</p>
}
```

### Step 5: Publish to Azure
1. Right-click the project in Visual Studio > **Publish** > Select **Azure** and your **App Service** instance.
2. Deploy, and Azure will handle authentication via Azure AD.

### Optional: Testing Managed Identity
If your application also uses Managed Identity to authenticate with other Azure resources, ensure the service code uses `DefaultAzureCredential` for resource access, and that the Managed Identity has the required permissions.