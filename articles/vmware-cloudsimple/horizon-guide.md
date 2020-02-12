---
title: 'Azure VMware Solutions (AVS): uso del sitio de la nube privada de AVS para hospedar una infraestructura de escritorio virtual mediante VMware Horizon'
description: Describe cómo puede usar su sitio de la nube privada de AVS para hospedar una infraestructura de escritorio virtual mediante VMware Horizon.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f909ce297344e80ed6004631d5218d6a7fcd5085
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025255"
---
# <a name="use-avs-private-cloud-site-to-host-a-virtual-desktop-infrastructure-using-vmware-horizon"></a>Uso del sitio de la nube privada de AVS para hospedar una infraestructura de escritorio virtual mediante VMware Horizon

Puede usar el sitio de la nube privada de AVS para hospedar una infraestructura de escritorio virtual (VDI) mediante VMware Horizon 7.x. En la ilustración siguiente se muestra la arquitectura de la solución lógica para VDI.

![Implementación de Horizon](media/horizon-deployment.png)

Con esta solución, tiene control total respecto a Horizon View Manager y App Volumes. La UI y las interfaces conocidas de la API y la CLI permiten el uso de los scripts y las herramientas existentes.

La solución AVS requiere que haga lo siguiente:

* Instale, configure y administre VMware Horizon 7.x en su nube privada de AVS.
* Proporciónese sus propias licencias de Horizon.

## <a name="deploy-the-solution"></a>Implementación de la solución

En las secciones siguientes se describe cómo implementar una solución de VDI con Horizon en la nube privada de AVS.

1. [Verificar que las versiones de los productos de VMware son compatibles](#verify-that-vmware-product-versions-are-compatible)
2. [Calcular el tamaño del entorno de escritorio](#estimate-the-size-of-your-desktop-environment)
3. [Crear una nube privada de AVS para su entorno](#create-an-avs-private-cloud-for-your-environment)
4. [Instalar VMware Horizon en la nube privada de AVS](#install-vmware-horizon-in-your-avs-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>Verificar que las versiones de los productos de VMware son compatibles

* Verifique que las versiones actuales y planeadas de Horizon, App Volumes, Unified Access Gateway y User Environment Manager sean compatibles entre sí y con vCenter y PSC en la nube privada de AVS. Para obtener información sobre la compatibilidad, consulte [Matriz de compatibilidad de VMware para Horizon 7.5](https://www.vmware.com/resources/compatibility/sim/interop_matrix.php#interop&260=2877&0=).
* Para averiguar las versiones actuales de vCenter y PSC en la nube privada de AVS, vaya a **Recursos** en el [portal de AVS](access-cloudsimple-portal.md), seleccione la nube privada de AVS y haga clic en la pestaña **Red de administración de vSphere**.

![Versiones de vCenter y PSC](media/private-cloud-vsphere-versions.png)

### <a name="estimate-the-size-of-your-desktop-environment"></a>Calcular el tamaño del entorno de escritorio

* Verifique que la configuración identificada se encuentre dentro de los límites operativos de VMware.
* Estime los recursos necesarios para todos los equipos de escritorio y los componentes de administración de Horizon.

### <a name="create-an-avs-private-cloud-for-your-environment"></a>Crear una nube privada de AVS para su entorno

1. Cree una nube privada de AVS desde el portal de AVS siguiendo las instrucciones que se proporcionan en el artículo [Configuración del entorno de una nube privada de AVS](quickstart-create-private-cloud.md). AVS crea un usuario predeterminado de vCenter denominado "cloudowner" en cada nube privada de AVS recién creada. Para obtener detalles sobre el modelo predeterminado de permisos y usuarios de la nube privada de AVS, consulte [Información sobre el modelo de permisos de la nube privada de AVS](learn-private-cloud-permissions.md).
2. Cree una VLAN en la nube privada de AVS para el plano de administración de Horizon y asígnele un valor de CIDR de subred. Para obtener instrucciones, consulte el artículo [Creación y administración de subredes y VLAN.](create-vlan-subnet.md) Esta es la red en que se instalarán todos los componentes de las soluciones (Unified Access Gateway, Connection Server, App Volume Server y los servidores de User Environment Manager).
3. Decida si quiere utilizar un proveedor de identidades externo con la instancia de vCenter de la nube privada de AVS. En caso afirmativo, elija una de estas opciones:
    * Use la instancia local de Active Directory como proveedor de identidades externo. Para obtener instrucciones, consulte [Orígenes de identidades de vCenter](set-vcenter-identity.md).
    * Configure un servidor de Active Directory en la nube privada de AVS de la VLAN del plano de administración de Horizon para usarlo como proveedor de identidades externo. Para obtener instrucciones, consulte [Orígenes de identidades de vCenter](set-vcenter-identity.md).
    * Configure un servidor DHCP y DNS en la VLAN del plano de administración de Horizon de la nube privada de AVS. Para obtener instrucciones, consulte [Configuración de cargas de trabajo y aplicaciones de DNS y DHCP en la nube privada de AVS](dns-dhcp-setup.md).
4. Configure el reenvío de DNS en el servidor DNS instalado en la nube privada de AVS. Para obtener instrucciones, consulte [Creación de reenviador condicional](on-premises-dns-setup.md#create-a-conditional-forwarder).

### <a name="install-vmware-horizon-in-your-avs-private-cloud"></a>Instalar VMware Horizon en la nube privada de AVS

En el siguiente diagrama de implementación se describe una solución de Horizon implementada en una nube privada de AVS. Unified Access Gateway, AD/DC, View y App Volume Server están instalados en la VLAN 234 creada por el usuario. Unified Access Gateway tiene una dirección IP pública asignada que es accesible desde Internet. Las máquinas virtuales del grupo de escritorios de Horizon se implementan en la VLAN 235 para proporcionar aislamiento y seguridad adicionales.

![Implementación de Horizon en la nube privada de AVS](media/horizon-private-cloud.png)

En las secciones siguientes se detallan las instrucciones para configurar una implementación similar a la que se representa en la ilustración. Antes de comenzar, verifique que dispone de lo siguiente:

* Una nube privada de AVS creada mediante el portal de AVS con capacidad suficiente para ejecutar grupos de escritorios.
* Suficiente ancho de banda entre el entorno local y el de la nube privada de AVS para admitir el tráfico de red de los escritorios.
* Un túnel VPN de sitio a sitio configurado entre su centro de datos local y la nube privada de AVS.
* Disponibilidad de IP desde las subredes del usuario final del entorno local a las subredes de la nube privada de AVS.
* AD/DHCP/DNS instalados para la nube privada de AVS.

#### <a name="avs-portal-create-a-dedicated-vlansubnet-for-desktop-pools"></a>Portal de AVS: crear una VLAN o subred dedicada para grupos de escritorios

Cree una VLAN para los grupos de escritorios de Horizon y asígnele un valor de CIDR de subred. Para obtener instrucciones, consulte el artículo [Creación y administración de subredes y VLAN.](create-vlan-subnet.md) Esta es la red en la que se ejecutarán todas las máquinas virtuales de escritorio.

Siga los procedimientos recomendados de seguridad estándar para proteger la implementación de Horizon:

* Permita solo el tráfico de SSH o de RDP de escritorio a las máquinas virtuales de escritorio.
* Permita solo el tráfico de administración entre la VLAN del plano de administración de Horizon y la VLAN del grupo de escritorios.
* Permita solo el tráfico de administración desde la red local.

Puede aplicar estos procedimientos recomendados mediante la configuración de [reglas de firewall](firewall.md) desde el portal de AVS.

#### <a name="avs-portal-configure-firewall-rules-to-secure-horizon-management-plane"></a>Portal de AVS: configurar reglas de firewall para proteger el plano de administración de Horizon

Configure las siguientes reglas en el portal de AVS. Para obtener instrucciones, consulte [Configuración de tablas y reglas de firewall](firewall.md).

1. Configure las reglas de firewall en el firewall N-S de AVS para permitir la comunicación entre las subredes locales y la VLAN de administración de Horizon y, de este modo, solo se permitan los puertos de red que se enumeran en la [lista de puertos de Horizon](https://docs.vmware.com/en/VMware-Horizon-7/7.1/com.vmware.horizon-client-agent.security.doc/GUID-52807839-6BB0-4727-A9C7-EA73DE61ADAB.html) del documento de VMware.

2. Cree las reglas de firewall E-W entre la VLAN de administración de Horizon y la VLAN del grupo de escritorios de la nube privada de AVS.

#### <a name="avs-portal-create-a-public-ip-address-for-unified-access-gateway"></a>Portal de AVS: crear una dirección IP pública para Unified Access Gateway

Cree una dirección IP pública para el dispositivo Unified Access Gateway a fin de habilitar las conexiones de cliente de escritorio desde Internet. Para obtener instrucciones, consulte el artículo [Asignación de direcciones IP públicas](public-ips.md).

Una vez se completa la instalación, la dirección IP pública se asigna y se muestra en la página IP públicas.

#### <a name="avs-portal-escalate-privileges"></a>Portal de AVS: Escalado de privilegios

El usuario "cloudowner" predeterminado no tiene privilegios suficientes en la instancia de vCenter de la nube privada de AVS para instalar Horizon, por lo que se deben escalar los privilegios de vCenter del usuario. Para obtener más información, consulte [Escalar privilegios](escalate-private-cloud-privileges.md).

#### <a name="vcenter-ui-create-a-user-in-avs-private-cloud-for-horizon-installation"></a>Interfaz de usuario de vCenter: crear un usuario en la nube privada para la instalación de Horizon

1. Inicie sesión en vCenter con las credenciales de usuario de "cloudowner".
2. Cree un nuevo usuario, "horizon-soln-admin", en vCenter y agréguelo al grupo de administradores de vCenter.
3. Cierre sesión de vCenter con el usuario "cloudowner" e iníciela con el usuario "horizon-soln-admin".

#### <a name="vcenter-ui-install-vmware-horizon"></a>Interfaz de usuario de vCenter: instalar VMware Horizon

Tal y como se mencionó en la sección de arquitectura lógica anterior, la solución de Horizon tiene los siguientes componentes:

* VMware Horizon View
* VMware Unified Access Gateway
* VMware App Volume Manager
* VMware User Environment Manager

Instale los componentes como se indica a continuación:

1. Para instalar y configurar Unified Access Gateway, siga las instrucciones que se proporcionan en el documento [Implementar y configurar VMware Unified Access Gateway](https://docs.vmware.com/en/Unified-Access-Gateway/3.3.1/com.vmware.uag-331-deploy-config.doc/GUID-F5CE0D5E-BE85-4FA5-BBCF-0F86C9AB8A70.html) de VMware.

2. Para instalar Horizon View en la nube privada de AVS, siga las instrucciones que se indican en el artículo [Guía de instalación de View](https://docs.vmware.com/en/VMware-Horizon-7/7.4/horizon-installation/GUID-37D39B4F-5870-4188-8B11-B6C41AE9133C.html).

3. Para instalar App Volume Manager, siga las instrucciones del artículo [Instalar y configurar VMware App Volumes](https://docs.vmware.com/en/VMware-App-Volumes/2.10/com.vmware.appvolumes.user.doc/GUID-5E8BAF8C-F5A6-412C-9424-266BA7109BA4.html).

4. Para instalar y configurar User Environment Manager, siga las instrucciones incluidas en [Cómo instalar y configurar VMware User Environment Manager](https://docs.vmware.com/en/VMware-User-Environment-Manager/9.4/com.vmware.user.environment.manager-install-config/GUID-DBBC82E4-483F-4B28-9D49-4D28E08715BC.html).

#### <a name="file-a-support-request-to-upload-vmware-horizon-pre-packaged-app-volumes"></a>Archive una solicitud de soporte técnico para cargar los volúmenes de las aplicaciones preconfiguradas de VMware Horizon.

Como parte del proceso de instalación, App Volume Manager usa volúmenes empaquetados previamente para aprovisionar pilas de aplicaciones y volúmenes de escritura. Estos volúmenes sirven como plantillas para las pilas de aplicaciones y los volúmenes de escritura.

La carga de volúmenes en el almacén de datos de la nube privada de AVS requiere la contraseña raíz de ESXi. Para obtener ayuda, envíe una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Adjunte el conjunto del instalador de App Volumes para que el personal de soporte técnico de AVS pueda cargar las plantillas en su entorno de nube privada AVS.

#### <a name="avs-portal-de-escalate-privileges"></a>Portal de AVS: Anular el escalado de privilegios

Ahora puede [anular el escalado de privilegios](escalate-private-cloud-privileges.md#de-escalate-privileges) del usuario "cloudowner".

## <a name="ongoing-management-of-your-horizon-solution"></a>Administración continua de la solución de Horizon

Tiene control total sobre el software de App Volume Manager y Horizon en su entorno de nube privada de AVS y se espera que realice la administración del ciclo de vida del software necesaria. Asegúrese de que cualquier versión nueva del software sea compatible con PSC y la instancia de vCenter de la nube privada de AVS antes de actualizar Horizon o App Volume.
