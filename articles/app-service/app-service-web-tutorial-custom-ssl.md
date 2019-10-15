---
title: 'Carga y enlace de certificados SSL: Azure App Service | Microsoft Docs'
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
ms.date: 06/06/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c5095bc8c274ef0985b00459b0d088371ab24d88
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177047"
---
# <a name="tutorial-upload-and-bind-ssl-certificates-to-azure-app-service"></a>Tutorial: Carga y enlace de certificados SSL en Azure App Service

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo proteger dominio personalizado en App Service con un certificado adquirido a una entidad de certificación de confianza. También muestra cómo cargar los certificados privados y públicos que su aplicación necesita. Cuando haya terminado, podrá acceder a la aplicación en el punto de conexión HTTPS de su dominio DNS personalizado.

![Aplicación web con certificado SSL personalizado](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Actualizar el plan de tarifa de la aplicación
> * Proteger un dominio personalizado con un certificado
> * Carga de un certificado privado
> * Carga de un certificado público
> * Renovar certificados
> * Aplicación de HTTPS
> * Aplicación de TLS 1.1 y 1.2
> * Automatización de la administración de TLS con scripts

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

- [Crear una aplicación de App Service](/azure/app-service/)
- [Asignar un nombre DNS personalizado a la aplicación de App Service](app-service-web-tutorial-custom-domain.md) (si se protege un dominio personalizado)
- Adquirir un certificado de una entidad de certificación de confianza
- Tener la clave privada que se usó para firmar la solicitud de certificado (para certificados privados)

<a name="requirements"></a>

## <a name="prepare-a-private-certificate"></a>Preparación de un certificado privado

Para proteger un dominio, el certificado debe cumplir los siguientes requisitos:

* Estar configurado para la autenticación de servidor
* Estar firmado por una entidad de certificación de confianza
* Haberse exportado como archivo PFX protegido por contraseña
* Contener una clave privada con una longitud de al menos 2048 bits
* Contener todos los certificados intermedios de la cadena de certificados

> [!TIP]
> Si necesita un certificado SSL personalizado, puede obtenerlo directamente en Azure Portal e importarlo a la aplicación. Siga el tutorial [Incorporación de un certificado SSL a la aplicación App Service](web-sites-purchase-ssl-web-site.md).

> [!NOTE]
> Los **certificados de criptografía de curva elíptica (ECC)** pueden funcionar con App Service, pero están fuera del ámbito de este artículo. Trabaje con la entidad de certificación sobre los pasos exactos para crear certificados ECC.

Una vez que obtenga un certificado de su proveedor de certificados, siga los pasos que se describen en esta sección para prepararlo para App Service.

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

Ya está listo para cargar el certificado en App Service.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Protección de un dominio personalizado

> [!TIP]
> Si necesita un certificado SSL personalizado, puede obtener uno directamente en Azure Portal y enlazarlo a la aplicación. Siga el tutorial [Incorporación de un certificado SSL a la aplicación App Service](web-sites-purchase-ssl-web-site.md).

Para proteger un [dominio personalizado](app-service-web-tutorial-custom-domain.md) con un certificado de terceros, se carga el [certificado privado preparado](#prepare-a-private-certificate) y, después, se enlaza con el dominio personalizado, pero App Service simplifica este proceso. Siga estos pasos:

Haga clic en **Dominios personalizados** en el panel izquierdo de la aplicación y, después, haga clic en **Agregar enlace** en el dominio que desea proteger. Si no ve **Agregar enlace** en algún dominio, se debe a que ya es seguro, por lo que debería tener un estado de SSL **Seguro**.

![Agregar enlace a un dominio](./media/app-service-web-tutorial-custom-ssl/secure-domain-launch.png)

Haga clic en **Cargar certificado**.

En **Archivo de certificado PFX**, seleccione el archivo PFX. En **Contraseña del certificado**, escriba la contraseña que creó al exportar el archivo PFX.

Haga clic en **Cargar**.

![Carga del certificado en el dominio](./media/app-service-web-tutorial-custom-ssl/secure-domain-upload.png)

Espere hasta que Azure cargue el certificado e inicie el cuadro de diálogo de enlaces de SSL.

En dicho cuadro de diálogo, seleccione el certificado que cargó y el tipo SSL y haga clic en **Agregar enlace**.

> [!NOTE]
> Se admiten los siguientes tipos de SSL:
>
> - **[SSL basada en SNI](https://en.wikipedia.org/wiki/Server_Name_Indication)** : se pueden agregar varios enlaces SSL basados en SNI. Esta opción permite que varios certificados SSL protejan varios dominios en una misma dirección IP. Los exploradores más modernos (como Internet Explorer, Chrome, Firefox y Opera) admiten SNI (encontrará información de compatibilidad con exploradores más completa en [Indicación de nombre de servidor](https://wikipedia.org/wiki/Server_Name_Indication)).
> - **SSL basada en IP**: solo pueden agregarse enlaces SSL basados en IP. Esta opción solo permite que un único certificado SSL proteja una dirección IP dedicada. Para proteger varios dominios, debe protegerlos todos con el mismo certificado SSL. Se trata de la opción tradicional para enlaces SSL.

![Enlace de SSL a un dominio](./media/app-service-web-tutorial-custom-ssl/secure-domain-bind.png)

El estado de SSL del dominio se debe cambiar ahora a **Seguro**.

![Dominio protegido](./media/app-service-web-tutorial-custom-ssl/secure-domain-finished.png)

> [!NOTE]
> Un estado **Seguro** en **Dominios personalizados** significa que está protegido con un certificado, pero App Service no comprueba si el certificado es autofirmado o ha expirado, por ejemplo, lo que también puede provocar que los exploradores muestren un error o una advertencia.

## <a name="remap-a-record-for-ip-ssl"></a>Reasignar un registro D en SSL de IP

Si no usa SSL basado en IP en la aplicación, vaya directamente a [Probar HTTPS para el dominio personalizado](#test).

De manera predeterminada, la aplicación usa una dirección IP pública compartida. Cuando se enlaza un certificado con SSL basada en IP, App Service crea otra dirección IP dedicada para la aplicación.

Si ha asignado un registro D a la aplicación, actualice el registro de dominio con esta nueva dirección IP dedicada.

La página **Dominio personalizado** de la aplicación se actualiza con la nueva dirección IP dedicada. [Copie esta dirección IP](app-service-web-tutorial-custom-domain.md#info) y luego [reasigne el registro D](app-service-web-tutorial-custom-domain.md#map-an-a-record) a esta nueva dirección IP.

<a name="test"></a>

## <a name="test-https"></a>Probar HTTPS

Ahora todo lo que queda por hacer es asegurarse de que HTTPS funciona para el dominio personalizado. En varios exploradores, vaya a `https://<your.custom.domain>` para ver que da servicio a la aplicación.

![Navegación en el portal a la aplicación de Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Si la aplicación genera errores de validación del certificado, probablemente esté utilizando un certificado autofirmado.
>
> Si no es así, puede que haya olvidado certificados intermedios cuando exportó el certificado al archivo PFX.

## <a name="renew-certificates"></a>Renovar certificados

Su dirección IP de entrada puede cambiar al eliminar un enlace, incluso si este se basa en IP. Esto es especialmente importante al renovar un certificado que ya está en un enlace basado en IP. Para evitar un cambio en la dirección IP de su aplicación, siga estos pasos en orden:

1. Carga el nuevo certificado.
2. Enlaza el nuevo certificado al dominio personalizado que desee sin eliminar el antiguo. Esta acción reemplaza el enlace en lugar de quitar el antiguo.
3. Elimine el antiguo certificado. 

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Aplicación de HTTPS

De forma predeterminada, cualquier usuario puede acceder todavía a su aplicación mediante HTTP. Puede redirigir todas las solicitudes HTTP al puerto HTTPS.

En la página de la aplicación, en el panel de navegación izquierdo, seleccione **Configuración de SSL**. A continuación, en **Solo HTTPS**, seleccione **On**.

![Aplicación de HTTPS](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Una vez completada la operación, vaya a cualquiera de las direcciones URL HTTP que apuntan a la aplicación. Por ejemplo:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>Exigencia de las versiones TLS

La aplicación permite [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 de forma predeterminada, que es el nivel TLS recomendado por los estándares del sector, como [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Para exigir versiones diferentes de TLS, siga estos pasos:

En la página de la aplicación, en el panel de navegación izquierdo, seleccione **Configuración de SSL**. A continuación, en **TLS version** (Versión de TLS), seleccione la versión mínima de TLS que desee. Esta configuración controla solo las llamadas entrantes. 

![Exigir aplicación de TLS 1.1 o 1.2](./media/app-service-web-tutorial-custom-ssl/enforce-tls1.2.png)

Una vez completada la operación, la aplicación rechaza todas las conexiones con versiones inferiores de TLS.

## <a name="automate-with-scripts"></a>Automatizar con scripts

Puede automatizar enlaces SSL de la aplicación con scripts, mediante la [CLI de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>CLI de Azure

El comando siguiente carga un archivo PFX exportado y obtiene la huella digital.

```azurecli-interactive
thumbprint=$(az webapp config ssl upload \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --certificate-file <path-to-PFX-file> \
    --certificate-password <PFX-password> \
    --query thumbprint \
    --output tsv)
```

El comando siguiente usa la huella digital del comando anterior para agregar un enlace SSL basado en SNI.

```azurecli-interactive
az webapp config ssl bind \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

El comando siguiente obliga a la aplicación a usar https.

```azurecli-interactive
az webapp update \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --https-only true
```

El siguiente comando aplica la versión mínima de TLS: la 1.2.

```azurecli-interactive
az webapp config set \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --min-tls-version 1.2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

El comando siguiente carga un archivo PFX exportado y agrega un enlace SSL basado en SNI.

```powershell
New-AzWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="use-certificates-in-your-code"></a>Uso de certificados en el código

Si la aplicación necesita conectarse a recursos remotos y el recurso remoto requiere la autenticación de certificados, puede cargar certificados públicos o privados en la aplicación. No es necesario enlazar estos certificados a ningún dominio personalizado en la aplicación. Para más información, consulte [Uso de un certificado SSL en el código de aplicación de Azure App Service](app-service-web-ssl-cert-load.md).

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
