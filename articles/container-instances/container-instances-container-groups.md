---
title: Grupos de contenedores de Azure Container Instances
description: Comprender los grupos de varios contenedores cómo funcionan en Azure Container Instances
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/20/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8724bd7e13b0d8607ad5a6814b27c8c06681f331
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58202017"
---
# <a name="container-groups-in-azure-container-instances"></a>Grupos de contenedores en Azure Container Instances

El recurso de nivel superior de Azure Container Instances es el *grupo de contenedores*. Este artículo describe qué son los grupos de contenedores y qué tipos de escenarios permiten.

## <a name="how-a-container-group-works"></a>Cómo funciona un grupo de contenedores

Un grupo de contenedores es una colección de contenedores que se programan en la misma máquina host. Los contenedores en un grupo de contenedores comparten un ciclo de vida, los recursos, red local y volúmenes de almacenamiento. Es similar en concepto a un *pod* en [Kubernetes][kubernetes-pod].

El siguiente diagrama muestra un ejemplo de un grupo de contenedores que incluye varios contenedores:

![Diagrama de grupos de contenedores][container-groups-example]

Este grupo de contenedores de ejemplo:

* Se programó en una única máquina host.
* Se le asigna una etiqueta de nombre DNS.
* Expone una única dirección IP pública, con un puerto expuesto.
* Consta de dos contenedores. Un contenedor escucha en el puerto 80, mientras el otro escucha en el puerto 5000.
* Incluye dos recursos compartidos de archivos de Azure como montajes de volumen y cada contenedor monta uno de los recursos compartidos de forma local.

> [!NOTE]
> Grupos de varios contenedores actualmente admiten solo los contenedores de Linux. Para los contenedores de Windows, Azure Container Instances solo admite la implementación de una sola instancia. Aunque estamos trabajando para traer todas las características a los contenedores de Windows, puede encontrar diferencias en el servicio de la actual plataforma [Introducción](container-instances-overview.md#linux-and-windows-containers).

## <a name="deployment"></a>Implementación

Estos son dos formas habituales de implementar un grupo de varios contenedor: usar un [plantilla de Resource Manager] [ resource-manager template] o un [archivo YAML][yaml-file]. Usar una plantilla de Resource Manager cuando se necesita implementar los recursos del servicio de Azure adicionales (por ejemplo, un [compartir archivos de Azure][azure-files]) en el momento en implementa las instancias de contenedor. Debido a la naturaleza de más concisa del formato YAML, un archivo YAML se recomienda cuando la implementación incluye solo las instancias de contenedor.

## <a name="resource-allocation"></a>Asignación de recursos

Azure Container Instances asigna recursos como CPU, memoria y, opcionalmente, [GPU] [ gpus] (versión preliminar) para un grupo de contenedores mediante la adición de la [las solicitudes de recursos] [ resource-requests] de las instancias en el grupo. Tomando los recursos de CPU como por ejemplo, si crea un grupo de contenedores con dos instancias, cada solicitud 1 CPU, a continuación, en el grupo de contenedores se asigna 2 CPU.

Los máximos recursos disponibles para un grupo de contenedores varían en función del [región de Azure] [ region-availability] usada para la implementación.

### <a name="container-resource-requests-and-limits"></a>Límites y las solicitudes de recursos de contenedor

* De forma predeterminada, las instancias de contenedor en un grupo comparten los recursos solicitados del grupo. En un grupo con dos instancias de cada solicitante 1 CPU, el grupo como todo tiene acceso a 2 CPU. Cada instancia puede utilizar hasta el 2 CPU y las instancias pueden competir por recursos de CPU mientras se están ejecutando.

* Para limitar el uso de recursos por una instancia de un grupo, establezca, opcionalmente, un [límite de recursos] [ resource-limits] para la instancia. En un grupo con dos instancias solicitar 1 CPU, uno de los contenedores puede requerir más CPU en ejecutarse que el otro.

  En este escenario, puede establecer un límite de recursos de 0,5 CPU para una instancia y un límite de 2 CPU para el segundo. Esta configuración limita el uso de recursos del primer contenedor en 0,5 CPU, lo que permite el segundo contenedor que puede usar la completa 2 CPU si está disponible.

Para obtener más información, consulte el [ResourceRequirements] [ resource-requirements] propiedad en el contenedor de grupos de API de REST.

### <a name="minimum-and-maximum-allocation"></a>Como mínimo y máximo asignación

* Asignar un **mínimo** 1 CPU y 1 GB de memoria para un grupo de contenedores. Las instancias de contenedor individual dentro de un grupo se pueden aprovisionar con menor que 1 CPU y 1 GB de memoria. 

* Para el **máximo** recursos en un grupo de contenedores, la disponibilidad, consulte [aci-región-availability] para Azure Container Instances en la región de implementación.

## <a name="networking"></a>Redes

Los grupos de contenedores comparten una dirección IP y un espacio de nombres de puerto en esa dirección IP. Para permitir que los clientes externos lleguen a un contenedor dentro del grupo, debe exponer el puerto en la dirección IP y desde el contenedor. Dado que los contenedores dentro del grupo comparten un espacio de nombres de puerto, no se admite la asignación de puertos. Los contenedores dentro de un grupo pueden comunicar entre sí a través de localhost en los puertos que han expuesto, incluso si estos puertos no se exponen externamente en la dirección IP del grupo.

Opcionalmente, implementar grupos de contenedores en un [red virtual de Azure] [ virtual-network] (versión preliminar) para permitir que los contenedores para comunicarse de forma segura con otros recursos de la red virtual.

## <a name="storage"></a>Almacenamiento

Puede especificar volúmenes externos para montar dentro de un grupo de contenedores. Puede asignar los volúmenes en rutas de acceso específicas dentro de los contenedores individuales en un grupo.

## <a name="common-scenarios"></a>Escenarios comunes

Los grupos de varios contenedores son útiles en casos donde quiera dividir una única tarea funcional en un número pequeño de imágenes de contenedor. Luego, estas imágenes las pueden entregar diferentes equipos y pueden tener diversos requisitos de recursos.

Ejemplos posibles de uso serían:

* Un contenedor para servir una aplicación web y un contenedor para extraer el contenido más reciente desde el control de código fuente.
* Un contenedor de aplicación y un contenedor de registro. El contenedor de registro recopila la salida de registros y métricas de la aplicación principal y los escribe en un almacenamiento a largo plazo.
* Un contenedor de aplicación y un contenedor de supervisión. Periódicamente, el contenedor de supervisión realiza una solicitud a la aplicación para asegurarse de que se está ejecutando y responde correctamente, y genera una alerta si no es así.
* Un contenedor de front-end y un contenedor de back-end. El front-end puede servir una aplicación web, con el back-end que ejecutan un servicio para recuperar datos. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de cómo implementar un grupo de múltiples contenedores con una plantilla de Azure Resource Manager:

> [!div class="nextstepaction"]
> [Implementar un grupo de contenedores][resource-manager template]

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
