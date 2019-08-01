---
title: 'Uso de un certificado SSL de cliente en el código de aplicación: Azure App Service | Microsoft Docs'
description: Obtenga información sobre cómo utilizar certificados de cliente para conectarse a recursos remotos que los necesiten.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ead1892062912840c9931ae60d11c90975ad26ac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475093"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Uso de un certificado SSL en el código de aplicación de Azure App Service

Esta guía muestra cómo utilizar certificados públicos o privados en el código de aplicación. Un ejemplo de caso de uso se produce cuando la aplicación accede a un servicio externo que requiere la autenticación de certificados.

Este enfoque para el uso de certificados en el código usa la funcionalidad SSL de App Service, que requiere que la aplicación tenga el nivel **Básico** u otro superior. Como alternativa, puede [incluir el archivo de certificado en el repositorio de aplicación](#load-certificate-from-file), pero no es un procedimiento recomendado para los certificados privados.

Cuando permite que App Service administre los certificados SSL, puede mantener por separado los certificados y el código de la aplicación y proteger así la información confidencial.

## <a name="upload-a-private-certificate"></a>Carga de un certificado privado

Antes de cargar un certificado privado, asegúrese de que [cumple todos los requisitos](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate), salvo que no necesita ser configurado para la autenticación de servidor.

Cuando esté preparado para la carga, ejecute el siguiente comando en <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Copie la huella digital del certificado y consulte [Que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Carga de un certificado público

Se admiten certificados públicos en el formato *.cer*. Para cargar un certificado público, vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a> y, después, a la aplicación.

Haga clic en **Configuración SSL** > **Certificados públicos (.cer)**  > **Cargar certificado público** desde el panel de navegación izquierdo de la aplicación.

En **Nombre**, escriba un nombre para el certificado. En **Archivo de certificado CER**, seleccione el archivo CER.

Haga clic en **Cargar**.

![Carga de un certificado público](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Una vez que el certificado se cargue, copie la huella digital del certificado y consulte [Que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>Importación de un certificado de App Service

Consulte [Compra y configuración de un certificado SSL para Azure App Service](web-sites-purchase-ssl-web-site.md).

Una vez que el certificado se importe, copie la huella digital del certificado y consulte [Que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Que el certificado sea accesible

Para usar un certificado cargado o importado en el código de aplicación, haga que su huella digital sea accesible con la opción de la aplicación `WEBSITE_LOAD_CERTIFICATES`; para ello, ejecute el comando siguiente en <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Para que todos los certificados sean accesibles, establezca el valor en `*`.

> [!NOTE]
> Esta configuración coloca los certificados especificados en el almacén [Current User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) para la mayoría de los niveles de precios, pero en el nivel **aislado** (es decir, la aplicación se ejecuta en un entorno [App Service Environment](environment/intro.md)) y coloca los certificados en el almacén [Local Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).
>

![Configurar las opciones de la aplicación](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Cuando termine, haga clic en **Guardar**.

Ahora, ya puede usar en su código los certificados configurados.

## <a name="load-the-certificate-in-code"></a>Carga del certificado en el código

Una vez que el certificado es accesible, puede acceder a él con código C# mediante la huella digital del certificado. El código siguiente carga un certificado con la huella digital `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="load-certificate-from-file"></a>Carga del certificado desde el archivo

Si tiene que cargar un archivo de certificado desde el directorio de la aplicación, es mejor cargarlo mediante [FTPS](deploy-ftp.md) en lugar de [Git](deploy-local-git.md), por ejemplo. Debe mantener los datos confidenciales, como un certificado privado, fuera del control del código fuente.

Aunque el archivo se cargue directamente en el código. NET, la biblioteca sigue comprobando si el perfil de usuario actual se carga. Para cargar el perfil de usuario actual, establezca la configuración de la aplicación `WEBSITE_LOAD_USER_PROFILE` con el siguiente comando en <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Una vez se establece esta opción, el ejemplo de C# siguiente carga un certificado denominado `mycert.pfx` desde el directorio `certs` del repositorio de la aplicación.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
