---
title: Protección de API mediante la autenticación de certificados de cliente en API Management - Azure API Management | Microsoft Docs
description: Obtenga información acerca de cómo proteger el acceso a las API mediante certificados de cliente.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: apimpm
ms.openlocfilehash: 5427c4050b6b70c18da7a1899d16e448c41e81c6
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427354"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Protección de API mediante la autenticación de certificados de cliente en API Management

API Management proporciona la capacidad de proteger el acceso a las API (es decir, de cliente a API Management) mediante certificados de cliente. Puede validar el certificado entrante y comprobar las propiedades del certificado con los valores deseados mediante expresiones de directiva.

Para obtener información acerca de cómo proteger el acceso al servicio back-end de una API mediante certificados de cliente (es decir, API Management a back-end), vea [cómo asegurar servicios back-end mediante el cliente de autenticación de certificados](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

> [!IMPORTANT]
> Para recibir y comprobar los certificados de cliente en el nivel de consumo debe activar "Certificado de cliente de solicitud" establecer en la hoja "Dominios personalizados", tal como se muestra a continuación.

![Solicitud de certificado de cliente](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Comprobación del emisor y el asunto

Es posible configurar las directivas siguientes para comprobar el emisor y el firmante de un certificado de cliente:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Para deshabilitar el uso de lista de revocación de certificados comprobación `context.Request.Certificate.VerifyNoRevocation()` en lugar de `context.Request.Certificate.Verify()`.
> Si el certificado de cliente es un certificado autofirmado, raíz (o intermedia) deben ser certificados de CA [cargado](api-management-howto-ca-certificates.md) a la API de administración de `context.Request.Certificate.Verify()` y `context.Request.Certificate.VerifyNoRevocation()` funcione.

## <a name="checking-the-thumbprint"></a>Comprobación de la huella digital

Es posible configurar las directivas siguientes para comprobar la huella digital de un certificado de cliente:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Para deshabilitar el uso de lista de revocación de certificados comprobación `context.Request.Certificate.VerifyNoRevocation()` en lugar de `context.Request.Certificate.Verify()`.
> Si el certificado de cliente es un certificado autofirmado, raíz (o intermedia) deben ser certificados de CA [cargado](api-management-howto-ca-certificates.md) a la API de administración de `context.Request.Certificate.Verify()` y `context.Request.Certificate.VerifyNoRevocation()` funcione.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Comprobación de una huella digital en relación con certificados cargados en API Management

En el ejemplo siguiente se muestra cómo comprobar la huella digital de un certificado de cliente en relación con los certificados cargados en API Management:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Para deshabilitar el uso de lista de revocación de certificados comprobación `context.Request.Certificate.VerifyNoRevocation()` en lugar de `context.Request.Certificate.Verify()`.
> Si el certificado de cliente es un certificado autofirmado, raíz (o intermedia) deben ser certificados de CA [cargado](api-management-howto-ca-certificates.md) a la API de administración de `context.Request.Certificate.Verify()` y `context.Request.Certificate.VerifyNoRevocation()` funcione.

> [!TIP]
> Problema de interbloqueo de certificado de cliente se describe en este [artículo](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) puede manifestarse de varias maneras, por ejemplo, las solicitudes inmovilizar, el resultado de las solicitudes `403 Forbidden` código de estado después de agotar el tiempo, `context.Request.Certificate` es `null`. Este problema afecta a normalmente `POST` y `PUT` las solicitudes con la longitud del contenido de aproximadamente 60 KB o superior.
> Para evitar este problema produzca habilite "Certificado de cliente Negotiate" para los nombres de host deseado en la hoja "Dominios personalizados" tal como se muestra a continuación. Esta característica no está disponible en el nivel de consumo.

![Negociar el certificado de cliente](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Pasos siguientes

-   [Cómo asegurar servicios back-end con la autenticación de certificados de cliente](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [¿Cómo se cargan certificados?](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
