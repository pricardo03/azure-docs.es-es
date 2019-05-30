---
title: Arquitectura de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Arquitectura de implementación de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d077487f85c789bcdfea3d91e29ee0d44ce82de0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239436"
---
# <a name="sap-hana-large-instances-architecture-on-azure"></a>Arquitectura de SAP HANA en Azure (instancias grandes)

En un nivel genérico, en la solución SAP HANA en Azure (instancias grandes), el nivel de aplicación de SAP reside en máquinas virtuales. La capa de base de datos reside en hardware configurado según SAP TDI ubicado en una demarcación de instancias grandes en la misma región de Azure que está conectada a la IaaS de Azure.

> [!NOTE]
> Debe implementar el nivel de aplicación de SAP en la misma región de Azure que la capa de DBMS de SAP. Esta regla está bien documentada en la información publicada sobre cargas de trabajo de SAP en Azure. 

La arquitectura global de SAP HANA en Azure (instancias grandes) proporciona una configuración de hardware con certificación SAP TDI, que es un servidor de alto rendimiento sin sistema operativo y sin virtualización para la base de datos de SAP HANA. También proporciona la capacidad y flexibilidad de Azure para escalar los recursos del nivel de aplicación de SAP según sus necesidades.

![Introducción a la arquitectura de SAP HANA en Azure (Instancias grandes)](./media/hana-overview-architecture/image1-architecture.png)

La arquitectura que se muestra se divide en tres secciones:

- **derecha**: Se muestra en los centros de una infraestructura local que ejecuta aplicaciones diferentes en los datos para que puedan acceder a los usuarios finales empresariales de aplicaciones, como SAP. Idealmente, esto local infraestructura está conectada a Azure con [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center**: Muestra la IaaS de Azure y, en este caso, uso de máquinas virtuales para hospedar SAP u otras aplicaciones que usan SAP HANA como sistema DBMS. Las instancias de HANA menores que funcionan con la memoria proporcionada por las máquinas virtuales se implementan en máquinas virtuales junto con el nivel de aplicación. Para más información sobre máquinas virtuales, consulte [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).

   Se usan los servicios de red de Azure para agrupar los sistemas SAP junto con otras aplicaciones en redes virtuales. Estas redes virtuales se conectan a los sistemas locales, así como a SAP HANA en Azure (instancias grandes).

   Para las aplicaciones de SAP NetWeaver y las bases de datos que se pueden para ejecutar en Azure, consulte [SAP soporte Nota 1928533 – aplicaciones SAP en Azure: Tipos de máquina virtual de Azure y productos admitidos](https://launchpad.support.sap.com/#/notes/1928533). Para obtener documentación sobre cómo implementar soluciones de SAP en Azure, consulte:

  -  [Uso de SAP en máquinas virtuales Windows](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Uso de soluciones de SAP en Azure Virtual Machines](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **izquierdo**: Muestra el hardware con certificación SAP HANA TDI en el sello de instancias grandes de Azure. Las unidades de instancia grande de HANA se conectan a las redes virtuales de su suscripción de Azure mediante el uso de la misma tecnología como la conectividad de local a Azure. A partir de mayo de 2019, se introdujo una optimización que puede comunicarse entre las unidades de instancia grande de HANA y las máquinas virtuales de Azure sin la participación de la puerta de enlace de ExpressRoute. Esta optimización que se denomina ruta con Fast de ExpressRoute se muestra en esta arquitectura (líneas rojas). 

El sello de instancias grandes de Azure en sí combina los componentes siguientes:

- **Informática**: Servidores que se basan en diferentes generación de procesadores Intel Xeon que proporcionan la funcionalidad de computación necesaria y poseen certificación para SAP HANA.
- **Network** (Red): Un tejido de red de alta velocidad unificada que interconecta la computación, almacenamiento y componentes de red local.
- **Almacenamiento**: Una infraestructura de almacenamiento que se accede a través de un tejido de red unificada. La capacidad de almacenamiento específico que se proporciona depende de la configuración específica de SAP HANA en Azure (instancias grandes) que se implementa. Está disponible más capacidad de almacenamiento con un costo mensual adicional.

Dentro de la infraestructura multiinquilino del sello de instancias grandes, los clientes se implementan como inquilinos aislados. En la implementación del inquilino, asigne un nombre a una suscripción de Azure dentro de la inscripción de Azure. HANA (instancias grandes) se factura a esta suscripción de Azure. Estos inquilinos mantienen una relación de 1:1 con la suscripción de Azure. Para una red, es posible acceder a una unidad de HANA (instancias grandes) implementada en un inquilino de una región de Azure desde redes virtuales diferentes que pertenecen a diferentes suscripciones de Azure. Esas suscripciones de Azure deben pertenecer a la misma inscripción de Azure. 

Al igual que las máquinas virtuales, SAP HANA en Azure (instancias grandes) se ofrece en varias regiones de Azure. Para ofrecer funcionalidades de recuperación ante desastres, puede elegir participar. Los diferentes sellos de instancia grande en una región geopolítica están conectados entre sí. Por ejemplo, las demarcaciones de HANA (instancias grandes) en Oeste de EE. UU. y Este de EE. UU. se conectan a través de una conexión de red dedicada para la replicación de recuperación ante desastres. 

Al igual que puede elegir entre diferentes tipos de máquinas virtuales con Azure Virtual Machines, también puede elegir entre diferentes SKU de HANA (instancias grandes) que se adapten a distintos tipos de carga de trabajo de SAP HANA. SAP aplica una relación de memoria a sockets de procesador para diferentes cargas de trabajo en función de la generación de los procesadores Intel. En la tabla siguiente muestra los tipos de SKU que se ofrecen.

Puede buscar las SKU disponibles en [SKU disponibles para HLI](hana-available-skus.md).

**Pasos siguientes**
- Consulte [Arquitectura de red de SAP HANA (instancias grandes)](hana-network-architecture.md).