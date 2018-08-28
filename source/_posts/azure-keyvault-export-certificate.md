---
title: Azure KeyVault - How to download my password protected pfx?
categories:
 - [Azure, Azure KeyVault]
 - [Azure, Azure PowerShell]
date: 2018-08-28 09:57:17
tags:
- Azure
- Azure KeyVault
- PowerShell
- Azure PowerShell
- Security
- Certificate
---

Today I discovered a feature of the Azure KeyVault certificate store.

We have a bunch of Azure Function Apps that have a certificate attached to them in order to connect to the shared KeyVault. I added a new Azure Function App and needed to upload the PFX so that Azure Function would have access to the KeyVault too. I thought this would be as simple as downloading the certificate through the Azure Portal and re-uploading to to my Azure Function App, but Microsoft for some reason strips the password from the certificate, and a password is required when uploading through the portal.

<!-- more -->

## Trying in the portal

When attempting to upload my certificate in the Azure Portal for my Function App, I was greeted with the following error:

{% asset_img "failed-upload.png" "Successful Certificate Upload"%}

"The password is incorrect, or the certificate is not valid".

This didn't really make any sense to me as I was using the certificate I uploaded earlier and was certain that my password was correct. It was only after downloading the certificate and examining it on my machine that I realised that the password had been removed from the certificate. I am really not sure why Microsoft does this; but I found it a bit strange to say the least.

It also added a problem as you can see for the screenshot above, the certificate password is a required field when adding a certificate to an Azure Function App. After a bit of digging around I found that there would be no simple way to complete this action through the Azure Portal, and decided to try and solve the problem with the Azure PowerShell module.

## Installing Azure PowerShell

To install the Azure PowerShell module, you first need to have at least version 5.0 of PowerShell and less than version 6.0. Version 6.0 runs on .NET Core which this module is not available for at the time of this writing.

To check what version of PowerShell you have run this command:

```powershell
$PSVersionTable.PSVersion
```

To install the Azure PowerShell module, run the following command:

```powershell
Install-Module -Name AzureRM
```
If you haven't configured the PowerShell gallery as a trusted repository you will be prompted checking that you want to install from an unstrusted repository, agree to this to continue.

## Export certificate with password

Import the Azure PowerShell module and login to your subscription with the following commands. You will get an interactive window to enter your Azure credentials after the second command.

```powershell
Import-Module AzureRM

Connect-AzureRmAccount
```

When you have logged in to your Azure subscription in your PowerShell session, you will be able to run the following script to generate a PFX with your desired password:

```powershell
# Replace these variables with your own values
$vaultName = "YOUR-KEYVAULT-NAME"
$certificateName = "YOUR-CERTIFICATE-NAME"
$pfxPath = [Environment]::GetFolderPath("Desktop") + "\$certificateName.pfx"
$password = "YOUR-CERTIFICATE-PASSWORD"

$pfxSecret = Get-AzureKeyVaultSecret -VaultName $vaultName -Name $certificateName
$pfxUnprotectedBytes = [Convert]::FromBase64String($pfxSecret.SecretValueText)
$pfx = New-Object Security.Cryptography.X509Certificates.X509Certificate2
$pfx.Import($pfxUnprotectedBytes, $null, [Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable)
$pfxProtectedBytes = $pfx.Export([Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $password)
[IO.File]::WriteAllBytes($pfxPath, $pfxProtectedBytes)

```

You will now have a PFX generated with a password at your desired location on your computer (for me this just went to the desktop). You can now use this certificate on an Azure Function App through the portal as you have a password on it.

{% asset_img "successful-upload.png" "Successful Certificate Upload"%}

When trying to upload now, you should get the success message rather than the error message.