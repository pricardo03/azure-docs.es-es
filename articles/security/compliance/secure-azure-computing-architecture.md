---
title: Protección de la arquitectura informática de Azure
description: En esta arquitectura de referencia para una arquitectura de red perimetral de nivel empresarial se usan aplicaciones virtuales de red y otras herramientas. Esta arquitectura se diseñó para cumplir los requisitos funcionales de la protección de la arquitectura de informática en la nube del Department of Defense. También puede usarse para cualquier organización. Esta referencia incluye dos opciones automatizadas que usan dispositivos Citrix o F5.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: a12f5643c96b855d07bd038fcc96100a87f1252d
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71001874"
---
# <a name="secure-azure-computing-architecture"></a>Protección de la arquitectura informática de Azure

Datos de agencias y Los clientes del Department of Defense (DoD) que implementan cargas de trabajo en Azure han pedido instrucciones para configurar redes virtuales seguras y configurar las herramientas y los servicios de seguridad estipulados por los estándares y las prácticas del DoD. 

Defense Information System Agency (DISA) publicó [Secure Cloud Computing Architecture (SCCA) Functional Requirements Document (FRD)](https://dl.dod.cyber.mil/wp-content/uploads/cloud/pdf/SCCA_FRD_v2-9.pdf) (Documento de requisitos funcionales (FRD) de la protección de la arquitectura de informática en la nube [SCCA]) en 2017. SCCA describe los objetivos funcionales para proteger la red Defense Information System Network (DISN) y los puntos de conexión del proveedor de nube comercial. SCCA también describe cómo los propietarios de la misión protegen las aplicaciones en la nube en el límite de la conexión. Todas las entidades del DoD que se conectan a la nube comercial deben seguir las instrucciones establecidas en SCCA FRD.
 
SCCA tiene cuatro componentes:
 
- Punto de acceso a la nube límite (BCAP)
- Pila de seguridad de centro de datos virtual (VDSS)
- Servicio administrado del centro de datos virtual (VDMS)
- Administrador de credenciales de nube de confianza (TCCM) 

Microsoft ha desarrollado una solución que cumple los requisitos de SCCA para las cargas de trabajo IL4 y IL5 que se ejecutan en Azure. Esta solución específica de Azure se llama Protección de la arquitectura informática de Azure (SACA). Los clientes que implementen SACA serán conformes a SCCA FRD. Pueden permitir que los clientes del DoD muevan las cargas de trabajo a Azure después de que se conecten.

La guía y las arquitecturas SCCA son específicas para los clientes del DoD, pero las revisiones más recientes de SACA ayudan a los clientes civiles a cumplir con la guía de conexión segura a Internet (TIC). Las revisiones más recientes también ayudan a los clientes comerciales que quieren implementar una red perimetral segura para proteger sus entornos de Azure.


## <a name="secure-cloud-computing-architecture-components"></a>Protección de los componentes de la arquitectura de informática en la nube

### <a name="bcap"></a>BCAP

El propósito de BCAP es proteger la DISN frente a ataques que se originan en el entorno de nube. BCAP realiza la detección y prevención de intrusiones. También filtra el tráfico no autorizado. Este componente puede coubicarse con otros componentes de SCCA. Le recomendamos que implemente este componente mediante el uso de hardware físico. Los requisitos de seguridad de BCAP se muestran en la tabla siguiente.

#### <a name="bcap-security-requirements"></a>Requisitos de seguridad de BCAP

![Matriz de requisitos de BCAP](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

El propósito de VDSS es proteger las aplicaciones de propietario de la misión del DoD que se hospedan en Azure. VDSS realiza la mayor parte de las operaciones de seguridad en SCCA. Lleva a cabo la inspección del tráfico para proteger las aplicaciones que se ejecutan en Azure. Este componente se puede proporcionar en el entorno de Azure.

#### <a name="vdss-security-requirements"></a>Requisitos de seguridad de VDSS

![Matriz de requisitos de VDSS](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

El propósito de VDMS consiste en proporcionar seguridad de host y servicios de centro de datos compartidos. Las funciones de VDMS pueden ejecutarse en el centro de SCCA o el propietario de la misión puede implementar partes en su propia suscripción de Azure. Este componente se puede proporcionar en el entorno de Azure.

#### <a name="vdms-security-requirements"></a>Requisitos de seguridad de VDMS

![Matriz de requisitos de VDMS](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM es un rol de negocio. Esta persona es responsable de administrar SCCA. Sus obligaciones son las siguientes: 

- Establecer planes y directivas para el acceso de la cuenta al entorno de nube. 
- Asegurarse de que la administración de identidades y accesos funcione correctamente. 
- Mantener el plan de administración de credenciales en la nube. 

La entidad de autorización oficial nombra esta persona. BCAP, VDSS y VDMS proporcionan las capacidades que TCCM necesita para realizar su trabajo.

#### <a name="tccm-security-requirements"></a>Requisitos de seguridad de TCCM

![Matriz de requisitos de TCCM](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>Consideraciones sobre la planeación y los componentes de SACA 

La arquitectura de referencia de SACA está diseñada para implementar los componentes VDSS y VDMS en Azure y habilitar TCCM. Esta arquitectura es modular. Todas las piezas de VDSS y VDMS pueden residir en un centro centralizado. Algunos de los controles se pueden cumplir en el espacio del propietario de la misión o incluso de forma local. Microsoft recomienda coubicar los componentes de VDSS y VDM en una red virtual central a través de la cual todos los propietarios de la misión puedan conectarse. En el siguiente diagrama se muestra esta arquitectura: 


![Diagrama de arquitectura de referencia de SACA](media/sacav2generic.png)

Cuando planee la estrategia de cumplimiento de SCCA y la arquitectura técnica, tenga en cuenta los siguientes temas desde el principio ya que afectan a todos los clientes. Los siguientes problemas han surgido con los clientes del DoD y tienden a ralentizar el planeamiento y la ejecución. 

#### <a name="which-bcap-will-your-organization-use"></a>¿Qué BCAP usará la organización?
   - BCAP de DISA:
        - DISA tiene dos BCAP operativos, en el Pentágono y en Camp Roberts, California. Está previsto que haya un tercero en línea en breve. 
        - Todos los BCAP de DISA tienen circuitos Azure ExpressRoute en Azure, que los clientes del DoD pueden usar para la conectividad. 
        - DISA tiene una sesión de emparejamiento de Microsoft de nivel empresarial para los clientes del DoD que quieran suscribirse a herramientas de software como servicio (SaaS) de Microsoft, como Office 365. Mediante el uso del BCAP de DISA, puede habilitar la conectividad y el emparejamiento con la instancia de SACA. 
    - Creación de su propio BCAP:
        - Esta opción requiere que alquile espacio en un centro de datos coubicado y que configure un circuito de ExpressRoute en Azure. 
        - Esta opción requiere aprobación adicional. 
        - Debido a la aprobación adicional y la creación física, con esta opción se tarda más tiempo. 
    - Recomendamos que use el BCAP de DISA. Esta opción está disponible fácilmente, tiene redundancia integrada y tiene clientes que operan en ella hoy en producción.
- Espacio de IP enrutable del DoD:
    - Debe usar el espacio de IP enrutable del DoD en su perímetro. Está disponible la opción de usar NAT para conectar esos espacios con el espacio de IP privado en Azure.
    - Póngase en contacto con Network Information Center (NIC) del DoD para obtener el espacio de IP. Se necesita como parte del envío del proceso de aprobación de la red o del sistema (SNAP) con DISA. 
    - Si tiene previsto usar NAT para conectar el espacio de direcciones privadas en Azure, necesita un mínimo de una subred /24 de espacio de direcciones asignada desde NIC para cada región donde planee implementar SACA.
- Redundancia:
    - Implemente una instancia de SACA en al menos dos regiones. En la nube del DoD, se implementa en ambas regiones del DoD disponibles.
    - Conecte al menos dos BCAP a través de circuitos ExpressRoute independientes. A continuación, ambas conexiones ExpressRoute, se pueden vincular a cada instancia de SACA de cada región. 
- Requisitos específicos de componente del DoD:
    - ¿Tiene su organización otros requisitos específicos aparte de los requisitos de SCCA? Algunas organizaciones tienen requisitos de IPS específicos.
- SACA es una arquitectura modular:
    - Use solo los componentes que necesite para su entorno. 
        - Implemente aplicaciones virtuales de red en un solo nivel o en varios niveles.
        - Use IPS integrado o traiga su propio IPS.
- Nivel de impacto del DoD de sus aplicaciones y datos:
    - Si existe la posibilidad de que haya aplicaciones que se ejecuten en regiones Microsoft IL5, cree la instancia de SACA en IL5. La instancia se puede usar delante de las aplicaciones IL4 e IL5. Una instancia de SACA de IL4 delante de una aplicación IL5 probablemente no recibirá la acreditación.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>¿Qué proveedor de dispositivo virtual de red va a usar para VDSS?
Como se mencionó anteriormente, puede crear esta referencia de SACA mediante una variedad de dispositivos y servicios de Azure. Microsoft ha automatizado plantillas de solución para implementar la arquitectura de SACA con F5 y Citrix. Estas soluciones se tratan en la siguiente sección.

#### <a name="which-azure-services-will-you-use"></a>¿Qué servicios de Azure usará?
- Existen servicios de Azure que pueden satisfacer los requisitos de Log Analytics, la protección basada en host y la funcionalidad de IDS. Es posible que algunos servicios no estén disponibles con carácter general en regiones de Microsoft IL5. En este caso, es posible que deba usar herramientas de terceros si estos servicios de Azure no pueden satisfacer sus requisitos. Examine las herramientas con las que se sienta cómodo y la viabilidad del uso de las herramientas nativas de Azure.
- Se recomienda usar tantas herramientas nativas de Azure como sea posible. Se compilan teniendo en cuenta la seguridad en la nube y se integran perfectamente con el resto de la plataforma Azure. Use las herramientas nativas de Azure de la lista siguiente para cumplir distintos requisitos de SCCA:

    - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Introducción a Puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview) 
    - [Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Grupos de seguridad de Azure](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Ajuste de tamaño
    - Se debe realizar un ajuste de tamaño. Busque el número de conexiones simultáneas que pueden existir a través de la instancia de SACA y los requisitos de rendimiento de red. 
    - Este paso es crítico. Resulta útil para cambiar el tamaño de las máquinas virtuales e identificar las licencias necesarias procedentes de diversos proveedores que utiliza en su instancia de SACA. 
    - No se puede realizar un análisis del coste correcto sin este ajuste de tamaño. El tamaño correcto también permite el rendimiento óptimo. 


## <a name="most-common-deployment-scenario"></a>Escenario de implementación más común

 Varios clientes de Microsoft han realizado la implementación completa o al menos las fases de planeamiento de sus entornos de SACA. Sus experiencias revelan información detallada sobre el escenario de implementación más común. En el siguiente diagrama se muestra la arquitectura más común: 


![Diagrama de arquitectura de referencia de SACA](media/sacav2commonscenario.png) 


Como puede ver en el diagrama, los clientes del DoD normalmente se suscriben a dos de los BCAP de DISA. Uno de estos reside en la Costa Occidental y el otro reside en la Costa Oriental. Se habilita un elemento del mismo nivel privado de ExpressRoute para Azure en cada ubicación de BCAP de DISA. Estos elementos del mismo nivel de ExpressRoute se vinculan luego a la puerta de enlace de la red virtual en las regiones de Azure de DoD (este) y DoD (centro). Se implementa una instancia de SACA en las regiones de Azure del DoD (este) y DoD (centro). Todo el tráfico de entrada y salida fluye a través de ella hacia y desde la conexión ExpressRoute para el BCAP de DISA.

A continuación, las aplicaciones de propietario de la misión eligen las regiones de Azure en las que van a implementar sus aplicaciones. Usan el emparejamiento de red virtual para conectar la red virtual de la aplicación a la red virtual SACA. Después fuerzan a tunelizar todo su tráfico a través de la instancia VDSS.

Se recomienda esta arquitectura porque cumple los requisitos de SCCA. Está altamente disponible y es fácil de escalar. Simplifica la implementación y la administración.

## <a name="automated-saca-deployment-options"></a>Opciones de implementación automatizada de SACA

 Como se mencionó anteriormente, Microsoft se ha asociado con dos proveedores para crear una plantilla de infraestructura de SACA automatizada. Ambas plantillas implementan los siguientes componentes de Azure: 

- Red virtual SACA
    - Subred de administración
        - Esta subred es donde se implementan los servicios y las máquinas virtuales de administración, también conocida como cuadro de saltos.
        - Subred VDMS
            - Esta subred es donde se implementan las máquinas virtuales y los servicios que se usan para VDMS.
        - Subredes de confianza y de no confianza
            - Estas subredes son donde se implementan las aplicaciones virtuales.
        - Subred de puerta de enlace
            - Esta subred es donde se implementa la puerta de enlace de ExpressRoute.
- Máquinas virtuales de cuadro de salto de administración
    - Se usan para la administración fuera de banda del entorno.
- Aplicaciones virtuales de red
    - Utilice Citrix o F5 en función de la plantilla que implemente.
- Direcciones IP públicas
    - Se usan para el front-end hasta que ExpressRoute se pone en línea. Estas direcciones IP se convierten en el espacio de direcciones privadas de Azure de back-end.
- Tablas de ruta 
    - Estas tablas de ruta se aplican durante la automatización y fuerzan la tunelización de todo el tráfico a través de la aplicación virtual.
- Equilibradores de carga de Azure: SKU estándar
    - Se usan para equilibrar la carga del tráfico entre todos los dispositivos.
- Grupos de seguridad de red
    - Se usan para controlar qué tipos de tráfico pueden atravesar a determinados puntos de conexión.


### <a name="citrix-saca-deployment"></a>Implementación de SACA de Citrix

Una plantilla de implementación de Citrix implementa dos capas de dispositivos ADC de Citrix de alta disponibilidad. Esta arquitectura cumple los requisitos de VDSS. 

![Diagrama de SACA de Citrix](media/citrixsaca.png)


Para la documentación de Citrix y el script de implementación, consulte [este vínculo de GitHub](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca).


 ### <a name="f5-saca-deployment"></a>Implementación de SACA de F5

Dos plantillas de implementación independientes de F5 cubren dos arquitecturas diferentes. La primera plantilla tiene solo una capa de dispositivos F5 en una configuración activo-activo de alta disponibilidad. Esta arquitectura cumple los requisitos de VDSS. La segunda plantilla agrega una segunda capa de F5 de alta disponibilidad de activo-activo. Esta segunda capa permite a los clientes agregar su propios IPS independientes de F5 entre las capas de F5. No todos los componentes del DoD tienen IPS específicos prescritos para su uso. Si es así, la única capa de dispositivos de F5 funciona para la mayoría porque esa arquitectura incluye IPS de los dispositivos de F5.

![Diagrama de SACA de F5](media/f5saca.png)

Para la documentación de F5 y el script de implementación, consulte [este vínculo de GitHub](https://github.com/f5devcentral/f5-azure-saca).












