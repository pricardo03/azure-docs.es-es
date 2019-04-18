---
title: Actividad de Webhook en Azure Data Factory | Microsoft Docs
description: La actividad de Webhook no continuar la ejecución de la canalización hasta que valida el conjunto de datos conectado con determinados criterios que el usuario especifica.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 6ec43b06ce266b9ceaddb5dd21cbf52f509d6596
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495915"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Actividad de Webhook en Azure Data Factory
Puede usar una actividad de enlace web para controlar la ejecución de canalizaciones a través de su código personalizado. Uso de la actividad de webhook, los clientes pueden llamar a un punto de conexión y pasar una dirección URL de devolución de llamada. La ejecución de canalización espera a que la devolución de llamada se invoca antes de continuar con la siguiente actividad.

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



Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
Nombre | Nombre de la actividad de enlace web | string | Sí |
Tipo | Debe establecerse en **WebHook**. | string | Sí |
estático | Método de API de REST para el punto de conexión de destino. | String. Tipos admitidos: 'POST' | Sí |
URL | Punto de conexión y ruta de acceso de destino | Cadena (o expresión con un valor resultType de cadena). | Sí |
encabezados | Encabezados que se envían a la solicitud. Por ejemplo, para establecer el idioma y el tipo en una solicitud: "headers": {"Accept-Language": "en-us", "Content-Type": "application/json"}. | Cadena (o expresión con un valor resultType de cadena) | Sí, el encabezado Content-type es necesario. "headers":{ "Content-Type":"application/json"} |
Cuerpo | Representa la carga útil que se envía al punto de conexión. | El cuerpo de la que se pasa a la devolución de llamada el URI debe ser un valor JSON válido. Vea el esquema de la carga de solicitud en la sección [Solicitar un esquema de carga](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0). | Sí |
Autenticación | Método de autenticación usado para llamar al punto de conexión. Tipos admitidos son "Basic" o "ClientCertificate". Para más información, vea la sección [Autenticación](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0). Si la autenticación no es necesaria, excluya esta propiedad. | Cadena (o expresión con un valor resultType de cadena) | Sin  |
timeout | ¿Durante cuánto tiempo esperará a que la actividad la &#39;el callBackUri&#39; que se debe invocar. ¿Durante cuánto tiempo la actividad esperará 'el callBackUri' que se debe invocar. Valor predeterminado es 10mins ("00:10:00"). El formato es el intervalo de tiempo, es decir, d.hh:mm:ss | string | Sin  |

## <a name="additional-notes"></a>Notas adicionales

Azure Data Factory pasará una propiedad adicional "el callBackUri" en el cuerpo para el punto de conexión de dirección url y se espera que sea este identificador uri que se invocará antes del valor de tiempo de espera especificado. Si no se invoca el uri, se producirá un error en la actividad con estado 'TimedOut'.

La propia actividad de enlace web produce un error solo cuando falla la llamada al punto de conexión personalizado. Cualquier mensaje de error se puede agregar en el cuerpo de la devolución de llamada y usar en una actividad posterior.

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory:

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
