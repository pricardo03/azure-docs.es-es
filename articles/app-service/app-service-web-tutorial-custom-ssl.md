---
title: 'Enlazar un certificado SSL personalizado existente: Azure Web Service | Microsoft Docs'
description: Aprenda a enlazar un certificado SSL personalizado a aplicaciones web, back-ends para aplicaciones móviles o aplicaciones de API en Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: ''
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 57046b9e199fbe5e88d0ea7fa25248641693508a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257002"
---
# <a name="tutorial-bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Tutorial: Enlazar un certificado SSL personalizado a Azure Web Apps

Azure Web Apps proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo enlazar un certificado SSL personalizado adquirido de una entidad de certificación de confianza para [Azure Web Apps](app-service-web-overview.md). Cuando haya terminado, podrá acceder a la aplicación web en el punto de conexión HTTPS de su dominio DNS personalizado.

![Aplicación web con certificado SSL personalizado](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Actualizar el plan de tarifa de la aplicación
> * Enlazar el certificado personalizado con App Service
> * Renovar certificados
> * Aplicación de HTTPS
> * Aplicación de TLS 1.1 y 1.2
> * Automatización de la administración de TLS con scripts

> [!NOTE]
> Si necesita un certificado SSL personalizado, puede obtener uno directamente en Azure Portal y enlazarlo a la aplicación web. Siga el tutorial [Incorporación de un certificado SSL a la aplicación App Service](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

- [Crear una aplicación de App Service](/azure/app-service/)
- [Asignar un nombre DNS personalizado a la aplicación web](app-service-web-tutorial-custom-domain.md)
- Adquirir un certificado SSL de una entidad de certificación de confianza
- Tener la clave privada que se usó para firmar la solicitud de certificado SSL

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Requisitos para el certificado SSL

Para usar un certificado en App Service, el certificado debe cumplir los siguientes requisitos:

* Estar firmado por una entidad de certificación de confianza
* Haberse exportado como archivo PFX protegido por contraseña
* Contener una clave privada con una longitud de al menos 2048 bits
* Contener todos los certificados intermedios de la cadena de certificados

> [!NOTE]
> Los **certificados de criptografía de curva elíptica (ECC)** pueden funcionar con App Service, pero están fuera del ámbito de este artículo. Trabaje con la entidad de certificación sobre los pasos exactos para crear certificados ECC.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Enlazar el certificado SSL

Está listo para cargar el certificado SSL en la aplicación web.

### <a name="merge-intermediate-certificates"></a>Combinación de certificados intermedios

Si la entidad emisora de certificados ofrece varios certificados en la cadena de certificados, debe combinar los certificados en orden. 

Para ello, abra cada certificado que ha recibido en un editor de texto. 

Cree un archivo para el certificado combinado, denominado _mergedcertificate.crt_. En un editor de texto, copie el contenido de cada certificado en este archivo. Los certificados deben seguir el orden de la cadena de certificados, comenzando por el certificado y terminando por el certificado raíz. Debe ser similar al ejemplo siguiente:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Exportar el certificado a PFX

Exporte el certificado SSL personalizado con la clave privada con la que se generó la solicitud de certificado.

Si la solicitud de certificado se genera con OpenSSL, se crea un archivo de clave privada. Para exportar el certificado a PFX, ejecute el comando siguiente: Reemplace los marcadores de posición _&lt;private-key-file>_ y _&lt;merged-certificate-file>_ por la ruta a la clave privada y al archivo de certificado combinado.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Cuando se le pida, defina una contraseña de exportación. Esta contraseña deberá usarla al cargar el certificado SSL posteriormente en App Service.

Si usó IIS o _Certreq.exe_ para generar la solicitud de certificado, instale el certificado en la máquina local y luego [exporte el certificado a PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Cargar el certificado SSL

Para cargar el certificado SSL, haga clic en **Configuración de SSL** en el panel de navegación izquierdo de la aplicación web.

Haga clic en **Cargar certificado**. 

En **Archivo de certificado PFX**, seleccione el archivo PFX. En **Contraseña del certificado**, escriba la contraseña que creó al exportar el archivo PFX.

Haga clic en **Cargar**.

![Carga del certificado](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Cuando App Service termina de cargar el certificado, este aparece en la página **Configuración de SSL**.

![Certificado cargado](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Enlazar el certificado SSL

En la sección **Enlaces SSL**, haga clic en **Agregar enlace**.

En la página **Agregar enlace SSL**, use las listas desplegables para seleccionar el nombre de dominio que va a proteger, así como el certificado que pretende utilizar.

> [!NOTE]
> Si ha cargado el certificado pero no ve los nombres de dominio en la lista desplegable **Nombre de host**, pruebe a actualizar la página del explorador.
>
>

En **Tipo de SSL**, seleccione si se va a usar **[Indicación de nombre de servidor (SNI)](https://en.wikipedia.org/wiki/Server_Name_Indication)** o SSL basada en IP.

- **SSL basada en SNI**: pueden agregarse varios enlaces SSL basados en SNI. Esta opción permite que varios certificados SSL protejan varios dominios en una misma dirección IP. Los exploradores más modernos (como Internet Explorer, Chrome, Firefox y Opera) admiten SNI (encontrará información de compatibilidad con exploradores más completa en [Indicación de nombre de servidor](https://wikipedia.org/wiki/Server_Name_Indication)).
- **SSL basada en IP**: solo pueden agregarse enlaces SSL basados en IP. Esta opción solo permite que un único certificado SSL proteja una dirección IP dedicada. Para proteger varios dominios, debe protegerlos todos con el mismo certificado SSL. Se trata de la opción tradicional para enlaces SSL.

Haga clic en **Agregar enlace**.

![Enlazar certificado SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Cuando App Service termina de cargar el certificado, este aparece en la sección **Enlaces SSL**.

![Certificado enlazado a la aplicación web](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Reasignar un registro D en SSL de IP

Si no usa SSL basado en IP en la aplicación web, vaya directamente a [Probar HTTPS para el dominio personalizado](#test).

De manera predeterminada, la aplicación web usa una dirección IP pública compartida. Cuando se enlaza un certificado con SSL basada en IP, App Service crea otra dirección IP dedicada para la aplicación web.

Si ha asignado un registro D a la aplicación web, actualice el registro de dominio con esta nueva dirección IP dedicada.

La página **Dominio personalizado** de la aplicación web se actualiza con la nueva dirección IP dedicada. [Copie esta dirección IP](app-service-web-tutorial-custom-domain.md#info) y luego [reasigne el registro D](app-service-web-tutorial-custom-domain.md#map-an-a-record) a esta nueva dirección IP.

<a name="test"></a>

## <a name="test-https"></a>Probar HTTPS

Ahora todo lo que queda por hacer es asegurarse de que HTTPS funciona para el dominio personalizado. En varios exploradores, vaya a `https://<your.custom.domain>` para ver que atiende la aplicación web.

![Navegación en el portal a la aplicación de Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Si la aplicación web genera errores de validación del certificado, probablemente se esté usando un certificado autofirmado.
>
> Si no es así, puede que haya olvidado certificados intermedios cuando exportó el certificado al archivo PFX.

<a name="bkmk_enforce"></a>

## <a name="renew-certificates"></a>Renovar certificados

Su dirección IP de entrada puede cambiar al eliminar un enlace, incluso si este se basa en IP. Esto es especialmente importante al renovar un certificado que ya está en un enlace basado en IP. Para evitar un cambio en la dirección IP de su aplicación, siga estos pasos en orden:

1. Carga el nuevo certificado.
2. Enlaza el nuevo certificado al dominio personalizado que desee sin eliminar el antiguo. Esta acción reemplaza el enlace en lugar de quitar el antiguo.
3. Elimine el antiguo certificado. 

## <a name="enforce-https"></a>Aplicación de HTTPS

De forma predeterminada, cualquier usuario puede acceder todavía a su aplicación web mediante HTTP. Puede redirigir todas las solicitudes HTTP al puerto HTTPS.

En la página de la aplicación web, en el panel de navegación izquierdo, seleccione **Configuración de SSL**. A continuación, en **Solo HTTPS**, seleccione **On**.

![Aplicación de HTTPS](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Una vez completada la operación, vaya a cualquiera de las direcciones URL HTTP que apuntan a la aplicación. Por ejemplo: 

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>Exigencia de las versiones TLS

La aplicación permite [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 de forma predeterminada, que es el nivel TLS recomendado por los estándares del sector, como [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Para exigir versiones diferentes de TLS, siga estos pasos:

En la página de la aplicación web, en el panel de navegación izquierdo, seleccione **Configuración de SSL**. A continuación, en **TLS version** (Versión de TLS), seleccione la versión mínima de TLS que desee. Esta configuración controla solo las llamadas entrantes. 

![Exigir aplicación de TLS 1.1 o 1.2](./media/app-service-web-tutorial-custom-ssl/enforce-tls1.2.png)

Una vez completada la operación, la aplicación rechaza todas las conexiones con versiones inferiores de TLS.

## <a name="automate-with-scripts"></a>Automatizar con scripts

Puede automatizar enlaces SSL de la aplicación web con scripts, mediante la [CLI de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

El comando siguiente carga un archivo PFX exportado y obtiene la huella digital.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

El comando siguiente usa la huella digital del comando anterior para agregar un enlace SSL basado en SNI.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

El siguiente comando aplica la versión mínima de TLS: la 1.2.

```bash
az webapp config set \
    --name <app_name> \
    --resource-group <resource_group_name>
    --min-tls-version 1.2
```

### <a name="azure-powershell"></a>Azure PowerShell

El comando siguiente carga un archivo PFX exportado y agrega un enlace SSL basado en SNI.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Certificados públicos (opcionales)
Si la aplicación necesita acceder a recursos remotos como un cliente y el recurso remoto requiere autenticación de certificados, puede cargar los [certificados públicos](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) en la aplicación web. Los certificados públicos no son necesarios para los enlaces SSL de la aplicación.

Para obtener más información sobre cómo cargar y usar un certificado público en su aplicación, consulte [Uso de un certificado SSL en el código de aplicación de Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-ssl-cert-load). También puede utilizar certificados públicos con las aplicaciones de App Service Environment. Si tiene que almacenar el certificado en el almacén de certificados del equipo local, debe usar una aplicación web en App Service Environment. Para más información, consulte [Configuración de certificados públicos en una aplicación web](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Carga de un certificado público](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Actualizar el plan de tarifa de la aplicación
> * Enlazar el certificado personalizado con App Service
> * Renovar certificados
> * Aplicación de HTTPS
> * Aplicación de TLS 1.1 y 1.2
> * Automatización de la administración de TLS con scripts

Para aprender a usar Azure Content Delivery Network, avance al siguiente tutorial.

> [!div class="nextstepaction"]
> [Incorporación de una red Content Delivery Network a Azure App Service](../cdn/cdn-add-to-web-app.md)

Para más información, consulte [Uso de un certificado SSL en el código de aplicación de Azure App Service](app-service-web-ssl-cert-load.md).
