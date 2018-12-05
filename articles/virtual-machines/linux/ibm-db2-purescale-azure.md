---
title: IBM Db2 pureScale en Azure
description: En este artículo, se muestra una arquitectura para ejecutar un entorno de IBM Db2 pureScale en Azure.
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
ms.openlocfilehash: 5a7ae96a3730df2d0dae04dde6d7609ce69cdee5
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "51978214"
---
# <a name="ibm-db2-purescale-on-azure"></a>IBM Db2 pureScale en Azure

El entorno de IBM Db2 pureScale proporciona una solución de clúster de base de datos de Azure con alta disponibilidad y escalabilidad en sistemas operativos Linux. En este artículo, se muestra una arquitectura para ejecutar Db2 pureScale en Azure.

## <a name="overview"></a>Información general

Durante mucho tiempo, las empresas han usado plataformas tradicionales para sistemas de administración de bases de datos relacionales (RDBMS) a fin de satisfacer las necesidades de procesamiento de transacciones en línea (OLTP). En la actualidad, muchas están migrando sus entornos de base de datos basados en sistemas centrales a Azure como una forma de expandir la capacidad, reducir los costos y mantener una estructura de costos operacionales constante.

A menudo, la migración es el primer paso en la modernización de una plataforma heredada. Por ejemplo, una empresa recientemente cambió el hospedaje de su entorno de IBM Db2 con z/OS a IBM Db2 pureScale en Azure. Aunque no es idéntico al entorno original, IBM Db2 pureScale en Linux ofrece características de alta disponibilidad y escalabilidad similares a las de IBM Db2 para z/OS ejecutado en una configuración de Parallel Sysplex del sistema central.

En este artículo se describe la arquitectura que se usó para esta migración de Azure. Se usó Red Hat Linux 7.4 para probar la configuración. Esta versión está disponible en Microsoft Azure Marketplace. Antes de elegir una distribución de Linux, asegúrese de comprobar las versiones admitidas actualmente. Para obtener más información, consulte la documentación de [IBM Db2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) y [GlusterFS](https://docs.gluster.org/en/latest/).

En este artículo, simplemente se muestra el punto de partida para su plan de implementación de Db2. Sus requisitos empresariales variarán, pero se puede aplicar el mismo patrón básico. Este modelo de arquitectura también se puede utilizar para aplicaciones de procesamiento analítico en línea (OLAP) en Azure.

En este artículo no se cubren las diferencias ni las posibles tareas de migración para mover una base de datos de IBM Db2 para z/OS a IBM Db2 pureScale con Linux. Tampoco se proporcionan estimaciones de ajuste de tamaño equivalente ni análisis de carga de trabajo para migrar de Db2 z/OS a Db2 pureScale. Para ayudarle a decidir cuál es la mejor arquitectura Db2 pureScale para su entorno, se recomienda encarecidamente que realice un ejercicio de estimación de tamaño completo y cree una hipótesis. Entre otros factores, asegúrese de que en el sistema de origen considera a Db2 z/OS Parallel Sysplex con la arquitectura de uso compartido de datos, la configuración de instalaciones de acoplamiento y las estadísticas de uso DDF.

> [!NOTE]
> En este artículo se describe un enfoque a la migración en Db2, pero existen otros. Por ejemplo, Db2 pureScale también puede ejecutarse en entornos virtualizados de forma local. IBM admite Db2 en Microsoft Hyper-V con diversas configuraciones. Para obtener más información, consulte [arquitectura de virtualización de Db2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) en IBM Knowledge Center.

## <a name="architecture"></a>Arquitectura

Para admitir la alta disponibilidad y escalabilidad en Azure, se puede usar una arquitectura con escalabilidad horizontal y datos compartidos para Db2 pureScale. La siguiente arquitectura de ejemplo se usó para la migración de nuestros clientes.

![](media/db2-purescale-on-azure/pureScaleArchitecture.png "Db2 pureScale en máquinas virtuales de Azure, se muestran el almacenamiento y las redes")


En el diagrama de arquitectura se muestran las capas lógicas necesarias para un clúster de Db2 pureScale. Estas incluyen máquinas virtuales para un cliente, para la administración, para almacenar en caché, para el motor de base de datos y para el almacenamiento compartido. Además de los nodos del motor de base de datos, el diagrama también incluye dos nodos que se usan para lo que se conoce como servicios de caché para clústeres (CF). Se utiliza un mínimo de dos nodos para el motor de base de datos. Se denomina como "miembro" al servidor Db2 que pertenece a un clúster de pureScale. El clúster se conecta mediante iSCSI a un clúster de almacenamiento compartido de GlusterFS con tres nodos para proporcionar almacenamiento con escalabilidad horizontal y alta disponibilidad. Db2 pureScale está instalado en máquinas virtuales de Azure que ejecutan Linux.

Este enfoque es simplemente una plantilla que se puede modificar según el tamaño y escala necesarios para su organización y se basa en la siguiente:

-   Dos o más miembros de la base de datos se combinan con al menos dos nodos de CF, que administran un grupo de búferes global (GBP) para que los servicios de memoria compartida y del gestor de bloqueo global (GLM) controlen el acceso compartido y bloqueen la contención de varios usuarios activos. Una vez que el nodo CF actúa como nodo principal y el otro como el secundario, conmute por error al nodo CF. Para evitar la creación de un entorno que tiene un único punto de error, se requiere un mínimo de cuatro nodos para cada clúster de Db2 pureScale.

-   Almacenamiento compartido de alto rendimiento almacenamiento (se muestra en el tamaño P30 de la figura 1), que utiliza cada uno de los nodos Gluster FS.

-   Redes de alto rendimiento para los miembros de datos y el almacenamiento compartido.

### <a name="compute-considerations"></a>Consideraciones de proceso

Esta arquitectura ejecuta las capas de datos, de aplicación y de almacenamiento en las máquinas virtuales de Azure. Los [scripts de configuración de implementación](http://aka.ms/db2onazure) crean lo siguiente:

-   Un clúster de Db2 pureScale. El tipo de recursos de proceso que necesita en Azure depende de su configuración. En general, puede usar dos métodos:

    -   Usar una red de informática de alto rendimiento (HPC) de varios nodos en la que varias instancias pequeñas y medianas acceden al almacenamiento compartido. Para este tipo de configuración de HPC, las [máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) de Azure de la serie E optimizadas para memoria o de la serie L optimizadas para almacenamiento proporcionan la potencia de proceso necesaria.

    -   Usar menos instancias grandes de máquinas virtuales para los motores de datos. Para las instancias grandes, las máquinas virtuales más grandes de la [serie M](https://azure.microsoft.com/pricing/details/virtual-machines/series/) optimizadas para memoria son ideales para las cargas de trabajo en memoria, pero puede requerir una instancia dedicada según el tamaño de la partición lógica (LPAR) que se use para ejecutar Db2.

-   Db2 CF utiliza máquinas virtuales optimizadas para memoria, como las de la serie E o la serie L.

-   El almacenamiento de GlusterFS utiliza máquinas virtuales Standard\_DS4\_v2 con Linux.

-   El JumpBox de GlusterFS utiliza máquinas virtuales Standard\_DS2\_v2 con Linux.

-   El cliente utiliza máquinas virtuales Standard\_DS3\_v2 con Windows (se usan para pruebas).

-   Un servidor testigo utiliza máquinas virtuales Standard\_DS3\_v2 con Linux (se usan para Db2 pureScale).

> [!NOTE]
> Se requiere un mínimo de dos instancias de Db2 en un clúster de Db2 pureScale. También se requieren una instancia de memoria caché y un gestor de bloqueos.

### <a name="storage-considerations"></a>Consideraciones sobre el almacenamiento

Al igual que Oracle RAC, Db2 pureScale es una base de datos con escalabilidad horizontal y E/S de bloque de alto rendimiento. Se recomienda usar la opción de [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) más grande que mejor se adapte a sus necesidades. Por ejemplo, las opciones de almacenamiento más pequeñas pueden ser adecuadas para entornos de desarrollo y pruebas, mientras que los entornos de producción a menudo requieren mayor capacidad de almacenamiento. La arquitectura de ejemplo usa [P30](https://azure.microsoft.com/pricing/details/managed-disks/) debido a la relación entre E/S por segundo y su tamaño y precio. Independientemente del tamaño, utilice Premium Storage para obtener el mejor rendimiento.

Db2 pureScale usa una arquitectura que lo comparte todo, en la que todos los datos son accesibles desde todos los nodos del clúster. Premium Storage debe compartirse entre varias instancias, sin importar si son instancias a petición o dedicadas.

Un clúster de Db2 pureScale grande puede requerir un almacenamiento compartido Premium de 200 terabytes (TB) o más, con un número de E/S por segundo de 100 000. Db2 pureScale es compatible con una interfaz de bloque de iSCSI que puede usarse en Azure. La interfaz iSCSI requiere un clúster de almacenamiento compartido que se pueda implementar con GlusterFS, S2D u otra herramienta. Este tipo de solución crea un dispositivo de red de área de almacenamiento (vSAN) en Azure. Sb2 pureScale Db2 usa la vSAN para instalar el sistema de archivos en clúster que se utiliza para compartir datos entre varias máquinas virtuales.

Para esta arquitectura, hemos utilizado el sistema de archivos de GlusterFS, un sistema de archivos distribuido de código abierto, gratuito y escalable optimizado específicamente para el almacenamiento en la nube.

### <a name="networking-considerations"></a>Consideraciones sobre redes

IBM recomienda las redes InfiniBand para todos los miembros de un clúster de Db2 pureScale; además, Db2 pureScale también utiliza, cuando es posible, el acceso directo a memoria remota (RDMA) para los CF utilizados.

Durante la instalación, se crea un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) de Azure para contener todas las máquinas virtuales. En general, los recursos se agrupan según su duración y quién los administra. Las máquinas virtuales en esta arquitectura requieren [redes aceleradas](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/), una característica de Azure que proporciona una latencia de red muy baja y coherente a través de la virtualización de E/S de raíz única (SR-IOV) a una máquina virtual.

Cada máquina virtual de Azure se implementa en una red virtual que está segmentada en varias subredes: principal, front-end de Gluster FS (gfsfe), back-end de Gluster FS (bfsbe), Db2 pureScale (db2be) y front-end de Db2 purescale (db2fe). El script de instalación también crea la [tarjeta de interfaz de red](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) principal en las máquinas virtuales de la subred principal.

[Los grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) se usan para restringir el tráfico de red en la red virtual y para aislar las subredes.

En Azure, Db2 pureScale debe usar TCP/IP como la conexión de red para el almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

-   [Implementar esta arquitectura en Azure](deploy-ibm-db2-purescale-azure.md)
