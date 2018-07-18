---
title: 'CLI de Azure Service Fabric: sfctl property | Microsoft Docs'
description: Describe los comandos de sfctl property de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: acade3d828c785af9468baa30086d3b79542f9b7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763979"
---
# <a name="sfctl-property"></a>sfctl property
Almacene y consulte las propiedades con nombres de Service Fabric.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| delete | Elimina la propiedad de Service Fabric especificada. |
| get | Obtiene la propiedad de Service Fabric especificada. |
| list | Obtiene información sobre todas las propiedades de Service Fabric con un nombre concreto. |
| put | Crea o actualiza una propiedad de Service Fabric. |

## <a name="sfctl-property-delete"></a>sfctl property delete
Elimina la propiedad de Service Fabric especificada.

Elimina la propiedad de Service Fabric especificada con un nombre concreto. Es necesario haber creado una propiedad para eliminarla.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | Especifica el nombre de la propiedad que se va a obtener. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-get"></a>sfctl property get
Obtiene la propiedad de Service Fabric especificada.

Obtiene la propiedad de Service Fabric especificada con un nombre concreto. Esto devolverá siempre valor y metadatos.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | Especifica el nombre de la propiedad que se va a obtener. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-list"></a>sfctl property list
Obtiene información sobre todas las propiedades de Service Fabric con un nombre concreto.

Un nombre de Service Fabric puede tener una o varias propiedades con nombre que almacenan información personalizada. Esta operación obtiene la información acerca de estas propiedades en una lista paginada. La información incluye nombre, valor y metadatos sobre cada una de las propiedades.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --name-id [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --include-values | Permite especificar si se deben incluir los valores de las propiedades devueltas. True si se deben devolver valores con los metadatos; False para devolver solo los metadatos de propiedad. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-put"></a>sfctl property put
Crea o actualiza una propiedad de Service Fabric.

Crea o actualiza la propiedad de Service Fabric especificada con un nombre concreto.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | El nombre de la propiedad de Service Fabric. |
| --value         [obligatorio] | Describe un valor de propiedad de Service Fabric. Se trata de una cadena JSON. <br><br> La cadena json tiene dos campos, el "tipo" de los datos y el "valor" de los datos. El valor de "tipo" debe ser el primer elemento en aparecer en la cadena JSON, y puede ser "Binary", "Int64", "Double", "String" o "Guid". El valor debe poder compatible con la serialización para los tipos especificados. Los valores de "tipo" y "datos" deben proporcionarse como cadenas. |
| --custom-id-type | El identificador del tipo personalizado de la propiedad. Con esta propiedad, el usuario es capaz de etiquetar el tipo del valor de la propiedad. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).