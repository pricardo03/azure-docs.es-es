---
title: Administración de la nube privada de Azure VMware Solutions (AVS)
description: Describe las funcionalidades disponibles para administrar la actividad y los recursos de nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 47bf2251f71204b99245c1a9d55ef87157c41dd8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014834"
---
# <a name="manage-avs-private-cloud-resources-and-activities"></a>Administración de las actividades y los recursos de la nube privada de AVS

Las nubes privadas de AVS se administran desde el portal de AVS. Compruebe el estado, los recursos disponibles, la actividad en la nube privada de AVS y otras opciones de configuración desde el portal de AVS.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-avs-portal"></a>Acceso al portal de AVS

Acceda al [portal de AVS](access-cloudsimple-portal.md).

## <a name="view-the-list-of-avs-private-clouds"></a>Visualización de la lista de nubes privadas de AVS

En la pestaña de **nubes privadas de AVS** en la página **Recursos**, se enumeran todas las nubes privadas en su suscripción. La información incluye el nombre, el número de clústeres de vSphere, la ubicación, el estado actual de la nube privada de AVS e información de los recursos.

![Página de la nube privada de AVS](media/manage-private-cloud.png)

Seleccione una nube privada de AVS para acciones e información adicionales.

## <a name="avs-private-cloud-summary"></a>Resumen de la nube privada de AVS

Vea un resumen completo de la nube privada de AVS seleccionada. La página Resumen incluye los servidores DNS que se han implementado en la nube privada de AVS. Puede configurar el reenvío DNS desde los servidores DNS locales hacia los servidores DNS de la nube privada de AVS. Para más información sobre el reenvío de DNS, consulte [Configuración de DNS para la resolución de nombres para el vCenter de la nube privada de AVS desde el entorno local](https://docs.azure.cloudsimple.com/on-premises-dns-setup/).

![Resumen de la nube privada de AVS](media/private-cloud-summary.png)

### <a name="available-actions"></a>Acciones disponibles

* [Inicie el cliente de vSphere](https://docs.azure.cloudsimple.com/vsphere-access/). Acceda a vCenter para esta nube privada de AVS.
* [Compre nodos](create-nodes.md). Agregue nodos a esta nube privada de AVS.
* [Expanda](expand-private-cloud.md). Agregue nodos a esta nube privada de AVS.
* **Actualice**. Actualice la información de esta página.
* **Eliminar**. Puede eliminar la nube privada de AVS en cualquier momento. **Antes de eliminar, asegúrese de haber hecho copias de seguridad de todos los sistemas y datos.** Al eliminar una nube privada de AVS, se eliminan todas las VM, la configuración de vCenter y los datos. Haga clic en **Eliminar** en la sección del resumen para la nube privada de AVS seleccionada. Después de su eliminación, todos los datos de la nube privada de AVS se borran en un proceso de eliminación seguro y altamente compatible.
* [Cambie los privilegios de vSphere](escalate-private-cloud-privileges.md). Escale sus privilegios en esta nube privada de AVS.

## <a name="avs-private-cloud-vlanssubnets"></a>VLAN y subredes de nubes privadas de AVS

Vea la lista de VLAN y subredes definidas para la nube privada de AVS seleccionada. En la lista se incluye la administración de VLAN y subredes que se crearon cuando se creó la nube privada de AVS.

![VLAN y subredes de nubes privadas de AVS](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Acciones disponibles

* [Agregue las VLAN y subredes](https://docs.azure.cloudsimple.com/create-vlan-subnet/). Agregue una VLAN o un subconjunto a esta nube privada de AVS.

Seleccione una VLAN o subred para las acciones siguientes:
* [Adjuntar una tabla de firewall](https://docs.azure.cloudsimple.com/firewall/). Adjunte una tabla de firewall a esta nube privada de AVS.
* **Edición**
* **Eliminar** (solo VLAN y subredes definidas por el usuario)

## <a name="avs-private-cloud-activity"></a>Actividad de la nube privada de AVS

Vea la siguiente información para la nube privada de AVS seleccionada. La información de actividades es una lista filtrada de todas las actividades de la nube privada de AVS seleccionada. Esta página muestra hasta 25 actividades recientes.

* Alertas recientes
* Eventos recientes
* Tareas recientes
* Auditoría reciente

![Nube privada de AVS: actividad](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Bastidores de nube

Los bastidores de nube son los componentes básicos de la nube privada de AVS. Cada bastidor proporciona una unidad de capacidad. AVS configura automáticamente los bastidores de nube según las selecciones hechas al crear o expandir una nube privada de AVS. Vea la lista completa de bastidores de nube, incluida la nube privada de AVS a la que está asignado cada uno.

![Nube privada de AVS: bastidores de nube](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>Red de administración de vSphere

Lista de máquinas virtuales y recursos de administración de VMware que están configurados actualmente en la nube privada de AVS. La información incluye la versión de software, el nombre de dominio completo (FQDN) y la dirección IP de los recursos.

![Nube privada de AVS: red de administración de vSphere](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Más información sobre las [nubes privadas de AVS](cloudsimple-private-cloud.md)