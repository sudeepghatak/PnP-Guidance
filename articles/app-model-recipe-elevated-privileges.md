App model recipe - Elevated privileges
======================================

Summary
-------

The approach you take to elevate privileges in your code is different in the new app model than it was with Full Trust Code.  In a typical Full Trust Code (FTC) / Farm Solution scenario, the RunWithElevatedPrivileges  API is used with the SharePoint Server Side Object Model code and deployed via Farm Solutions.

In an app model scenario, the AllowAppOnlyPolicy permission or a service account is used to allow the current user to execute operations they are not authorize to perform.

High Level Guidelines
---------------------

As a rule of a thumb, we would like to provide the following high level guidelines for elevating privileges in code.

- AllowAppOnlyPolicy does not work with 
	+ Search
	+ User Profile CSOM operations
	**Note:** In these scenarios you need to use a specific service account.
- AllowAppOnlyPolicy is similar to RunWithElevatedPrivileges, but not exactly the same.
	+ AllowAppOnlyPolicy executes code based on the permissions granted to the app, not on behalf of another user who has the appropriate permissions to perform an operation.

Here is an example of returning an App Only Policy token and using it to create a context object.

	Uri siteUrl = new Uri(ConfigurationManager.AppSettings["MySiteUrl"]);
	try
    {
    	//Connect to the give site using App Only token
    	string realm = TokenHelper.GetRealmFromTargetUrl(siteUrl);
    	var token = TokenHelper.GetAppOnlyAccessToken(TokenHelper.SharePointPrincipal, siteUrl.Authority, realm).AccessToken;

    	using (var ctx = TokenHelper.GetClientContextWithAccessToken(siteUrl.ToString(), token))
    	{
    		// Perform operations using the app only token access. 
    	}
    }
    catch (Exception ex)
    {
    	Console.WriteLine("Error in execution: " + ex.Message);
    }

- When using the AllowAppOnlyPolicy keep in mind it only works in Provider-hosted apps.
- AllowAppOnlyPolicy does not execute code on behalf of a user and therefore may not be appropriate for all scenarios.
- Service accounts are defined in SharePoint.
	+ In an Office 365 tenancy, depending what functionality your code requires have, the service accounts may need an Office 365 license assigned to them.
	+ You can create service accounts on a per app basis, or use a single account for all apps.
	+ Create clear and descriptive names for the service accounts so you can easily track the operations they perform.
	
	For example: If your app modifies list items, the Modified By column for the list items will display the name of the service account associated with the app.

- When authenticating with service accounts, you must retrieve a user name and password for the service account.
	+ The code snippet below illustrates using a user name and password to authenticate.
	+ Take care to store and retrieve the user name and password in a secure fashion.

	```
	using (ClientContext context = new ClientContext("https://tenancy.sharepoint.com"))
	{
	
		// Use default authentication mode
		context.AuthenticationMode = ClientAuthenticationMode.Default;	
		// Specify the credentials for the account that will execute the request
		context.Credentials = new SharePointOnlineCredentials("User Name", "Password");
	}
	```

Options to elevate permissions
------------------------------

You have a couple of options to elevate permissions.

- OAuth (AllowAppOnlyPolicy)
	+ S2S (sub option)
	+ ACS (sub option)
- Service Account
	+ Remotely hosted code (Example: Azure WebJob)

OAuth (AllowAppOnlyPolicy)
--------------------------
In this pattern the AllowAppOnlyPolicy is set to true in the AppPermissionRequests element and permissions are set in the app manifest. OAuth is used to return access tokens to allow the app to execute operations it has permissions to perform.

**S2S sub option**

The S2S sub option only works in on-premises SharePoint environments.

When authenticating via OAuth in an S2S scenario the **TokenHelper::GetS2SAccessTokenWithWindowsIdentity** method is used to return the access token for the app.  The access token allows the app to perform any operations the app is granted in the app manifest.

This option does not execute code on behalf of a user and therefore may not be appropriate for all scenarios.

**When is it a good fit?**

When you need to elevate privileges in a SharePoint S2S scenario this is a good option because this option works with S2S and is very easy to implement.

**Getting Started**

The following article demonstrates how to use AllowAppOnlyPolicy with S2S.

- [SharePoint 2013 App Only Policy Made Easy (Kirk Evans - MSDN Blog Post)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)

**ACS sub option**

The ACS sub option work in on-premises and Office 365 SharePoint environments.

When authenticating via OAuth in an ACS scenario the **TokenHelper::GetAppOnlyAccessTokenmethod** is used to return the access token for the app.  Then, the **TokenHelper::GetClientContextWithAccessToken** method is invoked to return the client context necessary to perform any operations the app has permission to do based on the permissions granted in the app manifest.

This option does not execute code on behalf of a user and therefore may not be appropriate for all scenarios.

**When is it a good fit?**

When you need to elevate privileges in a SharePoint ACS scenario this is a good option because this option works with ACS and is very easy to implement.  This option is a good fit when you have an on-premises SharePoint environment that has an established trust with ACS.  This is your only OAuth option when you have a Office 365 SharePoint environment.

**Getting Started**

The following article demonstrates how to use AllowAppOnlyPolicy with ACS.

- [SharePoint 2013 App Only Policy Made Easy (Kirk Evans - MSDN Blog Post)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)

Service Account
---------------
In this pattern the SharePointOnlineCredentials class is used to establish the context of a user that executes code.

**When is it a good fit?**

When you need to execute code on behalf of a specific user (service account) this is a good option because it performs actions on the user's (service account) and the app's permissions.

**Getting Started**

The following article demonstrates how the SharePointOnlineCredentials class is used to establish the context of a user that executes code.

- [Getting Started with building Azure WebJobs ("Timer Jobs") for your Office 365 sites (Authentication considerations section) - Tobias Zimmergren Blog Article](http://azure.microsoft.com/en-us/documentation/articles/websites-dotnet-deploy-webjobs/)

Related links
=============
- [SharePoint 2013 App Only Policy Made Easy (Kirk Evans - MSDN Blog Post)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)
- [Getting Started with building Azure WebJobs ("Timer Jobs") for your Office 365 sites (Authentication considerations section) - Tobias Zimmergren Blog Article](http://azure.microsoft.com/en-us/documentation/articles/websites-dotnet-deploy-webjobs/)
- [SharePointOnlineCredentials class (MSDN API Documentation)](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.client.sharepointonlinecredentials.aspx)
- Guidance articles at [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "Guidance Articles")
- References in MSDN at [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "References in MSDN")
- Videos at [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPVideos "Videos")

Related PnP samples
===================

- [Core.SimpleTimerJob (O365 PnP Sample)](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.SimpleTimerJob)
- Samples and content at https://github.com/OfficeDev/PnP

Applies to
==========
- Office 365 Multi Tenant (MT)
- Office 365 Dedicated (D) *partly*
- SharePoint 2013 on-premises – *partly*

*Patterns for dedicated and on-premises are identical with app model techniques, but there are differences on the possible technologies that can be used.*

Author
------
Todd Baginski (Canviz LLC) - [@toddbaginski](https://twitter.com/toddbaginski)

Version history
---------------
Version  | Date | Comments | Author
---------| -----| ---------| ------
0.1  | May 18, 2015 | Initial draft | Todd Baginski (Canviz LLC)
0.2  | May 26, 2015 | Updates based on Vesa's feedback| Todd Baginski (Canviz LLC)
