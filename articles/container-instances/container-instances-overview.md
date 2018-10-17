---
title: Introducción a Azure Container Instances
description: Descripción de Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 10/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 33d6d89e91ecdec00c1b17ecddf91128e9d07526
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802107"
---
# <a name="azure-container-instances"></a>Azure Container Instances

Los contenedores están convirtiéndose en la manera preferida de empaquetar, implementar y administrar aplicaciones en la nube. Azure Container Instances ofrece la forma más rápida y sencilla de ejecutar un contenedor en Azure, sin tener que administrar ninguna máquina virtual y sin necesidad de adoptar un servicio de nivel superior.

Azure Container Instances es una excelente solución para cualquier escenario que pueda funcionar en contenedores aislados, incluidas las aplicaciones simples, la automatización de tareas y los trabajos de compilación. En aquellos escenarios donde se necesita una orquestación completa de contenedores, incluida la detección de servicios en varios contenedores, el escalado automático y las actualizaciones de aplicaciones coordinadas, se recomienda [Azure Kubernetes Service (AKS)](../aks/index.yml).

## <a name="fast-startup-times"></a>Tiempos de inicio rápido

Los contenedores ofrecen importantes ventajas de inicio sobre las máquinas virtuales (VM). Azure Container Instances puede iniciar un contenedor en Azure en segundos sin que sea necesario aprovisionar y administrar máquinas virtuales.

## <a name="public-ip-connectivity-and-dns-name"></a>Conectividad IP pública y nombre DNS

Azure Container Instances permite exponer los contenedores directamente a Internet con una dirección IP y un nombre de dominio completo (FQDN). Al crear una instancia de contenedor, se puede especificar una etiqueta de nombre DNS personalizada, con el fin de que se pueda acceder a la aplicación en *customlabel*. *azureregion*. azurecontainer.io.

## <a name="hypervisor-level-security"></a>Seguridad de nivel de hipervisor

Históricamente, los contenedores han ofrecido aislamiento a la dependencia entre aplicaciones y gobierno de recursos, pero no se han considerado suficientemente protegidos para el uso de varios inquilinos hostiles. Azure Container Instances garantiza que la aplicación está tan aislada en un contenedor como lo estaría en una máquina virtual.

## <a name="custom-sizes"></a>Tamaños personalizados

Los contenedores normalmente están optimizados para ejecutar una sola aplicación, pero las necesidades exactas de esas aplicaciones pueden diferir considerablemente. Azure Container Instances proporciona un uso óptimo al permitir especificaciones exactas de los núcleos y la memoria de la CPU. El usuario paga según lo que necesita y se le factura por segundo, para que pueda optimizar con precisión los gastos según sus necesidades reales.

## <a name="persistent-storage"></a>Almacenamiento persistente

Para recuperar y conservar el estado con Azure Container Instances, ofrecemos el [montaje directo de los recursos compartidos de Azure Files](container-instances-mounting-azure-files-volume.md).

## <a name="linux-and-windows-containers"></a>Contenedores de Linux y Windows

Azure Container Instances puede programar los contenedores Windows y Linux con la misma API. Simplemente especifique el tipo de sistema operativo al crear los [grupos de contenedor](container-instances-container-groups.md).

Algunas características están restringidas actualmente a los contenedores Linux. Aunque trabajamos para traer la paridad de características a los contenedores Windows, puede encontrar diferencias en la plataforma actual en la [disponibilidad de cuotas y regiones en Azure Container Instances](container-instances-quotas.md).

Azure Container Instances admite imágenes de Windows basadas en versiones del canal de servicio a largo plazo (LTSC). No se admiten las versiones 1709 y 1803 de canal semianual (SAC) de Windows.

## <a name="co-scheduled-groups"></a>Grupos con programación compartida

Azure Container Instances admite la programación de [grupos con varios contenedores](container-instances-container-groups.md) que comparten una máquina host, la red local, el almacenamiento y el ciclo de vida. Esto le permite combinar el contenedor de la aplicación principal con otros contenedores que actúan en una función auxiliar, como los patrones sidecar de registro.

## <a name="virtual-network-deployment-preview"></a>Implementación de red virtual (versión preliminar)

Actualmente en versión preliminar, esta característica de Azure Container Instances permite la [implementación de instancias de contenedor en una red virtual de Azure](container-instances-vnet.md). Al implementar instancias de contenedor en una subred dentro de la red virtual, pueden comunicarse de forma segura con otros recursos de la red virtual, incluidos los que están en el entorno local (mediante [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ ExpressRoute](../expressroute/expressroute-introduction.md)).

> [!IMPORTANT]
> La implementación de grupos de contenedores en una red virtual está actualmente en versión preliminar y se aplican algunas [limitaciones](container-instances-vnet.md#preview-limitations). Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="next-steps"></a>Pasos siguientes

Pruebe a implementar un contenedor en Azure con un único comando mediante nuestra Guía de inicio rápido:

> [!div class="nextstepaction"]
> [Guía de inicio rápido de Azure Container Instances](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
