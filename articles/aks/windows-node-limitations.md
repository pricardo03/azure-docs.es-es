---
title: Limitaciones de los grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)
description: Obtenga información sobre las limitaciones conocidas al ejecutar cargas de trabajo de aplicación y los grupos de nodos de Windows Server en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 3d249271995d96307722dadf6b3e012e63565e6a
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956263"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Limitaciones actuales para grupos de nodos de Windows Server y las cargas de trabajo de aplicación en Azure Kubernetes Service (AKS)

En Azure Kubernetes Service (AKS), puede crear un grupo de nodos que ejecuta Windows Server como el sistema operativo invitado en los nodos. Estos nodos pueden ejecutar aplicaciones nativas de contenedor de Windows, como las basadas en .NET Framework. Como existen diferencias importantes en cómo el sistema operativo Windows y Linux proporciona compatibilidad con contenedores, algunas Kubernetes comunes y las características relacionadas con el pod no son actualmente disponibles para grupos de nodos de Windows.

En este artículo se describe algunas de las limitaciones y los conceptos del sistema operativo para los nodos de Windows Server en AKS. Grupos de nodos para Windows Server están actualmente en versión preliminar.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio y participación. Las versiones preliminares se proporcionan para recopilar comentarios y los errores de nuestra comunidad. Sin embargo, no se admiten por soporte técnico de Azure. Si crea un clúster, o agregar estas características para clústeres existentes, ese clúster no se admite hasta que la característica ya no está en versión preliminar y se aprueba para disponibilidad general (GA).
>
> Si tiene problemas con las características de vista previa, [abra una incidencia en el repositorio de GitHub de AKS] [ aks-github] con el nombre de la característica de vista previa en el título del error.

## <a name="limitations-for-windows-server-in-kubernetes"></a>Limitaciones de Windows Server en Kubernetes

Contenedores de Windows Server deben ejecutarse en un host de contenedor basadas en Windows. Para ejecutar contenedores de Windows Server en AKS, puede [crear un grupo de nodos que ejecuta Windows Server] [ windows-node-cli] como el sistema operativo invitado. Soporte de grupo de nodos de servidor de ventana incluye algunas limitaciones que forman parte de Windows Server nivel superior en el proyecto de Kubernetes. Estas limitaciones no son específicas de AKS. Para obtener más información sobre esta compatibilidad ascendente con Windows Server en Kubernetes, consulte [contenedores de Windows Server en las limitaciones de Kubernetes][upstream-limitations].

Las siguientes limitaciones de nivel superior para contenedores de Windows Server en Kubernetes son relevantes para AKS:

- Contenedores de Windows Server solo pueden usar Windows Server 2019, que coincide con el nodo de Windows Server subyacente del sistema operativo.
    - Imágenes de contenedor creadas con Windows Server 2016 ya no es compatibles con el sistema operativo base.
- No se puede usar contenedores con privilegios.
- Características específicas de Linux como capacidades RunAsUser, SELinux, AppArmor o POSIX no están disponibles en los contenedores de Windows Server.
    - Limitaciones del sistema de archivos que son específicas de Linux como UUI/GUID, por los permisos de usuario no están también disponibles en contenedores de Windows Server.
- Discos de Azure y Azure Files son los tipos de volúmenes admitidos, puede tener accesibles como volúmenes NTFS en el contenedor de Windows Server.
    - Almacenamiento basado en NFS / no se admiten volúmenes.

## <a name="aks-limitations-for-windows-server-node-pools"></a>Limitaciones de AKS para grupos de nodos de Windows Server

Se aplican las siguientes limitaciones adicionales a soporte de grupo de nodos de Windows Server en AKS:

- Un clúster de AKS siempre contiene un grupo de nodos de Linux como el primer grupo de nodos. No se puede eliminar este grupo de nodos basado en Linux primero, a menos que se elimina el propio clúster AKS.
- Clústeres AKS deben utilizar el modelo de red de Azure CNI (avanzado).
    - No se admiten redes Kubenet (basic). No se puede crear un clúster AKS que usa kubenet. Para obtener más información sobre las diferencias en los modelos de red, consulte [red conceptos relacionados con aplicaciones en AKS][azure-network-models].
    - El modelo de red de Azure CNI requiere una planeación adicional y consideraciones para la administración de direcciones IP. Para obtener más información sobre cómo planear e implementar Azure CNI, consulte [redes configurar Azure CNI en AKS][configure-azure-cni].
- Deben ser nodos de Windows Server en AKS *actualizado* a una versión más reciente de Windows Server 2019 para mantener la revisión más reciente correcciones y actualizaciones. Las actualizaciones de Windows no están habilitadas en la imagen base del nodo de AKS. Según una programación regular en torno a su propio proceso de validación y el ciclo de lanzamiento de Windows Update, debe realizar una actualización en los grupos de nodos de Windows Server en el clúster de AKS. Para obtener más información sobre cómo actualizar un grupo de nodos de Windows Server, vea [actualizar un grupo de nodos de AKS][nodepool-upgrade].
    - Estas actualizaciones del nodo de Windows Server consumen temporalmente las direcciones IP adicionales en la subred de red virtual se implementa un nuevo nodo, antes de quita el nodo antiguo.
    - las cuotas de vCPU se consumen también temporalmente en la suscripción como se implementa un nuevo nodo, a continuación, quite el nodo antiguo.
    - No se puede actualizar automáticamente y administrar los reinicios mediante `kured` igual que con los nodos de Linux en AKS.
- El clúster de AKS puede tener un máximo de ocho grupos de nodos.
    - Puede tener un máximo de 400 nodos entre esos grupos de ocho nodos.
- El nombre del grupo de nodos de Windows Server tiene un límite de 6 caracteres.
- Vista previa de las características de AKS, como el Escalador automático del clúster y directivas de redes no están aprobadas para los nodos de Windows Server.
- Solo se deben programar los controladores de entrada en nodos de Linux mediante un NodeSelector.
- Espacios de desarrollo de Azure actualmente solo está disponible para grupos de nodos basado en Linux.

## <a name="os-concepts-that-are-different"></a>Conceptos del sistema operativo que son diferentes

Kubernetes es históricamente centrado en Linux. Muchos ejemplos usados en el canal de subida [Kubernetes.io] [ kubernetes] sitio Web están diseñados para su uso en los nodos de Linux. Al crear implementaciones que usan contenedores de Windows Server, las consideraciones siguientes en las condiciones de nivel de sistema operativo:

- **Identidad** -Linux utiliza el identificador de usuario (UID) y groupID (GID), representados como tipos de entero. Los nombres de usuario y grupo no son canónicos: son simplemente un alias en */etcetera/grupos* o */etcetera/passwd* al UID + GID.
    - Windows Server usa un identificador de seguridad binario de mayor tamaño (SID) que se almacena en la base de datos de administrador de acceso de seguridad (SAM) de Windows. Esta base de datos no se comparte entre el host y los contenedores, o entre contenedores.
- **Permisos de archivos** -Windows Server utiliza una lista de control de acceso basada en SID, en lugar de una máscara de bits de permisos y UID + GID
- **Rutas de archivo** -convención en Windows Server consiste en usar \ en lugar de /.
    - En las especificaciones de pod que montan los volúmenes, especifique la ruta correctamente para los contenedores de Windows Server. Por ejemplo, en lugar de un montaje de punto de */mnt/volumen* en un contenedor de Linux, especifique una letra de unidad y la ubicación como */K/volumen* montar como el *K:* unidad.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con contenedores de Windows Server en AKS, [crear un grupo de nodos que ejecuta Windows Server en AKS][windows-node-cli].

<!-- LINKS - external -->
[upstream-limitations]: https://kubernetes.io/docs/setup/windows/#limitations
[aks-github]: https://github.com/azure/aks/issues]
[kubernetes]: https://kubernetes.io

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
