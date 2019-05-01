---
title: Arquitectura de referencia de la empresa para Azure DevTest Labs
description: Este artículo proporciona orientación de arquitectura de referencia para Azure DevTest Labs en una empresa.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 73a3d426e9040525b0c631db273e59c49a6a9eb0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705888"
---
# <a name="azure-devtest-labs-reference-architecture-for-enterprises"></a>Arquitectura de referencia de Azure DevTest Labs para empresas
En este artículo se proporciona la arquitectura de referencia para ayudarle a implementar una solución basada en Azure DevTest Labs en una empresa. Incluye lo siguiente:
- Conectividad local a través de ExpressRoute de Azure
- Una puerta de enlace de escritorio remoto para iniciar sesión remotamente en máquinas virtuales
- Conectividad a un repositorio de artefactos para los artefactos privados
- Otros servicios de PaaS que se usan en los laboratorios

![Diagrama de arquitectura de referencia](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Arquitectura
Estos son los elementos clave de la arquitectura de referencia:

- **Azure Active Directory (Azure AD)**: DevTest Labs usa el [servicio de Azure AD para la administración de identidad](../active-directory/fundamentals/active-directory-whatis.md). Cuando concede a los usuarios acceso a un entorno basado en DevTest Labs, tenga en cuenta estos dos aspectos clave:
    - **Administración de recursos**: Proporciona acceso a Azure portal para administrar los recursos (creación de máquinas virtuales; crear entornos; iniciar, detener, reiniciar, eliminar y aplicar artefactos etc.). Administración de recursos se realiza en Azure mediante control de acceso basado en roles (RBAC). Asignar roles a usuarios y establecer permisos de nivel de acceso y recursos.
    - **Las máquinas virtuales (nivel de red)**: En la configuración predeterminada, las máquinas virtuales use una cuenta de administrador local. Si hay un dominio ([Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), un dominio local o un dominio en la nube), las máquinas pueden unirse al dominio. Los usuarios, a continuación, pueden usar sus identidades basado en dominio para conectarse a las máquinas virtuales.
- **Conectividad local**: En nuestro diagrama de arquitectura, [ExpressRoute](../expressroute/expressroute-introduction.md) se utiliza. Pero también puede usar un [VPN de sitio a sitio](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). Aunque ExpressRoute no es necesario para DevTest Labs, se utiliza habitualmente en las empresas. ExpressRoute es necesario sólo si necesita acceso a recursos corporativos. Escenarios comunes son:
    - Tiene datos locales que no se puede mover a la nube.
    - Prefiere unir las máquinas del laboratorio al dominio local.
    - Desea forzar todo el tráfico de red dentro y fuera del entorno de nube a través de un firewall local para la seguridad y cumplimiento normativo.
- **Grupos de seguridad de red**: Una manera común para restringir el tráfico en el entorno de nube (o en el entorno de nube) en función de origen y las direcciones IP de destino consiste en utilizar un [grupo de seguridad de red](../virtual-network/security-overview.md). Por ejemplo, desea permitir únicamente el tráfico que se origina en la red corporativa en redes del laboratorio.
- **La puerta de enlace de escritorio remoto**: Las empresas normalmente bloquean conexiones salientes de escritorio remoto en el firewall corporativo. Hay varias opciones para habilitar la conectividad con el entorno en la nube en DevTest Labs, incluidos:
  - Use un [puerta de enlace de escritorio remoto](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)y la lista blanca la dirección IP estática de la puerta de enlace de equilibrador de carga.
  - [Dirigir todo el tráfico RDP entrante](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) a través de la conexión VPN de ExpressRoute/sitio a sitio. Esta funcionalidad es una consideración habitual cuando las empresas a planear una implementación de DevTest Labs.
- **(Redes virtuales, subredes) de servicios de red**: El [redes de Azure](../networking/networking-overview.md) topología es otro elemento clave en la arquitectura de DevTest Labs. Controla si los recursos del laboratorio de pueden comunicarse y tener acceso a un entorno local y a internet. Nuestro diagrama de arquitectura incluye las formas más comunes que los clientes usan DevTest Labs: Todos los laboratorios que se conectan a través de [emparejamiento de redes virtuales](../virtual-network/virtual-network-peering-overview.md) mediante el uso de un [modelo radial](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) para la conexión VPN de ExpressRoute/sitio a sitio en el entorno local. Pero DevTest Labs usa la red Virtual de Azure directamente, por lo que no hay ninguna restricción sobre cómo configurar la infraestructura de red.
- **DevTest Labs**:  DevTest Labs es una parte fundamental de la arquitectura global. Para obtener más información sobre el servicio, consulte [sobre DevTest Labs](devtest-lab-overview.md).
- **Las máquinas virtuales y otros recursos (SaaS, PaaS, IaaS)**:  Las máquinas virtuales son una carga de trabajo clave que DevTest Labs es compatible con junto con otros recursos de Azure. DevTest Labs hace fácil y rápido para que una empresa proporcionar acceso a los recursos de Azure (incluidas las máquinas virtuales y otros recursos de Azure). Más información acerca del acceso a Azure para [desarrolladores](devtest-lab-developer-lab.md) y [evaluadores](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Consideraciones sobre escalabilidad
A pesar de DevTest Labs no tiene límites o cuotas integradas, tienen otros recursos de Azure que se usan en la operación típica de un laboratorio [las cuotas de nivel de suscripción](../azure-subscription-service-limits.md). Por lo tanto, en una implementación empresarial típico, tiene varias suscripciones de Azure para cubrir una gran implementación de DevTest Labs. Las cuotas que las empresas a alcanzar con más frecuencia son:

- **Grupos de recursos**: En la configuración predeterminada, DevTest Labs crea un grupo de recursos para cada nueva máquina virtual o el usuario crea un entorno con el servicio. Las suscripciones pueden contener [980 hasta grupos de recursos](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager). Por lo tanto, que es el límite de máquinas virtuales y entornos en una suscripción. Hay dos otras configuraciones que se deben considerar:
    - **[Vaya de todas las máquinas virtuales al mismo grupo de recursos](resource-group-control.md)**: Aunque esta configuración le ayuda a cumplir el límite del grupo de recursos, afecta el límite de recurso de tipo por recurso de grupo.
    - **Uso compartido de direcciones IP públicas**: Todas las máquinas virtuales del mismo tamaño y la región entran en el mismo grupo de recursos. Esta configuración es un "punto medio" entre las cuotas del grupo de recursos y las cuotas del recurso tipo por recurso de grupo, si las máquinas virtuales pueden tener direcciones IP públicas.
- **Grupo de recursos por recurso por tipo de recurso**: El límite predeterminado para [recursos por grupo de recursos por tipo de recurso es de 800](../azure-subscription-service-limits.md#resource-group-limits).  Cuando se usa el *vaya todas las máquinas virtuales al mismo grupo de recursos* configuración, el posicionamiento de los usuarios esta suscripción limitar más rápidamente, especialmente si las máquinas virtuales tienen muchos discos adicionales.
- **Cuentas de almacenamiento**: Un laboratorio de DevTest Labs viene con una cuenta de almacenamiento. La cuota de Azure para [número de cuentas de almacenamiento por región por suscripción es de 250](../azure-subscription-service-limits.md#storage-limits). El número máximo de DevTest Labs en la misma región también es 250.
- **Las asignaciones de roles**: Una asignación de roles es cómo proporcionar un usuario o entidad de seguridad de acceso a un recurso (propietario, recursos, nivel de permiso). En Azure, hay un [límite de 2000 asignaciones de roles por suscripción](../azure-subscription-service-limits.md#role-based-access-control-limits). De forma predeterminada, el servicio DevTest Labs crea un grupo de recursos para cada máquina virtual. El propietario se designe *propietario* permiso para la máquina virtual de DevTest Labs y *lector* permiso al grupo de recursos. De este modo, cada nueva máquina virtual que cree utiliza dos asignaciones de roles, además de las asignaciones que se usan al proporcionar a los usuarios permiso para el laboratorio.
- **Lecturas y escrituras de API**: Hay varias maneras de automatizar Azure y DevTest Labs, incluidas las API de REST, PowerShell, CLI de Azure y Azure SDK. Gracias a la automatización, podría alcanzar otro límite en las solicitudes de API: Cada suscripción permite hasta [leer de 12.000 solicitudes por hora de escritura de las solicitudes y 1.200](../azure-resource-manager/resource-manager-request-limits.md). Tenga en cuenta este límite la automatización de DevTest Labs.

## <a name="manageability-considerations"></a>Consideraciones sobre la manejabilidad
DevTest Labs tiene una interfaz de usuario administrativo excelentes para trabajar con un único laboratorio. Pero en una empresa, es probable que tenga varias suscripciones de Azure y cuántos laboratorios. Realizar cambios de forma coherente en todos los laboratorios requiere scripting y automatización. Estos son algunos ejemplos y procedimientos recomendados de administración para una implementación de DevTest Labs:

- **Los cambios de configuración de laboratorio**: Un escenario común es actualizar un entorno de laboratorio específico en todos los laboratorios en la implementación. Por ejemplo, un tamaño de instancia de máquina virtual nueva está disponible y se deben actualizar todos los laboratorios para permitirlo. Es mejor automatizar estos cambios mediante el uso de scripts de PowerShell, CLI o API de REST.  
- **Token de acceso personal del repositorio de artefactos**:  Normalmente, los tokens de acceso personal para un repositorio de Git expiren en 90 días, un año o dos años. Para garantizar la continuidad, es importante ampliar el token de acceso personal o crear uno nuevo. A continuación, usar automation para el nuevo token de acceso personal se aplican a todos los laboratorios.
- **Restringir los cambios en un entorno de laboratorio**: A menudo, una configuración concreta debe estar restringida (por ejemplo, lo que permite el uso de imágenes de marketplace). Puede usar Azure Policy para impedir cambios en un tipo de recurso. O bien puede crear un rol personalizado y conceder a los usuarios de ese rol en lugar de la *propietario* rol para el laboratorio. Puede hacerlo para la mayoría de las configuraciones en el laboratorio (soporte interno, anuncio del laboratorio, permite tamaños de máquina virtual y así sucesivamente).
- **Requerir máquinas virtuales seguir una convención de nomenclatura**: Normalmente, los administradores desean identificar fácilmente las máquinas virtuales que forman parte de un entorno de prueba y desarrollo basado en la nube. Puede hacerlo mediante el uso de [Azure Policy](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

Es importante tener en cuenta que DevTest Labs usa recursos de Azure subyacentes que se administran del mismo modo: redes, discos, proceso y así sucesivamente. Por ejemplo, la directiva de Azure se aplica a máquinas virtuales que se crean en un laboratorio. Azure Security Center puede informar sobre el cumplimiento de la máquina virtual. Y el servicio Azure Backup puede proporcionar copias de seguridad periódicas para las máquinas virtuales en el laboratorio.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad
Azure DevTest Labs usa los recursos existentes en Azure (compute, redes etc.). Por lo que automáticamente se beneficia de las características de seguridad que están integradas en la plataforma. Por ejemplo, para requerir conexiones a Escritorio remoto entrantes que se origine solo desde la red corporativa, basta con agregar un grupo de seguridad de red a la red virtual en la puerta de enlace de escritorio remoto. La consideración de seguridad adicional sólo es el nivel de permisos que concede a los miembros del equipo que usan los laboratorios en forma diaria. Los permisos más comunes son [ *propietario* y *usuario*](devtest-lab-add-devtest-user.md). Para obtener más información sobre estos roles, consulte [agregar propietarios y usuarios en Azure DevTest Labs](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Pasos siguientes
Lea el siguiente artículo de esta serie: [Escalar verticalmente la infraestructura de Azure DevTest Labs](devtest-lab-guidance-scale.md).
