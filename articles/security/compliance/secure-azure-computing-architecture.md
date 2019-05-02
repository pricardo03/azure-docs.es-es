---
title: Secure Azure arquitectura informática
description: Se trata de una arquitectura de referencia para una arquitectura de red Perimetral de nivel empresarial, mediante aplicaciones virtuales de red y otras herramientas. Esta arquitectura se diseñó para cumplir Secure Cloud Computing arquitectura requisitos del departamento de defensa funcionales. Sin embargo, puede usarse para cualquier organización. Esta referencia incluye dos opciones automatizadas con aplicaciones de Citrix o F5.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: f2e3d72db3f29dbc6d03b3259acb18daf684fb12
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917586"
---
# <a name="secure-azure-computing-architecture"></a>Secure Azure arquitectura informática

Para obtener instrucciones sobre cómo configurar redes virtuales seguras y configurar las herramientas de seguridad y servicios estipulados por práctica y los estándares de DoD han formulado un rápido aumento del número de clientes de DoD implementar cargas de trabajo en Azure. DISA publicado el [documento requisitos funcionales de arquitectura de informática en la nube seguro (SCCA)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) en 2017. SCCA describe los objetivos funcionales para proteger la defensa información del sistema de la red (DISN) y apunta hacia la conexión del proveedor de nube comercial y cómo los propietarios de aplicaciones de nube segura en el límite de conexión de misión. Es obligatorio que todas las entidades del DoD que se conecta a la nube comercial sigue las directrices establecidas en el FRD SCCA.
 
Hay cuatro componentes de la SCCA. El punto de acceso en la nube del límite (BCAP), la pila de seguridad de centro de datos Virtual (VDSS) Centro de datos Virtual (VDM) de servicios administrados y el Administrador de credenciales en la nube (TCCM) de confianza. Microsoft ha desarrollado una solución que satisfaga los requisitos de SCCA IL4 y IL5 las cargas de trabajo que se ejecuta en Azure. Esta solución específica de Azure se llama a la arquitectura de informática de Azure seguro (SACA). Los clientes que implementen SACA serán conforme a la FRD SCCA y permitirán a los clientes del DoD mover las cargas de trabajo a Azure una vez conectado. 

Mientras que las arquitecturas y orientación SCCA son específicas a los clientes de DoD, las revisiones más recientes para saca también le ayudará a los clientes para cumplir con las instrucciones de conexión (tres en raya) confianza de internet, así como los clientes comerciales que desean implementar una red Perimetral segura para proteger sus entornos de azure.


## <a name="secure-cloud-computing-architecture-components"></a>Proteger los componentes de arquitectura de informática en la nube

**BCAP**

El propósito de la BCAP es proteger el DISN frente a ataques que se originan en el entorno de nube. BCAP se realice la detección de intrusiones y prevención, así como filtrar el tráfico no autorizado. Este componente puede coexistir con otros componentes de la SCCA. Se recomienda que este componente está implementado con el hardware físico. A continuación encontrará la lista de los requisitos de seguridad BCAP.

***Requisitos de seguridad BCAP***

![Matriz de los requisitos de BCAP](media/bcapreqs.png)


**VDSS**

El propósito de la VDSS es proteger las aplicaciones de propietario de la misión de DoD que se hospedan en Azure. VDSS realiza la mayor parte de las operaciones de seguridad en el SCCA. Inspección del tráfico llevará a cabo con el fin de proteger las aplicaciones que se ejecutan en Azure. Este componente se puede proporcionar en su entorno de Azure.

***Requisitos de seguridad VDSS***

![Matriz de los requisitos de VDSS](media/vdssreqs.png)

**VDMS**

El propósito de VDM consiste en proporcionar seguridad de host así como servicios de centro de datos compartidos. Las funciones de VDM pueden ejecutar en el centro de su SCCA o el propietario de la misión puede implementar partes de él en su propia suscripción de Azure específica. Este componente se puede proporcionar en su entorno de Azure.

***Requisitos de seguridad VDM***

![Matriz de los requisitos de VDM](media/vdmsreqs.png)


**TCCM**

TCCM es una función de negocio. Esta persona será responsable de administrar la SCCA. Sus funciones incluyen el establecimiento de directivas de acceso a la cuenta en el entorno de nube, lo que garantiza la identidad y los planes y administración de acceso está funcionando correctamente y para mantener el Plan de administración de credenciales en la nube. Esta persona es nombrada por oficial de autorización. El BCAP y VDSS VDM proporcionará las capacidades necesarias para que el TCCM realizar su función de trabajo.

***Requisitos de seguridad TCCM***

![Matriz de los requisitos de TCCM](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>Consideraciones de planeación y de componentes de saca. 

La arquitectura de referencia de saca está diseñada para implementar los componentes VDSS y VDM en azure, así como habilitar el TCCM. Esta arquitectura es modular, lo que significa que todas las piezas de VDSS y VDM pueden residir en un concentrador centralizado o algunos de los controles se pueden cumplir en el espacio de propietario de misión o incluso de forma local. La recomendación de nuestro equipo de Microsoft es colocar los componentes VDSS y VDM en una central Virtual neto que todos los propietarios de la misión pueden conectar a través. El siguiente diagrama describe la arquitectura recomendada. 


![Diagrama de arquitectura de referencia de saca.](media/sacav2generic.png)

Al planear su estrategia de cumplimiento SCCA y la arquitectura técnica, hay muchas cosas a tener en cuenta. Es importante que los temas siguientes se toman en consideración desde el principio, como todos los clientes se deben tratar todo. Los temas siguientes han sido problemas que brindan a los clientes de DoD real y tienden a ralentizar la planeación y ejecución. 

- ¿Qué BCAP usará su organización?
    - DISA BCAP
        - DISA tiene dos BCAPs operativas en el sistema Pentágono y entidad de certificación campamento Roberts, con una tercera pronto estará disponible en línea. 
        - DISA BCAPs que todos tienen los circuitos ExpressRoute en Azure, que puede usarse por los clientes del DoD para la conectividad. 
        - DISA ya tiene una sesión de Microsoft Peering de nivel empresarial para los clientes de DoD que desean suscribirse a las herramientas de Microsoft SaaS, como Office 365. Mediante el uso de DISA BCAP, puede habilitar la conectividad y el emparejamiento a la instancia de saca. 
    - Crear su propio BCAP
        - Esto requeriría la concesión de espacio en un centro de datos colocalizados y configurar un circuito de ExpressRoute en Azure. 
        - Esta opción requiere aprobación adicional 
        - Debido a una aprobación adicional y una compilación física horizontal, esta opción lleva más tiempo. 
    - La recomendación de Microsoft es utilizar el BCAP DISA. Esta opción está disponible, tiene redundancia integrada y ya tiene clientes que operan en él hoy mismo en producción.
- Espacio de IP enrutable de DoD
    - Deberá usar el espacio de IP enrutable DoD en su perímetro. La opción de NAT que se van a espacio IP privado en Azure está disponible.  
    - Póngase en contacto con el NIC de DoD para obtener el espacio de IP, se necesitarán como parte de su envío con DISA para el complemento. 
    - Si va a NAT para el espacio de direcciones privadas en Azure, necesitará un mínimo de/24 subred del espacio de direcciones asignado de la NIC para cada región tiene previsto implementar SACA. 
- Redundancia 
    - Microsoft le sugiere que implementar una instancia de saca en al menos dos regiones. En la nube de DoD, esto significaría que se implementa en ambas regiones del DoD disponibles. 
    - También se recomienda que se conecta al menos dos BCAPs a través de circuitos ExpressRoute independientes. Ambas rutas de Express, a continuación, se pueden vincular a instancia de saca cada región. 
- Requisitos específicos del componente de DoD
    - ¿Su organización tiene requisitos específicos fuera de los requisitos de SCCA? (Algunas organizaciones tienen requisitos específicos de las direcciones IP)
- SACA es una arquitectura modular  
    - Use sólo los componentes necesita para su entorno. 
        - Implementar aplicaciones virtuales de red en un solo nivel o de varios niveles
        - Integra las direcciones IP o traiga sus propias direcciones IP
- Nivel de impacto de DoD de sus aplicaciones y datos
    - Si no hay posibilidad alguna de las aplicaciones que se ejecutan en nuestras regiones IL5, se recomienda que cree la instancia de saca en IL5. La instancia se puede usar delante de las aplicaciones IL4 como IL5. Sin embargo, una instancia de saca IL4 delante de una aplicación IL5 más probable es que no recibirá acreditación. 
- ¿Proveedor de dispositivo de red Virtual que usará para VDSS?
    - Como se mencionó anteriormente, esta referencia de saca puede crearse mediante una variedad de dispositivos y servicios de Azure. Sin embargo, tenemos las plantillas de solución automatizada para implementar la arquitectura de saca con F5 y Citrix. Estas soluciones se tratarán con más detalle a continuación. 
- ¿Qué servicios de Azure usará?
    - Existen servicios de Azure que pueden cumplir los requisitos en torno a log analytics, la protección basada en host y la funcionalidad de los identificadores. Sin embargo, es posible que algunos servicios no están disponibles con carácter general en nuestras regiones IL5. Esto puede dar lugar a la necesidad de usar algunas herramientas de terceros 3rd si estos servicios de Azure no pueden satisfacer sus necesidades. Debe mirar las herramientas que son cómodo y la viabilidad del uso de las herramientas nativas de Azure. 
    - Es la recomendación de Microsoft que usar tantos Azure las herramientas nativas como sea posibles, ya que se crean teniendo en cuenta la seguridad en la nube e integran sin problemas con el resto de la plataforma Azure. A continuación es una lista de herramientas nativas de Azure que se pueden aprovechar para cumplir distintos requisitos de SCCA. 
        - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
        - [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) 
        - [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
        - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
        - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
        - [Application Gateway](https://docs.microsoft.com/azure/application-gateway/overview)
        - [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview) 
        - [Puerta de Azure](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
        - [Grupos de seguridad](https://docs.microsoft.com/azure/virtual-network/security-overview)
        - [Protección contra DDoS de Azure](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
        - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Ajuste de tamaño
    - Debe realizarse un ajuste de tamaño. Deberá observar el número de conexiones simultáneas, es posible que tenga a través de la instancia de saca así como los requisitos de rendimiento de red. 
    - Esto es un paso crítico, ya que le ayudan a cambiar el tamaño de las máquinas virtuales, así como ayudar a identificar las licencias que serán necesarias procedentes de diversos proveedores que se va a usar en su instancia de saca. 
    - No se puede realizar un análisis del costo buena sin este ejercicio de ajuste de tamaño, también es importante asegurarse de que todo lo que tenga el tamaño correcto para permitir el mejor rendimiento. 


## <a name="most-common-deployment-scenario"></a>Escenario de implementación más comunes

Microsoft tiene varios clientes que ya hayan pasado a través de la completa implementación o al menos planeando etapas de sus entornos de SACA. Esto nos ha permitido obtener información sobre el escenario de implementación más comunes. El siguiente diagrama describe la arquitectura más comunes. 


![Diagrama de arquitectura de referencia de saca.](media/sacav2commonscenario.png) 


Como puede ver en el diagrama, los clientes del DoD normalmente suscriben a dos de los BCAPs DISA, uno de estos se encuentra en la costa oeste y el otro se encuentra en la costa este. Un elemento del mismo nivel privado de ExpressRoute está habilitada para Azure en cada ubicación DISA BCAP. Estos pares de ExpressRoute, a continuación, se vinculan a la puerta de enlace de red Virtual en la DoD este y DoD Central regiones de Azure. Una instancia de saca se implementa en la región de DoD este y DoD Central Azure y todo entrada y salida el tráfico fluya a través de él hacia y desde la conexión Expressroute a la BCAP DISA. 

Las aplicaciones, a continuación, elija qué regiones de Azure va a implementar sus aplicaciones en el propietario de Mission y los use emparejamiento de red Virtual que para conectar su aplicación de red Virtual a la red Virtual de saca. Forzar la tunelización de todo su tráfico a través de la instancia VDSS. 

Esta arquitectura es muy recomendable por Microsoft, como cumplirán los requisitos de SCCA, es fácilmente escalable, altamente disponible y simplifica la implementación y administración.

## <a name="automated-saca-deployment-options"></a>Opciones de implementación automatizadas de saca.

 Anteriormente, mencionamos que Microsoft se ha asociado con dos proveedores para crear una plantilla de infraestructura de saca automatizada. Ambas plantillas implementará los siguientes componentes de Azure. 

- SACA Virtual Network
    - Subred de administración
        - Donde se implementan los servicios y máquinas virtuales de administración (los cuadros de salto)
        - Subred VDM
            - Donde se implementan las máquinas virtuales y servicios que se usan para VDM
        - Subredes de confianza y confianza 
            - Donde se implementan aplicaciones virtuales
        - Subred de puerta de enlace
            - Donde se implementará la puerta de enlace de ExpressRoute
- Máquinas virtuales de cuadro de salto de administración
    - Se usa para la administración fuera de banda del entorno.
- Aplicaciones virtuales de red
    - O bien, Citrix o F5, según la plantilla que implemente.
- Direcciones IP públicas
    - Se usa para front-end hasta que ExpressRoute se pone en línea. Estas direcciones IP se traducirán en el espacio de direcciones privadas Azure de back-end
- Tablas de ruta 
    - Aplicado durante la automatización, estas tablas de ruta forzar túnel todo el tráfico a través del dispositivo virtual
- Equilibradores de carga de Azure - SKU estándar
    - Se usan para equilibrar la carga en todos los dispositivos
- Grupos de seguridad de red
    - Se utiliza para controlar qué tipos de tráfico pueden atravesar para determinados puntos de conexión


**Implementación de Citrix SACA**

Citrix ha creado una plantilla de implementación que implementa dos capas de aplicaciones de alta disponibilidad Citrix ADC. Esta arquitectura cumple los requisitos de VDSS. 

![Diagrama de saca de Citrix.](media/citrixsaca.png)


Puede encontrar el script de implementación y documentación de Citrix [aquí.](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca)


 **Implementación de saca de F5.**

F5 ha creado dos plantillas de implementación independiente que abarcan dos arquitecturas diferentes. La primera de ellas tiene solo una capa de F5 dispositivos en una configuración activo-activo de alta disponibilidad. Esta arquitectura cumple los requisitos para VDSS. El segundo, agrega una segunda capa de F5s de alta disponibilidad activa / activa. El propósito de este segundo nivel es permitir a los clientes agregar su propias direcciones IP independiente de F5 entre las capas de F5. No todos los componentes de DoD tienen direcciones IP específicas prescrita para su uso. Si es así, la capa de F5 dispositivos solo funcionará para más desde que la arquitectura incluye las direcciones IP en los dispositivos de F5.  

![Diagrama de saca de Citrix.](media/f5saca.png)

Puede encontrar documentación F5 y script de implementación [aquí.](https://github.com/f5devcentral/f5-azure-saca) 












