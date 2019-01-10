---
title: Preguntas comunes sobre Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información sobre las preguntas más frecuentes y respuestas sobre Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 12/12/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 2f9800deedb2e8d55362920fed2493a42290e764
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555853"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Preguntas frecuentes sobre Service Fabric Mesh

Azure Service Fabric Mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin necesidad de administrar máquinas virtuales, almacenamiento o redes. Este artículo contiene respuestas a las preguntas más habituales.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>¿Cómo notifico un problema o formulo una pregunta?

Formule preguntas, obtenga respuestas de los ingenieros de Microsoft e informe de problemas en el [repositorio de GitHub de service-fabric-mesh-preview](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Cuota y costo

### <a name="what-is-the-cost-of-participating-in-the-preview"></a>¿Cuál es el costo de participar en la versión preliminar?

No hay ningún cargo por implementar aplicaciones o contenedores actualmente en la versión preliminar de Mesh. Sin embargo, le recomendamos que elimine los recursos que implemente y que no los deje ejecutándose, a menos que los esté probando activamente.

### <a name="is-there-a-quota-limit-of-the-number-of-cores-and-ram"></a>¿Hay un límite de cuota del número de núcleos y RAM?

Sí. Las cuotas para cada suscripción son:

- Número de aplicaciones: 5
- Núcleos por aplicación: 12
- Total de memoria RAM por aplicación: 48 GB
- Puntos de conexión de red y de entrada: 5
- Volúmenes de Azure que puede adjuntar: 10
- Número de réplicas de servicio: 3
- El contenedor más grande que puede implementar está limitado a 4 núcleos y 16 GB de RAM.
- Puede asignar núcleos parciales a los contenedores en incrementos de 0,5 núcleos hasta un máximo de 6 núcleos.

### <a name="how-long-can-i-leave-my-application-deployed"></a>¿Durante cuánto tiempo puedo dejar mi aplicación implementada?

Actualmente, limitamos la duración de una aplicación a dos días. El objetivo es maximizar el uso de los núcleos libres asignados a la versión preliminar. Como resultado, solo se permite ejecutar una implementación determinada continuamente durante 48 horas, tiempo tras el cual se apagará.

Si se diera el caso, puede validar que el sistema la apague mediante la ejecución de un comando `az mesh app show` en la CLI de Azure. Compruebe si se devuelve `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."`. 

Por ejemplo:  

```cli
~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Para eliminar el grupo de recursos, use el comando `az group delete <nameOfResourceGroup>`.

## <a name="supported-container-os-images"></a>Imágenes del sistema operativo de contenedor compatibles

Si va a desarrollar en una máquina con Windows Fall Creators Update (versión 1709), solo puede usar las imágenes de Docker de la versión 1709 de Windows.

Si va a desarrollar en un equipo con la Actualización de abril de 2018 de Windows 10 (versión 1803), puede usar las imágenes de Docker de las versiones 1709 o 1803 de Windows.

Las siguientes imágenes del sistema operativo de contenedor pueden usarse para implementar servicios:

- Windows: windowsservercore y nanoserver
    - Windows Server versión 1709
    - Windows Server versión 1803
- Linux
    - Ninguna limitación conocida

## <a name="developer-experience-issues"></a>Problemas de experiencia para el desarrollador

### <a name="dns-resolution-from-an-outbound-container-doesnt-work"></a>La resolución de DNS desde un contenedor de salida no funciona

La comunicación de servicio a servicio puede producir errores en determinadas circunstancias. Se está investigando este problema. Para mitigarlo:

- Use Windows Fall Creators Update (versión 1709 o superior) como imagen del contenedor base.
- Si el nombre del servicio por sí solo no funciona, pruebe el nombre completo: ServiceName.ApplicationName.
- En el archivo de Docker para el servicio, agregue `EXPOSE <port>`, donde port es el puerto en el que expone el servicio. Por ejemplo: 

```DockerFile
EXPOSE 80
```

### <a name="dns-does-not-work-the-same-as-it-does-for-service-fabric-development-clusters-and-in-mesh"></a>El DNS no funciona del mismo modo que para los clústeres de desarrollo de Service Fabric y en Mesh

Es posible que deba hacer referencia a servicios de manera diferente en el clúster de desarrollo local y en Azure Mesh.

En el clúster de desarrollo local use `{serviceName}.{applicationName}`. En Azure Service Fabric Mesh use `{servicename}`. 

Azure Mesh no admite actualmente la resolución de DNS en todas las aplicaciones.

Para ver otros problemas conocidos de DNS con la ejecución de un clúster de desarrollo de Service Fabric en Windows 10, consulte: [Depuración de contenedores de Windows](/azure/service-fabric/service-fabric-how-to-debug-windows-containers) y [Problemas conocidos de DNS](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#known-issues).

### <a name="networking"></a>Redes

Es posible que la NAT de red de Service Fabric desaparezca al usar la ejecución de la aplicación en la máquina local. Para diagnosticar si se ha producido esto, ejecute el siguiente comando desde un símbolo del sistema:

`docker network ls` y tenga en cuenta si aparece `servicefabric_nat`.  En caso contrario, ejecute el siguiente comando: `docker network create -d=nat --subnet 10.128.0.0/24 --gateway 10.128.0.1 servicefabric_nat`.

De esta forma, se abordará el problema aunque la aplicación ya se haya implementado localmente y esté en un estado incorrecto.

### <a name="issues-running-multiple-apps"></a>Problemas al ejecutar varias aplicaciones

Podría resultar que los límites y la disponibilidad de la CPU se hayan resuelto en todas las aplicaciones. Para mitigar este problema:
- Cree un clúster de cinco nodos.
- Reduzca el uso de CPU en los servicios de la aplicación que se haya implementado. Por ejemplo, en el archivo service.yaml del servicio, cambie `cpu: 1.0` a `cpu: 0.5`.

No se pueden implementar varias aplicaciones en un clúster de un nodo. Para mitigar este problema:
- Use un clúster de cinco nodos al implementar varias aplicaciones en un clúster local.
- Quite las aplicaciones que no se estén probando actualmente.

## <a name="feature-gaps-and-other-known-issues"></a>Lagunas de características y otros problemas conocidos

### <a name="after-deploying-my-application-the-network-resource-associated-with-it-does-not-have-an-ip-address"></a>Después de implementar mi aplicación, el recurso de red asociado con ella no tiene una dirección IP

Hay un problema conocido según el que la dirección IP no está disponible inmediatamente. Compruebe el estado del recurso de red en unos minutos para ver la dirección IP asociada.

### <a name="my-application-fails-to-access-the-right-networkvolume-resource"></a>Mi aplicación no puede tener acceso al recurso de red o por volumen correcto

En el modelo de aplicación, use el identificador de recurso completo para que las redes y volúmenes puedan tener acceso al recurso asociado. A continuación, se muestra un ejemplo del inicio rápido:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

### <a name="when-i-scale-out-all-of-my-containers-are-affected-including-running-ones"></a>Al escalar horizontalmente, todos mis contenedores están afectados, incluidos los que se están ejecutando

Se trata de un error y se está implementando una corrección.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Service Fabric Mesh, lea la [introducción](service-fabric-mesh-overview.md).
