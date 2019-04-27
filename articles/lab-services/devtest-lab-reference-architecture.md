---
title: Arquitectura de referencia para Azure DevTest Labs para una empresa
description: En este artículo se proporciona una guía de arquitectura de referencia para Azure DevTest Labs en una empresa.
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
ms.openlocfilehash: bcb154f7cffb92ef23fc2606e1f604bb12f8d1a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561526"
---
# <a name="azure-devtest-labs---reference-architecture-for-an-enterprise"></a>Azure DevTest Labs: arquitectura de referencia para la empresa
En este artículo se proporciona una arquitectura de referencia para la implementación de una solución basada en Azure DevTest Labs en una empresa. Incluye conectividad local a través de Expressroute, una puerta de enlace de escritorio remoto para iniciar sesión remotamente en máquinas virtuales, la conectividad con un repositorio de artefactos para los artefactos privados y otros servicios de PaaS que se usan en un laboratorio.

![Arquitectura de referencia](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Arquitectura
Los elementos claves de la arquitectura de referencia son:

- **Azure Active Directory (AAD)**: Azure DevTest Labs usa el [servicio de Azure Active Directory para la administración de identidad](../active-directory/fundamentals/active-directory-whatis.md). Hay dos cuestiones clave a tener en cuenta al proporcionar acceso a un entorno basado en DevTest Labs a los usuarios:
    - **Administración de recursos**:  Proporciona acceso a Azure portal para administrar los recursos (creación de máquinas virtuales, crear entornos, start/stop/reinicio/delete/applyartifacts y así sucesivamente). Se hace en Azure mediante control de acceso basado en Roble (RBAC) y aplicando una asignación de roles para el usuario, los recursos de configuración y los permisos de nivel de acceso.
    - **Las máquinas virtuales (nivel de red)**:  En la configuración predeterminada, las máquinas virtuales use una cuenta de administrador local.  Si hay un dominio ([servicios de dominio de AAD](../active-directory-domain-services/active-directory-ds-overview.md), un dominio local o un dominio en la nube), las máquinas pueden unirse al dominio. Una vez combinadas, usuarios utilicen sus identidades basado en dominio para conectarse a las máquinas virtuales.
- **Conectividad local**: En el diagrama de arquitectura anterior, [Express Route](../expressroute/expressroute-introduction.md) i usa, pero también puede usar un [VPN de sitio a sitio](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). Aunque no es necesario para DevTest Labs, normalmente se ve en las empresas. Es obligatorio solo si hay una razón para conectarse a los recursos corporativos. Causas comunes son: 
    - Datos locales que no se pueden mover a la nube
    - Preferencias para unir las máquinas del laboratorio para el dominio local
    - Forzar todo el tráfico de red dentro y fuera del entorno de nube a través de un firewall en el entorno local por motivos de seguridad y cumplimiento normativo
- **Grupos de seguridad de red**: Una manera común para restringir el tráfico en el entorno de nube (o en el entorno de nube) en función de origen y las direcciones IP de destino consiste en utilizar un [grupo de seguridad de red](../virtual-network/security-overview.md). Por ejemplo, lo que permite el tráfico de red que se origina en la red corporativa en redes del laboratorio.
- **La puerta de enlace de escritorio remoto**:  Las empresas normalmente bloquean conexiones salientes de escritorio remoto en el firewall corporativo. Para habilitar la conectividad con el entorno en la nube en DevTest Labs, hay varias opciones, como el uso un [puerta de enlace de escritorio remoto](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture) (lista blanca de direcciones IP estáticas para el equilibrador de carga de la puerta de enlace) o [dirigir todas las El tráfico RDP](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) a través de la conexión VPN de Express Route/sitio a sitio. Es una consideración habitual al planear una implementación de DevTest Labs en la empresa.
- **Las redes de Azure (redes virtuales, subredes)**:  El [redes de Azure](../networking/networking-overview.md) topología es otro elemento clave en la arquitectura general de DevTest Labs. Permite que los recursos de laboratorios de comunicarse (o no), acceso a un entorno local (o no) y tener acceso a internet (o no). El diagrama de arquitectura incluye la manera más común de los clientes usan DevTest Labs (conectados a través de todos los laboratorios [emparejamiento de VNet](../virtual-network/virtual-network-peering-overview.md) mediante un [modelo radial](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) para la conexión VPN de Express Route/sitio a sitio en el entorno local), pero desde DevTest Labs usa redes de Azure directamente no existen restricciones sobre cómo configurar la infraestructura de red.
- **DevTest Labs**:  DevTest Labs es una parte fundamental de la arquitectura global. Para obtener información sobre el servicio, consulte [sobre DevTest Labs](devtest-lab-overview.md).
- **Las máquinas virtuales y otros recursos (SaaS, PaaS, IaaS)**:  Una de las cargas de trabajo claves compatibles con DevTest Labs son máquinas virtuales junto con otros recursos de Azure.  DevTest Labs hace fácil y rápido para una empresa proporcionar acceso a los recursos de Azure (incluidas las máquinas virtuales y otros recursos de Azure).  Más información acerca del acceso a Azure para [desarrolladores](devtest-lab-developer-lab.md) y [evaluadores](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Consideraciones sobre escalabilidad
Aunque DevTest Labs no tiene integradas cuotas ni límites, otros recursos de Azure se usan en la típica tiene operaciones de un laboratorio [las cuotas de nivel de suscripción](../azure-subscription-service-limits.md). Como resultado, en una implementación empresarial típico, debe tener varias suscripciones de Azure para cubrir una gran implementación de DevTest Labs. Las cuotas de acceder a las empresas con más frecuencia son:

- **Grupos de recursos**:  En la configuración predeterminada, DevTest Labs crea un grupo de recursos para cada nueva máquina virtual o en un entorno que crea un usuario con el servicio. Las suscripciones pueden contener un [máximo 980 grupos de recursos](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager), por lo que este límite es el límite superior de máquinas virtuales y entornos en una suscripción. Hay dos otras configuraciones que debe considerar:
    - **[Vaya de todas las máquinas virtuales al mismo grupo de recursos](resource-group-control.md)**:  Aunque puede ayudarle con el límite del grupo de recursos, afecta el tipo de recurso por límite de grupo de recursos.
    - **Uso compartido de direcciones IP públicas**:  Todas las máquinas virtuales del mismo tamaño y la misma región entran en el mismo grupo de recursos. Es un término medio entre las cuotas del grupo de recursos y el tipo de recurso por las cuotas del grupo de recursos si las máquinas virtuales pueden tener direcciones IP públicas. 
- **Grupo de recursos por recurso por tipo de recurso**: El límite predeterminado para [recursos por grupo de recursos por tipo de recurso es de 800](../azure-subscription-service-limits.md#resource-group-limits).  Cuando el uso de todas las máquinas virtuales que se vaya a la misma configuración de grupo de recursos, llegarán a los usuarios este límite de suscripción mucho antes, especialmente si las máquinas virtuales tienen muchos discos adicionales.
- **Cuentas de almacenamiento**: Un laboratorio de DevTest Labs viene con una cuenta de almacenamiento y la cuota de Azure para [número de cuentas de almacenamiento por región por suscripción es de 250](../azure-subscription-service-limits.md#storage-limits). Significa que el límite superior para el número de DevTest Labs en la misma región también es 250.
- **Las asignaciones de roles**: Una asignación de roles es cómo proporcionar a un usuario o entidad de seguridad de acceso a un recurso (propietario, recursos, nivel de permiso). En Azure, un [límite de 2000 asignaciones de roles por suscripción](../azure-subscription-service-limits.md#role-based-access-control-limits). El servicio DevTest Labs (en la configuración predeterminada), crea un grupo de recursos para cada máquina virtual y el propietario se concederá el **propietario** permiso para la máquina virtual de DevTest Labs y **lector** permiso para el grupo de recursos.  De este modo, cada nueva máquina creada utiliza dos asignaciones de roles adicionales para las asignaciones de roles que se crean al conceder a los usuarios permiso para el laboratorio.
- **Lecturas y escrituras de API**: Hay varias maneras (API de REST, PowerShell, CLI, SDK de Azure etc.) para automatizar Azure y DevTest Labs y a través de la automatización es posible que alcance el límite de otro en las solicitudes de API. Cada suscripción permite hasta [12000 leer las solicitudes por hora de escritura de las solicitudes y 1200](../azure-resource-manager/resource-manager-request-limits.md).  Es un límite para tener en cuenta cuando la automatización de DevTest Labs.

## <a name="manageability-considerations"></a>Consideraciones sobre la manejabilidad
DevTest Labs tiene una interfaz de usuario administrativo excelente cuando se trabaja con un único laboratorio. En una empresa, es probable que hay varias suscripciones de Azure y cuántos laboratorios. Significa que realizar cambios en todos los laboratorios de forma coherente requiere scripting y automatización.  Estos son algunos ejemplos y mejores formas de administrar la implementación de DevTest Labs:

- **Los cambios de configuración de laboratorio**: Un escenario común es actualizar un entorno de laboratorio específico a través de todos los laboratorios en la implementación. Por ejemplo, un tamaño de instancia de máquina virtual nueva está disponible y se deben actualizar todos los laboratorios para permitirlo.  Es mejor automatizar estos cambios mediante el uso de scripts de Azure PowerShell, CLI de Azure o las API de REST.  
- **Token de acceso personal del repositorio de artefactos**:  Normalmente, los tokens de acceso personal para un repositorio de Git expiran (90 días, 1 año, 2 años). Para garantizar la continuidad, es importante ampliar el token de acceso personal o crear uno nuevo y utilizar la automatización para aplicar el nuevo token de acceso personal a todos los laboratorios.
- **Restringir los cambios en un entorno de laboratorio**:  Suele ser el caso donde hay una configuración determinada (por ejemplo, permitiendo imágenes de Marketplace que se usará) que debe estar restringida. Se pueden realizar a través de la directiva de Azure (que impidan realizar cambios a ese tipo de recurso) o mediante la creación de un rol personalizado y conceder a ese rol en lugar de "owner" al laboratorio. Se puede realizar para la mayoría de la configuración del laboratorio (soporte interno, anuncio del laboratorio, tamaños de máquina virtual permitidos etc.)
- **Necesidad de las máquinas virtuales siguen una convención de nomenclatura**: Es una solicitud común para identificar fácilmente las máquinas virtuales que forman parte del entorno de prueba y desarrollo basado en la nube. Puede hacerlo [con Azure Policy](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

Es importante tener en cuenta que DevTest Labs usa los recursos subyacentes en Azure (redes, discos, proceso, etc.) que se administran de la misma manera en Azure.  Por ejemplo, la directiva de Azure se aplica a las máquinas virtuales creadas en un laboratorio. El centro de seguridad de Azure puede informar sobre el cumplimiento de la máquina virtual. El servicio Azure Backup puede proporcionar copias de seguridad periódicas para las máquinas virtuales en el laboratorio y así sucesivamente. 

## <a name="security-considerations"></a>Consideraciones sobre la seguridad
Azure DevTest Labs usa los recursos existentes en Azure (compute, redes etc.) y, por tanto, automáticamente se beneficia de todas las características de gran seguridad integradas para la plataforma. Por ejemplo, para proteger las conexiones a Escritorio remoto entrantes que sólo se origine desde la red corporativa, es tan sencillo como agregar un grupo de seguridad de red a la red virtual en la puerta de enlace de escritorio remoto. La consideración de seguridad adicional solo para Azure DevTest Labs es el nivel de permisos que proporciona a los miembros del equipo que van a usar los laboratorios en una base de un día para otro.  Los permisos comunes proporcionados son ["Propietario" y "Usuario de DevTest Labs"](devtest-lab-add-devtest-user.md). Para obtener más información sobre estos roles, consulte [agregar propietarios y usuarios en Azure DevTest Labs](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Pasos siguientes
Lea el siguiente artículo de esta serie: [Escalar verticalmente la infraestructura de Azure DevTest Labs](devtest-lab-guidance-scale.md)
