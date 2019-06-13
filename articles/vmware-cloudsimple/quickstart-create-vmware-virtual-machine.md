---
title: 'Inicio rápido de la Solución de VMware en Azure de CloudSimple: consumo de máquinas virtuales de VMware en Azure'
description: En este inicio rápido, aprenderá a configurar y a consumir máquinas virtuales de VMware desde Azure Portal mediante la Solución de VMware en Azure de CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b430efbc931d72de4b095a7eac4c1e7ca496b1b9
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393504"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Inicio rápido: Uso de máquinas virtuales de VMware en Azure

Para crear una máquina virtual en Azure Portal, puede usar las plantillas de máquina virtual disponibles en la instancia de vCenter de la nube privada. Un administrador de vCenter puede crear plantillas adicionales en la nube privada.

## <a name="create-a-vm-template"></a>Creación de una plantilla de máquina virtual

En primer lugar, cree una máquina virtual en la nube privada mediante la interfaz de usuario de vCenter. Para crear una plantilla, siga las instrucciones que se indican en el artículo [Clonar una máquina virtual en una plantilla en vSphere Web Client](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Almacene la plantilla de máquina virtual en la instancia de vCenter de la nube privada.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Creación de una máquina virtual en Azure Portal

1. Seleccione **Todos los servicios**.

2. Busque **CloudSimple Virtual Machines**.

3. Seleccione **Agregar**.

    ![Selección de Agregar](media/create-cloudsimple-virtual-machine.png)

4. Escriba la siguiente información sobre la máquina virtual y, luego, seleccione **Siguiente: Tamaño**.

    ![Creación de una máquina virtual de CloudSimple: conceptos básicos](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Campo | DESCRIPCIÓN |
    | ------------ | ------------- |
    | **Suscripción** | La suscripción de Azure asociada con la nube privada.  |
    | **Grupos de recursos** | El grupo de recursos al que se asignará la máquina virtual. Puede seleccionar un grupo existente o crear uno. |
    | **Nombre** | Un nombre para identificar la máquina virtual.  |
    | **Ubicación** | La región de Azure en el que se hospeda la máquina virtual.  |
    | **Nube privada** | La nube privada de CloudSimple en la que quiere crear la máquina virtual. |
    | **ResourcePool** | Un grupo de recursos asignado de la máquina virtual. Seleccione entre los grupos de recursos disponibles. |
    | **vSphere Template** (Plantilla de vSphere) | La plantilla de vSphere de la máquina virtual.  |
    | **Nombre de usuario** | El nombre de usuario del administrador de la máquina virtual (para plantillas de Windows).|
    | **Contraseña** |  La contraseña del administrador de la máquina virtual (para plantillas de Windows). |
    | **Confirmar contraseña** | La contraseña proporcionada en el campo anterior. |

5. Seleccione el número de núcleos y la capacidad de memoria de la máquina virtual. Seleccione **Expose to Guest OS** (Exponer al SO invitado) si quiere exponer la virtualización completa de la CPU al sistema operativo invitado. Las aplicaciones que requieren virtualización de hardware se pueden ejecutar en máquinas virtuales sin traducción binaria o paravirtualización. Para más información, consulte el artículo de VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">Exponer la virtualización asistida por hardware de VMware</a>. Cuando finalice, seleccione **Siguiente: Configuraciones**.

    ![Creación de una máquina virtual de CloudSimple: tamaño](media/create-cloudsimple-virtual-machine-size.png)

6. Configure las interfaces de red y los discos como se describe en las tablas siguientes y, luego, seleccione **Revisar y crear**.

    ![Creación de una máquina virtual de CloudSimple: configuraciones](media/create-cloudsimple-virtual-machine-configurations.png)

    Para las interfaces de red, seleccione **Add network interface** (Agregar interfaz de red) y, luego, configure los siguientes valores:
    
    | Configuración | DESCRIPCIÓN |
    | ------------ | ------------- |
    | **Nombre** | Escriba un nombre para identificar la interfaz.  |
    | **Red** | Seleccione de la lista de grupos de puertos distribuidos configurados en la instancia de vSphere de la nube privada.  |
    | **Adaptador** | Seleccione un adaptador de vSphere de la lista de tipos disponibles configurado para la máquina virtual. Para más información, consulte el artículo de VMware <a href="https://kb.vmware.com/s/article/1001805" target="_blank">Elegir un adaptador de red para su máquina virtual</a>. |
    | **Power on at Boot** (Encender al arrancar) | Elija si quiere habilitar el hardware de NIC al arrancar la máquina virtual. El valor predeterminado es **Habilitar**. |

    Para los discos, seleccione **Agregar disco** y, luego, configure las siguientes opciones:

    | Configuración | DESCRIPCIÓN |
    | ------------ | ------------- |
    | **Nombre** | Escriba un nombre para identificar el disco.  |
    | **Tamaño** | Seleccione uno de los tamaños disponibles.  |
    | **SCSI Controller** (Controladora SCSI) | Seleccione una controladora SCSI para el disco.  |
    | **Modo** | El modo especifica cómo participa el disco en las instantáneas. Elija una de estas opciones: <br> **Independent persistent** (Independiente persistente): todos los cambios escritos en el disco se escriben de forma permanente.<br> **Independent non-persistent** (Independiente no persistente): los cambios escritos en el disco se descartan cuando se apaga o restablece la máquina virtual. El modo independiente no persistente permite reiniciar siempre la máquina virtual en el mismo estado. Para más información, consulte la <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">documentación de VMware</a>.

7. Una vez finalizada la validación, revise la configuración y seleccione **Crear**. Para realizar cambios, seleccione las pestañas de la parte superior o seleccione **Anterior: Configuraciones**.

    ![Creación de una máquina virtual de CloudSimple: revisión y creación](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Pasos siguientes

* [View list of CloudSimple virtual machines](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines) (Visualización de la lista de máquinas virtuales de CloudSimple)
* [Manage CloudSimple virtual machines from Azure](https://docs.azure.cloudsimple.com/azure-manage-vm/) (Administración de máquinas virtuales de CloudSimple desde Azure)
