---
title: Protección de la arquitectura informática de Azure
description: Esta arquitectura de referencia para una arquitectura de red Perimetral de nivel empresarial usa aplicaciones virtuales de red y otras herramientas. Esta arquitectura se diseñó para cumplir Secure Cloud Computing arquitectura requisitos del departamento de defensa funcionales. También puede usarse para cualquier organización. Esta referencia incluye dos opciones automatizadas que usan dispositivos Citrix o F5.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: 017a26d5672f666d4d8eaf629a0f53fe0cfe517f
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963243"
---
# <a name="secure-azure-computing-architecture"></a>Protección de la arquitectura informática de Azure

Datos de agencias y Los clientes del departamento de defensa (DoD) que implementan las cargas de trabajo en Azure han preguntado para que obtener instrucciones para configurar redes virtuales seguras y configurar las herramientas de seguridad y servicios que son estipulados por estándares de DoD y práctica. 

Defense Information System Agency (DISA) publicada el [documento de requisitos funcionales de arquitectura de informática en la nube seguro (SCCA) (FRD)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) en 2017. SCCA describe los objetivos funcionales para proteger la defensa información del sistema de la red (DISN) y en la nube comercial de puntos de conexión del proveedor. SCCA también describe cómo los propietarios de misión seguros aplicaciones en la nube en el límite de conexión. Todas las entidades del DoD que se conecta a la nube comercial deben seguir las instrucciones establecidas en el FRD SCCA.
 
El SCCA tiene cuatro componentes:
 
- Punto de acceso en la nube del límite (BCAP)
- Pila de seguridad de centro de datos virtual (VDSS)
- Centro de datos virtual Managed Service (VDM)
- Administrador de credenciales de confianza en la nube (TCCM) 

Microsoft ha desarrollado una solución que cumpla los requisitos de SCCA para IL4 y IL5 cargas de trabajo que se ejecutan en Azure. Esta solución específica de Azure se llama a la arquitectura de informática de Azure de Secure (SACA). Los clientes que implementen SACA son conforme a la FRD SCCA. Pueden habilitar los clientes del DoD mover las cargas de trabajo a Azure después de que están conectados.

Arquitecturas y orientación SCCA son específicas para los clientes de DoD, pero las revisiones más recientes para los clientes de saca ayuda para cumplan con confianza instrucciones de conexión (tres en raya) de internet. Las revisiones más recientes también ayudan a los clientes comerciales que desean implementar una red Perimetral segura para proteger sus entornos de Azure.


## <a name="secure-cloud-computing-architecture-components"></a>Proteger los componentes de arquitectura de informática en la nube

### <a name="bcap"></a>BCAP

El propósito de la BCAP es proteger el DISN frente a ataques que se originan en el entorno de nube. BCAP realiza prevención y detección de intrusiones. También filtra el tráfico no autorizado. Este componente puede coexistir con otros componentes de la SCCA. Le recomendamos que implemente este componente mediante el uso de hardware físico. Requisitos de seguridad BCAP se muestran en la tabla siguiente.

#### <a name="bcap-security-requirements"></a>Requisitos de seguridad BCAP

![Matriz de los requisitos de BCAP](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

El propósito de la VDSS es proteger las aplicaciones de propietario de la misión del DoD que se hospedan en Azure. VDSS realiza la mayor parte de las operaciones de seguridad en el SCCA. Lleva a cabo la inspección del tráfico para proteger las aplicaciones que se ejecutan en Azure. Este componente se puede proporcionar en su entorno de Azure.

#### <a name="vdss-security-requirements"></a>Requisitos de seguridad VDSS

![Matriz de los requisitos de VDSS](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

El propósito de VDM consiste en proporcionar seguridad de host y servicios de centro de datos compartidos. Las funciones de VDM pueden ejecutar en el centro de su SCCA o el propietario de la misión puede implementar partes de él en su propia suscripción de Azure específica. Este componente se puede proporcionar en su entorno de Azure.

#### <a name="vdms-security-requirements"></a>Requisitos de seguridad VDM

![Matriz de los requisitos de VDM](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM es una función de negocio. Esta persona es responsable de administrar la SCCA. Sus funciones son los siguientes: 

- Establecer planes y las directivas de acceso a la cuenta para el entorno de nube. 
- Asegúrese de que la administración de identidades y acceso está funcionando correctamente. 
- Mantener el Plan de administración de credenciales en la nube. 

Esta persona es nombrada por la entidad de autorización competente. El BCAP y VDSS VDM proporciona las capacidades que necesita el TCCM para realizar su trabajo.

#### <a name="tccm-security-requirements"></a>Requisitos de seguridad TCCM

![Matriz de los requisitos de TCCM](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>Consideraciones de planeación y de componentes de saca. 

La arquitectura de referencia de saca está diseñada para implementar los componentes VDSS y VDM en Azure y habilitar el TCCM. Esta arquitectura es modular. Todas las piezas de VDSS y VDM pueden residir en un concentrador centralizado. Algunos de los controles se pueden cumplir en el espacio de propietario de la misión o incluso de forma local. Microsoft recomienda que busque conjuntamente los componentes VDSS y VDM en una red virtual central que todos los propietarios de la misión pueden conectar a través. El siguiente diagrama muestra esta arquitectura: 


![Diagrama de arquitectura de referencia de saca.](media/sacav2generic.png)

Cuando planee la estrategia de cumplimiento SCCA y la arquitectura técnica, tenga en cuenta los siguientes temas desde el principio ya que afectan a todos los clientes. Los siguientes problemas han inventado con los clientes del DoD y tienden a ralentizar la planeación y ejecución. 

#### <a name="which-bcap-will-your-organization-use"></a>¿Qué BCAP usará su organización?
   - DISA BCAP:
        - DISA tiene dos BCAPs operativas en el Pentágono y en el campamento Roberts, California. Una tercera está prevista para ponerse en línea en breve. 
        - DISA BCAPs que todos tienen los circuitos ExpressRoute de Azure en Azure, que puede usarse por los clientes del DoD para la conectividad. 
        - DISA tiene una sesión de emparejamiento de Microsoft de nivel empresarial para los clientes de DoD que desean suscribirse para software de Microsoft como herramientas de un servicio (SaaS), como Office 365. Mediante el uso de la BCAP DISA, puede habilitar la conectividad y emparejamiento a la instancia de saca. 
    - Crear su propio BCAP:
        - Esta opción requiere espacio en un centro de datos colocalizados la concesión y configurar un circuito de ExpressRoute en Azure. 
        - Esta opción requiere aprobación adicional. 
        - Debido a la aprobación adicional y una salida de compilación físico, esta opción lleva más tiempo. 
    - Se recomienda que utilice el BCAP DISA. Esta opción está disponible, tiene redundancia integrada y tiene clientes que operan en él hoy mismo en producción.
- Espacio IP enrutable de DoD:
    - Debe usar el espacio de IP enrutable DoD en su perímetro. La opción para usar NAT para conectar esos espacios con espacio IP privado en Azure está disponible.
    - Póngase en contacto con el centro de información de red (NIC) de DoD para obtener el espacio de IP. Necesita como parte de su envío con DISA para el proceso de aprobación de red o del sistema (complemento). 
    - Si tiene previsto utilizar NAT para conectar el espacio de direcciones privadas en Azure, necesita un mínimo de/24 subred del espacio de direcciones asignado de la NIC para cada región donde planea implementar SACA.
- Redundancia:
    - Implemente una instancia de saca en al menos dos regiones. En la nube de DoD, implementarla en ambas regiones del DoD disponibles.
    - Conectarse al menos dos BCAPs a través de circuitos ExpressRoute independientes. Ambas conexiones de ExpressRoute, a continuación, se pueden vincular a instancia de saca cada región. 
- Requisitos específicos del componente de DoD:
    - ¿Su organización tiene requisitos específicos fuera de los requisitos de SCCA? Algunas organizaciones tienen requisitos específicos de las direcciones IP.
- SACA es una arquitectura modular:
    - Use sólo los componentes que necesita para su entorno. 
        - Implementar aplicaciones virtuales de red en un solo nivel o de varios niveles.
        - Usar direcciones IP integrada o traiga sus propias direcciones IP.
- Nivel de impacto de DoD de sus aplicaciones y datos:
    - Si no hay posibilidad alguna de las aplicaciones que se ejecutan en Microsoft IL5 regiones, cree la instancia de saca en IL5. La instancia se puede usar delante de las aplicaciones IL4 y IL5. Una instancia de saca IL4 delante de una aplicación IL5 probablemente no recibirá la acreditación.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>¿Qué proveedor de dispositivo virtual de red van a usar VDSS?
Como se mencionó anteriormente, puede crear esta referencia SACA mediante una variedad de dispositivos y servicios de Azure. Microsoft ha automatizado plantillas de solución para implementar la arquitectura de saca con F5 y Citrix. Estas soluciones se tratan en la sección siguiente.

#### <a name="which-azure-services-will-you-use"></a>¿Qué servicios de Azure usará?
- Existen servicios de Azure que pueden satisfacer los requisitos para log analytics, la protección basada en host y la funcionalidad de los identificadores. Es posible que algunos servicios no están disponibles con carácter general en regiones de Microsoft IL5. En este caso, es posible que deba usar herramientas de terceros si estos servicios de Azure no pueden cumplir sus requisitos. Examine las herramientas que se sienten cómodos y la viabilidad del uso de las herramientas nativas de Azure.
- Se recomienda que use herramientas nativas de Azure tantos como sea posible. Se construidos teniendo en cuenta la seguridad en la nube y se integran perfectamente con el resto de la plataforma Azure. Use las herramientas nativas de Azure en la lista siguiente para cumplir distintos requisitos de SCCA:

    - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Introducción a Puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview) 
    - [Puerta de Azure](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Grupos de seguridad de Azure](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Protección contra DDoS de Azure](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Tamaño
    - Se debe realizar un ajuste de tamaño. Busque el número de conexiones simultáneas, es posible que tenga a través de la instancia de saca y los requisitos de rendimiento de red. 
    - Este paso es fundamental. Resulta útil cambiar el tamaño de las máquinas virtuales e identificar las licencias necesarias procedentes de diversos proveedores que utilice en su instancia SACA. 
    - No se puede realizar un análisis del costo buena sin este ajuste de tamaño. También permite el tamaño correcto para un mejor rendimiento. 


## <a name="most-common-deployment-scenario"></a>Escenario de implementación más comunes

 Varios clientes de Microsoft han pasado a través de la implementación completa o al menos las fases de planeamiento de sus entornos de SACA. Sus experiencias revelan información detallada sobre el escenario de implementación más comunes. El siguiente diagrama muestra la arquitectura más comunes: 


![Diagrama de arquitectura de referencia de saca.](media/sacav2commonscenario.png) 


Como puede ver en el diagrama, los clientes del DoD normalmente suscriben a dos de los BCAPs DISA. Uno de estos reside en la costa oeste y la otra reside en la costa este. Un elemento del mismo nivel privado de ExpressRoute está habilitada para Azure en cada ubicación DISA BCAP. Estos pares de ExpressRoute, a continuación, se vinculan a la puerta de enlace de red virtual en las regiones del DoD este y DoD Central Azure. Se implementa una instancia de saca en las regiones del DoD este y DoD Central Azure. Todo el tráfico de entrada y salida fluye a través de él hacia y desde la conexión ExpressRoute a la BCAP DISA.

Las aplicaciones de propietario de la misión, a continuación, elegir las regiones de Azure en el que van a implementar sus aplicaciones. Se usan para conectar la red virtual de la aplicación a la red virtual SACA del emparejamiento de red virtual. Después de que todo su tráfico a través de la instancia VDSS de túnel forzado.

Esta arquitectura se recomienda porque cumple los requisitos de SCCA. Es fácilmente escalable y altamente disponible. También simplifica la implementación y administración.

## <a name="automated-saca-deployment-options"></a>Opciones de implementación automatizadas de saca.

 Como se mencionó anteriormente, Microsoft se ha asociado con dos proveedores para crear una plantilla de infraestructura de saca automatizada. Ambas plantillas implementan los siguientes componentes de Azure: 

- Red virtual de saca.
    - Subred de administración
        - Esta subred es donde se implementan los servicios y máquinas virtuales de administración, también conocido como un cuadros de salto.
        - Subred VDM
            - Esta subred es donde se implementan las máquinas virtuales y servicios que se usan para VDM.
        - Subredes de confianza y confianza
            - Estas subredes son donde se implementan aplicaciones virtuales.
        - Subred de puerta de enlace
            - Esta subred es donde se implementa la puerta de enlace de ExpressRoute.
- Máquinas virtuales de cuadro de salto de administración
    - Se usan para la administración fuera de banda del entorno.
- Aplicaciones virtuales de red
    - Utilice cualquier Citrix o F5 basándose en la plantilla que implementa.
- IP públicas
    - Se usan para el front-end hasta que ExpressRoute se pone en línea. Estas direcciones IP se convierten en el espacio de direcciones privadas Azure de back-end.
- Tablas de rutas 
    - Se aplica durante la automatización, estos enrutan tablas forzar túnel todo el tráfico a través del dispositivo virtual.
- Equilibradores de carga de Azure - SKU estándar
    - Se usan para equilibrar la carga en todos los dispositivos.
- Grupos de seguridad de red
    - Se usan para controlar qué tipos de tráfico pueden atravesar para determinados puntos de conexión.


### <a name="citrix-saca-deployment"></a>Implementación de Citrix SACA

Una plantilla de implementación de Citrix implementa dos capas de aplicaciones de alta disponibilidad Citrix ADC. Esta arquitectura cumple los requisitos de VDSS. 

![Diagrama de Citrix SACA](media/citrixsaca.png)


Para la documentación de Citrix y script de implementación, consulte [este vínculo GitHub](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca).


 ### <a name="f5-saca-deployment"></a>Implementación de saca de F5.

Dos plantillas de implementación independientes de F5 abarcan dos arquitecturas diferentes. La primera plantilla tiene solo una capa de F5 dispositivos en una configuración activo-activo de alta disponibilidad. Esta arquitectura cumple los requisitos para VDSS. La segunda plantilla agrega una segunda capa de F5s de alta disponibilidad activa / activa. Este segundo nivel permite a los clientes agregar su propias direcciones IP independiente de F5 entre las capas de F5. No todos los componentes de DoD tienen direcciones IP específicas prescrita para su uso. Si es así, la capa de F5 dispositivos solo funciona para la mayoría porque esa arquitectura incluye las direcciones IP en los dispositivos de F5.

![Diagrama de saca de F5.](media/f5saca.png)

Para la documentación de F5 y el script de implementación, consulte [este vínculo GitHub](https://github.com/f5devcentral/f5-azure-saca).












