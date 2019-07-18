---
title: Restricciones y problemas conocidos en la importación de la API Azure API Management | Microsoft Docs
description: Detalles de los problemas conocidos y las restricciones en la importación en Azure API Management con los formatos de Open API, WSDL o WADL.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2019
ms.author: apimpm
ms.openlocfilehash: af550d3cdf359fc79b3cc2c799e531e5ec491c4e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613625"
---
# <a name="api-import-restrictions-and-known-issues"></a>Restricciones de importación de API y problemas conocidos

## <a name="about-this-list"></a>Acerca de esta lista

Al importar una API, podría encontrarse con algunas restricciones o identificar problemas que es necesario corregir antes de realizar esta operación. En este artículo se documenta todo esto, organizado por el formato de importación de la API.

## <a name="open-api"> </a>OpenAPI/Swagger

Si recibe errores al importar el documento de OpenAPI, asegúrese de haberlo validado con antelación. Para ello, use el diseñador en Azure Portal (Diseño - Front-End - OpenAPI Specification Editor [Editor de especificaciones de OpenAPI]) o con una herramienta de terceros como <a href="https://editor.swagger.io">Swagger Editor</a>.

### <a name="open-api-general"> </a>General

-   Los parámetros necesarios en la ruta de acceso y en la consulta deben tener nombres únicos. (En OpenAPI, un nombre de parámetro solo debe ser único dentro de una ubicación; por ejemplo, ruta de acceso, consulta, encabezado. Pero en API Management se permite que los parámetros tanto de la ruta de acceso como de la consulta [que OpenAPI no admite] discriminen las operaciones. Por eso es necesario que los nombres de los parámetros sean únicos en toda la plantilla de la dirección URL).
-   Los punteros de **\$$ref** no pueden hacer referencia a archivos externos.
-   **x-ms-paths** y **x-servers** son las únicas extensiones admitidas.
-   Las extensiones personalizadas se omiten durante la importación y no se guardan ni conservan para la exportación.
-   **Recursión**: API Management no es compatible con definiciones establecidas de forma recursiva (por ejemplo, esquemas que hacen referencia a sí mismos).
-   La dirección URL (si está disponible) del archivo de origen se aplica a direcciones URL del servidor relativas.

### <a name="open-api-v2"> </a>OpenAPI versión 2

-   Solo se admite el formato JSON.

### <a name="open-api-v3"> </a>OpenAPI versión 3

-   Si se especifican muchos **servidores**, API Management tratará de seleccionar la primera dirección URL HTTPS. Si no hay ninguna dirección URL HTTPS: la primera dirección URL HTTP. Si no hay ninguna dirección URL HTTP: la dirección URL del servidor estará vacía.
-   **Ejemplos** no se admite, pero **ejemplo** sí.
-   **Multipart/form-data** no se admite.

> [!IMPORTANT]
> Consulte este [documento](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) para obtener información importante y consejos relacionados con la importación de OpenAPI.

## <a name="wsdl"></a>WSDL

Los archivos WSDL se usan para crear API de paso a través de SOAP y de SOAP a REST.

-   **Enlaces SOAP**: solo se admiten los enlaces SOAP cuya codificación es de tipo "documento" o "literal". No se admiten las codificaciones de estilo "rpc" o SOAP.
-   **WSDL:Import**: no se admite este atributo. Los clientes deben combinar las importaciones en un único documento.
-   **Mensajes con varias partes**: en la actualidad no se admiten estos tipos de mensajes.
-   **WCF wsHttpBinding**: los servicios SOAP creados con Windows Communication Foundation deben usar basicHttpBinding. No se admite el uso de wsHttpBinding.
-   **MTOM**: los servicios que usan MTOM <em>podrían</em> funcionar. No se ofrece soporte técnico oficial en este momento.
-   **Recursión**: APIM no admite los tipos que se definen de modo recursivo (por ejemplo, hacen referencia a una matriz formada por ellos mismos).
-   **Varios espacios de nombres**: en un esquema se pueden usar varios espacios de nombres, pero el único que se puede usar para definir las partes del mensaje es el de destino. Los espacios de nombres que se usen para definir otros elementos de entrada o salidas, y que no sean el destino, no se conservan. Aunque se puede importar un documento WSDL, en la exportación todas las partes del mensaje tendrá el espacio de nombres de destino de WSDL.
-   **Matrices**: la transformación de SOAP a REST admite solo las matrices encapsuladas que se muestran en el siguiente ejemplo:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"></a>WADL

Actualmente, no hay ningún problema de importación conocido de WADL.
