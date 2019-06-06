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
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475093"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Uso de un certificado SSL en el código de aplicación de Azure App Service

Esta guía muestra cómo utilizar certificados públicos o privados en el código de aplicación. Un ejemplo de caso de uso se produce cuando la aplicación accede a un servicio externo que requiere la autenticación de certificados.

Este enfoque al uso de certificados en el código hace uso de SSL en App Service, lo que requiere la aplicación en la funcionalidad **básica** nivel o superior. Como alternativa, puede [incluir el archivo de certificado en el repositorio de aplicación](#load-certificate-from-file), pero no es una práctica recomendada para los certificados privados.

Cuando permite que App Service administre los certificados SSL, puede mantener por separado los certificados y el código de la aplicación y proteger así la información confidencial.

## <a name="upload-a-private-certificate"></a>Carga de un certificado privado

Antes de cargar un certificado privado, asegúrese de que [cumple todos los requisitos](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate), salvo que no necesita ser configurado para la autenticación de servidor.

Cuando esté listo para cargar, ejecute el siguiente comando el <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Copie la huella digital del certificado y vea [que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Carga de un certificado público

Se admiten certificados públicos en el *.cer* formato. Para cargar un certificado público, el <a href="https://portal.azure.com" target="_blank">portal Azure</a>y vaya a la aplicación.

Haga clic en **configuración SSL** > **certificados públicos (.cer)**  > **cargar certificado público** desde el panel de navegación izquierdo de la aplicación.

En **nombre**, escriba un nombre para el certificado. En **archivo de certificado CER**, seleccione el archivo CER.

Haga clic en **Cargar**.

![Carga de un certificado público](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Una vez cargado el certificado, copie la huella digital del certificado y vea [que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>Importar un certificado de App Service

Consulte [comprar y configurar un certificado SSL para Azure App Service](web-sites-purchase-ssl-web-site.md).

Una vez importado el certificado, copie la huella digital de certificado y ver [que el certificado sea accesible](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Que el certificado sea accesible

Para usar un certificado cargado o importado en el código de aplicación, que su huella digital sea accesible con la `WEBSITE_LOAD_CERTIFICATES` configuración de la aplicación, ejecutando el siguiente comando en el <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Para que todos los certificados sean accesibles, establezca el valor en `*`.

> [!NOTE]
> Este valor coloca los certificados especificados en el [jose\mis actual](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) almacén para la mayoría los niveles de precios, pero en el **aislado** nivel (es decir, la aplicación se ejecuta en un [App Service Environment](environment/intro.md)), coloca los certificados en el [Machine\My Local](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) almacenar.
>

![Configurar las opciones de la aplicación](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Cuando termine, haga clic en **Guardar**.

Los certificados configurados ahora están listos para ser utilizado por el código.

## <a name="load-the-certificate-in-code"></a>Cargar el certificado en el código

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
## <a name="load-certificate-from-file"></a>Carga del certificado desde archivo

Si tiene que cargar un archivo de certificado desde el directorio de la aplicación, es mejor cargar mediante [FTPS](deploy-ftp.md) en lugar de [Git](deploy-local-git.md), por ejemplo. Debe mantener los datos confidenciales, como un certificado privado fuera del control de código fuente.

Aunque el archivo se carga directamente en el código. NET, la biblioteca comprueba todavía si se carga el perfil de usuario actual. Para cargar el perfil de usuario actual, establezca el `WEBSITE_LOAD_USER_PROFILE` configuración de la aplicación con el siguiente comando en el <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Una vez que se establece esta configuración, el siguiente C# ejemplo carga un certificado denominado `mycert.pfx` desde el `certs` directorio del repositorio de la aplicación.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
