---
title: Rehospedaje del sistema central en Azure virtual machines
description: Rehospedar las cargas de trabajo de gran sistema, como los sistemas basados en IBM Z con máquinas virtuales (VM) en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: c1d7b52bdce77ca108781a999a8a85b3e3fca0b8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60004421"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Rehospedaje del sistema central en Azure virtual machines

Las cargas de trabajo de migración desde mainframe entornos a la nube le permite modernizar su infraestructura y a menudo ahorrar en costos. Muchas cargas de trabajo pueden transferirse a Azure con solo leves cambios en el código, como la actualización de los nombres de las bases de datos.

En general, el término *mainframe* significa un sistema grande. En concreto, la inmensa mayoría actualmente en uso son servidores IBM Z de sistema o los sistemas de IBM compatible que ejecutan MVS, denegación de servicio, VSE, OS/390 o z/OS.

Una máquina virtual (VM) de Azure se utiliza para aislar y administrar los recursos para una aplicación específica en una sola instancia. Grandes sistemas, como IBM z/OS utilice particiones lógicas (LPARS) para este propósito. Un gran sistema podría utilizar un LPAR para una región CICS con programas asociados de COBOL y un LPAR independiente para la base de datos IBM Db2. Una típica [aplicación de n niveles en Azure](/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) implementa máquinas virtuales de Azure en una red virtual que se puede dividir en subredes para cada nivel.

Entornos de emulación de gran sistema y los compiladores que admiten escenarios de migración mediante lift-and-shift, pueden ejecutar máquinas virtuales de Azure. Desarrollo y pruebas suelen entre las primeras cargas de trabajo para migrar desde un gran sistema a un entorno de desarrollo/pruebas de Azure. Los componentes de servidor comunes que puede emular incluyen el proceso de transacciones en línea (OLTP), batch y sistemas de ingesta de datos como se muestra en la figura siguiente.

![Entornos de emulación en Azure le permiten ejecutar sistemas basados en z/OS.](media/01-overview.png)

Algunas cargas de trabajo de mainframes se pueden migrar a Azure con relativa facilidad, mientras que otros pueden rehospedarse en Azure mediante una solución de asociado. Para obtener instrucciones detalladas sobre cómo elegir una solución de asociado, el [centro de migración de mainframes Azure](https://azure.microsoft.com/migration/mainframe/) puede ayudar a.

## <a name="mainframe-migration"></a>Migración del sistema central

Rehospedar, volver a generar, reemplazar o retirar? ¿IaaS o PaaS? Para determinar la estrategia de migración adecuada para su aplicación de gran sistema, consulte el [migración Mainframe](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview) guía en el centro de arquitectura de Azure.

## <a name="micro-focus-rehosting-platform"></a>Plataforma de rehospedaje foco Micro

Micro Focus Enterprise Server es uno de mainframe mayor rehospedaje plataformas disponibles. Se puede usar ejecutar sus cargas de trabajo de z/OS en un económico x86 platform en Azure.

Primeros pasos:

- [Instalación de Server Enterprise y Developer de Enterprise en Azure](./microfocus/set-up-micro-focus-azure.md)
- [Configuración de CICS BankDemo para desarrollador empresarial en Azure](./microfocus/demo.md)
- [Ejecute Enterprise Server en un contenedor de Docker en Azure](./microfocus/run-enterprise-server-container.md)


## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame en Azure

TmaxSoft OpenFrame es una solución de rehospedaje para mainframe utilizada en escenarios de migración mediante lift-and-shift. Un entorno OpenFrame en Azure es adecuado para el desarrollo, demostraciones, pruebas o las cargas de trabajo de producción.

Primeros pasos:

- [Introducción a TmaxSoft OpenFrame](./tmaxsoft/get-started.md)
- [Descargar el libro electrónico](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/)

## <a name="ibm-zdt-120"></a>IBM zD&T 12.0

Establece un entorno que no sea de producción en Azure que puede usar para el desarrollo, pruebas y demostraciones de aplicaciones basado en z/OS de IBM Z Development y entorno de prueba (IBM zD & T).

El entorno de emulación en Azure puede hospedar diferentes tipos de instancias de Z a través de la aplicación a los desarrolladores controlan distribuciones (ADCDs). Puede ejecutar zD & T Personal Edition, zD & T Sysplex paralelas y zD & T Enterprise Edition en Azure y Azure Stack.

Primeros pasos:

- [Configuración de IBM zD & 12.0 T en Azure](./ibm/install-ibm-z-environment.md)
- [Configurar ADCD zD & T](./ibm/demo.md)

## <a name="ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale en Azure

El entorno de pureScale de IBM DB2 proporciona un clúster de base de datos de Azure. No es idéntico del entorno original, pero ofrece escalabilidad y disponibilidad similares como IBM DB2 para z/OS que se ejecuta en un programa de instalación de Parallel Sysplex.

Para empezar, vea [pureScale de IBM DB2 en Azure](/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Consideraciones

Al migrar las cargas de trabajo del sistema central a la infraestructura de Azure como servicio (IaaS), puede elegir entre varios tipos de los recursos informáticos a petición y escalables, incluidas las máquinas virtuales de Azure. Azure ofrece una gama de [Linux](/azure/virtual-machines/linux/overview) y [Windows](/azure/virtual-machines/windows/overview) máquinas virtuales.

### <a name="compute"></a>Proceso

Capacidad de proceso de Azure se compara favorablemente con capacidad de un gran sistema. Si está pensando en mover una carga de trabajo de mainframes en Azure, compare la métrica del sistema central de un millón de instrucciones por segundo (MIPS) a la CPU virtuales. 

Obtenga información sobre cómo [mover mainframe compute en Azure](./concepts/mainframe-compute-azure.md).

### <a name="high-availability-and-failover"></a>Alta disponibilidad y conmutación por error

Azure ofrece basado en el compromiso de contratos de nivel de servicio (SLA). Disponibilidad nueves múltiplo es el valor predeterminado y SLA se pueden optimizar con la replicación local o según la ubicación de servicios. En el [SLA de Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) completo se explica la disponibilidad garantizada de Azure como un conjunto.

Con IaaS de Azure como una máquina virtual, las funciones específicas del sistema proporcionan compatibilidad con conmutación por error, por ejemplo, conmutación por error las instancias y [conjuntos de disponibilidad](/azure/virtual-machines/windows/regions-and-availability#availability-sets). Cuando se usa la plataforma Windows Azure como recursos de un servicio (PaaS), la plataforma controla automáticamente la conmutación por error. Algunos ejemplos son [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) y [Azure Cosmos DB](/azure/cosmos-db/introduction).

### <a name="scalability"></a>Escalabilidad

Los sistemas centrales suelen escalarse verticalmente, mientras que los entornos de nube se escalan horizontalmente. Azure ofrece una gama de [Linux](/azure/virtual-machines/linux/sizes) y [Windows](/azure/virtual-machines/windows/sizes) tamaños para satisfacer sus necesidades. La nube también se puede escalar hacia arriba o hacia abajo hasta las especificaciones de coincidencia exacta del usuario. Power, almacenamiento y servicios de proceso [escala](/azure/architecture/best-practices/auto-scaling) bajo demanda en un modelo de facturación basada en uso.

### <a name="storage"></a>Almacenamiento

En la nube, tiene una gama de opciones de almacenamiento flexible, escalable y solo paga por lo que necesita. [Azure Storage](/azure/storage/common/storage-introduction) ofrece un almacén de objetos que se puede escalar de forma masiva destinado a objetos de datos, un servicio de sistema de archivos para la nube, un almacén de mensajería para mensajería confiable y un almacén NoSQL. En cuanto a las máquinas virtuales, los discos administrados y no administrados proporcionan un almacenamiento de disco persistente y seguro.

Obtenga información sobre cómo [mover el almacenamiento de mainframe a Azure](./concepts/mainframe-storage-azure.md).

### <a name="backup-and-recovery"></a>Copia de seguridad y recuperación

Mantener su propio sitio de recuperación ante desastres, puede ser una propuesta costosa. Azure tiene opciones rentables y fácil de implementar para [copia de seguridad](/azure/backup/backup-introduction-to-azure-backup), [recuperación](/azure/site-recovery/site-recovery-overview), y [redundancia](/azure/storage/common/storage-redundancy) en niveles regionales o locales, o a través de la redundancia geográfica.

## <a name="azure-government-for-mainframe-migrations"></a>Azure Government para las migraciones del sistema central

Muchas entidades del sector público encantaría mover sus aplicaciones del sistema central a una plataforma flexible, más moderna. Microsoft Azure Government es una instancia separada físicamente de la plataforma global de Microsoft Azure: empaqueta para sistemas gubernamentales federales, estatales y locales. Proporciona servicios de cumplimiento específicamente para las agencias gubernamentales de Estados Unidos y sus asociados, protección y seguridad de talla mundial.

Azure Government había obtenido una autoridad Provisional para operar (P-ATO) de FedRAMP gran impacto para los sistemas que necesiten este tipo de entorno.

Para comenzar, descargue [en la nube de Microsoft Azure Government para aplicaciones de mainframe](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="next-steps"></a>Pasos siguientes

Formular nuestro [asociados](partner-workloads.md) que le ayudarán a migrar o rehospedar aplicaciones del sistema central. Para obtener instrucciones detalladas sobre cómo elegir una solución de asociado, vea el [plataforma Modernización Alliance](https://www.platformmodernization.org/pages/mainframe.aspx) sitio Web.

Consulte también:

- [Notas del producto acerca de temas del sistema central](mainframe-white-papers.md)
- [Migración del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Solución de problemas](/azure/virtual-machines/troubleshooting/)
- [Desmitificación del sistema central para la migración a Azure](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
