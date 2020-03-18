---
title: Actividad de webhook en Azure Data Factory
description: La actividad de webhook no continúa la ejecución de la canalización hasta que valida el conjunto de datos adjunto con determinados criterios que el usuario especifica.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: 8c52bb21276071581a83fb3ee6a3a4a31ba0bb4a
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78400004"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Actividad de webhook en Azure Data Factory
Puede usar una actividad de webhook para controlar la ejecución de canalizaciones a través de su código personalizado. Con la actividad de webhook, los clientes pueden llamar a un punto de conexión y pasar una dirección URL de devolución de llamada. La ejecución de canalización espera a que la devolución de llamada se invoque antes de continuar con la siguiente actividad.

## <a name="syntax"></a>Sintaxis

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Propiedades de tipo



Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
name | Nombre de la actividad webhook | String | Sí |
type | Debe establecerse en **WebHook**. | String | Sí |
method | Método de API de REST para el punto de conexión de destino. | String. Tipos admitidos: "POST" | Sí |
url | Punto de conexión y ruta de acceso de destino | Cadena (o expresión con un valor resultType de cadena). | Sí |
headers | Encabezados que se envían a la solicitud. Por ejemplo, para establecer el idioma y el tipo en una solicitud: "headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }. | Cadena (o expresión con un valor resultType de cadena) | Sí, el encabezado Content-type es necesario. "headers":{ "Content-Type":"application/json"} |
body | Representa la carga útil que se envía al punto de conexión. | JSON válido (o expresión con un valor resultType de JSON). Vea el esquema de la carga de solicitud en la sección [Solicitar un esquema de carga](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0). | Sí |
autenticación | Método de autenticación usado para llamar al punto de conexión. Los tipos admitidos son "Basic" y "ClientCertificate". Para más información, vea la sección [Autenticación](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0). Si la autenticación no es necesaria, excluya esta propiedad. | Cadena (o expresión con un valor resultType de cadena) | Sin |
timeout | ¿Durante cuánto tiempo esperará la actividad a que se invoque &#39;callBackUri&#39;? ¿Durante cuánto tiempo esperará la actividad a que se invoque "callBackUri"? El valor predeterminado es 10 min ("00:10:00"). El formato es un intervalo de tiempo, es decir, d.hh:mm:ss | String | Sin |
Notificar el estado de la devolución de llamada | Permite al usuario informar del estado de error de la actividad de webhook, que marcará la actividad como con errores. | Boolean | Sin |

## <a name="authentication"></a>Authentication

A continuación se muestran los tipos de autenticación admitidos en la actividad de webhook.

### <a name="none"></a>None

Si la autenticación no es necesaria, no incluya la propiedad "authentication".

### <a name="basic"></a>Básica

Especifique el nombre de usuario y la contraseña que se usarán con la autenticación básica.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Certificado de cliente

Especifique un contenido codificado en base64 de un archivo PFX y la contraseña.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>Identidad administrada

Especifique el URI de recurso para el que el token de acceso se solicitará utilizando la identidad administrada para la factoría de datos. Para llamar a la API de Azure Resource Management, use `https://management.azure.com/`. Para más información sobre cómo funcionan las identidades administradas, consulte la página de información general [Identidades administradas de recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview).

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> Si la factoría de datos está configurada con un repositorio de Git, debe almacenar las credenciales en Azure Key Vault para usar la autenticación de certificado de cliente o básica. Azure Data Factory no almacena contraseñas en Git.

## <a name="additional-notes"></a>Notas adicionales

Azure Data Factory pasará una propiedad adicional "callBackUri" en el cuerpo al punto de conexión de URL, y esperará que este URI se invoque antes del valor de tiempo de espera especificado. Si no se invoca el URI, se producirá un error en la actividad con estado "TimedOut".

La propia actividad de webhook produce un error cuando la llamada al punto de conexión personalizado genera un error. Cualquier mensaje de error se puede agregar al cuerpo de la devolución de llamada y usarse en una actividad posterior.

Además, en cada llamada API REST, el cliente agotará el tiempo de espera si el punto de conexión no responde en 1 minuto. Se trata del procedimiento recomendado de http estándar. Para corregir este problema, debe implementar el patrón 202 en este caso, donde el punto de conexión devolverá 202 (aceptado) y el cliente realizará un sondeo.

El tiempo de espera de 1 minuto de la solicitud no tiene nada que ver con el tiempo de espera de la actividad. El primero se usará para esperar el valor de callbackUri.

El cuerpo devuelto al URI de devolución de llamada debe ser un JSON válido. Debe establecer el encabezado Content-Type en `application/json`.

Al usar la opción "Notificar el estado de la devolución de llamada", debe agregar el siguiente fragmento de código al cuerpo al hacer la devolución de llamada:

```
{
    "Output": {
        // output object will be used in activity output
        "testProp": "testPropValue"
    },
    "Error": {
        // Optional, set it when you want to fail the activity
        "ErrorCode": "testErrorCode",
        "Message": "error message to show in activity error"
    },
    "StatusCode": "403" // when status code is >=400, activity will be marked as failed
}
```



## <a name="next-steps"></a>Pasos siguientes

Consulte otras actividades de flujo de control compatibles con Data Factory:

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
