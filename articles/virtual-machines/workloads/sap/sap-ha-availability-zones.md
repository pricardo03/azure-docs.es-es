---
title: Configuraciones de cargas de trabajo de SAP con Azure Availability Zones | Microsoft Docs
description: Escenarios y arquitectura de alta disponibilidad para SAP NetWeaver con Azure Availability Zones
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 409a304296d3fdff897a203177e2c150162755c6
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746224"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configuraciones de cargas de trabajo de SAP con Azure Availability Zones

Una de las funcionalidades de alta disponibilidad que Azure ofrece para mejorar la disponibilidad general de la carga de trabajo de SAP en Azure es [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). Availability Zones ya está disponible en diferentes [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Se agregarán más regiones. 

La arquitectura básica de alta disponibilidad de SAP se puede describir como se muestra en este gráfico:

![Configuración de alta disponibilidad estándar](./media/sap-ha-availability-zones/standard-ha-config.png)

El nivel de aplicación de SAP se implementa en un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) de Azure. Para lograr una alta disponibilidad de los servicios centrales de SAP, debe implementar dos máquinas virtuales en un conjunto de disponibilidad independiente y usar servicios de clúster de conmutación por error de Windows o Pacemaker (Linux) para implementar un marco de alta disponibilidad que permita la conmutación automática por error en caso de que exista algún problema con la infraestructura o el software. Documentación que detalla dicha lista de implementaciones, como:

- [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un recurso compartido de archivos](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server para aplicaciones de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver en Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Se aplica una arquitectura similar a la capa de DBMS de SAP NetWeaver, S/4HANA o sistemas Hybris. Implemente la capa de DBMS de un modo activo/pasivo con una solución de clúster de conmutación por error, que usa marcos de trabajo de conmutación por error específicos de DBMS, servicios de clúster de conmutación por error de Windows o Pacemaker para iniciar las actividades de conmutación por error en caso de errores de la infraestructura o el software. 

Para implementar la misma arquitectura con Azure Availability Zones, deben realizarse algunos cambios en la arquitectura descrita. Estos cambios se describen en las distintas partes de este documento.

## <a name="considerations-deploying-across-availability-zones"></a>Consideraciones de implementación entre Availability Zones

Hay algunas cosas a tener en cuenta a la hora de usar Availability Zones. Cosas como:

- Azure Availability Zones no proporciona ninguna garantía de una determinada distancia entre las diferentes zonas de una región de Azure.
- Azure Availability Zones no es una solución ideal de recuperación ante desastres. En una época en la que grandes catástrofes naturales generan daños generalizados en algunas regiones del mundo, incluidos daños graves en la infraestructura eléctrica, puede que la distancia entre las diferentes zonas no sea lo suficientemente larga para calificar esta solución como idónea para una recuperación ante desastres.
- La latencia de red entre diferentes instancias de Azure Availability Zones en distintas regiones de Azure varía de una región a otra. Hay casos en los que puede ejecutar el nivel de aplicación de SAP implementado en diferentes zonas porque la latencia de red de una zona a la máquina virtual de DBMS activa es aceptable desde el punto de vista del impacto del proceso empresarial. Sin embargo, hay escenarios en los que la latencia entre la máquina virtual del DBMS activa de una zona y una instancia de la aplicación de SAP de otra zona puede ser demasiado intrusiva y no aceptable para los procesos empresariales de SAP. Como resultado, la arquitectura de implementación debe ser diferente con una arquitectura en modo activo/activo para la aplicación o activo/pasivo si la latencia entre zonas es demasiado alta.
- Decida en qué configuración podría usar Azure Availability Zones, según la latencia de red que se vaya a medir entre las diferentes zonas. La latencia de red desempeña un papel importante en dos áreas distintas:
    - Latencia entre las dos instancias de DBMS que deben tener una replicación sincrónica establecida. Cuanto mayor sea la latencia de red, mayor será el potencial de escalabilidad de impacto de la carga de trabajo.
    - La diferencia de latencia de red entre una máquina virtual que ejecuta una instancia de diálogo de SAP en la misma zona que la instancia de DBMS activa y una máquina virtual similar en otra zona. Cuanto mayor sea esta diferencia, mayor será el impacto en el tiempo de ejecución de procesos empresariales y trabajos por lotes, en función de si se ejecutan en la misma zona con DBMS o en una zona distinta.


A partir del uso de características de Azure, existen algunas restricciones de funcionalidad que pueden usarse al implementar máquinas virtuales de Azure en diferentes zonas y al establecer soluciones de conmutación por error en distintas instancias de Availability Zones dentro de la misma región de Azure. A continuación, se presenta la lista de tales restricciones:

- El uso de [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) es obligatorio para la implementación en Azure Availability Zones. 
- Se ha corregido la asignación de las enumeraciones de zona para las zonas físicas en cada suscripción de Azure. Si utiliza suscripciones diferentes para implementar los sistemas SAP, debe definir las zonas ideales para cada suscripción por separado.
- No se pueden implementar conjuntos de disponibilidad de Azure en una zona de disponibilidad de Azure. Elija una zona de disponibilidad o un conjunto de disponibilidad de Azure como marco de implementación para máquinas virtuales.
- No puede usar [Azure Load Balancer Básico](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) para crear soluciones de clúster de conmutación por error basadas en los servicios de clúster de conmutación por error de Windows o en Linux Pacemaker. En su lugar, deberá usar el [SKU Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-zone-combination"></a>Combinación ideal de zonas
Para decidir cómo aprovechar Availability Zones, debe realizar un análisis que le proporcione:

- La latencia de red entre las tres distintas zonas de una región de Azure. Por lo tanto, puede elegir las zonas que tienen la latencia de red mínima en el tráfico de red entre zonas.
- La diferencia entre la latencia de máquina virtual a máquina virtual dentro de una de las zonas elegidas y la latencia de red entre las dos zonas seleccionadas.
- Una declaración que indique si los tipos de máquinas virtuales que deben estar implementadas en las zonas de disponibilidad de su elección están disponibles en las dos zonas seleccionadas. Especialmente con las máquinas virtuales de la serie M, va a encontrarse con situaciones donde las diferentes SKU de máquina virtual de la serie M estarán disponibles solo en dos de las tres zonas.

### <a name="network-latency-between-zones-and-within-zone"></a>Latencia de red entre zonas y dentro de una zona
Para saber cuál es la latencia entre las distintas zonas, debe hacer lo siguiente:

- Implemente la SKU de la máquina virtual que desea utilizar para la instancia de DBMS en las tres zonas. Asegúrese de que [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) está habilitado al realizar esta medición.
- Tan pronto encuentre las dos zonas con la mínima latencia de red, implemente otras tres máquinas virtuales de la SKU de máquina virtual que desea usar como máquina virtual de nivel de aplicación en las tres zonas de disponibilidad. Mida la latencia de red de las dos "máquinas virtuales de DBMS" en las dos zonas de "DBMS" distintas que elija. 
- Use **niping** como herramienta de medición. Una herramienta de SAP, que funciona como se describe en las notas de soporte técnico de SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) y [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Céntrese en los comandos de SAP documentados para medir la latencia. **ping** no es una herramienta recomendada, ya que **ping** no funciona en las rutas de acceso a código de la red acelerada de Azure.

En función de las medidas y la disponibilidad de las SKU de máquina virtual de las distintas zonas de disponibilidad, debe tomar las siguientes decisiones:

- Defina las zonas ideales para la capa de DBMS.
- Responda a la pregunta de si, en función de las diferencias de latencia de red dentro de una zona o entre zonas, podría distribuir el nivel de aplicación de SAP activo a una, dos o las tres zonas diferentes.
- Responda a la pregunta de si desea implementar una configuración en modo activo/pasivo o activo/activo desde el punto de vista de una aplicación (consulte los detalles más adelante).

Para tomar estas decisiones, remítase también a las recomendaciones sobre latencia de red de SAP documentadas en la nota de SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Tenga en cuenta lo siguiente:

> [!IMPORTANT]
> Las medidas y decisiones tomadas son válidas para la suscripción de Azure utilizada para realizar estas medidas. Para usar otra suscripción de Azure, debe repetir las medidas, ya que la asignación de la enumeración de zonas dependiente de la suscripción puede variar con una suscripción distinta.


> [!IMPORTANT]
> Se espera que las medidas realizadas anteriormente muestren resultados distintos en cada región de Azure que admita [zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Aunque los requisitos de latencia de red no cambien, puede que deba adaptar diferentes estrategias de implementación en distintas regiones de Azure, ya que la latencia de red entre zonas puede variar. Se espera que, en algunas regiones de Azure, la latencia de red entre las tres zonas diferentes pueda variar significativamente. Sin embargo, en otras regiones, la latencia de red entre las tres zonas diferentes es más uniforme. La afirmación de que **siempre** hay una latencia de red entre zonas de 1 milisegundo a 2 milisegundos es **incorrecta**. La latencia de red entre las zonas de disponibilidad de las regiones de Azure no se puede generalizar.


## <a name="activeactive-deployment"></a>Implementación activa/activa
La arquitectura de implementación se denomina activa/activa porque se implementan las instancias de diálogo de SAP activas en dos o tres zonas. Los servicios centrales de SAP que usan la replicación de puesta en cola se van a implementar entre dos zonas. Lo mismo sucede con la capa de DBMS, que se va a implementar entre las mismas zonas que el servicio central de SAP.

Para contemplar dicha configuración, debe encontrar las dos zonas de disponibilidad de su región que ofrezcan una latencia de red entre zonas que sea aceptable para la carga de trabajo y la replicación de DBMS sincrónica. Además, desea que la diferencia entre la latencia de red dentro de las zonas seleccionadas y la latencia de red entre zonas no sea demasiado grande. Esto último se debe a que no desea que haya variaciones demasiado grandes en los tiempos de ejecución de los procesos empresariales o los procesos por lotes que dependen del hecho de que un trabajo se ejecute dentro de una zona con el servidor de DBMS o entre distintas zonas. Algunas variaciones son aceptables, pero no los factores de diferencia.

Un esquema simplificado de una implementación activa/activa entre dos zonas podría ser similar a este:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Las siguientes consideraciones se aplican a esta configuración:

- Debe tratar las zonas de disponibilidad de Azure como dominios de error y actualización para todas las máquinas virtuales, ya que los conjuntos de disponibilidad no se pueden implementar en zonas de disponibilidad de Azure.
- Los equilibradores de carga de Azure utilizados para los clústeres de conmutación por error de los servicios centrales de SAP y la capa de DBMS deben ser [equilibradores de carga de SKU estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). El equilibrador de carga básico no funcionará entre zonas.
- La red virtual de Azure y sus subredes implementadas para hospedar el sistema SAP se extienden entre zonas. **No necesita** separar las redes virtuales para cada zona.
- Para todas las máquinas virtuales implementadas, use [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Los discos no administrados no se admiten para implementaciones zonales.
- Azure Premium Storage o el [almacenamiento SSD Ultra](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) no admiten ningún tipo de replicación de almacenamiento entre zonas. Como resultado, es responsabilidad de la aplicación (DBMS o servicios centrales de SAP) replicar datos importantes.
- Ocurre lo mismo con el directorio compartido sapmnt, que es un disco compartido (Windows), un recurso compartido CIFS (Windows) o un recurso compartido NFS (Linux). Debe utilizar una tecnología que replique dicho disco compartido o recurso compartido entre las zonas. En este momento se admiten las siguientes tecnologías:
    - Para Windows, se admite entre zonas una solución de clúster que usa SIOS Datakeeper como se documenta en [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Para SUSE Linux, se admite un recurso compartido NFS como se documenta en [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    - En este momento, la solución con el uso de los servicios de archivos de escalabilidad horizontal (SOFS) documentada en [Preparación de la infraestructura de Azure para la alta disponibilidad de SAP con un clúster de conmutación por error de Windows y el recurso compartido de archivos para instancias de SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **no se admite entre zonas**.
- La tercera zona se usa para hospedar el dispositivo SBD en caso de que compile un [clúster de Pacemaker en SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) o instancias de aplicación adicionales.
- Para lograr la coherencia del tiempo de ejecución para algunos trabajos o transacciones empresariales críticos, puede intentar dirigir determinados trabajos por lotes y usuarios directamente a instancias de aplicación específicas que se encuentran en la zona con la instancia de DBMS activa con el uso de grupos de servidor por lotes de SAP, grupos de inicio de sesión o grupos de RFC. Sin embargo, en el caso de una conmutación por error zonal, debe mover manualmente estos grupos a instancias de diálogo, que se encuentran en las demás zonas. 
- Decida si desea implementar algunas instancias de diálogo inactivas en cada una de las zonas para poder obtener inmediatamente la capacidad de recursos anterior en caso de que una zona implementada como parte de las instancias de aplicación esté fuera de servicio.


## <a name="activepassive-deployment"></a>Implementación activa/pasiva
En caso de que no observe una diferencia aceptable entre la latencia de red dentro de una zona y el tráfico de red entre zonas, la arquitectura que puede implementar tiene un carácter activo/pasivo desde el punto de vista del nivel de aplicación de SAP. Defina una zona "activa", que es la zona en la que implementa el nivel de aplicación completo y donde intenta ejecutar la instancia de DBMS activa y la instancia de servicios centrales de SAP activa. Con dicha configuración, se asegura de que no haya una diferencia muy elevada del tiempo de ejecución en las transacciones empresariales y los trabajos por lotes, en función de si un trabajo se ejecuta en la misma zona con la instancia de DBMS activa o no.

El diseño básico de este tipo de arquitectura tiene el siguiente aspecto:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Las siguientes consideraciones se aplican a esta configuración:

- Los conjuntos de disponibilidad no se pueden implementar en Azure Availability Zones. Por tanto, en este caso, finalmente acabará con un dominio de actualización y error para su nivel de aplicación. La razón es que solo se implementa en una zona. Esta configuración es un ligero retroceso en comparación con la arquitectura de referencia que prevé que se implemente el nivel de aplicación en un conjunto de disponibilidad de Azure.
- Al poner en funcionamiento esta arquitectura, debe realizar una estrecha supervisión y tratar de mantener las instancias de servicios centrales de SAP y DBMS activas en la misma zona en la que implementó el nivel de aplicación. En el caso de una conmutación por error de la instancia de DBMS o del servicio central de SAP, desea asegurarse de que puede realizar una conmutación por recuperación manual en la zona con el nivel de aplicación de SAP implementado lo antes posible.
- Los equilibradores de carga de Azure utilizados para los clústeres de conmutación por error de los servicios centrales de SAP y la capa de DBMS deben ser [equilibradores de carga de SKU estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). El equilibrador de carga básico no funcionará entre zonas.
- La red virtual de Azure y sus subredes implementadas para hospedar el sistema SAP se extienden entre zonas. **No necesita** separar las redes virtuales para cada zona.
- Para todas las máquinas virtuales implementadas, debe usar [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Los discos no administrados no se admiten para implementaciones zonales.
- Azure Premium Storage o el [almacenamiento SSD Ultra](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) no admiten ningún tipo de replicación de almacenamiento entre zonas. Como resultado, es responsabilidad de la aplicación (DBMS o servicios centrales de SAP) replicar datos importantes.
- Ocurre lo mismo con el directorio compartido sapmnt, que es un disco compartido (Windows), un recurso compartido CIFS (Windows) o un recurso compartido NFS (Linux). Debe utilizar una tecnología que replique dicho disco compartido o recurso compartido entre las zonas. En este momento se admiten las siguientes tecnologías:
    - Para Windows, se admite entre zonas una solución de clúster que usa SIOS Datakeeper como se documenta en [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Para SUSE Linux, se admite un recurso compartido NFS como se documenta en [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    - En este momento, la solución con el uso de los servicios de archivos de escalabilidad horizontal (SOFS) documentada en [Preparación de la infraestructura de Azure para la alta disponibilidad de SAP con un clúster de conmutación por error de Windows y el recurso compartido de archivos para instancias de SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **no se admite entre zonas**.
- La tercera zona se usa para hospedar el dispositivo SBD en caso de que compile un [clúster de Pacemaker en SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) o instancias de aplicación adicionales.
- Implemente máquinas virtuales inactivas en la zona pasiva (desde un punto de vista de DBMS) para poder iniciar los recursos de aplicación en caso de error de una zona.
    - No puede usar [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) para replicar máquinas virtuales activas en máquinas virtuales inactivas entre zonas. En este momento, Azure Site Recovery no puede satisfacer esa función.
- Invierta en una automatización que le permita, en caso de error de una zona, iniciar automáticamente el nivel de aplicación de SAP en la segunda zona.

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Configuración combinada de alta disponibilidad y recuperación ante desastres
A pesar de que Microsoft no ofrece ninguna información sobre la distancia geográfica entre las instalaciones que hospedan diferentes zonas de disponibilidad de Azure en una región de Azure determinada, algunos clientes aprovechan las zonas para una configuración combinada de alta disponibilidad y recuperación ante desastres que promete un **o**bjetivo de **p**unto de **r**ecuperación (RPO) de cero. Esto significa que no debe perder ninguna transacción de base de datos confirmada incluso en el caso de una recuperación ante desastres. 

> [!NOTE]
> Una configuración como esta se recomienda solo para circunstancias específicas. Por ejemplo, casos en los que los datos no puede dejar la región de Azure por motivos de seguridad y cumplimiento. 

En este gráfico se muestra un ejemplo de cuál podría ser el aspecto de esta configuración:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

Las siguientes consideraciones se aplican a esta configuración:

- Se supone que hay una distancia significativa entre las instalaciones que hospedan una zona de disponibilidad. O bien está obligado a quedarse en una región de Azure determinada. Los conjuntos de disponibilidad no se pueden implementar en Azure Availability Zones. Por tanto, en este caso, finalmente acabará con un dominio de actualización y error para su nivel de aplicación. La razón es que solo se implementa en una zona. Es un ligero retroceso en comparación con la arquitectura de referencia que prevé que se implemente el nivel de aplicación en un conjunto de disponibilidad de Azure.
- Al poner en funcionamiento esta arquitectura, debe realizar una estrecha supervisión y tratar de mantener las instancias de servicios centrales de SAP y DBMS activas en la misma zona en la que implementó el nivel de aplicación. En el caso de una conmutación por error de la instancia de DBMS o del servicio central de SAP, desea asegurarse de que puede realizar una conmutación por recuperación manual en la zona con el nivel de aplicación de SAP implementado lo antes posible.
- Tiene instancias de aplicación de producción preinstaladas en las máquinas virtuales que ejecutan las instancias de aplicación activas de control de calidad.
- En caso de error zonal, apaga las instancias de aplicación de control de calidad e inicia las instancias de producción en su lugar. Tenga en cuenta que necesita trabajar con los nombres virtuales de las instancias de aplicación para hacer esta tarea.
- Los equilibradores de carga de Azure utilizados para los clústeres de conmutación por error de los servicios centrales de SAP y la capa de DBMS deben ser [equilibradores de carga de SKU estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). El equilibrador de carga básico no funcionará entre zonas.
- La red virtual de Azure y sus subredes implementadas para hospedar el sistema SAP se extienden entre zonas. **No necesita** separar las redes virtuales para cada zona.
- Para todas las máquinas virtuales implementadas, debe usar [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Los discos no administrados no se admiten para implementaciones zonales.
- Azure Premium Storage o el [almacenamiento SSD Ultra](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) no admiten ningún tipo de replicación de almacenamiento entre zonas. Como resultado, es responsabilidad de la aplicación (DBMS o servicios centrales de SAP) replicar datos importantes.
- Ocurre lo mismo con el directorio compartido sapmnt, que es un disco compartido (Windows), un recurso compartido CIFS (Windows) o un recurso compartido NFS (Linux). Debe utilizar una tecnología que replique dicho disco compartido o recurso compartido entre las zonas. En este momento se admiten las siguientes tecnologías:
    - Para Windows, se admite entre zonas una solución de clúster que usa SIOS Datakeeper como se documenta en [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Para SUSE Linux, se admite un recurso compartido NFS como se documenta en [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    - En este momento, la solución con el uso de los servicios de archivos de escalabilidad horizontal (SOFS) documentada en [Preparación de la infraestructura de Azure para la alta disponibilidad de SAP con un clúster de conmutación por error de Windows y el recurso compartido de archivos para instancias de SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **no se admite entre zonas**.
- La tercera zona se usa para hospedar el dispositivo SBD en caso de que compile un [clúster de Pacemaker en SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) o instancias de aplicación adicionales.





## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes pasos para realizar implementaciones en las zonas de disponibilidad de Azure:

- [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Preparación de la infraestructura de Azure para la alta disponibilidad de SAP con un clúster de conmutación por error de Windows y el recurso compartido de archivos para instancias de SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






