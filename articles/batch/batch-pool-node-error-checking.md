---
title: Comprobación de errores de nodos y grupos de Batch
description: Errores que se deben comprobar y cómo evitarlos al crear los grupos y nodos
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: bc09089fa9984e9042166938da19499afca21509
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435845"
---
# <a name="checking-for-pool-and-node-errors"></a>Comprobación de errores de grupo y de nodo

Al crear y administrar grupos de Batch, hay operaciones que se realizan de inmediato y hay operaciones asincrónicas que no son inmediatas, se ejecutan en segundo plano y pueden tardar varios minutos en completarse.

Detectar errores para las operaciones que tienen lugar de inmediato es sencillo, ya que la API, la CLI o la interfaz de usuario devolverán errores al instante.

En este artículo se tratan las operaciones en segundo plano que pueden tener lugar para los grupos y los nodos de grupo: especifica cómo se pueden detectar los errores y cómo se pueden evitar.

## <a name="pool-errors"></a>Errores de grupo

### <a name="resize-timeout-or-failure"></a>Cambiar el tamaño del tiempo de espera o error

Cuando se crea un nuevo grupo o se cambia el tamaño de uno existente, se especifica el número de nodos de destino.  La operación finalizará inmediatamente, pero la asignación real de los nodos nuevos o la eliminación de nodos existentes se realiza en segundo plano en un plazo que puede llevar varios minutos.  Se especifica un tiempo de espera del cambio de tamaño en las API [create](https://docs.microsoft.com/rest/api/batchservice/pool/add) o [resize](https://docs.microsoft.com/rest/api/batchservice/pool/resize): si no se puede obtener el número de destino de nodos en el período de tiempo de espera del cambio de tamaño, se detendrá la operación y el grupo pasará a un estado estable con errores de cambio de tamaño.

La propiedad [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) notifica que se agotó el tiempo de espera del cambio de tamaño para la última evaluación, que enumera uno o varios errores que se produjeron.

Las causas comunes de los tiempos de espera del cambio de tamaño son:
- El tiempo de espera del cambio de tamaño es demasiado corto
  - El tiempo de espera predeterminado es 15 minutos, que es normalmente tiempo suficiente para asignar o quitar los nodos de grupo.
  - Cuando se asigna un gran número de nodos (más de 1000 nodos de una imagen de Marketplace o más de 300 desde una imagen personalizada) durante la creación o cambio de tamaño del grupo, se recomienda un tiempo de espera de 30 minutos.
- Cuota de núcleos insuficiente
  - Una cuenta de Batch tiene una cuota para el número de núcleos que se asignan a todos los grupos.  Batch no asignará nodos cuando se alcance esa cuota.  La cuota de núcleos [puede aumentarse](https://docs.microsoft.com/azure/batch/batch-quota-limit) para habilitar la asignación de más nodos.
- Direcciones IP de subred insuficientes cuando un [grupo está en una red virtual](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Una subred de red virtual debe tener suficientes direcciones IP sin asignar para asignárselas a todos los nodos de grupo que se solicitan, en caso contrario no se podrán crear los nodos.
- Recursos insuficientes cuando un [grupo está en una red virtual](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Los recursos como los equilibradores de carga, las direcciones IP públicas y los NSG se crean en la suscripción usada para crear la cuenta de Batch.  Las cuotas de suscripción para estos recursos deben ser suficientes.
- Usar una imagen de máquina virtual personalizada para grupos grandes
  - Los grupos grandes que usan imágenes personalizadas pueden tardar más tiempo en asignarse y pueden agotar el tiempo de espera para el cambio de tamaño.  Se proporcionan recomendaciones de límites y de configuración en un [artículo específico](https://docs.microsoft.com/azure/batch/batch-custom-images). 

### <a name="auto-scale-failures"></a>Errores de escalado automático

En lugar de establecer explícitamente el número de destino de nodos para un grupo al crear o cambiar el tamaño de un grupo, se puede escalar automáticamente el número de nodos de un grupo.  [Se puede crear una fórmula de escalado automático para un grupo](https://docs.microsoft.com/azure/batch/batch-automatic-scaling), que se evaluará en un intervalo periódico configurable para establecer el número de destino de nodos para el grupo.  Los siguientes tipos de problemas pueden producirse y detectarse:

- La evaluación de escalado automático puede producir un error.
- La operación de cambio de tamaño resultante puede producir un error y agotar el tiempo de espera.
- Puede haber un problema con la fórmula de escalado automático, dando lugar a valores de destino de nodo incorrectos, lo que llevará a que se agote el tiempo de espera para el cambio de tamaño en proceso.

La información acerca de la última evaluación de escalado automático se obtiene mediante la propiedad [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun), que informa sobre el momento de la evaluación, los valores y el resultado de la evaluación, además de los errores producidos al realizar la evaluación.

Un [evento completo de cambio de tamaño de grupo](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) captura automáticamente información acerca de todas las evaluaciones.

### <a name="delete"></a>Eliminar

Suponiendo que hay nodos en un grupo, una operación de eliminación de grupo lleva a que se eliminen primero los nodos y después el propio objeto de grupo.  Los nodos del grupo pueden tardar unos minutos en eliminar.

El [estado del grupo](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) se establecerá en “deleting” durante el proceso de eliminación.  La aplicación que realiza la llamada puede detectar si la eliminación del grupo está tardando demasiado tiempo mediante las propiedades “state” y “stateTransitionTime”.

## <a name="pool-compute-node-errors"></a>Errores de nodo de proceso de grupo

Los nodos pueden asignarse correctamente en un grupo, pero varios problemas pueden llevar a que los nodos estén en mal estado y no puedan utilizarse.  Una vez que se asignan los nodos en un grupo, incurren en gastos y, por tanto, es importante detectar problemas para evitar pagar por los nodos que no se pueden usar.

### <a name="start-task-failure"></a>Error de la tarea de inicio

Puede especificarse una [tarea de inicio](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) opcional para un grupo.  Al igual que con cualquier tarea, se pueden especificar una línea de comandos y archivos de recursos para descargar desde el almacenamiento.  La tarea de inicio se especifica para el grupo, pero se ejecuta para cada nodo: una vez que se inició cada nodo, se ejecuta la tarea de inicio.  Una propiedad adicional de la [tarea de inicio](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask), “waitForSuccess”, especifica si el lote debe esperar a que la tarea de inicio se complete correctamente antes de programar las tareas de un nodo.

Si se produce un error en una tarea de inicio y la configuración de la tarea de inicio especifica que se debe esperar a la finalización correcta, no se podrá usar el nodo y se seguirán generando cargos.

Se pueden detectar errores de tarea de inicio mediante las propiedades [result](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) y [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) de la propiedad de nodo [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) de nivel superior.

Además, una tarea de inicio con error lleva a que el [estado](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) del nodo se establezca en “starttaskfailed”, pero solo si se ha establecido “waitForSuccess” en “true”.

Al igual que con cualquier tarea, puede haber varias causas para que se produzcan errores en la tarea de inicio.  Para solucionar problemas, deben comprobarse los archivos de registro stdout, stderr y cualquier archivo de registro adicional específico de la tarea.

### <a name="application-package-download-failure"></a>Error al descargar el paquete de aplicación

Uno o varios paquetes de aplicación pueden especificarse opcionalmente para un grupo, y se descargarán los archivos de paquete especificado en cada nodo sin comprimir después de que se inicie el nodo, pero antes de que se programan las tareas.  Es habitual usar una línea de comandos de tarea de inicio, junto con paquetes de aplicación, para copiar archivos en una ubicación diferente o ejecutar el programa de instalación, por ejemplo.

La propiedad [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) del nodo notificará un error al descargar y descomprimir un paquete de aplicación.  El estado del nodo se establecerá en “unusable”.

### <a name="node-in-unusable-state"></a>Nodo en estado unusable

El [estado del nodo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) puede establecerse en “unusable” por diversos motivos.  Cuando está “unusable”, no se pueden programar tareas en el nodo, pero el nodo seguirá generando cargos.

Batch siempre intentará recuperar nodos inutilizables, pero la recuperación puede o no ser posible, dependiendo de la causa.

En los casos en los que se pueda determinar la causa, la propiedad [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) la notificará.

Algunos otros ejemplos de las causas de los nodos “unusable”:

- Imagen personalizada no válida; porque no se ha preparado correctamente, por ejemplo.
- Error de infraestructura o actualización de bajo nivel que lleva a que la máquina virtual subyacente se mueva; Batch recuperará el nodo.

### <a name="node-agent-log-files"></a>Archivos de registro del agente de nodo

Si es necesario ponerse en contacto con el soporte técnico debido a un problema de nodo de grupo, se pueden obtener los archivos de registro del proceso de agente de Batch que se ejecuta en cada nodo de grupo.  Se pueden cargar los archivos de registro para un nodo a través de Azure Portal, Batch Explorer o una [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs).  Cargar y guardar los archivos de registro puede ser muy útil ya que el nodo o grupo se puede eliminar para reducir el costo de los nodos en ejecución.

## <a name="next-steps"></a>Pasos siguientes

Asegúrese de que la aplicación ha implementado la comprobación de errores completa, especialmente para las operaciones asincrónicas, para que se puedan detectar y diagnosticar problemas rápidamente.
