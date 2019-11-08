---
title: 'Uso de un certificado SSL en el código de aplicación: Azure App Service | Microsoft Docs'
description: Obtenga información sobre cómo utilizar certificados de cliente para conectarse a recursos remotos que los necesiten.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/16/2019
ms.author: cephalin
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 1f042f72f82d2198472fe81670c697c0c4b28321
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510549"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Uso de un certificado SSL en el código de aplicación de Azure App Service

El código de la aplicación de App Service puede actuar como un cliente y acceder a un servicio externo que requiera autenticación de certificado. Esta guía muestra cómo utilizar certificados públicos o privados en el código de aplicación.

Este enfoque para el uso de certificados en el código usa la funcionalidad SSL de App Service, que requiere que la aplicación tenga el nivel **Básico** u otro superior. Como alternativa, puede [incluir el archivo de certificado en el repositorio de aplicación](#load-certificate-from-file), pero no es un procedimiento recomendado para los certificados privados.

Cuando permite que App Service administre los certificados SSL, puede mantener por separado los certificados y el código de la aplicación y proteger así la información confidencial.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía paso a paso, debe:

- [Crear una aplicación de App Service](/azure/app-service/)
- [Agregar un certificado a su aplicación](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Busque la huella digital

En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, en el menú de la izquierda, seleccione **App Services** >  **\<nombre-de-aplicación>** .

En el panel izquierdo de la aplicación, seleccione **Configuración de TLS/SSL** y, posteriormente, seleccione **Certificados de clave privada (.pfx)** o **Certificados de clave pública (.cer)** .

Encuentre el certificado que desea utilizar y copie la huella digital.

![Copie la huella digital del certificado](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="load-the-certificate"></a>Carga del certificado

Para usar un certificado en el código de aplicación, agregue su huella digital a la opción de la aplicación `WEBSITE_LOAD_CERTIFICATES`; para ello, ejecute el comando siguiente en <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Para que todos los certificados sean accesibles, establezca el valor en `*`.

> [!NOTE]
> Esta configuración coloca los certificados especificados en el almacén [Current User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) para la mayoría de los niveles de precios, pero en el nivel **aislado** (es decir, la aplicación se ejecuta en un entorno [App Service Environment](environment/intro.md)) y coloca los certificados en el almacén [Local Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).
>

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

## <a name="more-resources"></a>Más recursos

* [Protección de un nombre DNS personalizado con un enlace SSL](configure-ssl-bindings.md)
* [Aplicación de HTTPS](configure-ssl-bindings.md#enforce-https)
* [Aplicación de TLS 1.1 y 1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [Preguntas más frecuentes: Certificados de App Service](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
