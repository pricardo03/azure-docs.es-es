---
title: 'Plantillas de ejemplo de Azure Resource Manager: Azure Container Instances'
description: Plantillas de ejemplo de Azure Resource Manager para Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/07/2019
ms.author: danlep
ms.openlocfilehash: 3d73d05c64f4b4867c69a15089c19ab8c320b9a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60537962"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Plantillas de Azure Resource Manager para Azure Container Instances

Las siguientes plantillas de ejemplo implementan instancias de contenedor en diversas configuraciones.

Para opciones de implementación, consulte la sección [Implementación](#deployment). Si desea crear sus propias plantillas, la [Referencia de plantillas de Resource Manager][ref] de Azure Container Instances detalla el formato de la plantilla y las propiedades disponibles.

## <a name="sample-templates"></a>Plantillas de ejemplo

| | |
|-|-|
| **Aplicaciones** ||
| [WordPress][app-wp] | Crea un sitio Web de WordPress y su base de datos MySQL en un grupo de contenedores. El contenido del sitio de WordPress y la base de datos MySQL se conservan en un recurso compartido de Azure Files. También crea una instancia de application gateway para exponer el acceso a la red pública de WordPress. |
| [MS NAV con IIS y SQL Server][app-nav] | Implementa un único contenedor de Windows con un entorno de Dynamics NAV y Dynamics 365 Business Central con las características completas. |
| **Volúmenes** ||
| [emptyDir][vol-emptydir] | Implementa dos contenedores de Linux que comparten un volumen de emptyDir. |
| [gitRepo][vol-gitrepo] | Implementa un contenedor de Linux que clona un repositorio de GitHub y lo monta como un volumen. |
| [secret][vol-secret] | Implementa un contenedor de Linux con un certificado PFX montado como un volumen secreto. |
| **Redes** ||
| [Contenedor con UDP expuesto][net-udp] | Implementa un contenedor de Windows o Linux que expone un puerto UDP. |
| [Contenedor de Linux con dirección IP pública][net-publicip] | Implementa un único contenedor de Linux accesible a través de una dirección IP pública. |
| [Implementar un grupo de contenedores con una red virtual (versión preliminar)][net-vnet] | Implementa una nueva red virtual, subred, perfil de red y grupo de contenedores. |
| **Recursos de Azure** ||
| [Creación de cuenta de Azure Storage y recurso compartido de archivos][az-files] | Usa la CLI de Azure en una instancia del contenedor para crear una cuenta de almacenamiento y un recurso compartido de Azure Files.

## <a name="deployment"></a>Implementación

Tiene varias opciones para implementar recursos con plantillas de Resource Manager:

[CLI de Azure][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure Portal][deploy-portal]

[API REST][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[net-vnet]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/resource-group-template-deploy-cli.md
[deploy-portal]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/resource-group-template-deploy.md
[deploy-rest]: ../azure-resource-manager/resource-group-template-deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups
