---
title: 'Azure VMware Solutions (AVS): protección de la nube privada de AVS'
description: Describe cómo proteger la nube privada de Azure VMware Solutions (AVS).
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b053f1493c2380153711176a4748ebf90b479a6c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020087"
---
# <a name="how-to-secure-your-avs-private-cloud-environment"></a>Cómo proteger el entorno de nube privada de AVS

Defina el control de acceso basado en rol (RBAC) para el servicio de AVS, el portal de AVS y la nube privada de AVS desde Azure. Los usuarios, grupos y roles necesarios para acceder a la instancia de vCenter de la nube privada de AVS se especifican mediante SSO de VMware. 

## <a name="rbac-for-avs-service"></a>RBAC para el servicio de AVS

La creación del servicio de AVS requiere el rol **Propietario** o **Colaborador** en la suscripción de Azure. Todos los propietarios y colaboradores pueden crear un servicio de AVS y acceder al portal de AVS de forma predeterminada para crear y administrar nubes privadas de AVS. Solo se puede crear un servicio de AVS por región. Para restringir el acceso a solo algunos administradores, realice el procedimiento que se indica a continuación.

1. Cree un servicio de AVS en un **grupo de recursos** nuevo en Azure Portal.
2. Configure RBAC en el grupo de recursos.
3. Adquiera nodos y use el mismo grupo de recursos que el servicio de AVS.

Solo los usuarios que tengan los privilegios de **Propietario** o **Colaborador** en el grupo de recursos podrán ver el servicio de AVS e iniciar el portal de AVS.

Para más información sobre RBAC, vea [¿Qué es el control de acceso basado en rol (RBAC) para los recursos de Azure?](../role-based-access-control/overview.md)

## <a name="rbac-for-avs-private-cloud-vcenter"></a>RBAC para vCenter de nube privada de AVS

Cuando se crea una nube privada de AVS, se crea un usuario predeterminado `CloudOwner@AVS.local` en el dominio de SSO de vCenter. El usuario CloudOwner tiene privilegios para administrar vCenter. Se irán agregando más orígenes de identidades al SSO de vCenter para permitir el acceso a distintos usuarios. Se configurarán roles y grupos predefinidos en vCenter para usarlos para agregar más usuarios.

### <a name="add-new-users-to-vcenter"></a>Adición de nuevos usuarios a vCenter

1. [Escale privilegios ](escalate-private-cloud-privileges.md) para el usuario **CloudOwner@AVS.local** en la nube privada de AVS.
2. Inicie sesión en vCenter con **CloudOwner@AVS.local** .
3. [Agregue usuarios de inicio de sesión único de vCenter](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-72BFF98C-C530-4C50-BF31-B5779D2A4BBB.html).
4. Agregue usuarios a [grupos de inicio de sesión único de vCenter](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html).

Para más información sobre los roles y grupos predefinidos, vea el artículo [Modelo de permisos de la nube privada de AVS de VMware vCenter](learn-private-cloud-permissions.md).

### <a name="add-new-identity-sources"></a>Adición de nuevos orígenes de identidades

Se pueden agregar más proveedores de identidades para el dominio de SSO de vCenter de la nube privada de AVS. Los proveedores de identidades facilitan la característica de autenticación y los grupos de SSO de vCenter proporcionan autorización a los usuarios.

* [Use Active Directory como proveedor de identidades](set-vcenter-identity.md) en la instancia de vCenter de la nube privada de AVS.
* [Use Azure AD como proveedor de identidades](azure-ad.md) en la instancia de vCenter de la nube privada de AVS.

1. [Escale privilegios ](escalate-private-cloud-privileges.md) para el usuario **CloudOwner@AVS.local** en la nube privada de AVS.
2. Inicie sesión en vCenter con **CloudOwner@AVS.local** .
3. Agregue usuarios desde el proveedor de identidades a los [grupos de inicio de sesión único de vCenter](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html).

## <a name="secure-network-on-your-avs-private-cloud-environment"></a>Protección de la red en el entorno de nube privada de AVS

La seguridad de red del entorno de nube privada de AVS se controla mediante la protección del acceso a la red y el control del tráfico de red entre los recursos.

### <a name="access-to-avs-private-cloud-resources"></a>Acceso a los recursos de la nube privada de AVS

El acceso a los recursos y a la instancia de vCenter de la nube privada de AVS se realiza a través de una conexión de red segura:

* **[Conexión ExpressRoute](on-premises-connection.md)** . ExpressRoute proporciona una conexión segura de baja latencia y ancho de banda elevado desde el entorno local. La conexión permite a los servicios locales, las redes y los usuarios acceder a la instancia de vCenter de la nube privada de AVS.
* **[Puerta de enlace de VPN de sitio a sitio](vpn-gateway.md)** . La VPN de sitio a sitio proporciona acceso a los recursos de la nube privada de AVS desde el entorno local a través de un túnel seguro. Hay que especificar qué redes locales pueden enviar y recibir tráfico de red a la nube privada de AVS.
* **[Puerta de enlace de VPN de punto a sitio](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)** . Use una conexión VPN de punto a sitio para acceder de forma remota rápida a la instancia de vCenter de la nube privada de AVS.

### <a name="control-network-traffic-in-avs-private-cloud"></a>Control del tráfico de red en la nube privada de AVS

Las reglas y tablas de firewall controlan el tráfico de red en la nube privada de AVS. La tabla de firewall permite controlar el tráfico de red entre una red o una dirección IP de origen y una red o dirección IP de destino en función de la combinación de reglas definida en la tabla en cuestión.

1. Cree una [tabla de firewall](firewall.md#add-a-new-firewall-table).
2. [Agregue reglas](firewall.md#create-a-firewall-rule) a la tabla de firewall.
3. [Asocie una tabla de firewall a una VLAN/subred](firewall.md#attach-vlanssubnet.
