---
title: 'CLI de Azure Service Fabric: sfctl cluster | Microsoft Docs'
description: Describe los comandos de sfctl cluster de la CLI Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 7bb399472d7e0ab14e6399fc8652d2eb132a866a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837339"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Seleccione, administre y opere clústeres de Service Fabric.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| code-versions | Obtiene una lista de versiones de código de tejido que se aprovisionan en un clúster de Service Fabric. |
| config-versions | Obtiene una lista de versiones de configuración de tejido que se aprovisionan en un clúster de Service Fabric. |
| health | Obtiene el estado de un clúster de Service Fabric. |
| manifest | Obtiene el manifiesto de clúster de Service Fabric. |
| operation-cancel | Cancela una operación de error provocada por el usuario. |
| operation-list | Obtiene una lista de operaciones de error provocada por el usuario, filtrada por la entrada proporcionada. |
| provision | Aprovisiona el código o los paquetes de configuración de un clúster de Service Fabric. |
| recover-system | Indica al clúster de Service Fabric que debería intentar recuperar los servicios del sistema que actualmente están atascados en pérdida de cuórum. |
| report-health | Envía un informe de estado sobre el clúster de Service Fabric. |
| select | Se conecta a un punto de conexión del clúster de Service Fabric. |
| show-connection | Muestra el clúster de Service Fabric al que está conectada esta instancia de sfctl. |
| unprovision | Deshace el aprovisionamiento del código o los paquetes de configuración de un clúster de Service Fabric. |
| upgrade | Comienza a actualizar la versión del código o de la configuración de un clúster de Service Fabric. |
| upgrade-resume | Hace que la actualización del clúster avance al siguiente dominio de actualización. |
| upgrade-rollback | Revierte la actualización de un clúster de Service Fabric. |
| upgrade-status | Obtiene el progreso de la actualización del clúster actual. |
| upgrade-update | Actualiza los parámetros de actualización de una actualización de clúster de Service Fabric. |

## <a name="sfctl-cluster-code-versions"></a>sfctl cluster code-versions
Obtiene una lista de versiones de código de tejido que se aprovisionan en un clúster de Service Fabric.

Obtiene una lista de información sobre las versiones de código de tejido que se aprovisionan en un clúster. El parámetro CodeVersion se puede usar para filtrar de manera opcional la salida a solo una versión concreta.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --code-version | La versión del producto de Service Fabric. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-config-versions"></a>sfctl cluster config-versions
Obtiene una lista de versiones de configuración de tejido que se aprovisionan en un clúster de Service Fabric.

Obtiene una lista de versiones de configuración de Fabric que se aprovisionan en un clúster. El parámetro ConfigVersion se puede usar para filtrar de manera opcional la salida a solo una versión concreta.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --config-version | La versión de configuración de Service Fabric. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-health"></a>sfctl cluster health
Obtiene el estado de un clúster de Service Fabric.

Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en el clúster en función del estado de mantenimiento. Asimismo, utilice NodesHealthStateFilter y ApplicationsHealthStateFilter para filtrar la colección de nodos y aplicaciones devueltas, según su estado de mantenimiento agregado.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --applications-health-state-filter | Permite el filtrado de los objetos de estado de mantenimiento de aplicaciones devueltos en el resultado de la consulta de mantenimiento del clúster en función de su estado de mantenimiento. Los valores posibles para este parámetro incluyen un valor entero obtenido de los miembros u operaciones bit a bit en miembros de la enumeración HealthStateFilter. Solo se devuelven las aplicaciones que coinciden con el filtro. Todas las aplicaciones se usan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit 'OR'. Por ejemplo, si el valor proporcionado es 6, se devuelve el estado de mantenimiento de las aplicaciones con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado de mantenimiento Ok, Warning y Error. |
| --include-system-application-health-statistics | Indica si las estadísticas de mantenimiento deben incluir las estadísticas de mantenimiento de aplicaciones fabric\:/System. El valor predeterminado es false. Si IncludeSystemApplicationHealthStatistics se establece en true, las estadísticas de mantenimiento incluyen las entidades que pertenecen a la aplicación fabric\:/System. En caso contrario, el resultado de la consulta incluye las estadísticas de estado solo para las aplicaciones de usuario. Las estadísticas de mantenimiento deben incluirse en el resultado de la consulta para que se aplique este parámetro. |
| --nodes-health-state-filter | Permite filtrar los objetos de estado de mantenimiento de nodos devueltos en el resultado de la consulta de mantenimiento del clúster en función de su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los nodos que coinciden con el filtro. Todos los nodos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit 'OR'. Por ejemplo, si el valor proporcionado es 6, se devuelve el estado de mantenimiento de los nodos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-manifest"></a>sfctl cluster manifest
Obtiene el manifiesto de clúster de Service Fabric.

Obtiene el manifiesto de clúster de Service Fabric. El manifiesto de clúster contiene propiedades del clúster que incluyen distintos tipos de nodos en el clúster, las configuraciones de seguridad, los errores y las topologías de dominio de actualización, etc. Estas propiedades se especifican como parte del archivo ClusterConfig.JSON al implementar un clúster independiente. Sin embargo, el tejido del servicio genera internamente la mayor parte de la información en el manifiesto de clúster durante la implementación del clúster en otros escenarios de implementación (por ejemplo, cuando se usa Azure Portal). El contenido del manifiesto de clúster tiene solo fines informativos y no se espera que los usuarios tomen una dependencia en el formato del contenido del archivo ni su interpretación.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-operation-cancel"></a>sfctl cluster operation-cancel
Cancela una operación de error provocada por el usuario.

Las siguientes API de inician las operaciones de error que pueden cancelarse mediante el uso de CancelOperation\: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Si force es false, la operación inducida por el usuario especificado se detendrá y se limpiará correctamente.  Si force es true, se anulará el comando y puede quedar algún estado interno.  Debe utilizarse con cuidado la especificación de force como true. No se permite llamar a esta API con force establecido en true hasta que esta API se haya llamado en el mismo comando de prueba con force establecido en false primero, o a menos que el comando de prueba ya tenga un elemento OperationState de OperationState.RollingBack. 

Aclaración\: OperationState.RollingBack significa que el sistema limpia o limpiará el estado interno del sistema causado por la ejecución del comando.  No restaurará los datos si el comando de prueba iba a causar pérdida de datos.  Por ejemplo, si llama a StartDataLoss y luego llama a esta API, el sistema solo limpiará el estado interno de la ejecución del comando. No restaurará datos de la partición de destino si el comando ha progresado lo suficiente para provocar la pérdida de datos. 

> [!NOTE]
> Si esta API se invoca con force== true, el estado interno puede quedar atrás.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --operation-id [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --force | Indica si se debe revertir correctamente y limpiar el estado interno del sistema modificado mediante la ejecución de la operación inducida por el usuario. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-operation-list"></a>sfctl cluster operation-list
Obtiene una lista de operaciones de error provocada por el usuario, filtrada por la entrada proporcionada.

Obtiene la lista de operaciones de error provocada por el usuario, filtrada por la entrada proporcionada.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --state-filter | Se utiliza para filtrar por OperationState para operaciones inducidas por el usuario. <br> 65535 - select All <br> 1 - select Running <br> 2     - select RollingBack <br>8     - select Completed <br>16    - select Faulted <br>32    - select Cancelled <br>64    - select ForceCancelled.  <br>Valor predeterminado\: 65535. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --type-filter | Se utiliza para filtrar por OperationType para operaciones inducida por el usuario. <br> 65535 - select all <br> 1     - select PartitionDataLoss. <br> 2     - select PartitionQuorumLoss. <br> 4     - select PartitionRestart. <br> 8     - select NodeTransition.  <br> Valor predeterminado\: 65535. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-provision"></a>sfctl cluster provision
Aprovisiona el código o los paquetes de configuración de un clúster de Service Fabric.

Valida y aprovisiona el código o los paquetes de configuración de un clúster de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --cluster-manifest-file-path | La ruta de acceso del archivo de manifiesto de clúster. |
| --code-file-path | La ruta del archivo del paquete de código del clúster. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-recover-system"></a>sfctl cluster recover-system
Indica al clúster de Service Fabric que debería intentar recuperar los servicios del sistema que actualmente están atascados en pérdida de cuórum.

Indica al clúster de Service Fabric que debería intentar recuperar los servicios del sistema que actualmente están atascados en pérdida de cuórum. Esta operación solo debe realizarse si se sabe que no pueden recuperarse las réplicas que están fuera de servicio. El uso incorrecto de esta API puede provocar la pérdida potencial de datos.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-report-health"></a>sfctl cluster report-health
Envía un informe de estado sobre el clúster de Service Fabric.

El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. El informe se envía a un nodo de puerta de enlace de Service Fabric, que lo reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, compruebe que aparece en la sección HealthEvents del clúster.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
| --source-id       [Obligatorio] | El nombre de origen que identifica el componente de cliente/guardián/sistema que genera la información de estado. |
| --description | La descripción de la información de mantenimiento. <br><br> Representa el texto libre utilizado para agregar información legible por el ser humano sobre el informe. La longitud máxima de la cadena de la descripción es de 4096 caracteres. Si la cadena proporcionada es más larga, se truncará automáticamente. Cuando está truncado, los últimos caracteres de la descripción contienen un marcador "[Truncado]" y el tamaño total de la cadena es de 4096 caracteres. La presencia del marcador indica a los usuarios que el truncamiento se ha producido. Tenga en cuenta que, cuando se trunca, la descripción tiene menos de 4096 caracteres de la cadena original. |
| --immediate | Una marca que indica si el informe se debe enviar inmediatamente. <br><br> Se envía un informe de mantenimiento a una aplicación de puerta de enlace de Service Fabric, que la reenvía al almacén de estado. Si Immediate se establece en true, el informe se envía inmediatamente de la puerta de enlace HTTP al almacén de estado, independientemente de la configuración de cliente de Fabric que esté usando la aplicación de la puerta de enlace HTTP. Esto es útil para los informes críticos que deben enviarse tan pronto como sea posible. En función del tiempo y otras condiciones, el envío del informe puede aún generar un error, por ejemplo, si se cierra la puerta de enlace HTTP o el mensaje no llega a la puerta de enlace. Si Immediate se establece en false, el informe se envía en función de la configuración de cliente de mantenimiento de la puerta de enlace HTTP. Por lo tanto, se procesará por lotes de acuerdo con la configuración de HealthReportSendInterval. Esta es la configuración recomendada porque permite que el cliente de mantenimiento optimice los mensajes de notificación de estado destinados al almacén de estado, así como el procesamiento de informes de mantenimiento. De forma predeterminada, los informes no se envían inmediatamente. |
| --remove-when-expired | Valor que indica si el informe se quita del almacén de estado cuando expire. <br><br> Si se establece en true, el informe se quita del almacén de estado una vez que expire. Si se establece en false, el informe se trata como un error cuando expire. El valor de esta propiedad es false de forma predeterminada. Cuando los clientes notifican periódicamente, deben establecer RemoveWhenExpired en false (valor predeterminado). De esta manera, si el notificador tiene problemas (por ejemplo, un interbloqueo) y no puede informar, la entidad se evalúa al llegar el error cuando expira el informe de mantenimiento. De este modo, se marca que la entidad está en estado de mantenimiento Error. |
| --sequence-number | El número de secuencia para este informe de estado como una cadena numérica. <br><br> El número de secuencia del informe se usa por el almacén de estado para detectar informes obsoletos. Si no se especifica, se genera automáticamente un número de secuencia por el cliente de estado cuando se agrega un informe. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --ttl | La duración durante la cual este informe de mantenimiento es válido. Este campo usa el formato ISO8601 para especificar la duración. <br><br> Cuando los clientes notifican periódicamente, deben enviar informes con una frecuencia mayor que el período de vida. Si los clientes notifican en transición, pueden establecer el período de vida en Infinito. Cuando expira el período de vida, el evento de estado que contiene la información de estado se quita del almacén de estado, si RemoveWhenExpired es true, o se evalúa en el error, si RemoveWhenExpired es false. Si no se especifica, el período de vida se establece de forma predeterminada en un valor infinito. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-select"></a>sfctl cluster select
Se conecta a un punto de conexión del clúster de Service Fabric.

Si se conecta al clúster seguro, especifique una ruta de acceso absoluta a un archivo de certificado (.crt) y de clave (.key) o un único archivo con ambos (.pem). No especifique ambos. Como alternativa, si se conecta a un clúster seguro, también especifique una ruta de acceso absoluta a un archivo de paquetes de la entidad de certificación o al directorio de certificados de confianza de la entidad de certificación. Si usa un directorio de certificados de la entidad de certificación, `c_rehash <directory>` proporcionado por OpenSSL, se debe ejecutar en primer lugar para calcular los valores hash del certificado y crear los vínculos simbólicos adecuados.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --endpoint [Obligatorio] | Dirección URL del punto de conexión del clúster, incluido el puerto y el prefijo HTTP o HTTPS. |
| --aad | Utilice Azure Active Directory para la autenticación. |
| --ca | Ruta de acceso absoluta al directorio de certificados de la entidad emisora de certificados que se tratará como válido o al archivo de paquete de entidades emisoras de certificados. |
| --cert | Ruta de acceso absoluta a un archivo de certificados de cliente. |
| --key | Ruta de acceso absoluta a un archivo de claves de certificados de cliente. |
| --no-verify | Deshabilita la comprobación de certificados cuando se utiliza HTTPS. Tenga en cuenta\: Esta es una opción insegura y no debe usarse para entornos de producción. |
| --pem | Ruta de acceso absoluta a un certificado de cliente, como un archivo .pem. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-show-connection"></a>sfctl cluster show-connection
Muestra el clúster de Service Fabric al que está conectada esta instancia de sfctl.

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster unprovision
Deshace el aprovisionamiento del código o los paquetes de configuración de un clúster de Service Fabric.

Es posible deshacer el aprovisionamiento de código y la configuración por separado.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --code-version | La versión del paquete de código del clúster. |
| --config-version | La versión del manifiesto de clúster. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-upgrade"></a>sfctl cluster upgrade
Comienza a actualizar la versión del código o de la configuración de un clúster de Service Fabric.

Valida los parámetros de actualización suministrados e inicia la actualización del código o de la versión de configuración de un clúster de Service Fabric si los parámetros son válidos.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --app-health-map | Diccionario codificado en JSON de pares de nombres de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --app-type-health-map | Diccionario codificado en JSON de pares de nombres de tipo de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --code-version | La versión del código del clúster. |
| --config-version | La versión de configuración del clúster. |
| --delta-health-evaluation | Permite la evaluación de mantenimiento delta en lugar de absoluta tras la finalización de cada dominio de actualización. |
| --delta-unhealthy-nodes | El porcentaje máximo permitido de degradación del mantenimiento de los nodos durante las actualizaciones del clúster.  Valor predeterminado\: 10. <br><br> El delta se mide entre el estado de los nodos al principio de la actualización y el estado de los nodos en el momento de la evaluación de mantenimiento. La comprobación se realiza después de que finaliza la actualización de cada dominio de actualización para asegurarse de que el estado global del clúster está dentro de los límites permitidos. |
| --failure-action | Los valores posibles son\: "Invalid", "Rollback", "Manual". |
| --force-restart | Los procesos se reinician de forma forzosa durante la actualización incluso si no ha cambiado la versión del código. <br><br> La actualización solo cambia la configuración o los datos. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no son correctos. |
| --health-check-stable | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento. |
| --replica-set-check-timeout | El período de tiempo máximo para bloquear el procesamiento de un dominio de actualización y evitar la pérdida de disponibilidad cuando hay problemas inesperados. <br><br> Cuando este tiempo de espera expire, el procesamiento del dominio de actualización se llevará a cabo independientemente de los problemas de pérdida de disponibilidad. El tiempo de espera se restablece al principio de cada dominio de actualización. Los valores válidos oscilan entre 0 y 42949672925, ambos inclusive. |
| --rolling-upgrade-mode | Los valores posibles son\: "Invalid", "UnmonitoredAuto", "UnmonitoredManual", "Monitored".  Valor predeterminado\: UnmonitoredAuto. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado antes de informar de un error. <br><br> Por ejemplo, para permitir el 10 % de las aplicaciones en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de aplicaciones que pueden ser incorrectas antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos una aplicación en mal estado, el estado se evalúa como Warning. Esto se calcula dividiendo el número de aplicaciones en mal estado sobre el número total de instancias de la aplicación en el clúster, excluidas las aplicaciones de los tipos de aplicación que se incluyen en ApplicationTypeHealthPolicyMap. El cálculo se redondea hacia arriba para tolerar un error en números reducidos de aplicaciones. |
| --unhealthy-nodes | El porcentaje máximo permitido de nodos en mal estado antes de informar de un error. <br><br> Por ejemplo, para permitir el 10 % de los nodos en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de nodos que pueden ser incorrectos antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos un nodo en mal estado, el estado se evalúa como Warning. El porcentaje se calcula dividiendo el número de nodos en mal estado sobre el número total de nodos del clúster. El cálculo se redondea hacia arriba para tolerar un error en números reducidos de nodos. En los clústeres grandes, siempre habrá nodos inactivos o inoperativos debido a reparaciones, por lo que este porcentaje debe configurarse para tolerar ese hecho. |
| --upgrade-domain-delta-unhealthy-nodes | El porcentaje máximo permitido de degradación del mantenimiento de los nodos de dominio de actualización durante las actualizaciones del clúster.  Valor predeterminado\: 15. <br><br> El delta se mide entre el estado de los nodos del dominio de actualización al principio de la actualización y el estado de los nodos del dominio de actualización en el momento de la evaluación de mantenimiento. La comprobación se realiza después de que finaliza la actualización de cada dominio de actualización en todos los dominios de actualización completados para asegurarse de que el estado global del clúster está dentro de los límites permitidos. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --warning-as-error | Indica si las advertencias se tratan con el mismo nivel de gravedad que los errores. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl cluster upgrade-resume
Hace que la actualización del clúster avance al siguiente dominio de actualización.

Hace que el código del clúster o la actualización de configuración avance al siguiente dominio de actualización, si procede.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --upgrade-domain [Requerido] | El siguiente dominio de actualización para esta actualización de clúster. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl cluster upgrade-rollback
Revierte la actualización de un clúster de Service Fabric.

Revierte el código o la actualización de configuración de un clúster de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-upgrade-status"></a>sfctl cluster upgrade-status
Obtiene el progreso de la actualización del clúster actual.

Obtiene el progreso actual de la actualización del clúster en curso. Si no hay ninguna actualización en curso, obtenga el último estado de la actualización del clúster anterior.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-cluster-upgrade-update"></a>sfctl cluster upgrade-update
Actualiza los parámetros de actualización de una actualización de clúster de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --app-health-map | Diccionario codificado en JSON de pares de nombres de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --app-type-health-map | Diccionario codificado en JSON de pares de nombres de tipo de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --delta-health-evaluation | Permite la evaluación de mantenimiento delta en lugar de absoluta tras la finalización de cada dominio de actualización. |
| --delta-unhealthy-nodes | El porcentaje máximo permitido de degradación del mantenimiento de los nodos durante las actualizaciones del clúster.  Valor predeterminado\: 10. <br><br> El delta se mide entre el estado de los nodos al principio de la actualización y el estado de los nodos en el momento de la evaluación de mantenimiento. La comprobación se realiza después de que finaliza la actualización de cada dominio de actualización para asegurarse de que el estado global del clúster está dentro de los límites permitidos. |
| --failure-action | Los valores posibles son\: "Invalid", "Rollback", "Manual". |
| --force-restart | Los procesos se reinician de forma forzosa durante la actualización incluso si no ha cambiado la versión del código. <br><br> La actualización solo cambia la configuración o los datos. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no son correctos. |
| --health-check-stable | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento. |
| --replica-set-check-timeout | El período de tiempo máximo para bloquear el procesamiento de un dominio de actualización y evitar la pérdida de disponibilidad cuando hay problemas inesperados. <br><br> Cuando este tiempo de espera expire, el procesamiento del dominio de actualización se llevará a cabo independientemente de los problemas de pérdida de disponibilidad. El tiempo de espera se restablece al principio de cada dominio de actualización. Los valores válidos oscilan entre 0 y 42949672925, ambos inclusive. |
| --rolling-upgrade-mode | Los valores posibles son\: "Invalid", "UnmonitoredAuto", "UnmonitoredManual", "Monitored".  Valor predeterminado\: UnmonitoredAuto. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado antes de informar de un error. <br><br> Por ejemplo, para permitir el 10 % de las aplicaciones en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de aplicaciones que pueden ser incorrectas antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos una aplicación en mal estado, el estado se evalúa como Warning. Esto se calcula dividiendo el número de aplicaciones en mal estado sobre el número total de instancias de la aplicación en el clúster, excluidas las aplicaciones de los tipos de aplicación que se incluyen en ApplicationTypeHealthPolicyMap. El cálculo se redondea hacia arriba para tolerar un error en números reducidos de aplicaciones. |
| --unhealthy-nodes | El porcentaje máximo permitido de nodos en mal estado antes de informar de un error. <br><br> Por ejemplo, para permitir el 10 % de los nodos en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de nodos que pueden ser incorrectos antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos un nodo en mal estado, el estado se evalúa como Warning. El porcentaje se calcula dividiendo el número de nodos en mal estado sobre el número total de nodos del clúster. El cálculo se redondea hacia arriba para tolerar un error en números reducidos de nodos. En los clústeres grandes, siempre habrá nodos inactivos o inoperativos debido a reparaciones, por lo que este porcentaje debe configurarse para tolerar ese hecho. |
| --upgrade-domain-delta-unhealthy-nodes | El porcentaje máximo permitido de degradación del mantenimiento de los nodos de dominio de actualización durante las actualizaciones del clúster.  Valor predeterminado\: 15. <br><br> El delta se mide entre el estado de los nodos del dominio de actualización al principio de la actualización y el estado de los nodos del dominio de actualización en el momento de la evaluación de mantenimiento. La comprobación se realiza después de que finaliza la actualización de cada dominio de actualización en todos los dominios de actualización completados para asegurarse de que el estado global del clúster está dentro de los límites permitidos. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-kind | Los valores posibles incluyen\: "Invalid", "Rolling", "Rolling_ForceRestart".  Valor predeterminado\: Rolling. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --warning-as-error | Indica si las advertencias se tratan con el mismo nivel de gravedad que los errores. |

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