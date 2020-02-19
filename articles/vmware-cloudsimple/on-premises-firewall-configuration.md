---
title: Acceso a Azure VMware Solutions (AVS) desde el entorno local
description: Acceso a Azure VMware Solutions (AVS) desde la red local a través de un firewall
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a4a9760b5c7a70c58a1afe1b14b781a35f2b9b18
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77082975"
---
# <a name="accessing-your-avs-private-cloud-environment-and-applications-from-on-premises"></a>Acceso al entorno y las aplicaciones de la nube privada de AVS desde el entorno local

Se puede configurar una conexión desde la red local a AVS con Azure ExpressRoute o VPN de sitio a sitio. Acceda a la instancia de vCenter de la nube privada de AVS y a las cargas de trabajo que ejecute en la nube privada de AVS mediante la conexión. Puede controlar qué puertos están abiertos en la conexión mediante un firewall en la red local. En este artículo se abordan algunos de los requisitos habituales de los puertos para las aplicaciones. En el caso de otras aplicaciones, consulte la documentación pertinente de esta para obtener información sobre los requisitos de los puertos.

## <a name="ports-required-for-accessing-vcenter"></a>Puertos necesarios para acceder a vCenter

Para acceder a la instancia de vCenter de la nube privada de AVS y al administrador de NSX-T, los puertos definidos en la tabla siguiente deben estar abiertos en el firewall local. 

| Port       | Source                           | Destination                      | Propósito                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | Servidores DNS locales          | Servidores DNS de la nube privada de AVS        | Se requiere para reenviar la búsqueda de DNS de *az.cloudsimple.io* a los servidores DNS de la nube privada de AVS desde la red local.     |
| 53 (UDP)   | Servidores DNS de la nube privada de AVS        | Servidores DNS locales          | Se requiere para reenviar la búsqueda de DNS de los nombres de dominio locales de la instancia de vCenter de la nube privada de AVS a los servidores DNS locales. |
| 80 (TCP)   | Red local              | Red de administración de la nube privada de AVS | Se requiere para redirigir la dirección URL de vCenter de *http* a *https*.                                                         |
| 443 (TCP)  | Red local              | Red de administración de la nube privada de AVS | Se requiere para acceder al vCenter y al administrador de NSX-T desde la red local.                                           |
| 8000 (TCP) | Red local              | Red de administración de la nube privada de AVS | Se requiere para la migración de máquinas virtuales del entorno local a la nube privada de AVS con vMotion.                                          |
| 8000 (TCP) | Red de administración de la nube privada de AVS | Red local              | Se requiere para la migración de máquinas virtuales de la nube privada de AVS al entorno local con vMotion.                                          |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>Puertos necesarios para usar Active Directory local como origen de identidades

Para configurar Active Directory local como un origen de identidades en vCenter de la nube privada de AVS, los puertos definidos en la tabla deben estar abiertos. Para ver los pasos de configuración, consulte el artículo [Uso de Azure AD como un proveedor de identidades para vCenter en la nube privada de AVS](https://docs.azure.cloudsimple.com/azure-ad/).

| Port         | Source                           | Destination                                         | Propósito                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | Servidores DNS de la nube privada de AVS        | Servidores DNS locales                             | Se requiere para reenviar la búsqueda de DNS de los nombres de dominio de Active Directory local de la instancia de vCenter de la nube privada de AVS a los servidores DNS locales.        |
| 389 (TCP/UDP) | Red de administración de la nube privada de AVS | Controladores de dominio de Active Directory local     | Se requiere para la autenticación de usuario, para la comunicación LDAP desde el servidor vCenter de la nube privada de AVS con los controladores de dominio de Active Directory.              |
| 636 (TCP)     | Red de administración de la nube privada de AVS | Controladores de dominio de Active Directory local     | Se requiere para la autenticación de usuario, para la comunicación LDAP segura (LDAPS) desde el servidor vCenter de la nube privada de AVS con los controladores de dominio de Active Directory. |
| 3268 (TCP)    | Red de administración de la nube privada de AVS | Servidores de catálogo global de Active Directory local | Se requiere para la comunicación LDAP en las implementaciones de un controlador de varios dominios.                                                                      |
| 3269 (TCP)    | Red de administración de la nube privada de AVS | Servidores de catálogo global de Active Directory local | Se requiere para la comunicación LDAPS en las implementaciones de un controlador de varios dominios.                                                                     |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>Puertos comunes necesarios para acceder a las máquinas virtuales de carga de trabajo

El acceso a las máquinas virtuales de carga de trabajo que se ejecutan en la nube privada de AVS requiere que se abran los puertos en el firewall local. En la tabla siguiente se muestran algunos de los puertos comunes necesarios y su finalidad. En cuanto a los requisitos de los puertos específicos de una aplicación, consulte la documentación pertinente de esta.

| Port         | Source                         | Destination                          | Propósito                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | Red local            | Red de carga de trabajo de la nube privada de AVS       | Proteja el acceso del shell a las máquinas virtuales Linux que se ejecutan en la nube privada de AVS.            |
| 3389 (TCP)    | Red local            | Red de carga de trabajo de la nube privada de AVS       | Escritorio remoto para máquinas virtuales Windows que se ejecutan en la nube privada de AVS.               |
| 80 (TCP)      | Red local            | Red de carga de trabajo de la nube privada de AVS       | Acceda a los servidores web implementados en las máquinas virtuales que se ejecutan en la nube privada de AVS.      |
| 443 (TCP)     | Red local            | Red de carga de trabajo de la nube privada de AVS       | Acceda a los servidores web seguros implementados en las máquinas virtuales que se ejecutan en la nube privada de AVS. |
| 389 (TCP/UDP) | Red de carga de trabajo de la nube privada de AVS | Red de Active Directory local | Conecte las máquinas virtuales Windows de carga de trabajo a un dominio de Active Directory local.     |
| 53 (UDP)      | Red de carga de trabajo de la nube privada de AVS | Red local                  | Acceso del servicio DNS para las máquinas virtuales de carga de trabajo a servidores DNS en el entorno local.       |

## <a name="next-steps"></a>Pasos siguientes

* [Creación y administración de VLAN y subredes](https://docs.azure.cloudsimple.com/create-vlan-subnet/)
* [Conexión a la red local mediante Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Configurar una VPN de sitio a sitio desde un entorno local](https://docs.azure.cloudsimple.com/vpn-gateway/)
