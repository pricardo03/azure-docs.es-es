---
title: Migración de SAP HANA en Azure (instancias grandes) a máquinas virtuales de Azure | Microsoft Docs
description: Procedimiento para migrar SAP HANA en Azure (instancias grandes) a máquinas virtuales de Azure
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/11/2020
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2dd01bf60104939fcf1f9bd1ccec0e7d72710041
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77154787"
---
# <a name="sap-hana-on-azure-large-instance-migration-to-azure-virtual-machines"></a>Migración de SAP HANA en instancias grandes de Azure a máquinas virtuales de Azure
En este artículo se describen posibles escenarios de implementación de instancias grandes de Azure, y se ofrece un enfoque de planeación y migración con un tiempo de inactividad de transición minimizado

## <a name="overview"></a>Información general
Desde el anuncio de las instancias grandes de Azure para SAP HANA (HLI) en septiembre de 2016, muchos clientes han adoptado este hardware como servicio para su plataforma de proceso en memoria.  En los últimos años, la extensión de la escalabilidad de las VM de Azure, combinada con la compatibilidad del modelo de implementación de escalado horizontal de HANA, ha superado la demanda de capacidad de base de datos ERP de la mayoría de los clientes empresariales.  Empezamos a ver que los clientes expresan el deseo de migrar su carga de trabajo de SAP HANA de servidores físicos a VM de Azure.
Esta guía no es un documento de configuración paso a paso, sino que describe los modelos de implementación comunes, y ofrece consejos de planeación y migración con la intención de destacar las consideraciones necesarias para la preparación a fin de minimizar el tiempo de inactividad de transición.

## <a name="assumptions"></a>Supuestos
En este artículo se da por supuesto lo siguiente:
- El único interés que se tiene en cuenta es una migración del servicio de proceso de base de datos de HANA homogénea de la Instancia grande de HANA (HLI) a VM de Azure sin necesidad de aplicar revisiones o actualizaciones de software significativas. Estas actualizaciones secundarias incluyen el uso de una versión de sistema operativo más reciente o una versión de HANA que se indica explícitamente como compatible con las notas de SAP pertinentes. 
- Si se desea, es necesario realizar todas las actividades de actualización o actualización antes o después de la migración.  Por ejemplo, implementación de conversión de MCOS a MDC de SAP HANA. 
- El enfoque de migración que ofrecería el menor tiempo de inactividad es la replicación del sistema de SAP HANA. Otros métodos de migración no forman parte del ámbito de este documento.
- Esto se aplica tanto a las SKU de Rev3 como de Rev4 de HLI.
- En su mayor parte, la arquitectura de implementación de HANA permanece inalterada durante la migración.  Es decir, un sistema con una sola instancia de DR seguirá siendo igual en el destino.
- Los clientes han revisado y comprendido el contrato de nivel de servicio (SLA) de la arquitectura de destino. 
- Debido a las diferencias en los términos comerciales entre las HLI y las VM, los clientes deben supervisar el uso de sus VM para la administración de costos.
- Los clientes entienden y reconocen que HLI es una plataforma de proceso dedicada. Sin embargo, las VM se ejecutan en una infraestructura compartida, que es segura y está aislada de otros inquilinos.
- Los clientes han validado que las VM de destino admiten la arquitectura deseada. Para ver todas las SKU de VM admitidas certificadas para la implementación de SAP HANA, compruebe el [directorio de hardware de SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).
- Los clientes han validado el diseño y el plan de migración.
- Planear el nodo de recuperación ante desastres junto con el sitio primario.  Los clientes no podrán usar el nodo DR de HLI para el sitio primario que se ejecuta en VM después de la migración.
- Los clientes copiaron los archivos de copia de seguridad necesarios en las VM de destino, en función de los requisitos de cumplimiento y capacidad de recuperación de la empresa. Esto con el fin de cubrir la necesidad de recuperación a un momento dado durante el período de transición.
- En el caso de alta disponibilidad de HSR, los clientes deben instalar y configurar el dispositivo STONITH según las guías de alta disponibilidad de SAP HANA para [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) y [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker).  No está preconfigurada como en el caso de HLI.
- Este enfoque de migración no cubre las SKU de HLI con la configuración de Optane.

## <a name="deployment-scenarios"></a>Escenarios de implementación
En la tabla siguiente se resumen los modelos de implementación comunes con clientes de HLI.  Es posible la migración a VM de Azure para todos los escenarios de HLI.  Algunos escenarios pueden requerir una pequeña modificación arquitectónica para obtener las ventajas de las ofertas de servicio de Azure actuales.

| Id. de escenario | Escenario de HLI | ¿Migración textual a VM? | Comentario |
| --- | --- | --- | --- |
| 1 | [Nodo único con un SID](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-one-sid) | Sí | - |
| 2 | [Nodo único con MCOS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-mcos) | Sí | - |
| 3 | [Nodo único con DR mediante replicación de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-storage-replication) | No | La replicación de almacenamiento no está disponible con la plataforma virtual de Azure, cambie la solución actual de DR a HSR o copia de seguridad/restauración. |
| 4 | [Nodo único con DR (multipropósito) mediante replicación de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication) | No | La replicación de almacenamiento no está disponible con la plataforma virtual de Azure, cambie la solución actual de DR a HSR o copia de seguridad/restauración. |
| 5 | [HSR con STONITH para alta disponibilidad](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#hsr-with-stonith-for-high-availability) | Sí | Sin ningún SBD preconfigurado para las VM de destino.  Seleccione e implemente una solución de STONITH.  Opciones posibles: Agente de barreras de Azure (compatible con [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker), [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)), SBD |
| 6 | [Alta disponibilidad con HSR, DR con replicación de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-with-hsr-and-dr-with-storage-replication) | No | Reemplace la replicación de almacenamiento para las necesidades de DR con HSR o copia de seguridad/restauración. |
| 7 | [Conmutación por error automática de host (1+1)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#host-auto-failover-11) | Sí | Use ANF el almacenamiento compartido con VM de Azure. |
| 8 | [Escalabilidad horizontal con espera](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-standby) | Sí | BW/4HANA con VM M128s, M416s, M416ms con ANF para almacenamiento únicamente. |
| 9 | [Escalabilidad horizontal sin espera](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-without-standby) | Sí | BW/4HANA con VM M128s, M416s, M416ms (con o sin usar ANF para almacenamiento). |
| 10 | [Escalabilidad horizontal con DR mediante replicación de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-storage-replication) | No | Reemplace la replicación de almacenamiento para las necesidades de DR con HSR o copia de seguridad/restauración. |
| 11 | [Nodo único con DR mediante HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-hsr) | Sí | - |
| 12 | [HSR de nodo único a DR (optimizado para costo)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-hsr-to-dr-cost-optimized) | Sí | - |
| 13 | [Alta disponibilidad y DR con HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr) | Sí | - |
| 14 | [Alta disponibilidad y DR con HSR (optimizado para costo)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr-cost-optimized) | Sí | - |
| 15 | [Escalabilidad horizontal con DR mediante HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-hsr) | Sí | BW/4HANA con VM M128s, M416s, M416ms (con o sin usar ANF para almacenamiento). |


## <a name="source-hli-planning"></a>Planeación de origen (HLI)
Al incorporar un servidor de HLI, tanto la administración de servicios de Microsoft como los clientes han pasado por la planeación de la configuración específica de proceso, red, almacenamiento y sistema operativo para ejecutar la base de datos de SAP HANA.  Debe realizarse una planeación similar para la migración a VM de Azure.

### <a name="sap-hana-housekeeping"></a>Mantenimiento de SAP HANA 
Antes de migrar la base de datos de HANA, es aconsejable ordenar el contenido de la base de datos para que los datos no deseados, los datos obsoletos o los registros obsoletos no se migren a la nueva base de datos.  Por lo general, el mantenimiento implica la eliminación o el archivado de datos antiguos, caducados o inactivos.  De aquí que estas acciones de "higiene de datos" deban probarse en sistemas que no sean de producción para garantizar la validez de los recortes de datos antes del uso en producción.

### <a name="allow-network-connectivity-for-new-vms-and-or-virtual-network"></a>Permitir la conectividad de red para nuevas VM o la red virtual 
En la implementación de HLI de un cliente, se ha establecido la conectividad de red de las redes virtuales de Azure en función de la información que se describe en el artículo [Arquitectura de red de SAP HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture). Además, el enrutamiento del tráfico de red se realiza de la manera descrita en la sección "Enrutamiento en Azure".
1. Con el fin de configurar una nueva VM como destino de HSR para la migración, si la nueva VM de Azure se coloca en la red virtual existente con intervalos de direcciones IP, que ya tienen permiso para conectarse al nodo de HLI, no se requiere ninguna actualización adicional de la conectividad de HLI.
2. Si la nueva VM de Azure se coloca en una nueva red virtual (puede estar en otra región) y se empareja con la red virtual existente, la clave de servicio de ExpressRoute y el identificador de recurso del aprovisionamiento de HLI original se pueden usar para permitir el acceso a este nuevo intervalo de direcciones IP de la red virtual.  Coordine con la administración de servicios de Microsoft para habilitar la conectividad de la red virtual a HLI.  Nota: Para minimizar la latencia de red entre las capas de aplicación y base de datos, ambas deben estar en la misma red virtual.  

### <a name="existing-app-layer-availability-set-availability-zones-and-proximity-placement-group"></a>Conjunto de disponibilidad de la capa de aplicación existente, Availability Zones y grupos de selección de ubicación de proximidad
El modelo actual de implementación está hecho para cumplir determinados objetivos de nivel de servicio.  En este movimiento, asegúrese de que la infraestructura de destino cumpla o supere los objetivos establecidos.  
Es más probable que los servidores de aplicaciones SAP de los clientes se coloquen en un conjunto de disponibilidad.  Si el nivel de servicio actual de implementación es satisfactorio y, además: 
- Si el reemplazo del servidor de SAP HANA se realiza a través de la resolución de nombres mediante el intercambio de las direcciones IP para el nombre de host de la base de datos, no es necesario hacer nada más.  
- Si no usa PPG, asegúrese de colocar todos los servidores de aplicación y de base de datos en la misma zona para reducir la latencia de red.
- Si usa PPG, consulte la sección Planeación de destino: Conjuntos de disponibilidad, Availability Zones y grupos de selección de ubicación de proximidad (PPG) de este documento.

### <a name="storage-replication-discontinuance-process-if-used"></a>Proceso de interrupción de la replicación de almacenamiento (si se usa)
Si se usa la replicación de almacenamiento como solución de recuperación ante desastres, la replicación deberá terminarse (sin programar) una vez que la aplicación de SAP se haya cerrado, y el último catálogo de SAP HANA, el archivo de registro, las copias de seguridad de los datos se hayan replicado en los volúmenes de almacenamiento remoto.  Esta acción es una táctica de precaución donde tenemos una base de datos actual en HLI de recuperación ante desastres en caso de que se produzca un desastre durante la transición de la VM física a Azure.

### <a name="data-backups-preservation-consideration"></a>Consideración de preservación de copias de seguridad de datos
Después de la migración total a SAP HANA en VM de Azure, NO se podrá tener acceso fácilmente a ninguno de los datos basados en instantáneas o las copias de seguridad de registros en HLI, ni se podrán restaurar a una VM de ser necesario. Para el período de transición temprano, antes de que la copia de seguridad basada en Azure cree un historial suficiente para satisfacer los requisitos de recuperación a un momento dado, se recomienda realizar copias de seguridad de nivel de archivo, además de instantáneas en HLI, días o semanas antes de la migración completa.  Haga que estas copias de seguridad se copien en una cuenta de Azure Storage a la que tenga acceso la nueva VM de SAP HANA. Además de realizar una copia de seguridad del contenido de HLI, también es un procedimiento prudente hacer que se pueda acceder fácilmente a las copias de seguridad completas de la entorno de SAP en caso de que se necesite una reversión.

### <a name="adjusting-system-monitoring"></a>Ajuste de la supervisión de sistemas 
Los clientes usan muchas herramientas diferentes para supervisar y enviar notificaciones de alerta a los sistemas dentro de su entorno de SAP.  Este elemento es simplemente un simple llamado a la acción adecuada al agregar las nuevas VM que incorporan cualquier ajuste para supervisar y actualizar los destinatarios de las notificaciones de alerta, de ser necesario.

### <a name="microsoft-operations-team-involvement"></a>Implicación del equipo de Microsoft Operations 
Abra un vale de las bases de Azure Portal en la instancia de HLI existente.  Una vez que se cree la incidencia de soporte técnico, un ingeniero de soporte técnico se pondrá en contacto con usted por correo electrónico.  

### <a name="engage-microsoft-account-team"></a>Participación del equipo de cuentas de Microsoft
Planee la migración cerca de la fecha de aniversario de renovación del contrato de HLI, con el fin de minimizar los sobrecostos innecesarios en recursos de proceso. Para retirar la hoja de HLI, es necesario coordinar la finalización del contrato y el apagado real de la unidad.

## <a name="destination-planning"></a>Planeación de destino
La creación de una nueva infraestructura para sustituir otra existente merece cierta atención para asegurar que la nueva incorporación quepa en el esquema general.  A continuación, encontrará algunos puntos clave en los que pensar.

### <a name="resource-availability-in-the-target-region"></a>Disponibilidad de recursos en la región de destino 
Normalmente, la región de implementación actual del servidor de aplicaciones de SAP está en proximidad con las instancias de HLI asociadas.  Sin embargo, las instancias de HLI se ofrecen en menos ubicaciones que las regiones de Azure disponibles.  En la migración de la máquina física de HLI a la VM de Azure, se presenta la oportunidad de "afinar" la distancia de proximidad de todos los servicios relacionados para optimizar el rendimiento.  Al hacerlo, una consideración clave sería asegurarse de que la región elegida tenga los recursos de interés.  Por ejemplo, la disponibilidad de ciertas familias de VM o la oferta de zonas de Azure para tener en cuenta la alta disponibilidad.

### <a name="virtual-network"></a>Virtual network 
El arquitecto de la infraestructura deberá elegir si desea ejecutar la nueva base de datos de HANA dentro de la red virtual de la aplicación de SAP existente o crear una nueva.  El factor de decisión principal es el diseño de red actual de conexión en red para el entorno de SAP.  En caso de que se tenga en cuenta un cambio en la infraestructura de implementación, por ejemplo, al pasar de una implementación de una zona a dos zonas y aprovechar las ventajas de PPG. Esta mejora de la disponibilidad impone un cambio de arquitectura. Para obtener más información, consulte el artículo [Grupos de selección de ubicación de proximidad de Azure para una latencia de red óptima con aplicaciones SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios).   

### <a name="security"></a>Seguridad
Ya sea que la nueva VM de SAP HANA aterrice en una red virtual o subred nueva o existente, representa un nuevo servicio crítico para la empresa, que requiere protección.  Debe evaluarse e implementarse el control de acceso adecuado en cumplimiento de la directiva de seguridad de información de la compañía para esta nueva clase de servicio. 

### <a name="vm-sizing-recommendation"></a>Recomendación de ajuste de tamaño de la VM
Esta migración también es una oportunidad para ajustar el tamaño del motor de proceso de HANA.  Puede aprovechar las [vistas del sistema](https://help.sap.com/viewer/7c78579ce9b14a669c1f3295b0d8ca16/Cloud/3859e48180bb4cf8a207e15cf25a7e57.html) de HANA junto con HANA Studio para comprender el consumo de recursos del sistema, lo que permite dar con el tamaño adecuado para impulsar la eficacia de los gastos.

### <a name="storage"></a>Storage 
El rendimiento del almacenamiento es uno de los factores que afectan a la experiencia de usuario de la aplicación de SAP.  En función de una SKU de VM determinada, existen recomendaciones de diseño de almacenamiento mínimo publicadas en [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage). Se recomienda revisar estas especificaciones mínimas y compararlas con las estadísticas del sistema de HLI existentes para garantizar la capacidad de E/S y el rendimiento adecuados para la nueva VM de HANA.

Si configura PPG para la nueva VM de HANA y sus servidores asociados, envíe una incidencia de soporte técnico para inspeccionar y garantizar la colocación del almacenamiento y la VM de HANA.  
Dado que es posible que su solución de copia de seguridad tenga que cambiar, el costo de almacenamiento también debe revisarse para evitar sorpresas en los gastos operativos. 

### <a name="storage-replication-for-disaster-recovery"></a>Replicación de almacenamiento para recuperación ante desastres
En HLI, la replicación de almacenamiento se ofrecía como la opción predeterminada para la replicación de recuperación ante desastres para la base de datos de HANA. Esta característica no es la opción predeterminada con VM de Azure. Tenga en cuenta HSR, copia de seguridad/restauración u otras soluciones compatibles que satisfagan sus necesidades empresariales.

### <a name="availability-sets-availability-zones-and-proximity-placement-groups"></a>Conjuntos de disponibilidad, Availability Zones y grupos de selección de ubicación de proximidad 
A los efectos de minimizar la distancia entre la capa de aplicación y SAP HANA para mantener la latencia de red en un mínimo, la nueva VM de base de datos y los servidores de aplicaciones de SAP actuales deben colocarse en un grupo de selección de ubicación de proximidad (PPG). Consulte [Grupos de selección de ubicación de proximidad](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) para aprender cómo funcionan los conjuntos de disponibilidad y Azure Availability Zones con PPG para implementaciones de SAP.
Si los miembros del sistema HANA de destino se implementan en más de una zona de Azure, los clientes deben tener una visión clara del perfil de latencia de las zonas elegidas. La colocación de los componentes del sistema SAP es óptima con respecto a la distancia proximal entre la aplicación de SAP y la base de datos.  El dominio público [herramienta de prueba de latencia de zona de disponibilidad](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/master/AvZone-Latency-Test) ayuda a facilitar la medición.  


### <a name="backup-strategy"></a>Estrategia de copia de seguridad
Muchos clientes ya usan soluciones de copia de seguridad de terceros para SAP HANA en HLI.  En ese caso, solo deben configurarse una VM protegida adicional y bases de datos de HANA.  En adelante, los trabajos de copia de seguridad de HLI en curso se desprograman si el equipo se está dando de baja después de la migración.
La copia de seguridad de Azure para SAP HANA en VM ya está disponible con carácter general.  Para obtener información detallada, consulte estos vínculos: [Copia de seguridad](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database), [restauración](https://docs.microsoft.com/azure/backup/sap-hana-db-restore), [administración](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) de copias de seguridad de SAP HANA en VM de Azure.

### <a name="dr-strategy"></a>Estrategia de recuperación ante desastres
Si los objetivos de nivel de servicio admiten un tiempo de recuperación más largo de una copia de seguridad simple en el almacenamiento de blobs y la restauración en contexto o en una nueva VM, esta es la estrategia de recuperación ante desastres más sencilla y menos costosa.  
Al igual que la plataforma de instancias grandes en la que la recuperación ante desastres de HANA se realiza normalmente con HSR; en VM de Azure, HSR es también la solución de recuperación ante desastres más natural y nativa de SAP HANA.  Independientemente de si la implementación de origen es una instancia única, en clúster con o sin conmutación automática por error, o HANA de escalado horizontal de varios nodos, se requiere una réplica de HSR de destino de la infraestructura de origen en la región de recuperación ante desastres. Esta réplica de recuperación ante desastres de destino de HSR se establecerá después de que se complete la migración principal de HLI a VM.  La instancia de recuperación ante desastres de HANA se registrará en la instancia principal de SAP HANA en VM como sitio de replicación secundario.  

### <a name="sap-application-server-connectivity-destination-change"></a>Cambio de destino de conectividad del servidor de aplicaciones de SAP
El enfoque de migración de HSR produce un nuevo host de base de datos de HANA y, por lo tanto, un nuevo nombre de host de base de nombres para la capa de aplicación, y los perfiles de SAP deben modificarse para reflejar el nuevo nombre de host.  Si el cambio se realiza mediante la resolución de nombres que conserva el nombre de host, no se requiere ningún cambio de perfil.

### <a name="operating-system"></a>Sistema operativo
Las imágenes de sistema operativo para HLI y VM, aunque estén en el mismo nivel de versión, por ejemplo, SLES 11 SP4, no son idénticas. Los clientes deben validar los paquetes necesarios, las correcciones, las revisiones, el kernel y las correcciones de seguridad en HLI, de modo que se puedan instalar en el sistema operativo de la VM de destino.  Además, es habitual que los clientes configuren una versión más reciente del sistema operativo en la VM de destino para SAP HSR.  Esto se admite según la[nota de SAP 2763388](https://launchpad.support.sap.com/#/notes/2763388).

### <a name="new-sap-license-request"></a>Nueva solicitud de licencia de SAP
Una simple llamada para solicitar una nueva licencia de SAP para el nuevo sistema de HANA ahora que se ha migrado a VM.

### <a name="service-level-agreement-sla-differences"></a>Diferencias en los contratos de nivel de servicio (SLA)
Los autores quieren destacar la diferencia de SLA de disponibilidad entre HLI y VM de Azure.  Por ejemplo, los pares de alta disponibilidad en clúster de HLI ofrecen una disponibilidad del 99,99 %. Para lograr el mismo SLA, hay que implementar VM en zonas de disponibilidad. En este [artículo](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/) se describe la disponibilidad con arquitecturas de implementación asociadas para que los clientes puedan planear la infraestructura de destino en consecuencia.


## <a name="migration-strategy"></a>Estrategia de migración
En este documento, solo se aborda el enfoque de replicación del sistema de HANA para la migración de HLI a VM de Azure.  Dependiendo de la solución de almacenamiento de destino implementada, el proceso difiere ligeramente. A continuación se describen los pasos generales.

### <a name="vm-with-premiumultra-disks-for-data"></a>VM con discos Premium o Ultra para datos
En el caso de las VM que se implementan con discos Premium o Ultra, se aplica la configuración estándar de replicación del sistema de SAP HANA y se puede usar para configurar HSR.  El [artículo de ayuda de SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/099caa1959ce4b3fa1144562fa09e163.html) de referencia proporciona información general sobre los pasos necesarios para configurar la replicación del sistema entre dos sistemas, la toma de control de un sistema secundario, la conmutación por recuperación a un sistema principal y la deshabilitación de la replicación del sistema.  A los efectos de la migración, solo se necesitará la instalación, la toma de control y la deshabilitación de la replicación del sistema en los pasos del origen de HLI.  

### <a name="vm-with-anf-for-data-and-log-volumes"></a>VM con ANF para volúmenes de datos y de registro
En un nivel alto, las instantáneas más recientes de almacenamiento de HLI de los volúmenes de datos y de registro completos deben copiarse en Azure Storage, donde la VM de HANA de destino puede acceder a ellos y recuperarlos.  El proceso de copia se puede realizar con las herramientas de copia nativas de Linux.  

>[!Important]
> La copia y la transferencia de datos pueden tardar horas en función del tamaño de la base de datos de HANA y del ancho de banda de red.  La mayor parte del proceso de copia debe realizarse antes del tiempo de inactividad principal de la base de datos de HANA.

### <a name="mcos-to-mdc-conversion"></a>Conversión de MCOS a MDC
Algunos de nuestros clientes de HLI eligieron el modelo de implementación de Multiple Components in One System (MCOS).  La motivación era eludir la limitación de instantáneas de almacenamiento de Multiple Databases Container (MDC) de las versiones anteriores de SAP HANA.  En el modelo MCOS, varias instancias de SAP HANA independientes se apilan en una hoja de HLI.  El uso de HSR para la migración funcionaría bien, lo que daría lugar a varias VM de HANA con una base de trabajo de inquilino cada una.  Este estado final hace posible un panorama más complejo del aquel al que un cliente podría haber estado acostumbrado.  Con SAP HANA 2.0, la implementación predeterminada es MDC, una alternativa viable es realizar un [movimiento de inquilinos de HANA](https://launchpad.support.sap.com/#/notes/2472478) después de la migración de HSR.  Este proceso "consolida" estas bases de datos de HANA independientes en los coinquilinos en un solo contenedor de HANA.  

### <a name="application-layer-consideration"></a>Consideración de la capa de aplicación
El servidor de base de datos se ve como el centro de un sistema SAP.  Todos los servidores de aplicaciones se deben ubicar cerca de la base de datos de SAP HANA.  En algunos casos, cuando se desea un nuevo uso de PPG, puede ser necesaria la reubicación de los servidores de aplicaciones existentes en el PPG donde se encuentra la VM de HANA.  Es posible que la creación de nuevos servidores de aplicaciones sea más fácil si ya tiene plantillas de implementación útiles.  
Si los servidores de aplicaciones existentes y la nueva VM de HANA se encuentran en una ubicación óptima, no es necesario compilar nuevos servidores de aplicaciones, a menos que se desee capacidad adicional.  
Si se crea una nueva infraestructura para mejorar la disponibilidad del servicio, es posible que los servidores de aplicaciones existentes sean innecesarios y deban apagarse y eliminarse.
Si el nombre de host de la VM de destino ha cambiado y difiere del nombre de host de HLI, los perfiles de los servidores de aplicaciones de SAP deben ajustarse para que apunten al nuevo host.  Si solo ha cambiado la dirección IP de la base de datos de HANA, se necesita una actualización de registros DNS para dirigir las conexiones entrantes a la nueva VM de HANA.

### <a name="acceptance-test"></a>Prueba de aceptación
Aunque la migración de HLI a VM no realiza ningún cambio sustancial en el contenido de la base de datos en comparación con una migración heterogénea, todavía se recomienda validar las funcionalidades clave y el aspecto de rendimiento de la nueva configuración.  

### <a name="cutover-plan"></a>Plan de migración total
Si bien esta migración es sencilla, implica el retiro de una base de datos existente.  La planeación cuidadosa para conservar el sistema de origen con el contenido asociado y las imágenes de copia de seguridad es fundamental en caso de que sea necesaria la reserva.  Una buena planeación ofrece una reversión más rápida.   


## <a name="post-migration"></a>Después de la migración
El trabajo de migración no se realiza hasta que no se hayan desacoplado con seguridad los servicios o la conectividad dependientes de HLI para asegurar que se preserve la integridad de los datos.  Además, apague los servicios innecesarios.  En esta sección se describen algunos elementos que deben tenerse presentes.

### <a name="decommissioning-the-hli"></a>Retiro de HLI
Después de una migración correcta de la base de datos de HANA a VM de Azure, no se debe realizar ninguna transacción de negocio de producción en la base de datos de HLI.  Sin embargo, mantener la instancia de HLI activa y en ejecución durante un período igual a su ventana de retención de datos de copia de seguridad local es una práctica segura que garantiza una recuperación de datos más rápida si es necesario.  Solo entonces se debe retirar la hoja de HLI.  Además del procedimiento técnico, los clientes, por su propio interés, deben concluir contractualmente los compromisos de HLI con Microsoft.  Los clientes deben ponerse en contacto con sus representantes de Microsoft para trabajar en el proceso de retiro de HLI.

### <a name="remove-any-proxy-ex-iptables-bigip-configured-for-hli"></a>Eliminación de un proxy (por ejemplo: Iptables, BIGIP) configurado para HLI 
Si se usa un servicio de proxy como el de un IPTables para enrutar el tráfico local hacia y desde HLI, ese servicio ya no es necesario después de la migración correcta a VM.  Sin embargo, este servicio de conectividad debe conservarse mientras la hoja de HLI todavía esté en espera, como se describe en el tema de retiro.  Este servicio se puede apagar después de que se haya retirado por completo la hoja de HLI. 

### <a name="remove-global-reach-for-hli"></a>Eliminación de Global Reach para HLI 
Global Reach se usa normalmente para conectar la puerta de enlace de ExpressRoute del cliente con la puerta de enlace de ExpressRoute de HLI, lo que permite que el tráfico local del cliente llegue directamente al inquilino de HLI sin necesidad de un servicio de proxy.  Al igual que sucede con el servicio de proxy de IPTables, GlobalReach debe conservarse hasta que se haya retirado por completo la hoja de HLI.

### <a name="operating-system-subscription--movereuse"></a>Suscripción al sistema operativo: movimiento y reutilización
A medida que los servidores de VM se ponen en marcha y se retiran las hojas de HLI, las suscripciones de sistema operativo se pueden reemplazar o reutilizar para evitar el doble pago de licencias del sistema operativo.



## <a name="next-steps"></a>Pasos siguientes
Consulte estos artículos:
- [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
- [Lista de comprobación de planeamiento e implementación de cargas de trabajo de SAP en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist).
