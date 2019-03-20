---
title: Rehospedaje del sistema central en Azure virtual machines | Microsoft Docs
description: Rehospedar las cargas de trabajo de gran sistema, como los sistemas basados en IBM Z con máquinas virtuales (VM) en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 8be0ebc486739f8826e8a1d5a5307a219ba71b6f
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192724"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Rehospedaje del sistema central en Azure virtual machines

Las cargas de trabajo de migración desde mainframe entornos a la nube le permite modernizar su infraestructura y a menudo ahorrar en costos. Muchas cargas de trabajo pueden transferirse a Azure con solo leves cambios en el código, como la actualización de los nombres de las bases de datos.

El término *mainframe* generalmente se refiere a un sistema grande, pero la inmensa mayoría implementada actualmente son servidores IBM Z de sistema o sistemas de IBM compatible ejecuta MVS, denegación de servicio, VSE, OS/390 o z/OS.

Una máquina virtual (VM) de Azure se utiliza para aislar y administrar los recursos para una aplicación específica en una sola instancia. Grandes sistemas, como IBM z/OS utilice particiones lógicas (LPARS) para este propósito. Un gran sistema podría utilizar un LPAR para una región CICS con programas asociados de COBOL y un LPAR independiente para la base de datos IBM Db2. Una típica [aplicación de n niveles en Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) implementa máquinas virtuales de Azure en una red virtual que se puede dividir en subredes para cada nivel.

Entornos de emulación de gran sistema y los compiladores que admiten escenarios de migración mediante lift-and-shift, pueden ejecutar máquinas virtuales de Azure. Desarrollo y pruebas suelen entre las primeras cargas de trabajo para migrar desde un gran sistema a un entorno de desarrollo/pruebas de Azure. Los componentes de servidor comunes que puede emular incluyen el proceso de transacciones en línea (OLTP), batch y sistemas de ingesta de datos como se muestra en la figura siguiente.

![Entornos de emulación en Azure le permiten ejecutar sistemas basados en z/OS.](media/01-overview.png)

Algunas cargas de trabajo de mainframes se pueden migrar a Azure con relativa facilidad, mientras que otros pueden rehospedarse en Azure mediante una solución de asociado. Para obtener instrucciones detalladas sobre cómo elegir una solución de asociado, el [centro de migración de mainframes Azure](https://azure.microsoft.com/migration/mainframe/) puede ayudar a.

## <a name="set-up-devtest-environment-using-a-micro-focus-rehosting-platform"></a>Configurar el entorno de desarrollo y pruebas mediante una plataforma de rehospedaje Micro Focus

Micro Focus Enterprise Server es uno de mainframe mayor rehospedaje plataformas disponibles. Se puede usar ejecutar sus cargas de trabajo de z/OS en un económico x86 platform en Azure.

Para empezar, consulte los artículos siguientes:

- [Instalación de Micro Focus Enterprise Server 4.0 y desarrollador empresarial 4.0 en Azure](./microfocus/set-up-micro-focus-on-azure.md)
- [Configurar Micro foco CICS BankDemo Micro foco Enterprise Developer 4.0 en Azure](./microfocus/demo.md)

## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame en Azure

TmaxSoft OpenFrame es un popular mainframe rehospedaje solución que facilita a los recursos existentes de mainframe de elevación y desplazamiento de ellos en Azure. Un entorno OpenFrame en Azure es adecuado para el desarrollo, demostraciones, pruebas o las cargas de trabajo de producción.

Para comenzar, descargue el [instalar TmaxSoft OpenFrame en Azure](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/) libro electrónico.

## <a name="set-up-a-devtest-environment-using-ibm-z-devtest-120"></a>Configurar un entorno de desarrollo y pruebas con IBM Z para desarrollo y pruebas 12.0

Establece un entorno que no sea de producción en Azure que puede usar para el desarrollo, pruebas y demostraciones de aplicaciones basado en z/OS de IBM Z Development y entorno de prueba (IBM zD & T).

El entorno de emulación en Azure puede hospedar una variedad de instancias de Z a través de la aplicación a los desarrolladores controlan distribuciones (ADCDs). Puede ejecutar zD & T Personal Edition, zD & T Sysplex paralelas y zD & T Enterprise Edition en Azure y Azure Stack.

Para empezar, consulte los artículos siguientes:

-   [Configuración de IBM zD & 12.0 T en Azure](./ibm/install-ibm-z-environment.md)
-   [Configurar ADCD zD & T](./ibm/demo.md)

## <a name="migrate-ibm-db2-purescale-to-azure"></a>Migración de IBM DB2 pureScale a Azure

El entorno de IBM DB2 pureScale proporciona una solución de clúster de base de datos para Azure con alta disponibilidad y escalabilidad en sistemas operativos Linux. Aunque no es idéntico al entorno original, IBM DB2 pureScale en Linux ofrece características de alta disponibilidad y escalabilidad similares a las de IBM DB2 para z/OS ejecutado en una configuración de Parallel Sysplex del sistema central.

Para empezar, vea [pureScale de IBM DB2 en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Consideraciones

Al migrar las cargas de trabajo del sistema central a la infraestructura de Azure como servicio (IaaS), puede elegir entre varios tipos de los recursos informáticos a petición y escalables, incluidas las máquinas virtuales de Azure. Azure ofrece una gama de [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/overview) y [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/overview) máquinas virtuales.

### <a name="high-availability-and-failover"></a>Alta disponibilidad y conmutación por error

Azure ofrece en función de compromiso de acuerdos de nivel de servicio (SLA), donde varias 9s disponibilidad es el predeterminado, optimizado con la replicación local o según la ubicación de servicios. En el [SLA de Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) completo se explica la disponibilidad garantizada de Azure como un conjunto.

IaaS de Azure como las máquinas virtuales, conmutación por error se basa en la funcionalidad específica del sistema, como instancias de clústeres de conmutación por error y [conjuntos de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets). Cuando usa la plataforma Azure como recursos de un servicio (PaaS), como [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) y [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), la plataforma controla las conmutaciones por error automáticamente.

### <a name="scalability"></a>Escalabilidad

Mainframes normalmente se escalan verticalmente, mientras se escala horizontal del entorno de nube. Azure ofrece una gama de [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) y [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) tamaños para satisfacer sus necesidades. La nube también se puede escalar hacia arriba o hacia abajo hasta las especificaciones de coincidencia exacta del usuario. Power, almacenamiento y servicios de proceso [escala](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling) bajo demanda en un modelo de facturación basada en uso.

### <a name="storage"></a>Almacenamiento

En la nube, tiene una gama de opciones de almacenamiento flexible, escalable y solo paga por lo que necesita. [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) ofrece un almacén de objetos que se puede escalar de forma masiva destinado a objetos de datos, un servicio de sistema de archivos para la nube, un almacén de mensajería para mensajería confiable y un almacén NoSQL. En cuanto a las máquinas virtuales, los discos administrados y no administrados proporcionan un almacenamiento de disco persistente y seguro.

### <a name="backup-and-recovery"></a>Copia de seguridad y recuperación

Mantener su propio sitio de recuperación ante desastres, puede ser una propuesta costosa. Azure tiene opciones rentables y fácil de implementar para [copia de seguridad](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup), [recuperación](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), y [redundancia](https://docs.microsoft.com/azure/storage/common/storage-redundancy) en niveles regionales o locales, o a través de la redundancia geográfica.

## <a name="azure-government-for-mainframe-migrations"></a>Azure Government para las migraciones del sistema central

Muchas entidades del sector público encantaría mover sus aplicaciones del sistema central a una plataforma flexible, más moderna. Microsoft Azure Government es una instancia separada físicamente de los servicios en la nube en función de la plataforma global de Microsoft Azure pero empaquetado para sistemas gubernamentales federales, estatales y locales. Proporciona servicios de cumplimiento específicamente para las agencias gubernamentales de Estados Unidos y sus asociados, protección y seguridad de talla mundial.

Azure Government había obtenido una autoridad Provisional para operar (P-ATO) de FedRAMP gran impacto para los sistemas que necesiten este tipo de entorno. 

Para comenzar, descargue [en la nube de Microsoft Azure Government para aplicaciones de mainframe](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="learn-more"></a>Más información

Si está considerando una migración de mainframes, nuestra amplia [asociado](partner-workloads.md) ecosistema está disponible para ayudarle. Para más información sobre cómo elegir una solución de asociados, consulte [Platform Modernization Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

Consulte también:

-   [Migración del sistema central](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
-   [Solución de problemas](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
-   [Desmitificación del sistema central para la migración a Azure](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
