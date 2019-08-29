---
title: Lista de comprobación de planeamiento e implementación de cargas de trabajo de SAP | Microsoft Docs
description: Lista de comprobación de planeamiento e implementación de cargas de trabajo de SAP en Azure
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2d6170dead4e8577cea6883ffea25b90ebe39b88
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70079005"
---
# <a name="sap-workload-on-azure-planning-and-deployment-checklist"></a>Lista de comprobación de planeamiento e implementación de cargas de trabajo de SAP en Azure 

Esta lista de comprobación está diseñada para los clientes que trasladan sus aplicaciones SAP NetWeaver, S/4HANA e Hybris a la infraestructura como servicio de Azure.  Esta lista de comprobación debe ser revisada por un cliente o partner SAP durante la duración del proyecto. Es importante que muchas de las comprobaciones se realicen al principio del proyecto y en la fase de planeamiento. Una vez realizada la implementación, pueden complicarse los cambios elementales en la infraestructura de Azure o en las versiones de software de SAP implementadas. Revise esta lista de comprobación en los hitos clave a lo largo de un proyecto.  Los problemas pequeños pueden detectarse antes de que se conviertan en problemas grandes y existe tiempo suficiente para volver a diseñar y probar los cambios necesarios. La lista de comprobación no pretende ser completa en modo alguno. En función de sus situación individual, es posible que haya que realizar muchas más comprobaciones. 

La lista de comprobación recopilada no incluye tareas que son independientes de Azure.  Ejemplo: Las interfaces de aplicaciones SAP cambian durante un traslado a la nube pública de Azure o a un proveedor de hospedaje.    

Esta lista de comprobación puede usarse también con los sistemas ya implementados. Es posible que se hayan agregado nuevas características, como el Acelerador de escritura y Availability Zones, y nuevos tipos de máquina virtual desde su implementación.  Por lo tanto, resulta útil revisar la lista de comprobación periódicamente para asegurarse de que está al tanto de las nuevas características de la plataforma Azure. 

## <a name="project-preparation-and-planning-phase"></a>Fase de preparación y planificación del proyecto
En esta fase se planea una migración de cargas de trabajo de SAP a la nube pública de Azure. Se describe el conjunto mínimo de entidades y elementos que definen la lista tales como:

1. Documento de diseño de alto nivel: este documento debe contener:
    1. El inventario actual de componentes y aplicaciones de SAP y el inventario de aplicaciones de destino en Azure
    2. Cree y trabaje con una matriz de asignación de responsabilidades (RACI) que defina las responsabilidades y asignaciones de los elementos implicados. Empiece en el nivel superior y trabaje cada vez en niveles más detallados durante la planeación y las primeras implementaciones
    2. Arquitectura de una solución de alto nivel
    3. La decisión sobre las regiones de Azure en las que se va a implementar. Para obtener la lista de las regiones de Azure, consulte [Regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Para conocer los servicios disponibles en cada una de las regiones de Azure, consulte el artículo [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/).
    4. La arquitectura de red para conectarse desde el entorno local con Azure. Empiece a familiarizarse con el [plano técnico del centro de datos virtual de Azure](https://docs.microsoft.com/azure/architecture/vdc/).
    5. Los principios de seguridad para la ejecución de datos empresariales de gran impacto en Azure. Como material de lectura, comience por [Documentación de Azure Security Center](https://docs.microsoft.com/azure/security/).
2.  El documento de diseño técnico, que contiene:
    1.  Un diagrama de bloques de la solución. 
    2.  Tamaño de los componentes de proceso, almacenamiento y red en Azure. Para conocer el tamaño de VM de Azure en SAP, consulte la nota de soporte técnico de SAP [#1928533](https://launchpad.support.sap.com/#/notes/1928533). 
    3.  La arquitectura de continuidad empresarial y recuperación ante desastres.
    4.  Las versiones detalladas de los paquetes de soporte técnico del sistema, la base de datos, el kernel y SAP. El hecho de que SAP NetWeaver o S/4HANA admita una versión de sistema operativo determinada no implica que las máquinas virtuales de Azure también la admitan. Lo mismo puede decirse de las versiones de DBMS. Es obligatorio que se comprueben los siguientes orígenes para alinear y, si es necesario, actualizar las versiones de SAP, DBMS o OS para que aparezcan en una ventana compatible con Azure y SAP. Es obligatorio que se encuentre dentro de las combinaciones de versiones compatibles con SAP y Azure para obtener soporte técnico completo por parte de SAP y Microsoft. Si es necesario, deberá planear la actualización de algunos de los componentes de software. Se documentan más detalles sobre el software SAP, SO y DBMS compatible en estas ubicaciones:
        1.  Nota de soporte técnico de SAP [#1928533](https://launchpad.support.sap.com/#/notes/1928533). En esta nota se definen las versiones mínimas del SO compatibles con VM de Azure. También se definen las versiones mínimas de la base de datos necesarias para la mayoría de las bases de datos que no son HANA. La nota también presenta el tamaño de SAP de los distintos tipos de máquina virtual de Azure compatibles con SAP.
        2.  Nota de soporte técnico de SAP [2039619 #](https://launchpad.support.sap.com/#/notes/2039619). En esta nota se define la matriz de compatibilidad de Oracle en Azure. Tenga en cuenta que Oracle solo admite Windows y Oracle Linux como sistema operativo invitado en Azure para cargas de trabajo de SAP. Esta declaración de soporte se aplica también al nivel de aplicación de SAP que ejecuta instancias de SAP. No obstante, Oracle no permite utilizar una alta disponibilidad para SAP Central Services en Oracle Linux mediante Pacemaker. Si necesita tener una alta disponibilidad para ASCS en Oracle Linux, debe utilizar SIOS Protection Suite para Linux. Para obtener información detallada sobre la certificación de SAP, consulte la nota de soporte técnico de SAP [#1662610 - Support details for SIOS Protection Suite for Linux](https://launchpad.support.sap.com/#/notes/1662610) (1662610: información de soporte técnico sobre SIOS Protection Suite para Linux). Para Windows, la solución de clúster de conmutación por error de Windows que admite SAP para SAP Central Services se admite en unión con Oracle como capa de DBMS. 
        3.  La nota de soporte técnico de SAP [#2235581](https://launchpad.support.sap.com/#/notes/2235581) para obtener la matriz de compatibilidad con las versiones de SAP HANA en el otro sistema operativo.
        4.  Las máquinas virtuales de Azure compatibles con SAP HANA y las [instancias grandes de HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) aparecen [aquí](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).
        5.  [Matriz de disponibilidad de productos SAP](https://support.sap.com/en/).
        6.  Otras notas de SAP para otros productos específicos de SAP.  
    5.  Se recomienda utilizar diseños de 3 niveles estrictos para los sistemas de producción de SAP. No se recomienda combinar ASCS con servidores de aplicación en la misma máquina virtual.  Se admite el uso de configuraciones de clúster de varios SID de SAP Central Services con Windows como sistema operativo invitado en Azure. Mientras que las configuraciones de clúster de varios SID de SAP Central Services no se admiten con sistemas operativos Linux en Azure. Encontrará documentación sobre el caso del sistema operativo invitado de Windows en:
        1.  [Alta disponibilidad con varios identificadores de seguridad de instancia de ASCS/SCS de SAP para los clústeres de conmutación por error de Windows Server y el disco compartido en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
        2.  [Alta disponibilidad con varios identificadores de seguridad de instancia SAP ASCS/SCS para los clústeres de conmutación por error de Windows Server y los recursos compartidos de archivos en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)
    6.  Arquitectura de alta disponibilidad y recuperación ante desastres
        1.  Defina en función de RTO y RPO cómo debe ser la arquitectura de alta disponibilidad y recuperación de desastres.
        2.  Para lograr alta disponibilidad en la misma zona, compruebe lo que el DBMS deseado le ofrece en Azure. La mayoría de los DBMS ofrecen métodos sincrónicos de una espera activa sincrónica, lo que se recomienda para los sistemas de producción. Compruebe también la documentación relacionada con SAP para las diferentes bases de datos empezando por [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
            1.  El uso del servicio de clúster de conmutación por error de Windows con configuración de disco compartido para la capa de DBMS como, por ejemplo, se ha descrito [aquí](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server?view=sql-server-2017) para SQL Server **NO** se admite. En su lugar, use soluciones como:
                1.  [SQL Server AlwaysOn](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups) 
                2.  [Oracle Data Guard](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)
                3.  [HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
        3.  En cuanto a la recuperación ante desastres entre regiones de Azure, compruebe qué posibilidades ofrecen los distintos proveedores de DBMS. La mayoría de ellos admiten replicación asincrónica o trasvase de registros.
        4.  Para el nivel de aplicación de SAP, defina si quiere ejecutar sistemas de prueba de regresión empresarial que, en condiciones ideales, son réplicas de las implementaciones de producción en la misma región de Azure o la región de recuperación ante desastres. En el último caso, puede utilizar ese sistema de regresión empresarial como destino de recuperación ante desastres del entorno de producción.
        5.  Si decide no situar sistemas de no producción en el sitio de recuperación ante desastres, considere la posibilidad de usar Azure Site Recovery como método viable para replicar el nivel de aplicación de SAP en la región de recuperación ante desastres de Azure. Consulte también [Configuración de la recuperación ante desastres para la implementación de una aplicación de SAP NetWeaver de niveles múltiples](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap). 
        6.  Si decide usar una configuración de alta disponibilidad y recuperación ante desastres combinada que aproveche [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview), familiarícese con las regiones de Azure donde hay instancias de Availability Zones disponibles y con las restricciones que pueden presentarse al aumentar las latencias de red entre dos de estas instancias.  
3.  El cliente o partner debe crear un inventario de todas las interfaces de SAP (SAP y que no sean de SAP). 
4.  Diseño de Foundation Services Design: este diseño incluiría elementos tales como:
    1.  Active Directory y diseño de DNS
    2.  La topología de red dentro de Azure y la asignación de distintos sistemas SAP
    3.  La estructura del [acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/overview) para los diferentes equipos que administran la infraestructura y las aplicaciones de SAP en Azure.
    3.  La topología del grupo de recursos. 
    4.  La [estrategia de etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#tags-and-billing).
    5.  La convención de nomenclatura para las máquinas virtuales y otros componentes de la infraestructura y/o los nombres lógicos.
5.  Contrato de soporte técnico Premier de Microsoft Azure: identifique al responsable técnico de cuenta (TAM) de MS. Para conocer a los requisitos de soporte técnico de SAP, lea la nota de soporte técnico de SAP [#2015553](https://launchpad.support.sap.com/#/notes/2015553). 
6.  Defina el número de suscripciones de Azure y la cuota de núcleos para las distintas suscripciones. [Abra solicitudes de soporte técnico para aumentar las cuotas de suscripciones de Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) según sea necesario. 
7.  Reducción de datos y plan de migración de datos para migrar datos SAP a Azure. Para los sistemas SAP NetWeaver, SAP tiene instrucciones detalladas sobre cómo mantener limitado el volumen de una gran cantidad de datos. SAP ha publicado [esta guía exhaustiva](https://help.sap.com/http.svc/rc/2eb2fba8f8b1421c9a37a8d7233da545/7.0/en-US/Data_Management_Guide_Version_70E.PDF) sobre la administración de datos en los sistemas SAP ERP. Aunque parte del contenido se aplica a los sistemas de NetWeaver y S/4HANA en general.
8.  Defina y decida el enfoque de implementación automatizada. El objetivo de la automatización detrás de las implementaciones de infraestructura en Azure es implementar de manera determinista y obtener resultados deterministas. Muchos clientes usan scripts de Power Shell o de basados en la CLI. Pero hay diversas tecnologías de código abierto que pueden usarse para implementar la infraestructura de Azure para SAP e incluso instalar software de SAP. Encontrará ejemplos en GitHub:
    1.  [Automated SAP Deployments in Azure Cloud](https://github.com/Azure/sap-hana) (Implementaciones automatizadas de SAP en la nube de Azure)
    2.  [Instalación de SAP HANA](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)
9.  Defina una cadencia periódica de revisión del diseño y la implementación entre usted como cliente, el integrador de sistemas, Microsoft y otras partes implicadas.

 
## <a name="pilot-phase-strongly-recommended"></a>Fase piloto (se recomienda totalmente)
 
La fase piloto puede ejecutarse antes o en paralelo con la planeamiento y preparación del proyecto. Esta fase también puede utilizarse para probar los enfoques y el diseño realizados en la fase de planeamiento y preparación. La fase piloto puede extenderse hasta convertirse en una verdadera prueba de conceptos. Se recomienda configurar y validar una solución completa de alta disponibilidad y recuperación ante desastres, así como el diseño de la seguridad durante una implementación piloto. En el caso de algunos clientes, también se pueden realizar pruebas de escalabilidad en esta fase. Otros clientes usan la implementación de sistemas de espacio aislado de SAP como fase piloto. Por lo que es de suponer que ha identificado un sistema que quiere a migrar a Azure con el fin de ejecutar a una prueba piloto.

1. Optimice la transferencia de datos a Azure. Al depender en gran medida de los casos de cliente, la transferencia a través de [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) desde el entorno local era más rápida si el circuito de Express tenía suficiente ancho de banda. Con otros clientes, resultó ser más rápido hacerlo por Internet.
2. En el caso de una migración de plataforma heterogénea de SAP, que implica una exportación e importación de los datos de la base de datos, se deben probar y optimizar las fases de exportación e importación. Para migraciones de gran tamaño que implican SQL Server como plataforma de destino, encontrará recomendaciones [aquí](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAP-OS-DB-Migration-to-SQL-Server-8211-FAQ-v6-2-April-2017/ba-p/368070). Puede adoptar el enfoque de Migration Monitor/SWPM en caso de que no necesite una actualización de la versión combinada o el proceso [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) cuando combine la migración con una actualización de la versión de SAP y cumpla determinadas combinaciones de la plataforma DBMS de origen y destino, tal y como se documentó, por ejemplo, en [Database Migration Option (DMO) de SUM 2.0 SP03](https://launchpad.support.sap.com/#/notes/2631152). 
   1.  Rendimiento de la exportación al origen, la carga de archivos de exportación en Azure e y la importación.  Maximice la superposición entre la exportación e importación.
   2.  Evalúe el volumen de la base de datos entre el destino y la plataforma de destino para que se refleje en el tamaño de la infraestructura.    
   3.  Valide y optimice los intervalos. 
3. Validación técnica 
   1. Tipos de VM
      1.  Valide de nuevo los recursos en las notas de soporte técnico de SAP, el directorio de hardware de SAP HANA y SAP PAM para asegurarse de que no ha habido cambios en las máquinas virtuales compatibles con Azure, las versiones admitidas del sistema operativo para esos tipos de máquinas virtuales y las versiones admitidas de SAP DBMS.
      2.  Valide de nuevo el tamaño de la aplicación y la infraestructura que implementa en Azure. En caso de trasladar aplicaciones existentes, a menudo puede derivar los valores de SAPS necesarios de la infraestructura que utiliza y la [página web del banco de pruebas de SAP](https://www.sap.com/dmc/exp/2018-benchmark-directory/#/sd) y compararlos con los números de SAPS que aparecen en la nota de soporte técnico de SAP [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Tenga también en cuenta [este artículo](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAPS-ratings-on-Azure-VMs-8211-where-to-look-and-where-you-can/ba-p/368208).
      3.  Evalúe y pruebe el tamaño de las máquinas virtuales de Azure en relación con el rendimiento máximo de almacenamiento y el rendimiento de la red de los distintos tipos de máquina virtual que eligió en la fase de planeamiento de la red. Encontrará los datos en:
          1.  [Tamaños de las máquinas virtuales Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Es importante tener en cuenta el **rendimiento máximo de disco sin almacenamiento en caché** para ajustar el tamaño.
          2.  [Tamaños de las máquinas virtuales Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Es importante tener en cuenta el **rendimiento máximo de disco sin almacenamiento en caché** para ajustar el tamaño.
   2. Storage
      1.  Use el [almacenamiento SSD estándar de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) como mínimo para las máquinas virtuales que representan niveles de aplicación de SAP y para la implementación de DBMS no sensible al rendimiento.
      2.  Recomendamos no usar [discos HDD estándar de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) en general.
      2.  Use [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) para todas las máquinas virtuales de DBMS que sean remotamente sensibles al rendimiento.
      2.  Utilice [Discos administrados de Azure](https://azure.microsoft.com/services/managed-disks/).
      3.  Utilice el Acelerador de escritura de Azure para las unidades de registro DBMS con la serie M. Tenga en cuenta el uso y los límites del Acelerador de escritura, tal y como se documenta en [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).
      4.  Para ver los distintos tipos de DBMS, consulte la [documentación genérica de DBMS relacionada con SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) y las documentaciones específicas de DBMS a las que apunta el documento genérico.
      5.  Para SAP HANA, se documentan más detalles en [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
      6.  No monte discos de datos de Azure en una máquina virtual Linux de Azure mediante el identificador de dispositivo. En su lugar, use el identificador único universal (UUID). Tenga cuidado al utilizar herramientas gráficas para montar discos de datos de Azure, por ejemplo. Compruebe las entradas de /etc/fstab para asegurarse de que los discos se montan con el UUID
          1.  [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#connect-to-the-linux-vm-to-mount-the-new-disk)
   3. Redes
      1.  Pruebe y evalúe su infraestructura de red virtual y la distribución de las aplicaciones de SAP a través o dentro de las distintas redes virtuales de Azure.
          1.  Evalúe el enfoque de la arquitectura de red virtual de concentrador y radio o la microsegmentación dentro de una única red virtual Azure según:
              1.  Los costos debidos al intercambio de datos entre [redes virtuales de Azure emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Para saber los costos, consulte los [precios de Virtual Network](https://azure.microsoft.com/pricing/details/virtual-network/).
              2.  La ventaja de desconexión rápida del emparejamiento entre redes virtuales de Azure en comparación con el cambio de NSG para aislar una subred dentro de una red virtual para aquellos casos en los que las aplicaciones o las máquinas virtuales hospedadas en una subred de la red virtual se convirtieron en un riesgo de seguridad.
              3.  Registro y auditoría centralizados del tráfico de red entre el entorno local, el mundo exterior y el centro de datos virtual que creó en Azure.
          2.  Evalúe y pruebe la ruta de acceso de datos entre el nivel de aplicación de SAP y la capa de DBMS de SAP. 
              1.  No se admite en absoluto la ubicación de [aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) en la ruta de comunicación entre la aplicación de SAP y la capa DBMS de un sistema SAP basado en SAP NetWeaver, Hybris o S/4HANA.
              2.  No se admite la colocación del nivel de aplicación de SAP y SAP DBMS en diferentes redes virtuales de Azure que no están emparejadas.
              3.  Se admite el uso de [reglas de NSG y ASG de Azure](https://docs.microsoft.com/azure/virtual-network/security-overview) para definir rutas entre el nivel de aplicación de SAP y la capa de SAP DBMS.
          3.  Asegúrese de que habilita [redes aceleradas de Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) en las máquinas virtuales que se usan en el nivel de aplicación de SAP y la capa de DBMS de SAP. Tenga en cuenta que se necesitan distintos niveles de sistema operativo para admitir redes aceleradas en Azure:
              1.  Windows Server 2012 R2 o versiones más recientes
              2.  SUSE Linux 12 SP3 o versiones más recientes
              3.  RHEL 7.4 o versiones más recientes
              4.  Oracle Linux 7.5. Para utilizar el kernel RHCKL, la versión debe ser 3.10.0-862.13.1.el7. Se requiere el uso del kernel Oracle UEK versión 5.
          4.   Pruebe y evalúe la latencia de red entre las máquinas virtuales de nivel de aplicación de SAP y las máquinas virtuales de DBMS según la nota de soporte técnico de SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) y la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalúe los resultados con las instrucciones de latencia de red de la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). La latencia de red debe estar en el intervalo de moderada y buena. Se aplican excepciones al tráfico entre máquinas virtuales y las unidades de instancias grandes de HANA, tal y como se describe [aquí](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance).
          5.   Asegúrese de que las implementaciones de ILB se configuran para usar Direct Server Return. Este valor reducirá la latencia en los casos en que se usen los ILB de Azure para las configuraciones de alta disponibilidad en la capa de DBMS.
          6.   Si usa Azure Load Balancer junto con sistemas operativos invitados Linux, compruebe que el parámetro de red de Linux **net.ipv4.tcp_timestamps** esté establecido en **0**. Ignore las recomendaciones de versiones anteriores de SAP que indican [#2382421](https://launchpad.support.sap.com/#/notes/2382421). La nota de SAP se ha actualizado de modo que refleje que el parámetro debe establecerse en 0 para que funcione con Azure Load Balancer.
          7.   Considere la posibilidad de usar el [grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location), como se describe en el artículo [Azure Proximity Placement Groups for optimal network latency with SAP applications](sap-proximity-placement-scenarios.md) (Grupos de selección de ubicación de proximidad de Azure para lograr una latencia de red óptima con las aplicaciones SAP) para obtener la latencia de red óptima.
   4. Implementaciones de alta disponibilidad y recuperación ante desastres. 
      1. Si implementa el nivel de aplicación de SAP sin definir una zona de disponibilidad de Azure concreta, asegúrese de que todas las máquinas virtuales que ejecutan la instancia del cuadro de diálogo SAP o instancias middleware de un único sistema SAP se implementan en un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). 
         1.   En caso de que no requiera alta disponibilidad para SAP Central Services y DBMS, estas máquinas virtuales se pueden implementar en el mismo conjunto de disponibilidad que el nivel de aplicación de SAP
      2. Si protege SAP Central Services y la capa DBMS de alta disponibilidad con réplicas pasivas, tiene los dos nodos de SAP Central Services en un único conjunto de disponibilidad independiente y los dos nodos DBMS en otro conjunto de disponibilidad
      3. Si implementa en Azure Availability Zones no puede aprovechar los conjuntos de disponibilidad. Pero tendría asegurarse de que implementa los nodos activos y pasivos de Central Services en dos Availability Zones diferentes, lo que muestra la menor latencia entre zonas.
         1.   Tenga en cuenta que deberá usar [Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) en caso de que establezca clústeres de conmutación por error de Windows o Pacemaker para la capa de SAP Central Services y DBMS en Availability Zones. No se puede usar [Load Balancer básico](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) con implementaciones zonales. 
   5. Configuración del tiempo de expiración
      1. Compruebe los seguimientos de desarrollador de SAP NetWeaver de las distintas instancias de SAP y asegúrese de que no se indican interrupciones de conexión entre el servidor de puesta en cola y los procesos de trabajo SAP. Estas interrupciones de conexión se pueden evitar estableciendo estos parámetros del registro de dos:
         1.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime = 120000: consulte también [este artículo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957549(v=technet.10)).
         2.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveInterval = 120000: consulte también [este artículo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957548(v=technet.10)). 
      2. Con el fin de evitar tiempos de espera de la interfaz gráfica de usuario entre las interfaces GUI de SAP implementadas en un solo entorno local y los niveles de aplicación de SAP implementados en Azure, compruebe si los parámetros siguientes se establecen en default.pfl o el perfil de instancia:
         1.   rdisp/keepalive_timeout = 3600
         2.   rdisp/keepalive = 20
      3. Con el fin de evitar la interrupción de las conexiones establecidas entre el proceso de puesta en cola de SAP y workprocesse de SAP, es necesario establecer el parámetro enque/encni/set_so_keepalive en "true". Consulte también la [nota de SAP 2743751](https://launchpad.support.sap.com/#/notes/2743751).  
      3. Si usa una configuración de clúster de conmutación por error de Windows, asegúrese de que el tiempo para reaccionar en nodos que no responden esté correctamente configurado para Azure. En el artículo de Microsoft sobre el [ajuste de los umbrales de red en clúster de conmutación por error](https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834) se muestran los parámetros cómo afectan a las sensibilidades de la conmutación por error. Suponiendo que los nodos del clúster están en la misma subred, se deben cambiar los parámetros siguientes:
         1.   SameSubNetDelay = 2000
         2.   SameSubNetThreshold = 15
         3.   RoutingHistorylength = 30
4. Pruebe sus procedimientos de Alta disponibilidad y recuperación ante desastres.
   1. Simule situaciones de conmutación por error apagando las máquinas virtuales (sistema operativo invitado de Windows) o poniendo los sistemas operativos en modo de pánico (sistema operativo invitado de Linux) para averiguar si las configuraciones de conmutación por error funcionan como se han diseñado. 
   2. Mida el tiempo que tarda en ejecutarse una conmutación por error. Si tarda demasiado, tenga en cuenta:
      1.   Para SUSE Linux, use dispositivos SBD en lugar del agente de delimitación de Azure para acelerar la conmutación por error.
      2.   Para SAP HANA, si la recarga de datos tarda demasiado, considere la posibilidad de aprovisionar más ancho de banda de almacenamiento.
   3. Pruebe la secuencia y temporización de la copia de seguridad y restauración, y ajústela si es necesario. Asegúrese de que no solo las horas de copia de seguridad son suficientes. Compruebe también la restauración y tome los tiempos de las actividades de restauración. Asegúrese de que los tiempos de restauración están dentro del objetivo de tiempo de recuperación del Acuerdo de Nivel de Servicio en los casos en que este objetivo se basa en una base de datos o un proceso de restauración de máquina virtual
   4. Pruebe la funcionalidad y la arquitectura de la recuperación ante desastres en toda la región.
5. Comprobación de seguridad
   1.  Pruebe la validez de la arquitectura del acceso basado en rol (RBAC) que implementó. El objetivo es separar y limitar el acceso y los permisos de los diferentes equipos. Por ejemplo, los miembros del equipo de SAP Basis deberían poder implementar máquinas virtuales y asignar discos del almacenamiento Azure a una red virtual Azure determinada. Pero el equipo de SAP Basis no debería poder crear sus propias redes virtuales ni modificar las opciones de las redes virtuales existentes. Por otra parte, los miembros del equipo de la red no deberían poder desplegar máquinas virtuales en redes virtuales donde se ejecuten máquinas virtuales de DBMS y aplicaciones de SAP. Los miembros del equipo de red tampoco deberían poder cambiar los atributos de las máquinas virtuales, ni eliminar máquinas virtuales o discos.  
   2.  Compruebe que las reglas de [NSG y ASC](https://docs.microsoft.com/azure/virtual-network/security-overview) funcionan como se espera y blindan los recursos protegidos.
   3.  Asegúrese de que se cifren todos los recursos que deben cifrarse. Defina y ejecute procesos para realizar copias de seguridad de los certificados, almacenarlos y tener acceso a ellos, y restaurar las entidades cifradas. 
   4.  Utilice [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq) y/o discos del sistema operativo siempre que sea posible desde el punto de vista de la compatibilidad con el sistema operativo.
   5.  Compruebe que no se han usado demasiadas capas de cifrado. Tiene poco sentido usar Azure Disk Encryption y luego, encima, uno de los métodos de cifrado de base de datos transparente de DBMS.
6. Pruebas de rendimiento
   1.  En SAP basado en medidas y seguimiento de SAP, compare los 10 informes en línea más importantes con la implementación actual, cuando proceda. 
   2.  En SAP basado en medidas y seguimiento de SAP, compare los 10 trabajos por lotes más importantes con la implementación actual, cuando proceda. 
   3.  En SAP basado en medidas y seguimiento de SAP, compare las transferencias de datos a través de interfaces al sistema SAP. Céntrese en las interfaces en las que sepa que la transferencia se realiza entre distintas ubicaciones, por ejemplo, las que pasan del entorno local a Azure. 


## <a name="non-production-phase"></a>Fase de no producción 
En esta fase se supone que, después de completar un piloto o una prueba de concepto, se empiezan a implementar sistemas SAP de no producción en Azure. Todos los aprendizajes y las experiencias de la prueba de concepto deben adaptarse a esa implementación. Todos los criterios y pasos enumerados en la POC se aplican también en este tipo de implementaciones. En esta fase, normalmente se implementan sistemas de desarrollo, sistemas de pruebas unitarias y sistemas de pruebas de regresión empresarial en Azure. Se recomienda que al menos un sistema de no producción en una línea de aplicación de SAP tenga la configuración de alta disponibilidad completa que va a tener el futuro sistema de producción. Los pasos adicionales que debe tener en cuenta durante la fase son:  

1.  Antes de pasar los sistemas de la plataforma anterior a Azure, recopile datos de consumo de recursos, como el uso de CPU, el rendimiento de almacenamiento y los datos de E/S por segundo. Especialmente en las unidades de capa DBMS, pero también de las unidades del nivel de aplicación. Mida también la latencia de la red y del almacenamiento.
2.  Registre los patrones de tiempo de uso de disponibilidad de los sistemas. El objetivo es averiguar si los sistemas de no producción deben estar disponibles permanentemente o si hay sistemas de no producción que se pueden apagar durante ciertas fases de una semana o un mes.
3.  Pruebe y defina si quiere crear sus propias imágenes de sistema operativo para las máquinas virtuales en Azure o si desea utilizar una imagen de la galería de imágenes de Azure. Si utiliza una imagen de la galería de Azure, asegúrese de obtener la imagen correcta que refleja el contrato de soporte técnico con el proveedor del sistema operativo. Para algunos proveedores de sistemas operativos, las galerías de Azure permiten aportar sus propias imágenes de licencia. Para otras imágenes de sistema operativo, el soporte técnico se incluye en el precio presupuestado por Azure. Si decide crear sus propias imágenes de sistema operativo, encontrará documentación en estos artículos:
    1.  Puede crear una imagen generalizada de una máquina virtual de Windows implementada en Azure según [esta documentación](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).
    2.  Puede crear una imagen generalizada de una máquina virtual de Linux implementada en Azure según [esta documentación](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image).
3.  Si usa imágenes de SUSE o de Red Hat Linux procedentes de la galería de Azure VM, tendrá que usar las imágenes para SAP que proporcionan los proveedores de Linux en esta galería.
4.  Asegúrese de cumplir los requisitos de soporte técnico que SAP tiene con respecto a los contratos de soporte técnico de Microsoft. Encontrará información en la nota de soporte técnico de SAP [#2015553](https://launchpad.support.sap.com/#/notes/2015553). Para instancias grandes de HANA, consulte el documento [Requisitos de incorporación](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-onboarding-requirements).
4.  Asegúrese de que las personas adecuadas reciben [las notificaciones de mantenimiento planeado](https://azure.microsoft.com/blog/a-new-planned-maintenance-experience-for-your-virtual-machines/), con lo que puede elegir el tiempo de inactividad y el reinicio de máquinas virtuales a tiempo.
5.  Compruebe regularmente la documentación de Azure de las presentaciones de Microsoft en canales como [Channel9](https://channel9.msdn.com/) para conocer las nuevas funciones que pueden aplicarse a sus implementaciones.
6.  Compruebe las notas de SAP relacionadas con Azure, como la nota de soporte técnico [#1928533](https://launchpad.support.sap.com/#/notes/1928533) para nuevas SKU de máquina virtual o la versión más reciente del sistema operativo y DBMS. Compare los nuevos tipos de máquinas virtuales con los tipos de máquinas virtuales anteriores en cuanto a precios, de modo que pueda implementar máquinas virtuales con la mejor relación precio/rendimiento.
7.  Valide de nuevo los recursos en las notas de soporte técnico de SAP, el directorio de hardware de SAP HANA y SAP PAM para asegurarse de que no ha habido cambios en las máquinas virtuales compatibles con Azure, las versiones admitidas del sistema operativo en esas máquinas virtuales y las versiones admitidas de SAP DBMS.
8.  Compruebe [aquí](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) si hay nuevas SKU certificadas por HANA en Azure y compare los precios con los planeados y, en última instancia, cambie para obtener las unidades con mejor relación precio/rendimiento. 
9.  Adapte los scripts de implementación para aprovechar los nuevos tipos de máquina virtual e incorporar nuevas características de Azure que quiere usar.
10. Tras la implementación de la infraestructura, pruebe y evalúe la latencia de red entre las máquinas virtuales de nivel de aplicación de SAP y las máquinas virtuales de DBMS según la nota de soporte técnico de SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) y la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalúe los resultados con las instrucciones de latencia de red de la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). La latencia de red debe estar en el intervalo de moderada y buena. Se aplican excepciones al tráfico entre máquinas virtuales y las unidades de instancias grandes de HANA, tal y como se describe [aquí](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance). Asegúrese de que ninguna de las restricciones mencionadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#azure-network-considerations) y [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) se aplican a la implementación.
11. Asegúrese de que las máquinas virtuales se implementan mediante el [grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) correcto, como se describe en el artículo [Azure Proximity Placement Groups for optimal network latency with SAP applications](sap-proximity-placement-scenarios.md) (Grupos de selección de ubicación de proximidad de Azure para lograr una latencia de red óptima con las aplicaciones SAP).
11. Antes de aplicar la carga de trabajo, realice las demás comprobaciones, tal y como se indica en la fase de prueba de concepto.
12. A medida que se aplica la carga de trabajo, registre el consumo de recursos de dichos sistemas en Azure y compare con los registros que obtuvo de la plataforma anterior. Si observa grandes diferencias, ajuste tamaño de máquina virtual de las futuras implementaciones. Tenga en cuenta que en el caso de reducción de tamaño, también se reducirán el almacenamiento y los anchos de banda de red de una máquina virtual:
    1.  [Tamaños de las máquinas virtuales Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). 
    2.  [Tamaños de las máquinas virtuales Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) 
13. Trabaje en la funcionalidad y los procesos de copia del sistema. El objetivo es facilitarle la copia de un sistema de desarrollo o un sistema de pruebas, para que los equipos de proyecto puedan obtener rápidamente nuevos sistemas. Considere [SAP LaMa](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+Landscape+Management+%28SAP+LaMa%29+at+a+Glance) como una herramienta que realiza estas tareas.
14. Optimice y perfeccione los accesos basados en rol de Azure, los permisos y los procesos de su equipo para asegurarse de tener una separación de funciones por una parte. Por otra parte, quiere tener habilitados todos los equipos para realizar sus tareas en la infraestructura de Azure.
15. Practique, pruebe y documente los procedimientos de alta disponibilidad y recuperación de desastres para permitir que su personal ejecute tales tareas. Identifique las deficiencias y adapte la nueva funcionalidad de Azure que va a integrar en las implementaciones.

 
## <a name="production-preparation-phase"></a>Fase de preparación de producción 
En esta fase quiere recopilar todas las experiencias y enseñanzas de las implementaciones que no son de producción y aplicarlas en futuras implementaciones de producción. Además de las fases anteriores, también debe preparar el trabajo de la transferencia de datos entre la ubicación actual de hospedaje y Azure. 

1.  Trabaje con las actualizaciones de versiones de SAP necesarias de los sistemas de producción antes de pasar a Azure.
2.  Póngase de acuerdo con los propietarios de empresas sobre las pruebas funcionales y empresariales que deben realizarse después de la migración del sistema de producción.
    1.  Asegúrese de que todas estas pruebas se ejecutan con los sistemas de origen en la ubicación de hospedaje actual. Conviene evitar que se lleven a cabo pruebas por primera vez una vez trasladado el sistema a Azure.
2.  Pruebe el proceso de migración de producción a Azure. En caso de no trasladar todos los sistemas de producción a Azure en el mismo período de tiempo, cree grupos de sistemas de producción que deben estar en la misma ubicación de hospedaje. Practique y pruebe la migración de datos. Lista de métodos comunes tales como:
    1.  Utilice métodos de DBMS, como la copia de seguridad y restauración en combinación con SQL Server AlwaysOn, la replicación del sistema de HANA o el trasvase de registros para inicializar y sincronizar el contenido de la base de datos en Azure.
    2.  Use la copia de seguridad y restauración para las bases de datos más pequeñas.
    3.  Utilice SAP Migration Monitor implementado en la herramienta SAP SWPM para realizar migraciones heterogéneas.
    4.  Use el proceso [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) si tiene que realizar una combinación con una actualización de la versión SAP. Tenga en cuenta que no todas las combinaciones entre el DBMS de origen y de destino son compatibles. Encontrará más información en las notas de soporte técnico de SAP específicas para las diferentes versiones de DMO. Por ejemplo, [Database Migration Option (DMO) de SUM 2.0 SP04](https://launchpad.support.sap.com/#/notes/2644872)
    5.  Compruebe si la transferencia de datos por Internet o a través de ExpressRoute es mejor en cuanto al rendimiento en caso de que tenga que mover copias de seguridad o archivos de exportación de SAP. Si los datos se mueven por Internet, es posible que tenga que cambiar algunas de las reglas de seguridad de NSG/ASG que necesite aplicar en futuros sistemas de producción.
3.  Antes de pasar los sistemas de la plataforma anterior a Azure, recopile datos de consumo de recursos, como el uso de CPU, el rendimiento de almacenamiento y los datos de E/S por segundo. Especialmente en las unidades de capa DBMS, pero también de las unidades del nivel de aplicación. Mida también la latencia de la red y del almacenamiento.
4.  Valide de nuevo los recursos en las notas de soporte técnico de SAP, el directorio de hardware de SAP HANA y SAP PAM para asegurarse de que no ha habido cambios en las máquinas virtuales compatibles con Azure, las versiones admitidas del sistema operativo en esas máquinas virtuales y las versiones admitidas de SAP DBMS. 
4.  Adapte los scripts de implementación a los cambios más recientes que haya decidido sobre los tipos de máquina virtual y la funcionalidad de Azure.
5.  Después de la implementación de la infraestructura y la aplicación, pase por una serie de comprobaciones con el fin de validar lo siguiente:
    1.  Se implementaron los tipos correctos de máquina virtual con los atributos correctos y los tamaños de almacenamiento.
    2.  Compruebe que las máquinas virtuales se encuentran en las versiones del SO y las revisiones correctas que desea, y son uniformes.
    3.  Compruebe que el sistema de protección de las máquinas virtuales sea el necesario y uniforme.
    4.  Compruebe que se han instalado e implementado las versiones y revisiones correctas de la aplicación.
    5.  Las máquinas virtuales se implementaron en conjuntos de disponibilidad de Azure según lo previsto.
    6.  Se usó Azure Premium Storage para los discos sensibles a la latencia o cuando se requiere el [contrato de nivel de servicio del 99,9 % para una única máquina virtual](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).
    7.  Compruebe la correcta implementación del Acelerador de escritura de Azure.
        1.  Asegúrese de que, en la máquina virtual, se crearon espacios de almacenamiento o conjuntos de franjas correctamente en todos los discos que necesitan compatibilidad con el Acelerador de escritura de Azure.
            1.  Consulte [Configuración del software RAID en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
            2.  Consulte [Configuración del LVM en una máquina virtual Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)
    8.  Se usaron [Discos administrados de Azure](https://azure.microsoft.com/services/managed-disks/) exclusivamente.
    9.  Las máquinas virtuales se implementaron en las Availability Zones y los conjuntos de disponibilidad correctos.
    10. Asegúrese de que se habilitan [redes aceleradas de Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) en las máquinas virtuales que se usan en el nivel de aplicación de SAP y la capa de DBMS de SAP.
    11. No se admite la ubicación de [aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) en la ruta de comunicación entre la aplicación de SAP y la capa DBMS de un sistema SAP basado en SAP NetWeaver, Hybris o S/4HANA.
    12. Las reglas de NSG y de ASG permiten la comunicación deseada y planeada y bloquean la comunicación cuando es necesario.
    13. La configuración del tiempo de expiración se ha establecido correctamente, tal y como se describió anteriormente.
    14. Asegúrese de que las máquinas virtuales se implementan mediante el [grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) correcto, como se describe en el artículo [Azure Proximity Placement Groups for optimal network latency with SAP applications](sap-proximity-placement-scenarios.md) (Grupos de selección de ubicación de proximidad de Azure para lograr una latencia de red óptima con las aplicaciones SAP).
    15. Pruebe y evalúe la latencia de red entre las máquinas virtuales de nivel de aplicación de SAP y las máquinas virtuales de DBMS según la nota de soporte técnico de SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) y la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalúe los resultados con las instrucciones de latencia de red de la nota de soporte técnico de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). La latencia de red debe estar en el intervalo de moderada y buena. Se aplican excepciones al tráfico entre máquinas virtuales y las unidades de instancias grandes de HANA, tal y como se describe [aquí](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance).
    15. Compruebe si el cifrado se implementó cuando fue necesario y con el método de cifrado necesario.
    16. Compruebe si las interfaces y otras aplicaciones pueden conectarse la infraestructura recién implementada.
6.  Cree un cuaderno de estrategias para reaccionar ante el mantenimiento planeado de Azure. Defina el orden de los sistemas y las máquinas virtuales que se van a reiniciar en caso de mantenimiento planeado.
    

## <a name="go-live-phase"></a>Fase de puesta en marcha
Durante la fase de puesta en marcha, debe asegurarse de seguir los cuadernos de estrategias que desarrollo en las fases anteriores. Ejecute los pasos que probó y entrenó. No se aceptan cambios de última hora en el proceso y las configuraciones. Además de eso, se aplican las siguientes medidas:

1. Compruebe que la supervisión de Azure Portal y otras herramientas de supervisión funcionan.  Las herramientas recomendadas son Perfmon (Windows) o SAR (Linux): 
    1.  Contadores de CPU 
        1.  Tiempo promedio de CPU: total (todas las CPU)
        2.  Tiempo promedio de CPU: cada procesador individual (hasta 128 procesadores en la máquina virtual m128)
        3.  Kernel de tiempo de CPU: cada procesador individual
        4.  Usuario de tiempo de CPU: cada procesador individual
    5.  Memoria 
        1.  Memoria libre
        2.  Paginas de memoria de entrada/s
        3.  Paginas de memoria de salida/s
    4.  Disco 
        1.  Kb de lectura de disco/s: por disco individual 
        2.  Lectura de disco/s: por disco individual
        3.  Ms de lectura de disco/lectura: por disco individual
        4.  Kb de escritura de disco/s: por disco individual 
        5.  Escritura de disco/s: por disco individual
        6.  Ms de escritura de disco/lectura: por disco individual
    5.  Red 
        1.  Entrada de paquetes de red/s
        2.  Salida de paquetes de red/s
        3.  Entrada de kb de red/s
        4.  Salida de kb de red/s 
2.  Después de la migración de los datos, realice todas las pruebas de validación acordadas con los propietarios de empresas. Solo se aceptan los resultados de pruebas de validación en los que tenga resultados para los sistemas de origen originales.
3.  Compruebe si las interfaces funcionan y si otras aplicaciones pueden comunicarse con los sistemas de producción recién implementados.
4.  Compruebe el sistema de corrección y transporte mediante STMS de transacciones de SAP.
5.  Realice copias de seguridad de la base de datos una vez que el sistema está listo para producción.
6.  Realice copias de seguridad de la máquina virtual para el nivel de aplicación de SAP una vez que el sistema está listo para producción.
7.  Para los sistemas SAP que no formaban parte de la fase de puesta en marcha actual, pero que se comunican con los sistemas SAP que trasladó a Azure en esta fase de puesta en marcha, debe restablecer el búfer de nombre de host en SM51. Este paso eliminará las anteriores direcciones IP almacenadas en caché y asociadas a los nombres de las instancias de la aplicación que trasladó a Azure.  


## <a name="post-production"></a>Postproducción
En esta fase, se trata de la supervisión, el manejo y la administración del sistema. Desde un punto de vista de SAP, se aplican las tareas habituales que era necesario realizar con su anterior ubicación de hospedaje. Las tareas específicas de Azure que quiere realizar son:

1. Análisis de las facturas de Azure para los sistemas de carga alta
2. Optimización de la eficiencia precio/rendimiento en la máquina virtual y en el almacenamiento
3. Optimización del tiempo que los sistemas pueden estar apagados  


## <a name="next-steps"></a>Pasos siguientes
Consulte la documentación:

- [Implementación y planeamiento de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Implementación de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

