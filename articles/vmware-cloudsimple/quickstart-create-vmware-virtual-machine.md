---
title: Solución de VMware de Azure por CloudSimple Quickstart - consumen las máquinas virtuales de VMware en Azure
description: Obtenga información sobre cómo configurar y usar máquinas virtuales de VMware desde Azure portal con la solución de VMware de Azure por CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 58edadb553730b646f23f4981d6cbf1bdbfe76d5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577726"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Inicio rápido: usar máquinas virtuales de VMware en Azure

Para crear una máquina virtual en el portal de Azure, utilizar plantillas de máquina Virtual que el Administrador de CloudSimple ha habilitado para la suscripción. Estas plantillas de máquina virtual se encuentran en la infraestructura de VMware.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Creación de CloudSimple VM en Azure requiere una plantilla de máquina virtual

Crear una máquina virtual en la nube privada de la interfaz de usuario de vCenter. Para crear una plantilla, siga las instrucciones de [clonar una máquina Virtual a una plantilla en el cliente Web vSphere](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Store la plantilla de máquina virtual en el nube privada de vCenter.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Crear una máquina virtual en Azure portal

1. En el menú izquierdo, haga clic en **+** o **crear un recurso**.

2. En el menú izquierdo, haga clic en **proceso**y, a continuación, haga clic en **CloudSimple Virtual Machine**.

3. Haga clic en **confirmar** para comprobar que desea crear una nueva máquina virtual.

4. Establecer la configuración básica, como se describe en la tabla siguiente y, a continuación, haga clic en **siguiente: Tamaño**.

    | Campo | DESCRIPCIÓN |
    | ------------ | ------------- |
    | Subscription | Suscripción de Azure asociada con la implementación de nube privada.  |
    | Grupo de recursos | Grupo de implementación a la que se asignará la máquina virtual. Puede seleccionar un grupo existente o crear uno nuevo. |
    | NOMBRE | Nombre para identificar la máquina virtual.  |
    | Location | Región de Azure en el que se hospeda esta máquina virtual.  |
    | Grupo de recursos | Recursos físicos para la máquina virtual. Seleccione los grupos de recursos disponibles. |
    | vSphere Template | Tipo de plantilla de sistema operativo para la máquina virtual.  |
    | Nombre de usuario | Nombre de usuario del Administrador de máquina virtual. |
    | Confirmar contraseña: contraseña | Contraseña para el Administrador de la máquina virtual.  |

5. Seleccione el número de núcleos y la capacidad de memoria para la máquina virtual.

6. (Opcional) Si desea exponer la virtualización completa de la CPU para el sistema operativo invitado, seleccione el **exponer al SO invitado** casilla de verificación.
Esta selección permite que las aplicaciones que requieren virtualización de hardware que se ejecutan en máquinas virtuales sin traducción binaria o de paravirtualización. Para obtener más información, vea el artículo de VMware [exponer VMware Hardware Assisted Virtualization](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html).

7. Haga clic en **Siguiente: Configuración**.

8. Configurar las interfaces de red y discos, como se describe en las tablas siguientes.

    Para las interfaces de red, haga clic en **interfaz de red Add** y configure las siguientes opciones.

    | Control | DESCRIPCIÓN |
    | ------------ | ------------- |
    | NOMBRE | Escriba un nombre para identificar la interfaz.  |
    | Red | Seleccione en la lista de redes configuradas en la nube privada de vSphere.  |
    | Adaptador | Seleccione un adaptador de vSphere en la lista de tipos disponibles configurado para la máquina virtual. Para obtener más información, consulte el artículo de knowledge base VMware [elección de un adaptador de red para la máquina virtual](https://kb.vmware.com/s/article/1001805). |
    | Encendido durante el arranque | Elija si desea habilitar el hardware NIC al arranca la máquina virtual. El valor predeterminado es **habilitar**. |

    Los discos, haga clic en **Agregar disco** y configure las siguientes opciones.

    | item | DESCRIPCIÓN |
    | ------------ | ------------- |
    | NOMBRE | Escriba un nombre para identificar el disco.  |
    | Tamaño | Seleccione uno de los tamaños disponibles.  |
    | Controlador SCSI | Seleccione una controladora SCSI. Los controladores disponibles varían según los distintos sistemas operativos compatibles.  |
    | Mode | Determina cómo el disco participa en las instantáneas. Elija una de estas opciones: <br> -Persistentes independientes: Todos los datos escritos en el disco se escribe de forma permanente.<br> -No persistentes independientes: Al apagar o restablecer la máquina virtual, se descartan los cambios escritos en el disco.  Modo no persistentes independiente permite reiniciar la máquina virtual siempre en el mismo estado. Para obtener más información, consulte el [documentación de VMware.](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html)

9. Revise la configuración. Para realizar cambios, haga clic en las pestañas en la parte superior.

10. Haga clic en **crear** para guardar la configuración y crear la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

* [Ver la lista de máquinas virtuales de CloudSimple](https://docs.azure.cloudsimple.com/azurelistvms/)
* [Administrar la máquina virtual de CloudSimple de Azure](https://docs.azure.cloudsimple.com/azureoverviewpage/)