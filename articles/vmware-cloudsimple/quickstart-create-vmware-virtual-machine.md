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
ms.openlocfilehash: c0bb8d7a5a1ea30b704b44c9337cd28043597ff7
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209515"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Inicio rápido: usar máquinas virtuales de VMware en Azure

Para crear una máquina virtual en el portal de Azure, utilizar plantillas de máquina Virtual que el Administrador de CloudSimple ha habilitado para la suscripción. Estas plantillas de máquina virtual se encuentran en la infraestructura de VMware.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Creación de CloudSimple VM en Azure requiere una plantilla de máquina virtual

Crear una máquina virtual en la nube privada de la interfaz de usuario de vCenter. Para crear una plantilla, siga las instrucciones de [clonar una máquina Virtual a una plantilla en el cliente Web vSphere](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Store la plantilla de máquina virtual en el nube privada de vCenter.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Crear una máquina virtual en Azure portal

1. Seleccione **Todos los servicios**.

2. Busque **las máquinas virtuales de CloudSimple**.

3. Haga clic en **Agregar**.

    ![Crear la máquina virtual de CloudSimple](media/create-cloudsimple-virtual-machine.png)

4. Escriba la información básica haga clic en **siguiente: tamaño**.

    ![Crear la máquina virtual de CloudSimple: conceptos básicos](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Campo | DESCRIPCIÓN |
    | ------------ | ------------- |
    | Subscription | Suscripción de Azure asociada con su propia nube privada.  |
    | Grupo de recursos | Grupo de recursos al que se asignará la máquina virtual. Puede seleccionar un grupo existente o crear uno nuevo. |
    | NOMBRE | Nombre para identificar la máquina virtual.  |
    | Ubicación | Región de Azure en el que se hospeda esta máquina virtual.  |
    | Nube privada | En la nube de CloudSimple privada donde desea crear la máquina virtual. |
    | Grupo de recursos | Grupo de recursos asignados para la máquina virtual. Seleccione los grupos de recursos disponibles. |
    | vSphere Template | plantilla de vSphere para la máquina virtual.  |
    | Nombre de usuario | Nombre de usuario del Administrador de máquina virtual (para las plantillas de Windows)|
    | Contraseña |  Contraseña para el Administrador de la máquina virtual (para las plantillas de Windows). |
    | Confirmar contraseña | Confirme la contraseña |

5. Seleccione el número de núcleos y la capacidad de memoria para la máquina virtual y haga clic en **siguiente: configuraciones**. Seleccione la casilla de verificación si desea exponer la virtualización completa de la CPU para el sistema operativo invitado. Pueden ejecutar las aplicaciones que requieren virtualización de hardware en las máquinas virtuales sin traducción binaria o de paravirtualización. Para obtener más información, vea el artículo de VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">exponer VMware Hardware Assisted Virtualization</a>.

    ![Crear la máquina virtual de CloudSimple: tamaño](media/create-cloudsimple-virtual-machine-size.png)

6. Configurar las interfaces de red y discos, como se describe en las tablas siguientes y haga clic en **revisión + crear**.

    ![Creación de máquina virtual de CloudSimple - configuraciones](media/create-cloudsimple-virtual-machine-configurations.png)

    Para las interfaces de red, haga clic en **interfaz de red Add** y configure las siguientes opciones.
    
    | Control | DESCRIPCIÓN |
    | ------------ | ------------- |
    | NOMBRE | Escriba un nombre para identificar la interfaz.  |
    | Red | Seleccione en la lista del grupo de puertos distribuido configurado en la nube privada de vSphere.  |
    | Adaptador | Seleccione un adaptador de vSphere en la lista de tipos disponibles configurado para la máquina virtual. Para obtener más información, consulte el artículo de knowledge base VMware <a href="https://kb.vmware.com/s/article/1001805" target="_blank">elección de un adaptador de red para la máquina virtual</a>. |
    | Encendido durante el arranque | Elija si desea habilitar el hardware NIC al arranca la máquina virtual. El valor predeterminado es **habilitar**. |

    Los discos, haga clic en **Agregar disco** y configure las siguientes opciones.

    | Elemento | DESCRIPCIÓN | 
    | ------------ | ------------- | 
    | NOMBRE | Escriba un nombre para identificar el disco.  | 
    | Tamaño | Seleccione uno de los tamaños disponibles.  | 
    | Controlador SCSI | Seleccione un controlador de SCSI para el disco.  |
    | Mode | Determina cómo el disco participa en las instantáneas. Elija una de estas opciones: <br> -Persistentes independientes: Todos los datos escritos en el disco se escribe de forma permanente.<br> -No persistentes independientes: Al apagar o restablecer la máquina virtual, se descartan los cambios escritos en el disco.  Modo no persistentes independiente permite reiniciar la máquina virtual siempre en el mismo estado. Para obtener más información, consulte el <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">documentación de VMware</a>.

7. Una vez finalizada la validación, revise la configuración y haga clic en **crear**. Para realizar cambios, haga clic en las pestañas en la parte superior o haga clic en.

    ![Crear la máquina virtual de CloudSimple: revisar](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Pasos siguientes

* [Ver la lista de máquinas virtuales de CloudSimple](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Administrar la máquina virtual de CloudSimple de Azure](https://docs.azure.cloudsimple.com/azure-manage-vm/)
