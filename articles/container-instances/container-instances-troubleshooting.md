---
title: Solución de problemas de Azure Container Instances
description: Obtenga información acerca de la solución de problemas relacionados con Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 550b53cf40133c8a67306c61cbfa7dae21be4648
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163778"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Solución de problemas habituales de Azure Container Instances

En este artículo se muestra cómo solucionar problemas habituales al administrar o implementar contenedores en Azure Container Instances.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Al definir la especificación de contenedor, ciertos parámetros requieren tener en cuenta las restricciones de nomenclatura. A continuación se muestra una tabla con requisitos específicos para las propiedades del grupo de contenedores. Para más información sobre las convenciones de nomenclatura de Azure, consulte [Convenciones de nomenclatura][azure-name-restrictions] en el Centro de Arquitectura de Azure.

| Ámbito | Length | Uso de mayúsculas y minúsculas | Caracteres válidos | Patrón sugerido | Ejemplo |
| --- | --- | --- | --- | --- | --- | --- |
| Nombre del grupo de contenedores | 1-64 |No distingue mayúsculas de minúsculas |Caracteres alfanuméricos y guion en cualquier lugar, excepto el primer o último carácter |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Nombre del contenedor | 1-64 |No distingue mayúsculas de minúsculas |Caracteres alfanuméricos y guion en cualquier lugar, excepto el primer o último carácter |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Puertos del contenedor | Entre 1 y 65 535 |Entero |Un número entero comprendido entre 1 y 65 535 |`<port-number>` |`443` |
| Etiqueta de nombre DNS | 5-63 |No distingue mayúsculas de minúsculas |Caracteres alfanuméricos y guion en cualquier lugar, excepto el primer o último carácter |`<name>` |`frontend-site1` |
| Variable de entorno | 1-63 |No distingue mayúsculas de minúsculas |Caracteres alfanuméricos y guion bajo (_) en cualquier lugar, excepto el primer o último carácter |`<name>` |`MY_VARIABLE` |
| Nombre del volumen | 5-63 |No distingue mayúsculas de minúsculas |Letras minúsculas, números y guiones en cualquier lugar, excepto el primer o último carácter. No puede contener dos guiones consecutivos. |`<name>` |`batch-output-volume` |

## <a name="os-version-of-image-not-supported"></a>Versión del sistema operativo de imagen no admitida

Si se especifica una imagen que Azure Container Instances no admite, se devuelve un error `OsVersionNotSupported`. El error es similar a la siguiente, donde `{0}` es el nombre de la imagen que intentó implementar:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Este error se suele encontrar con más frecuencia al implementar imágenes de Windows basadas en una versión de canal semianual (SAC). Por ejemplo, las versiones de Windows 1709 y 1803 son versiones SAC y generan este error tras la implementación.

Azure Container Instances admite imágenes de Windows basadas únicamente en versiones de canal de servicio a largo plazo (LTSC). Para mitigar este problema al implementar contenedores de Windows, implemente siempre imágenes basadas en LTSC.

Para más información sobre las versiones LTSC y SAC de Windows, consulte [Introducción al Canal semianual de WindowsServer][windows-sac-overview].

## <a name="unable-to-pull-image"></a>No se puede extraer la imagen

Si inicialmente Azure Container Instances no puede extraer su imagen, lo reintenta durante un período de tiempo. Si la operación de extracción de imágenes sigue generando errores, ACI acaba por no realizar la implementación y se muestra un error `Failed to pull image`.

Para resolver este problema, elimine la instancia de contenedor y vuelva a intentar la implementación. Asegúrese de que existe la imagen en el registro y de que ha escrito correctamente el nombre de imagen.

Si no se puede extraer la imagen, se muestran eventos similares al siguiente en la salida de [az container show][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

## <a name="container-continually-exits-and-restarts"></a>El contenedor continuamente se cierra y se reinicia

Si el contenedor se ejecuta por completo y se reinicia automáticamente, tendrá que establecer la [directiva de reinicio](container-instances-restart-policy.md) **OnFailure** o **Never**. Si especifica **OnFailure** y sigue observando reinicios continuos, podría haber un problema con la aplicación o el script que se ejecuta en el contenedor.

La API de Container Instances incluye una propiedad `restartCount`. Para comprobar el número de reinicios de un contenedor, puede usar el comando [az container show][az-container-show] de la CLI de Azure. En la siguiente salida de ejemplo (que se ha truncado por razones de brevedad), puede ver la propiedad `restartCount` al final de la salida.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> La mayoría de las imágenes de contenedor para las distribuciones de Linux establecen una shell, como bash, como el comando predeterminado. Puesto que un shell por sí mismo no es un servicio de ejecución prolongada, estos contenedores se cierran inmediatamente y caen en un bucle de reinicio cuando se configuran con la directiva de reinicio predeterminada **Always**.

## <a name="container-takes-a-long-time-to-start"></a>El contenedor tarda mucho tiempo en iniciar

Los dos factores principales que contribuyen al tiempo de inicio del contenedor en Azure Container Instances son:

* [Tamaño de las imágenes](#image-size)
* [Ubicación de las imágenes](#image-location)

Las imágenes de Windows tienen [consideraciones adicionales](#cached-windows-images).

### <a name="image-size"></a>Tamaño de las imágenes

Si el contenedor tarda mucho tiempo en iniciar, pero al final se realiza correctamente, comience por mirar el tamaño de la imagen del contenedor. Como Azure Container Instances extrae la imagen del contenedor a petición, el tiempo de inicio está directamente relacionado con su tamaño.

Puede ver el tamaño de la imagen del contenedor mediante el comando `docker images` en la CLI de Docker:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

La clave para mantener tamaños de imagen pequeños es asegurarse de que la imagen final no contiene nada que no sea necesario en tiempo de ejecución. Una manera de hacerlo es con [compilaciones en varias etapas][docker-multi-stage-builds]. Las compilaciones en varias etapas hacen más sencillo asegurarse de que la imagen final contiene solo los artefactos necesarios para la aplicación y no los del contenido extra que se requiere en tiempo de compilación.

### <a name="image-location"></a>Ubicación de las imágenes

Otra forma de reducir el impacto de la extracción de la imagen en el tiempo de inicio del contenedor es hospedar la imagen del contenedor con [Azure Container Registry](/azure/container-registry/) en la misma región en la que van a implementar las instancias de contenedor. Esto reduce la ruta de acceso de red que debe recorrer la imagen del contenedor, lo que reduce significativamente el tiempo de descarga.

### <a name="cached-windows-images"></a>Almacenado en caché de imágenes de Windows

Azure Container Instances usa un mecanismo de almacenamiento en caché para acelerar el tiempo de inicio del contenedor para las imágenes basadas en determinadas imágenes de Windows.

Para garantizar el menor tiempo de inicio de contenedor de Windows, utilice una de las **tres versiones más recientes** de las siguientes **dos imágenes** como imagen base:

* [Windows Server 2016][docker-hub-windows-core] (solo LTS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

### <a name="windows-containers-slow-network-readiness"></a>Preparación para la red lenta de contenedores de Windows

Los contenedores de Windows no pueden mantener conexiones entrantes o salientes hasta un máximo de 5 segundos durante la creación inicial. Después de la instalación inicial, las redes de contenedores se deben reanudar adecuadamente.

## <a name="resource-not-available-error"></a>Error de recurso no disponible

Debido a la carga variable de recursos regionales en Azure, podría recibir el siguiente error al intentar implementar una instancia del contenedor:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Este error indica que, debido a una carga elevada en la región en la que está intentando la implementación, no se pueden asignar los recursos especificados para el contenedor en ese momento. Utilice uno o varios de los siguientes pasos de mitigación para ayudar a resolver el problema.

* Compruebe que la configuración de implementación del contenedor se encuentra dentro de los parámetros definidos en [Quotas and region availability for Azure Container Instances](container-instances-quotas.md#region-availability) (Disponibilidad de cuotas y regiones en instancias de Azure Container).
* Especifique la configuración de CPU y memoria más baja para el contenedor
* Implemente en una región distinta de Azure
* Implemente en un momento posterior

## <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>No se puede conectar a la API de Docker subyacente ni ejecutar contenedores con privilegios

Azure Container Instances no expone acceso directo a la infraestructura subyacente que hospeda los grupos de contenedores. Esto incluye el acceso a la API de Docker que se ejecuta en el host del contenedor y la ejecución de contenedores con privilegios elevados. Si se requiere la interacción de Docker, compruebe la [documentación de referencia de REST](https://aka.ms/aci/rest) para ver lo que admite la API de ACI. Si falta algo, envíe una solicitud en los [foro de comentarios de ACI](https://aka.ms/aci/feedback).

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [recuperar eventos y registros de contenedor](container-instances-get-logs.md) para ayudar a depurar los contenedores.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az_container_show
