# I've got a secret 
## and I don't even know what it is

What can be a secret?

* Server Names
* Database Names
* Account Names
* Role Names
* File/Folder Paths
* IP Addresses
* URLs
* Anything that can give clues to potential exploits

Maslow's Hierarchy of Secrets:

|State of Keys|Grade|
|---|---|
|Only Development Settings Known | Best|
|Secrets Not in Source Control but Generally Known | Okay|
|Secrets Checked into Source Control | Nope|

## Configuration providers in .NET Core

* CLI args
* User Secrets
* Environment Variables
* Files
* Azure Key Vault

---

Adding environment variables pulls in ALL environment variables to configuration. Need to keep note of that from a security persepctive, especially running on own hardware.

---

User secrets `dotnet user-secrets init` allows you to securly store and use secrets for local development. 

---

Azure KeyVault allows you to store secrets, keys, and certificates in a centralized location. THis is managed independently of the key vault clients.

KeyVault configuration provider exists and can be used just like all other config providers. Requires:

* KV Name
* App Id
* App Secret

App Id and App Secret can be generated via [App Registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps)

1. Create new registration
   * Creates the application client ID
   * Go to Certificates & secrets, add a client
      * Generates the application secret, only shown once
1. Store these values in user secrets for local or environment variables for prod
1. Create KeyVault access policy

This is only a good idea if you are *NOT* running in Azure. When working in Azure use managed service identity and the following code:

``` csharp
var azureServiceTokenProvider = new AzureServiceTokenProvider();

var keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTOkenProvider.KeyVaultTokenCallback));

config.AddAzureKeyVault($"url", keyVaultClient, new DefaultKeyVaultSecretManager());
```

1. Under App Service identities, create a system assigned identity
2. Under keyvaults, create an access policy for the system identity

## *It would be really if the KeyVault was not created for us and if we could just create it along with the app during start up*

## Configuration providers in .NET Framework

* Machine.config
* External config files
* Environment Variables


