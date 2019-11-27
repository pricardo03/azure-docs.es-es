---
title: Grupos de contenedores de Azure Container Instances
description: Descripción del funcionamiento de los grupos de varios contenedores en Azure Container Instances
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 11/01/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a785ecbfa09c54d3affa97c220d4808f9fe8d90b
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904445"
---
# <a name="container-groups-in-azure-container-instances"></a>Grupos de contenedores en Azure Container Instances

El recurso de nivel superior de Azure Container Instances es el *grupo de contenedores*. Este artículo describe qué son los grupos de contenedores y qué tipos de escenarios permiten.

## <a name="how-a-container-group-works"></a>Cómo funciona un grupo de contenedores

Un grupo de contenedores es una colección de contenedores que se programan en la misma máquina host. Los contenedores de un grupo comparten un ciclo de vida, los recursos, la red local y los volúmenes de almacenamiento. Es similar al concepto de *pod* en [Kubernetes][kubernetes-pod].

El siguiente diagrama muestra un ejemplo de un grupo de contenedores que incluye varios contenedores:

![Diagrama de grupos de contenedores][container-groups-example]

Este grupo de contenedores de ejemplo:

* Se programó en una única máquina host.
* Se le asigna una etiqueta de nombre DNS.
* Expone una única dirección IP pública, con un puerto expuesto.
* Consta de dos contenedores. Un contenedor escucha en el puerto 80, mientras el otro escucha en el puerto 5000.
* Incluye dos recursos compartidos de archivos de Azure como montajes de volumen y cada contenedor monta uno de los recursos compartidos de forma local.

> [!NOTE]
> Los grupos de varios contenedores solo admiten actualmente contenedores Linux. Para los contenedores Windows, Azure Container Instances solo admite la implementación de una única instancia. Aunque estamos trabajando para traer todas las características a los contenedores Windows, puede encontrar las diferencias actuales de la plataforma en [Información general](container-instances-overview.md#linux-and-windows-containers).

## <a name="deployment"></a>Implementación

Estas son dos formas habituales de implementar un grupo de varios contenedores: use una [plantilla de Resource Manager][resource-manager template] o un [archivo YAML][yaml-file]. Se recomienda usar una plantilla de Resource Manager cuando se necesite implementar recursos adicionales de un servicio de Azure (por ejemplo, un [recurso compartido de Azure Files][azure-files]) al implementar instancias de contenedor. Dada la naturaleza más concisa del formato YAML, se recomienda usar un archivo YAML cuando la implementación incluya solo instancias de contenedor. Para más información sobre las propiedades que se pueden establecer, consulte la documentación de [referencia de la plantilla de Resource Manager](/azure/templates/microsoft.containerinstance/containergroups) o de [referencia de YAML](container-instances-reference-yaml.md).

Para conservar la configuración de un grupo de contenedores, puede exportar la configuración a un archivo YAML mediante el comando de la CLI de Azure [az container export][az-container-export]. La exportación permite almacenar las configuraciones de grupos de contenedores en el control de versiones para la "configuración como código". O bien, use el archivo exportado como punto de partida al desarrollar una nueva configuración en YAML.



## <a name="resource-allocation"></a>Asignación de recursos

Azure Container Instances asigna recursos como CPU, memoria y, opcionalmente, [GPU][gpus] (versión preliminar) a un grupo de contenedores mediante la adición de las [solicitudes de recursos][resource-requests] de las instancias del grupo. Por ejemplo, cuando se toman recursos de CPU y se crea un grupo de contenedores con dos instancias, y cada una de ellas solicita 1 CPU, se asignan 2 CPU al grupo de contenedores.

### <a name="resource-usage-by-instances"></a>Uso de recursos por instancias

A cada instancia de contenedor se le asignan los recursos especificados en su solicitud de recursos. Aunque el uso de recursos por parte de una instancia de contenedor de un grupo depende de cómo se configure su propiedad opcional [límite de recursos][resource-limits].

* Si no especifica un límite de recursos, el uso máximo de recursos de la instancia es el mismo que el de su solicitud de recursos.

* Si especifica un límite de recursos para una instancia, puede ajustar el uso de recursos de la instancia para su carga de trabajo, ya sea reduciendo o aumentando el uso en relación con la solicitud de recursos. El límite máximo de recursos que puede establecer es el total de recursos asignados al grupo.
    
    Por ejemplo, en un grupo con dos instancias que solicitan 1 CPU, uno de los contenedores podría ejecutar una carga de trabajo que requiera mayor número de CPU en ejecución que el otro.

    En este escenario, puede establecer un límite de recursos de 0,5 CPU para una instancia y un límite de 2 CPU para el segundo. Esta configuración limita el uso de recursos del primer contenedor a 0,5 CPU, lo que permite que el segundo contenedor pueda usar las 2 CPU completas, si están disponibles.

Para obtener más información, consulte la propiedad [ResourceRequirements][resource-requirements] en la API de REST de grupos de contenedores.

### <a name="minimum-and-maximum-allocation"></a>Asignación mínima y máxima

* Asigne un **mínimo** de 1 CPU y 1 GB de memoria a un grupo de contenedores. Las instancias de contenedores individuales dentro de un grupo pueden aprovisionarse con menos de 1 CPU y 1 GB de memoria. 

* Para los recursos **máximos** de un grupo de contenedores, consulte la [disponibilidad de recursos][region-availability] para Azure Container Instances en la región de implementación.

## <a name="networking"></a>Redes

Los grupos de contenedores comparten una dirección IP y un espacio de nombres de puerto en esa dirección IP. Para permitir que los clientes externos lleguen a un contenedor dentro del grupo, debe exponer el puerto en la dirección IP y desde el contenedor. Dado que los contenedores dentro del grupo comparten un espacio de nombres de puerto, no se admite la asignación de puertos. Los contenedores dentro de un grupo pueden comunicarse entre sí a través de localhost en los puertos que han expuesto, incluso si estos puertos no se exponen externamente en la dirección IP del grupo.

Opcionalmente, implemente grupos de contenedores en una [red virtual de Azure][virtual-network] (versión preliminar) para permitir que los contenedores se comuniquen de forma segura con otros recursos de la red virtual.

## <a name="storage"></a>Storage

Puede especificar volúmenes externos para montar dentro de un grupo de contenedores. Puede asignar los volúmenes en rutas de acceso específicas dentro de los contenedores individuales en un grupo.

## <a name="common-scenarios"></a>Escenarios comunes

Los grupos de varios contenedores son útiles en casos donde quiera dividir una única tarea funcional en un número pequeño de imágenes de contenedor. Luego, estas imágenes las pueden entregar diferentes equipos y pueden tener diversos requisitos de recursos.

Ejemplos posibles de uso serían:

* Un contenedor para servir una aplicación web y un contenedor para extraer el contenido más reciente desde el control de código fuente.
* Un contenedor de aplicación y un contenedor de registro. El contenedor de registro recopila la salida de registros y métricas de la aplicación principal y los escribe en un almacenamiento a largo plazo.
* Un contenedor de aplicación y un contenedor de supervisión. Periódicamente, el contenedor de supervisión realiza una solicitud a la aplicación para asegurarse de que se está ejecutando y responde correctamente, y genera una alerta si no es así.
* Un contenedor de front-end y un contenedor de back-end. El front-end puede servir una aplicación web y el back-end ejecutar un servicio para recuperar datos. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de cómo implementar un grupo de múltiples contenedores con una plantilla de Azure Resource Manager:

> [!div class="nextstepaction"]
> [Implementación de un grupo de contenedores][resource-manager template]

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/

<!-- LINKS - Internal -->
[resource-manager template]: container-instances-multi-container-group.md
[yaml-file]: container-instances-multi-container-yaml.md
[region-availability]: container-instances-region-availability.md
[resource-requests]: /rest/api/container-instances/containergroups/createorupdate#resourcerequests
[resource-limits]: /rest/api/container-instances/containergroups/createorupdate#resourcelimits
[resource-requirements]: /rest/api/container-instances/containergroups/createorupdate#resourcerequirements
[azure-files]: container-instances-volume-azure-files.md
[virtual-network]: container-instances-vnet.md
[gpus]: container-instances-gpu.md
[az-container-export]: /cli/azure/container#az-container-export
