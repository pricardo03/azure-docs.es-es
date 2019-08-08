---
title: Asignación de una suscripción de Azure a grupos de recursos en la solución de VMware en Azure de CloudSimple
description: En este artículo se describe cómo asignar un grupo de recursos en la solución de VMware en Azure de CloudSimple a una suscripción de Azure.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1bf721f35500d2ff1344996e7750c5e574f40f31
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816271"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Asignación de grupos de recursos de la nube privada a la suscripción de Azure

La asignación de suscripciones de Azure le permite asignar grupos de recursos de la instancia de vCenter de la nube privada a su suscripción de Azure. Solo puede asignar la suscripción donde creó el servicio CloudSimple.  Al crear una máquina virtual de VMware desde Azure Portal, la máquina virtual se implementa en el grupo de recursos asignado.  En el portal de CloudSimple, puede ver y administrar la suscripción de Azure para las nubes privadas.

Una suscripción puede asignarse a varios grupos de recursos de vCenter de una nube privada.  Debe asignar los grupos de recursos de cada nube privada.  Solo los grupos de recursos asignados estarán disponibles para crear una máquina virtual de VMware desde Azure Portal.

> [!IMPORTANT]
> Cuando se asigna un grupo de recursos, también se asignan los grupos de recursos secundarios. No se puede asignar un grupo de recursos primario si los grupos de recursos secundarios ya están asignados.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por supuesto que tiene un servicio CloudSimple y una nube privada en su suscripción.  Para crear un servicio CloudSimple, consulte [Inicio rápido: creación de servicio](quickstart-create-cloudsimple-service.md).  Si necesita crear una nube privada, consulte [Inicio rápido: configuración del entorno de una nube privada](quickstart-create-private-cloud.md).

Puede asignar el clúster de vCenter (grupo de recursos de raíz) a su suscripción.  Si quiere crear un grupo de recursos, consulte el artículo [Crear un grupo de recursos](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) en el sitio de documentación de VMware.

## <a name="default-resource-group"></a>Grupo de recursos predeterminado

Al crear una máquina virtual de CloudSimple desde Azure Portal, puede seleccionar el grupo de recursos.  Una máquina virtual creada en la instancia de vCenter de la nube privada en un grupo de recursos asignado será visible en Azure Portal.  La máquina virtual detectada se colocará en el grupo de recursos de Azure predeterminado.  Puede cambiar el nombre del grupo de recursos predeterminado.

## <a name="map-azure-subscription"></a>Asignación de una suscripción de Azure

1. Acceda al [portal de CloudSimple](access-cloudsimple-portal.md).

2. Abra la página **Recursos** y seleccione la nube privada que quiere asignar.

3. Seleccione **Azure subscriptions mapping** (Asignación de suscripciones de Azure).

4. Seleccione **Edit Azure subscription mapping** (Editar la asignación de suscripciones de Azure).

5. Para asignar los grupos de recursos disponibles, selecciónelos a la izquierda y haga clic en la flecha que apunta hacia la derecha.

6. Para quitar asignaciones, selecciónelas a la derecha y haga clic en la flecha que apunta hacia la izquierda.

    ![Suscripciones de Azure](media/resources-azure-mapping.png)

7. Haga clic en **OK**.

## <a name="change-default-resource-group-name"></a>Cambio del nombre del grupo de recursos predeterminado

1. Acceda al [portal de CloudSimple](access-cloudsimple-portal.md).

2. Abra la página **Recursos** y seleccione la nube privada que quiere asignar.

3. Seleccione **Azure subscriptions mapping** (Asignación de suscripciones de Azure).

4. Haga clic en **Editar** bajo el nombre del grupo de recursos de Azure.

    ![Edición del nombre del grupo de recursos](media/resources-edit-resource-group-name.png)

5. Escriba un nombre nuevo para el grupo de recursos y haga clic en **Enviar**.

    ![Especificación de un nuevo nombre para el grupo de recursos](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Más información sobre las [máquinas virtuales de CloudSimple](cloudsimple-virtual-machines.md)