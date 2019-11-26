---
title: 'CLI de Azure Service Fabric: sfctl events | Microsoft Docs'
description: Describe los comandos de sfctl events de la CLI Service Fabric.
services: service-fabric
documentationcenter: na
author: jeffj6123
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 858fd1971a22b1db2d243838558c3792d3a60cc9
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901867"
---
# <a name="sfctl-events"></a>sfctl events
Recupere eventos del almacén de eventos (si ya está instalado el servicio EventStore).

El servicio del sistema EventStore se puede agregar a través de una actualización de la configuración a cualquier clúster de SFRP que ejecute > = 6.4. Compruebe la siguiente dirección URL\: https\://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-eventstore.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| all-applications-list | Obtiene los eventos relacionados con todas las aplicaciones. |
| all-nodes-list | Obtiene los eventos relacionados con todos los nodos. |
| all-partitions-list | Obtiene los eventos relacionados con todas las particiones. |
| all-services-list | Obtiene los eventos relacionados con todos los servicios. |
| application-list | Obtiene los eventos relacionados con una aplicación. |
| cluster-list | Obtiene los eventos relacionados con todos los clústeres. |
| node-list | Obtiene los eventos relacionados con un nodo. |
| partition-all-replicas-list | Obtiene los eventos relacionados con todas las réplicas para una partición. |
| partition-list | Obtiene los eventos relacionados con una partición. |
| partition-replica-list | Obtiene los eventos relacionados con una réplica de partición. |
| service-list | Obtiene los eventos relacionados con un servicio. |

## <a name="sfctl-events-all-applications-list"></a>sfctl events all-applications-list
Obtiene los eventos relacionados con todas las aplicaciones.

La respuesta es una lista de objetos ApplicationEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-all-nodes-list"></a>sfctl events all-nodes-list
Obtiene los eventos relacionados con todos los nodos.

La respuesta es una lista de objetos NodeEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-all-partitions-list"></a>sfctl events all-partitions-list
Obtiene los eventos relacionados con todas las particiones.

La respuesta es una lista de objetos PartitionEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-all-services-list"></a>sfctl events all-services-list
Obtiene los eventos relacionados con todos los servicios.

La respuesta es una lista de objetos ServiceEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-application-list"></a>sfctl events application-list
Obtiene los eventos relacionados con una aplicación.

La respuesta es una lista de objetos ApplicationEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-cluster-list"></a>sfctl events cluster-list
Obtiene los eventos relacionados con todos los clústeres.

La respuesta es una lista de objetos ClusterEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-node-list"></a>sfctl events node-list
Obtiene los eventos relacionados con un nodo.

La respuesta es una lista de objetos NodeEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-partition-all-replicas-list"></a>sfctl events partition-all-replicas-list
Obtiene los eventos relacionados con todas las réplicas para una partición.

La respuesta es una lista de objetos ReplicaEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --partition-id   [obligatorio] | La identidad de la partición. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-partition-list"></a>sfctl events partition-list
Obtiene los eventos relacionados con una partición.

La respuesta es una lista de objetos PartitionEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --partition-id   [obligatorio] | La identidad de la partición. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-partition-replica-list"></a>sfctl events partition-replica-list
Obtiene los eventos relacionados con una réplica de partición.

La respuesta es una lista de objetos ReplicaEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --partition-id   [obligatorio] | La identidad de la partición. |
| --replica-id     [obligatorio] | El identificador de la réplica. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-events-service-list"></a>sfctl events service-list
Obtiene los eventos relacionados con un servicio.

La respuesta es una lista de objetos ServiceEvent.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --end-time-utc   [obligatorio] | La hora de finalización de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --start-time-utc [obligatorio] | La hora de inicio de una consulta de búsqueda en UTC de ISO aaaa-MM-ddHH\:mm\:ssZ. |
| --events-types-filter | Se trata de una cadena separada por comas que especifica los tipos de FabricEvents que solo deben incluirse en la respuesta. |
| --exclude-analysis-events | Este parámetro deshabilita la recuperación de AnalysisEvents si se pasa true. |
| --skip-correlation-lookup | Este parámetro deshabilita la búsqueda de información de CorrelatedEvents si se pasa true. En otro caso, se procesa CorrelationEvents y se rellena el campo HasCorrelatedEvents de cada FabricEvent. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

