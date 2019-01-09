---
title: Tutorial Uso de Azure Key Vault desde una aplicación web - Azure Key Vault | Microsoft Docs
description: Use este tutorial para ayudarle a aprender a usar Azure Key Vault desde una aplicación web.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: barclayn
ms.openlocfilehash: 79bccbcbcf78de18504c5cb0235e29930d90ede8
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999312"
---
# <a name="tutorial-use-azure-key-vault-from-a-web-application"></a>Tutorial: Uso de Azure Key Vault desde una aplicación web

Use este tutorial como ayuda para aprender a usar Azure Key Vault desde una aplicación web. Muestra el proceso de acceder a un secreto de un Azure Key Vault para usarlo en una aplicación web. Luego, el tutorial se basa en el proceso y usa un certificado en lugar de un secreto de cliente. Este tutorial está diseñado para desarrolladores web que comprendan los conceptos básicos de la creación de aplicaciones web en Azure.

En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Incorporar la configuración de la aplicación al archivo web.config
> * Agregar un método para obtener un token de acceso
> * Recuperar el token en Inicio de aplicación
> * Autenticar con un certificado

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, necesitará los siguientes elementos:

* Un URI de un secreto en una instancia de Azure Key Vault.
* Un Id. de cliente y un secreto de cliente para una aplicación web registrada en Azure Active Directory que tenga acceso a su almacén de claves.
* Una aplicación web. En este tutorial se muestran los pasos para una aplicación ASP.NET MVC implementada en Azure como una aplicación web.

Complete los pasos que aparecen en [Introducción a Azure Key Vault](key-vault-get-started.md) para obtener el URI a un secreto, el identificador de cliente, el secreto de cliente y registrar la aplicación. La aplicación web accederá al almacén y tendrá que estar registrada en Azure Active Directory. También debe tener derechos de acceso a Key Vault. Si no es así, vuelva al apartado sobre cómo registrar una aplicación del tutorial de introducción y repita los pasos enumerados. Para más información sobre cómo crear instancias de Azure Web Apps, consulte [Información general de Web Apps](../app-service/overview.md).

Este ejemplo depende del aprovisionamiento manual de identidades de Azure Active Directory. Pero debe usar [Identidades administradas de recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) en su lugar, que aprovisiona automáticamente identidades de Azure AD. Para más información, consulte el [ejemplo en GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) y el [tutorial de App Service y Functions](https://docs.microsoft.com/azure/app-service/overview-managed-identity). También puede examinar el artículo [Configuración de una aplicación web de Azure para que lea un secreto desde el almacén de claves](tutorial-web-application-keyvault.md) específico de Key Vault.

## <a id="packages"></a>Incorporación de paquetes NuGet

Hay dos paquetes que la aplicación web debe tener instalados.

* Biblioteca de autenticación de Active Directory: contiene métodos para interactuar con Azure Active Directory y administrar la identidad de usuario.
* Biblioteca de Almacén claves de Azure: contiene métodos para interactuar con Azure Key Vault.

Los dos paquetes se pueden instalar mediante la Consola del Administrador de paquetes con el comando Install-Package.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory 
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Modificación de web.config

Hay tres configuraciones de aplicaciones que deben agregarse al archivo web.config como se indica a continuación. Agregaremos los valores reales en Azure Portal para lograr un nivel de seguridad adicional.

```xml
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
    <!-- If you aren't hosting your app as an Azure Web App, then you should use the actual ClientId, Client Secret, and Secret URI values -->
```

## <a id="gettoken"></a>Incorporación del método para obtener un token de acceso

Para usar la API de Key Vault necesita un token de acceso. El cliente de Key Vault controla las llamadas a la API de Key Vault, pero deberá suministrarle una función que obtenga el token de acceso. En el ejemplo siguiente se muestra el código para obtener un token de acceso de Azure Active Directory. Este código puede estar en cualquier parte de la aplicación. Quiero agregar una clase Utils o EncryptionHelper.  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
// Using Client ID and Client Secret is a way to authenticate an Azure AD application.
// Using it in your web application allows for a separation of duties and more control over your key management. 
// However, it does rely on putting the Client Secret in your configuration settings.
// For some people, this can be as risky as putting the secret in your configuration settings.
```

## <a id="appstart"></a>Recuperación del secreto en Inicio de aplicación

Ahora tenemos el código para llamar a la API de Key Vault y recuperar el secreto. El siguiente código se puede colocar en cualquier parte siempre que se llame antes de que sea necesario usarlo. Coloqué este código en el evento Inicio de aplicación en Global.asax para que se ejecute una vez en el inicio y permita que esté disponible el secreto para la aplicación.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>Incorporación de la configuración de la aplicación en Azure Portal

En Azure Web App, ahora puede agregar los valores reales para AppSettings en Azure Portal. Con este paso, los valores reales no estarán en web.config sino que estarán protegidos a través de Portal donde cuenta con funcionalidades de control de acceso independientes. Estos valores se sustituirán por los valores que escribió en web.config. Asegúrese de que los nombres sean los mismos.

![Configuración de la aplicación mostrada en Azure Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Autenticación con un certificado en lugar de hacerlo con un secreto de cliente

Ahora que comprende la autenticación de una aplicación de Azure AD con el identificador de cliente y el secreto de cliente, usemos un identificador de cliente y un certificado. Para usar un certificado en una instancia de Azure Web App, use estos pasos:

1. Obtener o crear un certificado
2. Asociar el certificado a una aplicación de Azure AD
3. Agregar código a la aplicación web para usar el certificado
4. Agregar un certificado a la aplicación web

### <a name="get-or-create-a-certificate"></a>Obtener o crear un certificado

 Nos aseguraremos de hacer un certificado de prueba para este tutorial. Este es un script para crear un certificado autofirmado. Cambie al directorio a aquel en el que desea que se creen los archivos de certificado.  Para las fechas de inicio y finalización del certificado, puede usar la fecha de hoy más un año.

```powershell
#Create self-signed certificate and export pfx and cer files 
$PfxFilePath = 'KVWebApp.pfx'
$CerFilePath = 'KVWebApp.cer'
$DNSName = 'MyComputer.Contoso.com'
$Password = 'MyPassword"'

$StoreLocation = 'CurrentUser' #be aware that LocalMachine requires elevated privileges
$CertBeginDate = Get-Date
$CertExpiryDate = $CertBeginDate.AddYears(1)

$SecStringPw = ConvertTo-SecureString -String $Password -Force -AsPlainText 
$Cert = New-SelfSignedCertificate -DnsName $DNSName -CertStoreLocation "cert:\$StoreLocation\My" -NotBefore $CertBeginDate -NotAfter $CertExpiryDate -KeySpec Signature
Export-PfxCertificate -cert $Cert -FilePath $PFXFilePath -Password $SecStringPw 
Export-Certificate -cert $Cert -FilePath $CerFilePath 
```

Tome nota de la fecha de finalización y la contraseña del archivo .pfx (en este ejemplo: 15 de mayo de 2019 y MyPassword). Necesitará esta información en el script siguiente. 
### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Asociar el certificado a una aplicación de Azure AD

Ahora que tiene un certificado, tendrá que asociarlo a una aplicación de Azure AD. La asociación se puede completar mediante PowerShell. Ejecute los siguientes comandos para asociar el certificado con la aplicación de Azure AD:

```powershell
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Después de ejecutar estos comandos, podrá ver la aplicación en Azure AD. Cuando busque los registros de la aplicación, asegúrese de seleccionar **My apps** (Mis aplicaciones) en lugar de "All apps" (Todas las aplicaciones) en el cuadro de búsqueda. 

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Agregar código a la aplicación web para usar el certificado

Ahora agregaremos código a Web App para tener acceso al certificado y usarlo para la autenticación. 

En primer lugar, este es el código para tener acceso al certificado. La ubicación de almacenamiento es CurrentUser y no LocalMachine. Fíjese también en que usamos "false" en el método Find, ya que usamos un certificado de prueba.

```cs
//Add this using statement
using System.Security.Cryptography.X509Certificates;  

public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```



El código siguiente usa CertificateHelper y crea ClientAssertionCertificate, que es necesario para la autenticación.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Este es el nuevo código para obtener el token de acceso. Este código reemplaza al método GetToken en el ejemplo anterior. Para su comodidad, le hemos dado un nombre diferente. Para facilitar su uso, hemos colocado todo este código en la clase Utils del proyecto de aplicación web.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```



El último cambio de código se da en el método Application_Start. En primer lugar, es preciso llamar al método GetCert() para cargar ClientAssertionCertificate. Y, a continuación, se cambia el método de devolución de llamada que se suministra al crear un nuevo KeyVaultClient. Este código reemplaza al código que teníamos en el ejemplo anterior.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Incorporación de un certificado a una aplicación web a través de Azure Portal

La incorporación de un certificado a la instancia de Web App es un sencillo proceso de dos pasos. En primer lugar, vaya a Azure Portal y navegue a Web App. En la configuración de la aplicación web, haga clic en la entrada de **configuración de SSL**. Cuando se abra. cargue el certificado que creó en el ejemplo anterior, KVWebApp.pfx. Asegúrese de recordar la contraseña para el archivo pfx.

![Incorporación de un certificado a Web App en Azure Portal][2]

Lo último que debe hacer es agregar una configuración de aplicación a la aplicación web cuyo nombre es WEBSITE\_LOAD\_CERTIFICATES y cuyo valor es *. Este paso garantizará que se cargan todos los certificados. Si desea cargar solo los certificados que ha cargado, puede escribir una lista separada por comas de sus huellas digitales.


## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no lo necesite, elimine el servicio de la aplicación, el almacén de claves y la aplicación de Azure AD que usó para el tutorial.  


## <a id="next"></a>Pasos siguientes
> [!div class="nextstepaction"]
>[Referencia de API de administración de Azure Key Vault](/dotnet/api/overview/azure/keyvault/management).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 