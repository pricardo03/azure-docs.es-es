---
title: Versiones de Azure Service Fabric
description: Notas de la versión de las características y mejoras más recientes de Service Fabric.
author: athinanthny
manager: chackdan
ms.author: atsenthi
ms.date: 6/10/2019
ms.topic: conceptual
ms.service: service-fabric
hide_comments: true
hideEdit: true
ms.openlocfilehash: 984e7ed82ba81d8c40688efd0a0a06e095be7c06
ms.sourcegitcommit: 3f78a6ffee0b83788d554959db7efc5d00130376
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2019
ms.locfileid: "70018982"
---
# <a name="service-fabric-releases"></a>Versiones de Service Fabric

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Guías de solución de problemas</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">Seguimiento de problemas</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">Opciones de soporte técnico</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">Versiones compatibles</a> 
| <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Ejemplos de código</a>

En este artículo se proporciona más información sobre las versiones y actualizaciones más recientes para los SDK y el runtime de Service Fabric.

## <a name="whats-new-in-service-fabric"></a>Novedades de Service Fabric

### <a name="service-fabric-65"></a>Service Fabric 6.5

La versión más reciente de Service Fabric incluye mejoras de compatibilidad, confiabilidad y rendimiento, características nuevas, correcciones de errores y mejoras para facilitar la administración del ciclo de vida de los clústeres y las aplicaciones.

> [!IMPORTANT]
> Service Fabric 6.5 es la última versión con compatibilidad con las herramientas de Service Fabric en Visual Studio 2015. Se recomienda a los clientes cambiar a [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) a partir de ahora.

Estas son las novedades de Service Fabric 6.5:

- Service Fabric Explorer incluye un [visor de almacén de imágenes](service-fabric-visualizing-your-cluster.md#image-store-viewer) para inspeccionar las aplicaciones que se han cargado en el almacén de imágenes.

- [Aplicación de orquestación de revisiones (POA)](service-fabric-patch-orchestration-application.md) versión [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) incluye muchas mejoras de autodiagnóstico. Se recomienda a los clientes de POA cambiar a esta versión.

- [El servicio EventStore está habilitado de forma predeterminada](service-fabric-visualizing-your-cluster.md#event-store) para los clústeres de Service Fabric 6.5 a menos que lo rechace.

- Se han agregado [eventos de ciclo de vida de las réplicas](service-fabric-diagnostics-event-generation-operational.md#replica-events) para los servicios con estado.

- [Visibilidad mejorada del estado del nodo de inicialización](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), incluidas advertencias de nivel de clúster si un nodo raíz es incorrecto (*Inactivo*, *Quitado* o *Desconocido*).

- [La herramienta de recuperación ante desastres para aplicaciones de Service Fabric](https://github.com/Microsoft/Service-Fabric-AppDRTool) permite que los servicios con estado de Service Fabric se recuperen rápidamente cuando se produce un desastre en el clúster principal. Los datos del clúster principal se sincronizan continuamente en la aplicación en espera secundaria mediante operaciones periódicas de restauración y copia de seguridad.

- Compatibilidad de Visual Studio para [publicar aplicaciones .NET Core en clústeres basados en Linux](service-fabric-how-to-publish-linux-app-vs.md).

- Service Fabric 6.5 (y versiones posteriores) instalará [la CLI de Service Fabric (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) de forma automática al actualizar o crear un clúster de Linux en Azure.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) se instala de forma predeterminada en los clústeres OneBox de MacOS y Linux.

Para obtener más información, vea las [notas de la versión de Service Fabric 6.5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

### <a name="service-fabric-65-releases"></a>Versiones de Service Fabric 6.5

| Fecha de lanzamiento | Release | Más información |
|---|---|---|
| 11 de junio de 2019 | [Azure Service Fabric 6.5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Notas de la versión](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2 de julio de 2019 | [Versión de actualización de Azure Service Fabric 6.5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Notas de la versión](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29 de julio de 2019 | [Versión de actualización de Azure Service Fabric 6.5](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Notas de la versión](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| 23 de agosto de 2019 | [Versión de actualización de Azure Service Fabric 6.5](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Notas de la versión](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |

## <a name="previous-versions"></a>Versiones anteriores

### <a name="service-fabric-64-releases"></a>Versiones de Service Fabric 6.4

| Fecha de lanzamiento | Release | Más información |
|---|---|---|
| 30 de noviembre de 2018 | [Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 de diciembre de 2018 | [Versión de actualización de Azure Service Fabric 6.4 para clústeres de Windows](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 de febrero de 2019 | [Versión de actualización de Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 de marzo de 2019 | [Versión de actualización de Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 de abril de 2019 | [Versión de actualización de Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 de mayo de 2019 | [Versión de actualización de Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 de mayo de 2019 | [Versión de actualización de Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Notas de la versión](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
