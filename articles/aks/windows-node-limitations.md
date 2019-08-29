---
title: Limitaciones de los grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)
description: Obtenga información sobre las limitaciones conocidas al ejecutar cargas de trabajo de aplicaciones y grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: c2c9e3d29ced5f75873656e253ecdbab5efe7df8
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114405"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Limitaciones actuales para las cargas de trabajo de aplicaciones y los grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)

En Azure Kubernetes Service (AKS) puede crear un grupo de nodos que ejecuta Windows Server como sistema operativo invitado en los nodos. Estos nodos pueden ejecutar aplicaciones de contenedor nativas de Windows, como las integradas en .NET Framework. Como existen diferencias importantes en la forma en que los sistemas operativos Windows y Linux proporcionan compatibilidad con contenedores, algunas características de Kubernetes comunes y relacionadas con el pod no están actualmente disponibles para grupos de nodos de Windows.

En este artículo se describen algunas de las limitaciones y los conceptos de sistema operativo para los nodos de Windows Server en AKS. Los grupos de nodos para Windows Server actualmente están en versión preliminar.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

## <a name="limitations-for-windows-server-in-kubernetes"></a>Limitaciones de Windows Server en Kubernetes

Los contenedores de Windows Server deben ejecutarse en un host de contenedor basado en Windows. Para ejecutar los contenedores de Windows Server en AKS puede [crear un grupo de nodos que ejecute Windows Server][windows-node-cli] como sistema operativo invitado. La compatibilidad del grupo de nodos de Windows Server incluye algunas limitaciones que forman parte de Windows Server ascendente en el proyecto de Kubernetes. Estas limitaciones no son específicas de AKS. Para obtener más información sobre esta compatibilidad ascendente con Windows Server en Kubernetes, consulte [Limitaciones actuales para las cargas de trabajo de aplicaciones y los grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)](https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations).

Las siguientes limitaciones ascendentes para contenedores de Windows Server en Kubernetes son pertinentes para AKS:

- Los contenedores de Windows Server solo pueden usar Windows Server 2019, que coincide con el sistema operativo del nodo de Windows Server subyacente.
    - No se admiten las imágenes de contenedor creadas con Windows Server 2016 como sistema operativo base.
- No se pueden utilizar los contenedores con privilegios.
- Las características específicas de Linux, como las funcionalidades RunAsUser, SELinux, AppArmor o POSIX, no están disponibles en los contenedores de Windows Server.
    - Las limitaciones del sistema de archivos que son específicas de Linux, como UUI/GUID, por los permisos de usuario tampoco están disponibles en los contenedores de Windows Server.
- Azure Disk y Azure Files son los tipos de volúmenes admitidos, a los que se accede como volúmenes NTFS en el contenedor de Windows Server.
    - No se admiten volúmenes ni almacenamiento basados en NFS.

## <a name="aks-limitations-for-windows-server-node-pools"></a>Limitaciones de AKS para los grupos de nodos de Windows Server

Se aplican las siguientes limitaciones adicionales a la compatibilidad de los grupos de nodos de Windows Server en AKS:

- Un clúster de AKS siempre contiene un grupo de nodos de Linux como el primer grupo de nodos. No se puede eliminar este primer grupo de nodos basado en Linux, a menos que se elimine el propio clúster de AKS.
- Los clústeres de AKS deben utilizar el modelo de redes (avanzadas) de Azure CNI.
    - No se admiten las redes Kubenet (básico). No se puede crear un clúster de AKS que use kubenet. Para obtener más información sobre las diferencias en los modelos de redes, consulte [Conceptos de redes para las aplicaciones en AKS][azure-network-models].
    - El modelo de redes de Azure CNI requiere una planeación y consideraciones adicionales para la administración de direcciones IP. Para obtener más información sobre cómo planear e implementar Azure CNI, consulte [Configuración de redes de Azure CNI en AKS][configure-azure-cni].
- Los nodos de Windows Server en AKS deben estar *actualizados* a una versión más reciente de Windows Server 2019 para mantener las correcciones y actualizaciones más recientes. Las actualizaciones de Windows no están habilitadas en la imagen de nodo base de AKS. Según la programación normal en el ciclo de versiones de Windows Update y su proceso de validación propio, debe realizar una actualización de los grupos de nodos de Windows Server en el clúster de AKS. Para más información sobre cómo actualizar un grupo de nodos de Windows Server, consulte el artículo de [actualización de un grupo de nodos en AKS][nodepool-upgrade].
    - Estas actualizaciones del nodo de Windows Server consumen temporalmente las direcciones IP adicionales en la subred de la red virtual mientras se implementa un nuevo nodo, antes de quitar el nodo antiguo.
    - Las cuotas de vCPU también se consumen temporalmente en la suscripción al implementar un nuevo nodo; después se quita el nodo antiguo.
    - No se puede actualizar y administrar automáticamente los reinicios mediante `kured` igual que con los nodos de Linux en AKS.
- El clúster de AKS puede tener un máximo de ocho grupos de nodos.
    - Puede tener un máximo de 400 nodos distribuidos entre esos ocho grupos de nodos.
- El nombre del grupo de nodos de Windows Server tiene un límite de 6 caracteres.
- Las característica en vista previa (GB) de AKS, como el escalador automático de clúster y las directivas de red, no están aprobadas para los nodos de Windows Server.
- Solo se deben programar los controladores de entrada en nodos de Linux con un valor NodeSelector.
- Azure Dev Spaces actualmente solo está disponible para grupos de nodos basados en Linux.
- Cuando los nodos de Windows Server no están unidos a un dominio de Active Directory, AKS no ofrece actualmente compatibilidad con las cuentas de servicio administradas de grupo (gMSA).
    - El proyecto ascendente de código abierto, [aks-engine][aks-engine] actualmente proporciona compatibilidad con gMSA si necesita usar esta característica.

## <a name="os-concepts-that-are-different"></a>Diferencias en los conceptos del sistema operativo

Kubernetes se ha centrado siempre en Linux. Muchos ejemplos usados en el sitio web [Kubernetes.io][kubernetes] ascendente están diseñados para su uso en nodos de Linux. Al crear implementaciones que usan contenedores de Windows Server, se aplican las consideraciones siguientes en el nivel de sistema operativo:

- **Identidad**: Linux utiliza los valores userID (UID) y groupID (GID), representados como tipos de enteros. Los nombres de usuario y de grupo no son canónicos: son simplemente un alias en */etc/groups* o */etc/passwd* en UID+GID.
    - Windows Server usa un identificador de seguridad binario de mayor tamaño (SID) que se almacena en la base de datos de Windows Security Access Manager (SAM). Esta base de datos no se comparte entre el host y los contenedores ni entre contenedores.
- **Permisos de archivos**: Windows Server utiliza una lista de control de acceso basada en SID, en lugar de una máscara de bits de permisos y UID+GID.
- **Rutas de archivo**: la convención en Windows Server consiste en usar \ en lugar de /.
    - En las especificaciones de pod que montan los volúmenes, especifique la ruta correctamente para los contenedores de Windows Server. Por ejemplo, en lugar de un punto de montaje de */mnt/volume* en un contenedor de Linux, especifique la letra de unidad y la ubicación como */K/Volume* para montarlo como la unidad *K:* .

## <a name="next-steps"></a>Pasos siguientes

Para comenzar con los contenedores de Windows Server en AKS, [cree un grupo de nodos que ejecute Windows Server en AKS][windows-node-cli].

<!-- LINKS - external -->
[upstream-limitations]: https://kubernetes.io/docs/setup/windows/#limitations
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
