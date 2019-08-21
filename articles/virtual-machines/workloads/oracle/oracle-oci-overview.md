---
title: Integración de Microsoft Azure con Oracle Cloud Infrastructure | Microsoft Docs
description: Obtenga información acerca de las soluciones que integran las aplicaciones de Oracle que se ejecutan en Microsoft Azure con bases de datos de Oracle Cloud Infrastructure (OCI).
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: 309c481c0ebf0e6061524a12356e67394e5db8d2
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880828"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Soluciones de aplicaciones de Oracle que integran Microsoft Azure y Oracle Cloud Infrastructure (versión preliminar)

Microsoft y Oracle se han asociado para proporcionar baja latencia y conectividad de alto rendimiento entre nubes, lo que permite sacar provecho de lo mejor de ambas nubes. 

Mediante esta conectividad entre nubes, puede particionar una aplicación de niveles múltiples para que ejecute su nivel de base de datos en Oracle Cloud Infrastructure (OCI) y la aplicación y los restantes niveles en Microsoft Azure. La experiencia es similar a ejecutar la pila de toda la solución en una nube individual. 

> [!IMPORTANT]
> Esta funcionalidad está actualmente en versión preliminar y se [aplican ciertas limitaciones](#preview-limitations). Para habilitar la conectividad de baja latencia entre Azure y OCI, la suscripción a Azure debe incluirse en la lista de elementos permitidos de esta funcionalidad. Debe enviar un correo electrónico a oracleconnect@microsoft.com con su identificador de suscripción para inscribirse en la versión preliminar. Recibirá un correo electrónico una vez inscrita la suscripción. No puede usar la funcionalidad hasta que reciba un correo electrónico de confirmación. También puede contactar con su representante de ventas de Microsoft para que le tengan en cuenta para esta versión preliminar. Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

Si está interesado en implementar soluciones de Oracle por completo en la infraestructura de Azure, consulte [Imágenes de máquina virtual de Oracle y su implementación en Microsoft Azure](oracle-vm-solutions.md).

## <a name="scenario-overview"></a>Información general de escenario

La conectividad entre nubes proporciona una solución para ejecutar no solo las aplicaciones líderes del sector de Oracle, sino también sus propias aplicaciones personalizadas, en máquinas virtuales de Azure y disfrutar de las ventajas de servicios de base de datos hospedada en OCI. 

Entre las aplicaciones que se pueden ejecutar en una configuración entre nubes se incluyen:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Aplicaciones comerciales de Oracle
* Oracle Hyperion Financial Management

El siguiente diagrama es una introducción de alto nivel de la solución conectada. Por motivos de simplicidad, el diagrama muestra solo una capa de aplicación y una capa de datos. En función de la arquitectura de la aplicación, la solución puede incluir niveles adicionales, como un nivel web en Azure. Para más información, consulte las siguientes secciones:

![Introducción a la solución OCI de Azure](media/oracle-oci-overview/crosscloud.png)

## <a name="preview-limitations"></a>Limitaciones de vista previa

* La conectividad entre nubes en versión preliminar está limitada a las regiones de Este de EE. UU. (eastus) de Azure y OCI de Ashburn OCI (us-ashburn-1).

## <a name="networking"></a>Redes

Los clientes empresariales a menudo deciden diversificar e implementar las cargas de trabajo a través de varias nubes por motivos empresariales y de funcionamiento. Para diversificar, los clientes interconectan las redes en la nube mediante Internet, IPSec VPN, Internet o mediante la solución de conectividad directa del proveedor de la nube a través de su red local. La interconexión de redes en la nube puede requerir considerables inversiones de tiempo, dinero, diseño, adquisición, instalación, pruebas y operaciones. 

Para abordar los desafíos de los clientes, Oracle y Microsoft han integrado varias nubes. Las redes entre nubes se establecen mediante la conexión de un circuito [ExpressRoute](../../../expressroute/expressroute-introduction.md) de Microsoft Azure con un circuito [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) de OCI. Esta conectividad es posible donde una ubicación de emparejamiento de Azure ExpressRoute está cerca o en la misma ubicación de emparejamiento que FastConnect de OCI. Esta configuración permite una conectividad rápida y segura entre las dos nubes sin necesidad de que haya ningún proveedor de servicios intermedio.

Con ExpressRoute y FastConnect, los clientes pueden emparejar una red virtual de Azure con una red virtual en la nube de OCI, siempre que el espacio de direcciones IP privado no se solape. El emparejamiento de las dos redes permite que un recurso de la red virtual se comunique con un recurso de la red virtual en la nube de OCI como si ambos estuvieran en la misma red.

## <a name="network-security"></a>Seguridad de las redes

La seguridad de la red es un componente fundamental de cualquier aplicación empresarial y es clave para esta solución de varias nubes. Todo el tráfico que atraviesa ExpressRoute y FastConnect pasa por una red privada. Esta configuración permite una comunicación segura entre una red virtual de Azure y una red virtual en la nube de Oracle. No es necesario proporcionar una dirección IP pública a las máquinas virtuales de Azure. Tampoco se necesita una puerta de enlace de Internet en OCI. Toda la comunicación se produce mediante la dirección IP privada de las máquinas.

Además, puede configurar [listas de seguridad](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) en la red virtual en la nube de OCI y en las reglas de seguridad (adjuntas a los[grupos de seguridad de red](../../../virtual-network/security-overview.md) de Azure). Dichas reglas se usan para controlar el tráfico que fluye entre las máquinas en las redes virtuales. Las reglas de seguridad de red se pueden agregar a nivel de máquina, a nivel de subred y a nivel de red virtual.
 
## <a name="identity"></a>Identidad

La identidad es uno de los pilares principales de la asociación entre Microsoft y Oracle. Se ha realizado un trabajo considerable para integrar [Oracle Identity Cloud Service](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) (IDCS) con [Azure Active Directory](../../../active-directory/index.yml) (Azure AD). Azure AD es un servicio de administración de acceso y de identidades basado en la nube de Microsoft. Ayuda a los usuarios a iniciar sesión y a acceder a varios recursos. Azure AD también permite administrar los usuarios y sus permisos.

Actualmente, esta integración permite realizar la adminsitración en ubicación central, que es Azure Active Directory. Azure AD sincroniza los cambios en el directorio con el directorio correspondiente de Oracle y se usa para el inicio de sesión único en las soluciones de Oracle entre nubes.

## <a name="next-steps"></a>Pasos siguientes

Empezar a trabajar con una [red entre nubes](configure-azure-oci-networking.md) de Azure y OCI. 

Para más información y las notas del producto acerca de OCI, consulte la documentación de [Oracle Cloud](https://docs.cloud.oracle.com/iaas/Content/home.htm).
