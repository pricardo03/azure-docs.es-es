---
title: Configuración de sondeos de ejecución en Azure Container Instances
description: Información sobre cómo configurar los sondeos de ejecución para reiniciar contenedores incorrectos en Azure Container Instances
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: 89b76fc68c113b7931894c0cf003ffd846c646ab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60583846"
---
# <a name="configure-liveness-probes"></a>Configuración de sondeos de ejecución

Las aplicaciones en contenedores pueden ejecutarse durante largos períodos de tiempo y provocar la interrupción de estados que quizás deban repararse con un reinicio del contenedor. Azure Container Instances admite que los sondeos de ejecución incluyan configuraciones para que pueda reiniciar el contenedor si una funcionalidad crítica no funciona.

En este artículo se explica cómo implementar un grupo de contenedores que incluya un sondeo de ejecución y se muestra el reinicio automático de un contenedor incorrecto simulado.

## <a name="yaml-deployment"></a>Implementación de YAML

Cree un archivo `liveness-probe.yaml` con el siguiente fragmento de código. Este archivo define un grupo de contenedores que consta de un contenedor NGNIX que, con el tiempo, se vuelve incorrecto.

```yaml
apiVersion: 2018-06-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Ejecute el siguiente comando para implementar este grupo de contenedores con la configuración de YAML anterior:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Comando de inicio

La implementación define un comando de inicio que se ejecutará cuando el contenedor empiece a funcionar por primera vez, definido por la propiedad `command` que acepta una matriz de cadenas. En este ejemplo, iniciará una sesión de Bash y creará un archivo denominado `healthy` dentro del directorio `/tmp` al pasar este comando:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 A continuación, se suspenderá durante 30 segundos antes de eliminar el archivo y entrará en modo de suspensión durante 10 minutos.

### <a name="liveness-command"></a>Comando de ejecución

Esta implementación define un `livenessProbe` que admite un comando de ejecución `exec` que actúa como comprobación de la ejecución. Si este comando finaliza con un valor distinto de cero, el contenedor se eliminará y se reiniciará para indicar que el archivo `healthy` no se encontró. Si este comando finaliza correctamente con el código de salida 0, no habrá que realizar ninguna acción.

La propiedad `periodSeconds` designa que el comando de ejecución debería ejecutarse cada 5 segundos.

## <a name="verify-liveness-output"></a>Comprobación de la salida de ejecución

Durante los primeros 30 segundos, el archivo `healthy` creado por el comando de inicio existe. Cuando el comando de ejecución comprueba la existencia del archivo `healthy`, el código de estado devuelve un cero, que indica que la operación se realizó correctamente y que no se producirá ningún reinicio.

Después de 30 segundos, el `cat /tmp/healthy` comenzará a generar errores, lo que dará lugar a eventos incorrectos y provocará su finalización.

Estos eventos se pueden ver desde Azure Portal o la CLI de Azure.

![Evento incorrecto del portal:][portal-unhealthy]

Al ver los eventos en Azure Portal, los eventos de tipo `Unhealthy` se desencadenarán al producirse un error del comando de ejecución. El evento posterior será del tipo `Killing`, lo que significa la eliminación de un contenedor y que puede comenzar un reinicio. El recuento de reinicio del contenedor incrementará cada vez que esto suceda.

Los reinicios se completan en contexto para recursos como las direcciones IP públicas y se conservará el contenido específico de nodo.

![Contador de reinicios del portal][portal-restart]

Si el sondeo de ejecución sigue generando errores y desencadena demasiados reinicios, el contenedor provocará un retraso de retroceso exponencial.

## <a name="liveness-probes-and-restart-policies"></a>Sondeos de ejecución y directivas de reinicio

Las directivas de reinicio reemplazan el comportamiento de reinicio que desencadenan los sondeos de ejecución. Por ejemplo, si establece una directiva `restartPolicy = Never` *y* un sondeo de ejecución, el grupo de contenedores no se reiniciará si se produce una comprobación de ejecución errónea. En su lugar, el grupo de contenedores se adherirá a la directiva de reinicio del grupo de contenedores de `Never`.

## <a name="next-steps"></a>Pasos siguientes

Los escenarios basados en tareas pueden requerir un sondeo de ejecución para habilitar los reinicios automáticos si una función de requisito previo no funciona correctamente. Para más información sobre cómo ejecutar contenedores basados en tareas, consulte [Ejecución de tareas en contenedores en Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
