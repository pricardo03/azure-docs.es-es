---
title: Integrar Microsoft Azure con la infraestructura de nube de Oracle | Microsoft Docs
description: Obtenga información sobre las soluciones que integran las aplicaciones de Oracle que se ejecutan en Microsoft Azure con bases de datos de infraestructura de nube de Oracle (OCI).
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
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
ms.openlocfilehash: 711fce9627537e68171c3b170121900d6b14ca1e
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743656"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Soluciones de aplicación de Oracle, la integración de Microsoft Azure y la infraestructura de nube de Oracle (versión preliminar)

Microsoft y Oracle se han asociado para proporcionar baja latencia, conectividad de toda la nube de alto rendimiento, lo que le permite aprovechar lo mejor de ambas nubes. 

Con esta conectividad entre la nube, puede dividir una aplicación de niveles múltiples para ejecutar su nivel de base de datos de infraestructura de nube de Oracle (OCI) y la aplicación y otros niveles en Microsoft Azure. La experiencia es similar a la ejecución de la pila de toda la solución en una nube única. 

> [!IMPORTANT]
> Esta capacidad de toda la nube está actualmente en versión preliminar y algunos [limitaciones se aplican](#preview-limitations). Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

Si está interesado en implementar las soluciones de Oracle por completo en la infraestructura de Azure, consulte [imágenes de máquina virtual de Oracle y su implementación en Microsoft Azure](oracle-vm-solutions.md).

## <a name="scenario-overview"></a>Información general de escenario

Conectividad entre la nube proporciona una solución para ejecutar aplicaciones de tecnología de vanguardia de Oracle y sus propias aplicaciones personalizadas, en Azure virtual machines mientras disfruta las ventajas de servicios de base de datos hospedada en OCI. 

Puede ejecutar en una configuración de la nube entre las aplicaciones incluyen:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Aplicaciones comerciales de Oracle
* Administración financiera de Oracle Hyperion

El diagrama siguiente es una descripción general de la solución conectada. Por motivos de simplicidad, el diagrama muestra solo una capa de aplicación y una capa de datos. Dependiendo de la aplicación architecutre, la solución podría incluir capas adicionales, como un nivel web en Azure. Para más información, consulte las siguientes secciones:

![Introducción a la solución OCI Azure](media/oracle-oci-overview/crosscloud.png)

## <a name="preview-limitations"></a>Limitaciones de vista previa

* Conectividad entre nubes en versión preliminar se limita a la región de Azure East US (eastus) y la región de Ashburn OCI (us-ashburn-1).

## <a name="networking"></a>Redes

Los clientes empresariales a menudo deciden diversificar e implementar las cargas de trabajo a través de varias nubes para varias empresas y razones operativas. Para diversificar, los clientes interconexión de redes de nube mediante VPN de IPSec, internet o con soluciones de conectividad directa del proveedor de nube a través de la red local. Interconexión de redes en la nube puede requerir grandes inversiones en tiempo, dinero, diseño, adquisición, instalación, pruebas y operaciones. 

Para abordar los desafíos del cliente, Oracle y Microsoft han habilitado una experiencia integrada de varias nubes. Las redes de toda la nube se establece mediante la conexión una [ExpressRoute](../../../expressroute/expressroute-introduction.md) circuito en Microsoft Azure con un [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) circuito en OCI. Esta conectividad es posible, donde una ubicación de emparejamiento de ExpressRoute de Azure está cerca o en la misma ubicación de emparejamiento que el FastConnect OCI. Esta configuración permite conectividad rápida y segura, entre las dos nubes sin necesidad de un proveedor de servicio intermedia.

Con ExpressRoute y FastConnect, los clientes pueden emparejar una red virtual en Azure con una red virtual en la nube en OCI, siempre que no se superpone con el espacio de direcciones IP privadas. Emparejamiento de las dos redes permite que un recurso en la red virtual para comunicarse con un recurso en la red virtual en la nube OCI como si ambos están en la misma red.

## <a name="network-security"></a>Seguridad de las redes

Seguridad de red es un componente fundamental de cualquier aplicación empresarial y es fundamental para esta solución en varias nubes. Todo el tráfico a través de ExpressRoute y FastConnect pasa a través de una red privada. Esta configuración permite una comunicación segura entre una red virtual de Azure y una red virtual en la nube de Oracle. No es necesario proporcionar una dirección IP pública a las máquinas virtuales de Azure. De forma similar, no necesita una puerta de enlace de internet en OCI. Toda la comunicación se produce a través de la dirección IP privada de las máquinas.

Además, puede configurar [listas seguridad](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) en las reglas de seguridad de red y de nube virtual OCI (conectado a Azure [grupos de seguridad de red](../../../virtual-network/security-overview.md)). Utilice estas reglas para controlar el tráfico que fluye entre las máquinas en las redes virtuales. En un nivel de equipo, en un nivel de subred, así como en el nivel de red virtual, se pueden agregar reglas de seguridad de red.
 
## <a name="identity"></a>identidad

Identidad es uno de los pilares principales de la asociación entre Microsoft y Oracle. Se ha realizado un esfuerzo importante para integrar [servicio en la nube de Oracle identidad](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) (IDCS) con [Azure Active Directory](../../../active-directory/index.yml) (Azure AD). Azure AD es en la nube identidad y acceso de servicio de administración. de Microsoft Ayuda a los usuarios iniciar sesión y acceso a varios recursos. Azure AD también permite administrar los usuarios y sus permisos.

Actualmente, esta integración le permite administrar en una ubicación central, que es Azure Active Directory. Azure AD sincroniza los cambios en el directorio con el directorio correspondiente de Oracle y se usa para el inicio de sesión único para la nube entre soluciones de Oracle.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información y las notas del producto sobre OCI, consulte el [Oracle Cloud](https://docs.cloud.oracle.com/iaas/Content/home.htm) documentación.
