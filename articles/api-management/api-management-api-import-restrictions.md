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
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: bad87931feb11012f23f0ef19bd853b38566c07c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106831"
---
# <a name="api-import-restrictions-and-known-issues"></a>Restricciones de importación de API y problemas conocidos
## <a name="about-this-list"></a>Acerca de esta lista
Al importar una API, podría encontrarse con algunas restricciones o identificar problemas que es necesario corregir antes de realizar esta operación. En este artículo se documenta todo esto, organizado por el formato de importación de la API.

## <a name="open-api"> </a>OpenAPI/Swagger
Si recibe errores al importar el documento de OpenAPI, asegúrese de que lo ha validado, ya sea mediante el diseñador en Azure Portal (Diseño - Front-End - OpenAPI Specification Editor [Editor de especificaciones de OpenAPI]) o con una herramienta de terceros como <a href="https://editor.swagger.io">Swagger Editor</a>.

* Se admite solo el formato JSON para OpenAPI.
* Los parámetros necesarios en la ruta de acceso y en la consulta deben tener nombres únicos. (En OpenAPI, un nombre de parámetro solo debe ser único dentro de una ubicación; por ejemplo, ruta de acceso, consulta, encabezado.  Pero en API Management se permite que los parámetros tanto de la ruta de acceso como de la consulta [que OpenAPI no admite] discriminen las operaciones. Por tanto, es necesario que los nombres de los parámetros sean únicos en toda la plantilla de la dirección URL).
* Los esquemas a los que se hace referencia con propiedades **$ref** no pueden contener otras propiedades **$ref**.
* Los punteros de **$ref** no pueden hacer referencia a archivos externos.
* **x-ms-paths** y **x-servers** son las únicas extensiones admitidas.
* Las extensiones personalizadas se omiten durante la importación y no se guardan ni conservan para la exportación.
* **Recursión**: APIM no admite las definiciones que se definen de modo recursivo (por ejemplo, hacen referencia a ellos mismos).

> [!IMPORTANT]
> Consulte este [documento](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) para obtener información importante y consejos relacionados con la importación de OpenAPI.

## <a name="wsdl"></a>WSDL
Los archivos WSDL se usan para generar las API de paso a través de SOAP, o para servir como back-end de una API de SOAP a REST.
* **Enlaces SOAP**: solo se admiten los enlaces SOAP cuya codificación es de tipo "documento" o "literal". No se admiten las codificaciones de estilo "rpc" o SOAP.
* **WSDL:Import**: no se admite este atributo. Los clientes deben combinar las importaciones en un único documento.
* **Mensajes con varias partes**: en la actualidad no se admiten estos tipos de mensajes.
* **WCF wsHttpBinding**: los servicios SOAP creados con Windows Communication Foundation deben usar basicHttpBinding. No se admite el uso de wsHttpBinding.
* **MTOM**: los servicios que usan MTOM <em>podrían</em> funcionar. No se ofrece soporte técnico oficial en este momento.
* **Recursión**: APIM no admite los tipos que se definen de modo recursivo (por ejemplo, hacen referencia a una matriz formada por ellos mismos).
* **Varios espacios de nombres**: en un esquema se pueden usar varios espacios de nombres, pero el único que se puede usar para definir las partes del mensaje es el de destino. Los espacios de nombres que se usen para definir otros elementos de entrada o salidas, y que no sean el destino, no se conservan. Aunque se puede importar un documento WSDL, en la exportación todas las partes del mensaje tendrá el espacio de nombres de destino de WSDL.

## <a name="wadl"></a>WADL
Actualmente, no hay ningún problema de importación conocido de WADL.
