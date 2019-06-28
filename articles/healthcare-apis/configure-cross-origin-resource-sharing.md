---
title: Configuración del uso compartido de recursos entre orígenes en Azure API for FHIR
description: En este artículo se describe cómo configurar el uso compartido de recursos entre orígenes en Azure API for FHIR.
author: matjazl
ms.author: matjazl
ms.date: 3/11/2019
ms.topic: reference
ms.service: healthcare-apis
ms.openlocfilehash: c6bb39508055926323dad3dd6b50f9d60fdecdab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033754"
---
# <a name="configure-cross-origin-resource-sharing-in-the-azure-api-for-fhir"></a>Configuración del uso compartido de recursos entre orígenes en Azure API for FHIR

La API de Azure para Fast Healthcare Interoperability Resources (FHIR) admite el [uso compartido de recursos entre orígenes (CORS)](https://wikipedia.org/wiki/Cross-Origin_Resource_Sharing). CORS le permite configurar los parámetros para que las aplicaciones de un dominio (origen) puedan acceder a los recursos de un dominio diferente, lo que se conoce como una solicitud entre dominios.

A menudo se usa CORS en una aplicación de una única página que debe llamar a una API de RESTful a un dominio diferente.

Para configurar un ajuste de CORS en Azure API for FHIR, especifique lo siguiente:

- **Orígenes (Access-Control-Allow-Origin)** . Una lista de dominios que puede realizar solicitudes entre orígenes a Azure API for FHIR. Cada dominio (origen) debe especificarse en una línea independiente. Puede indicar un asterisco (*) para permitir las llamadas de cualquier dominio, pero no se aconseja porque es un riesgo de seguridad.

- **Encabezados (Access-Control-Allow-Headers)** . Una lista de encabezados que contendrá la solicitud de origen. Para permitir todos los encabezados, especifique un asterisco (*).

- **Métodos (Access-Control-Allow-Methods)** . Los métodos permitidos (PUT, GET, POST, etc.) en una llamada API. Elija **Seleccionar todo** para todos los métodos.

- **Antigüedad máxima (Access-Control-Max-Age)** . El valor en segundos para almacenar en caché los resultados de la solicitud preparatoria para Access-Control-Allow-Headers y Access-Control-Allow-Methods.

- **Permitir credenciales (Access-Control-Allow-Credentials)** . Las solicitudes CORS normalmente no incluyen las cookies para evitar los ataques de [falsificación de solicitud entre sitios](https://en.wikipedia.org/wiki/Cross-site_request_forgery). Si selecciona esta opción, la solicitud se puede realizar para incluir credenciales, como las cookies. No se puede configurar esta opción si ya ha establecido Orígenes con un asterisco (*).

![Configuración del uso compartido de recursos entre orígenes (CORS)](media/cors/cors.png)

>[!NOTE]
>No se puede especificar distintas configuraciones para orígenes de dominio diferentes. Todas las configuraciones (**Encabezados**, **Métodos**, **Antigüedad máxima** y **Permitir credenciales**) se aplican a todos los orígenes especificados en la configuración Orígenes.