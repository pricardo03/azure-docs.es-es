---
title: IBM DB2 pureScale en Azure
description: En este artículo, se muestra una arquitectura para ejecutar un entorno de IBM DB2 pureScale en Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 901afc8f28b617eb5bada2a0f58761ddb9f67607
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327421"
---
# <a name="ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale en Azure

El entorno de IBM DB2 pureScale proporciona una solución de clúster de base de datos para Azure con alta disponibilidad y escalabilidad en sistemas operativos Linux. En este artículo se muestra una arquitectura para ejecutar DB2 pureScale en Azure.

## <a name="overview"></a>Información general

Durante mucho tiempo, las empresas han usado plataformas para sistemas de administración de bases de datos relacionales (RDBMS) para sus necesidades de procesamiento de transacciones en línea (OLTP). En la actualidad, muchas están migrando sus entornos de base de datos basados en sistemas centrales a Azure como una forma de expandir la capacidad, reducir los costos y mantener una estructura de costos operacionales constante.

A menudo, la migración es el primer paso en la modernización de una plataforma anterior. Por ejemplo, un cliente empresarial recientemente cambió el hospedaje de su entorno de IBM DB2 con z/OS a IBM DB2 pureScale en Azure. Aunque no es idéntico al entorno original, IBM DB2 pureScale en Linux ofrece características de alta disponibilidad y escalabilidad similares a las de IBM DB2 para z/OS ejecutado en una configuración de Parallel Sysplex del sistema central.

En este artículo se describe la arquitectura que se usó para esta migración de Azure. El cliente usó Red Hat Linux 7.4 para probar la configuración. Esta versión está disponible en Microsoft Azure Marketplace. Antes de elegir una distribución de Linux, asegúrese de comprobar las versiones admitidas actualmente. Para más información, consulte la documentación de [IBM DB2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) y [GlusterFS](https://docs.gluster.org/en/latest/).

Este artículo constituye un punto de partida para su plan de implementación de DB2. Sus requisitos empresariales variarán, pero se puede aplicar el mismo patrón básico. Este patrón de arquitectura también se puede utilizar para aplicaciones de procesamiento analítico en línea (OLAP) en Azure.

En este artículo no se cubren las diferencias ni las posibles tareas de migración para mover una base de datos de IBM DB2 para z/OS a IBM DB2 pureScale con Linux. Tampoco se proporcionan estimaciones de ajuste de tamaño ni análisis de carga de trabajo para migrar de DB2 z/OS a DB2 pureScale. 

Para ayudarle a decidir cuál es la mejor arquitectura DB2 pureScale para su entorno, se recomienda que realice un ejercicio de estimación de tamaño completo y formule una hipótesis. En el sistema de origen, asegúrese de que tiene en cuenta a DB2 z/OS Parallel Sysplex con la arquitectura de uso compartido de datos, la configuración de instalaciones de acoplamiento y las estadísticas de uso de la instalación de datos distribuidos (DDF).

> [!NOTE]
> En este artículo se describe un enfoque a la migración en DB2, pero existen otros. Por ejemplo, DB2 pureScale también puede ejecutarse en entornos virtualizados de forma local. IBM admite DB2 en Microsoft Hyper-V con diversas configuraciones. Para más información, consulte [arquitectura de virtualización de DB2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) en IBM Knowledge Center.

## <a name="architecture"></a>Arquitectura

Para admitir la alta disponibilidad y escalabilidad en Azure, puede usar una arquitectura con escalabilidad horizontal y datos compartidos para DB2 pureScale. La siguiente arquitectura de ejemplo se usó para la migración de clientes.

![Db2 pureScale en máquinas virtuales de Azure, se muestran el almacenamiento y las redes](media/db2-purescale-on-azure/pureScaleArchitecture.png "DB2 pureScale on Azure virtual machines showing storage and networking")


En el diagrama se muestran las capas lógicas necesarias para un clúster de DB2 pureScale. Estas incluyen máquinas virtuales para un cliente, para la administración, para almacenar en caché, para el motor de base de datos y para el almacenamiento compartido. 

Además de los nodos del motor de base de datos, el diagrama incluye dos nodos que se usan para los servicios de caché para clústeres (CF). Como mínimo se usan dos nodos para el propio motor de base de datos. A un servidor DB2 que pertenece a un clúster de pureScale se le llama miembro. 

El clúster se conecta mediante iSCSI a un clúster de almacenamiento compartido de GlusterFS con tres nodos para proporcionar almacenamiento con escalabilidad horizontal y alta disponibilidad. DB2 pureScale se instala en máquinas virtuales de Azure que ejecutan Linux.

Este enfoque es una plantilla que se puede modificar según el tamaño y escalado de su organización. Se basa en lo siguiente:

-   Dos o más miembros de la base de datos se combinan con al menos dos nodos de CF. Los nodos administran un grupo de búferes global (GBP) para que los servicios de memoria compartida y del gestor de bloqueo global (GLM) controlen el acceso compartido y bloqueen la contención de los usuarios activos. Una vez que el nodo CF actúa como nodo principal y el otro como el secundario, conmute por error al nodo CF. Para evitar un único punto de error en el entorno, un clúster de DB2 pureScale requiere al menos cuatro nodos.

-   Almacenamiento compartido de alto rendimiento (se muestra en el tamaño P30 en el diagrama). Todos los nodos de Gluster FS usan este almacenamiento.

-   Redes de alto rendimiento para los miembros de datos y el almacenamiento compartido.

### <a name="compute-considerations"></a>Consideraciones de proceso

Esta arquitectura ejecuta las capas de datos, de aplicación y de almacenamiento en las máquinas virtuales de Azure. Los [scripts de configuración de implementación](http://aka.ms/db2onazure) crean lo siguiente:

-   Un clúster de DB2 pureScale. El tipo de recursos de proceso que necesita en Azure depende de su configuración. En general, puede usar dos enfoques:

    -   Usar una red de informática de alto rendimiento (HPC) de varios nodos en la que varias instancias pequeñas y medianas acceden al almacenamiento compartido. Para este tipo de configuración de HPC, las [máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) de Azure de la serie E optimizadas para memoria o de la serie L optimizadas para almacenamiento proporcionan la potencia de proceso necesaria.

    -   Usar menos instancias grandes de máquinas virtuales para los motores de datos. Para instancias grandes, las máquinas virtuales de la [serie M](https://azure.microsoft.com/pricing/details/virtual-machines/series/) optimizadas para memoria son idóneas para cargas de trabajo intensas en memoria. Es posible que necesite una instancia dedicada, según el tamaño de la partición lógica (LPAR) que se usa para ejecutar DB2.

-   DB2 CF utiliza máquinas virtuales optimizadas para memoria, como las de la serie E o la serie L.

-   El almacenamiento de GlusterFS utiliza máquinas virtuales Standard\_DS4\_v2 con Linux.

-   El JumpBox de GlusterFS utiliza máquinas virtuales Standard\_DS2\_v2 con Linux.

-   El cliente utiliza máquinas virtuales Standard\_DS3\_v2 con Windows (se usan para pruebas).

-   Un servidor testigo utiliza máquinas virtuales Standard\_DS3\_v2 con Linux (se usan para DB2 pureScale).

> [!NOTE]
> Un clúster de DB2 pureScale requiere al menos dos instancias de DB2. También se requiere una instancia de memoria caché y un administrador de bloqueos.

### <a name="storage-considerations"></a>Consideraciones sobre el almacenamiento

Al igual que Oracle RAC, DB2 pureScale es una base de datos con escalabilidad horizontal y E/S de bloque de alto rendimiento. Se recomienda usar la opción de [disco SSD Premium de Azure](disks-types.md) mayor que mejor se adapte a sus necesidades. Las opciones de almacenamiento más pequeñas pueden ser adecuadas para entornos de desarrollo y pruebas, mientras que los entornos de producción a menudo requieren mayor capacidad de almacenamiento. La arquitectura de ejemplo usa [P30](https://azure.microsoft.com/pricing/details/managed-disks/) debido a la relación entre E/S por segundo y su tamaño y precio. Independientemente del tamaño, utilice Premium Storage para obtener el mejor rendimiento.

DB2 pureScale usa una arquitectura que lo comparte todo, en la que todos los datos son accesibles desde todos los nodos del clúster. Premium Storage debe compartirse entre instancias, sin importar si son instancias a petición o dedicadas.

Un clúster de DB2 pureScale grande puede requerir un almacenamiento compartido Premium de 200 terabytes (TB) o más, con un número de E/S por segundo de 100 000. DB2 pureScale es compatible con una interfaz de bloque de iSCSI que puede usar en Azure. La interfaz iSCSI requiere un clúster de almacenamiento compartido que se pueda implementar con GlusterFS, S2D u otra herramienta. Este tipo de solución crea un dispositivo de red de área de almacenamiento (vSAN) en Azure. DB2 pureScale usa la vSAN para instalar el sistema de archivos en clúster que se utiliza para compartir datos entre máquinas virtuales.

La arquitectura de ejemplo usa GlusterFS, un sistema de archivos distribuido, de código abierto, gratuito y escalable que está optimizado específicamente para el almacenamiento en la nube.

### <a name="networking-considerations"></a>Consideraciones sobre redes

IBM recomienda las redes InfiniBand para todos los miembros de un clúster de DB2 pureScale. DB2 pureScale también usa el acceso directo a memoria remota (RDMA), cuando es posible, para los CF.

Durante la instalación, se crea un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) de Azure para contener todas las máquinas virtuales. Por lo general, agrupa los recursos según su vigencia y quién los administra. Las máquinas virtuales de esta arquitectura requieren [redes aceleradas](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Es una característica de Azure que proporciona una latencia de red muy baja y coherente a través de la virtualización de E/S de raíz única (SR-IOV) a una máquina virtual.

Cada máquina virtual de Azure se implementa en una red virtual que tiene varias subredes: principal, front-end de Gluster FS (gfsfe), back-end de Gluster FS (bfsbe), DB2 pureScale (db2be) y front-end de DB2 purescale (db2fe). El script de instalación también crea la [tarjeta de interfaz de red](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) principal en las máquinas virtuales de la subred principal.

Use [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) para restringir el tráfico de red en la red virtual y para aislar las subredes.

En Azure, DB2 pureScale debe usar TCP/IP como la conexión de red para el almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

-   [Implementar esta arquitectura en Azure](deploy-ibm-db2-purescale-azure.md)
