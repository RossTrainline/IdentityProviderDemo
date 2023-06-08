## Identity Provider Demo
Example solution to demonstrate IdentityServer being used as an identity provider (IDP).

This solution contains two projects; an instance of IdentityServer running as an IDP and an instance of IdentityServer providing access control. Both projects have been created using Duende IdentityServer.Templates (dotnet new isinmem). They contain in-memory users, clients and resources rather than using a database implementation for these objects.

- The IdentityServer instance uses `AddOpenIdConnect()` to register the new identity provider in `HostingExtensions.cs`.
- The IdentityProvider instance has the IdentityServer instance added as a `Client` in `Config.cs`.

### Instructions for running locally

+ Set both IdentityProviderDemo and IdentityServerDemo as startup projects
+ Click ***Start***
+ Navigate to https://localhost:5002/Account/Login?ReturnUrl=%2Fdiagnostics (port 5002 is running the demo IdentityServer instance)
+ Click ***OpenIdConnect*** in the external account box (this redirects to the demo IdentityProvider instance on port 5001)
+ Enter credentials (bob/bob or alice/alice) and click ***Login***
+ You should then be authenticated and redirected back the the IdentityServer instance
+ On logging out you are redirected back to a logout page on the identity provider instance.

###Sequence Diagram
                    
```seq
Note left of IdentityServer: User Clicks OpenIdConnect on login screen
IdentityServer->IdentityProvider: Redirect to /connect/authorize endpoint
IdentityProvider->IdentityProvider: Redirect to /account/login page
Note right of IdentityProvider: User enters credentials and clicks Login
IdentityProvider->IdentityProvider: Redirect to /connect/authorize/callback
IdentityProvider->IdentityServer: Redirect to /signin-oidc
IdentityServer->IdentityServer: Redirect to /ExternalLogin/Callback
IdentityServer->IdentityServer: Redirect to /diagnostics
```