---
title: Configuración de HADR de Db2 de IBM en Azure virtual machines (VM) | Microsoft Docs
description: Establecer la alta disponibilidad de IBM Db2 LUW en Azure virtual machines (VM).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/10/2019
ms.author: juergent
ms.openlocfilehash: 3c1d0e252b5c658ab6da2b3932918f05ba651d52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835980"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[1612105]:https://launchpad.support.sap.com/#/notes/1612105

[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm
[sles-pacemaker]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker
[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[nfs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs
[sles-ha-guide]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/
[ascs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-suse-linux-enterprise-server-with-pacemaker"></a>Alta disponibilidad de IBM Db2 LUW en máquinas virtuales de Azure en SUSE Linux Enterprise Server con Pacemaker

IBM Db2 LUW (Linux, Unix y Windows) en [configuración de HADR](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) constan de un nodo que ejecuta una instancia de base de datos principal y al menos un nodo que ejecuta una instancia de base de datos secundaria. Los cambios realizados en la instancia de base de datos principal se replican a la instancia de base de datos secundaria sincrónica o asincrónicamente, depende de la configuración. 

En este artículo se describe cómo implementar y configurar las máquinas virtuales, instalar la plataforma del clúster e instalar y configurar LUW de IBM Db2 en la configuración de HADR. El artículo no explica cómo instalar y configurar LUW de IBM Db2 en la instalación de software HADR o SAP. Se proporcionan referencias a los manuales de instalación de SAP y IBM para lograr estas tareas. El foco está en elementos que son específicos para el entorno de Azure. 

Versiones de IBM Db2 admitidas son 10.5 y posterior, tal como se describe en la nota SAP #[1928533].

Lea las siguientes notas SAP y documentación antes de acerca a una instalación:

| Nota de SAP | DESCRIPCIÓN |
| --- | --- |
| [1928533] | SAP Applications on Azure: Supported Products and Azure VM Types (Aplicaciones de SAP en Azure: productos y tipos de máquina virtual de Azure compatibles) |
| [2015553] | SAP on Microsoft Azure: Support Prerequisites (Nota de soporte técnico 2015553 de SAP en Microsoft Azure: requisitos previos de soporte técnico) |
| [2178632] | Key Monitoring Metrics for SAP on Microsoft Azure (Métricas de supervisión clave para SAP en Microsoft Azure) |
| [2191498] | SAP on Linux with Azure: Enhanced Monitoring (Virtualización en Windows: supervisión mejorada) |
| [2243692] | Linux on Microsoft Azure (IaaS) VM: SAP license issues (Linux y máquinas virtuales de Microsoft Azure (IaaS): problemas de licencia de SAP) |
| [1984787] |SUSE LINUX Enterprise Server 12: Notas de instalación |
| [1999351] |Troubleshooting Enhanced Azure Monitoring for SAP (Solución de problemas de la supervisión mejorada de Azure para SAP) |
| [2233094] |DB6: Aplicaciones de SAP en Azure con IBM Db2 para Linux, UNIX y Windows - información adicional |
| [1612105] |DB6: Preguntas más frecuentes sobre la recuperación de desastres de alta disponibilidad (HADR) de Db2 |


| Documentación | 
| --- |
| [WIKI de la Comunidad SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tiene todas las notas de SAP necesaria para Linux |
| [Máquinas virtuales planeación e implementación de SAP en Linux de Azure] [ planning-guide] guía |
| [Implementación de máquinas virtuales de Azure para SAP en Linux] [ deployment-guide] (este artículo) |
| [Implementación de DBMS de máquinas virtuales de Azure para SAP en Linux] [ dbms-guide] guía |
| [Carga de trabajo SAP en la lista de comprobación de planeación e implementación de Azure][azr-sap-plancheck] |
| [Guías de procedimientos recomendados de SUSE Linux Enterprise Server para SAP Applications 12 SP3][sles-for-sap-bp] |
| [Alta disponibilidad SUSE Linux Enterprise extensión 12 SP3][sles-ha-guide] |
| [Implementación de DBMS de máquinas virtuales de Azure de IBM Db2 para cargas de trabajo SAP][dbms-db2] |
| [IBM Db2 alta disponibilidad con recuperación de desastres 11.1][db2-hadr-11.1] |
| [IBM Db2 alta disponibilidad con recuperación de desastres 10.5 de R][db2-hadr-10.5] |

## <a name="overview"></a>Información general
Para lograr una alta disponibilidad, IBM Db2 LUW con HADR está instalado en al menos dos máquinas virtuales de Azure, que se implementan en un [conjunto de disponibilidad de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) o en [zonas de disponibilidad de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). Los gráficos siguientes muestran un programa de instalación del servidor de base de datos de dos máquinas virtuales de Azure. Ambas máquinas virtuales de Azure del servidor de base de datos tiene su propio almacenamiento conectado y estén en funcionamiento. En HADR, una instancia de base de datos en una de las máquinas virtuales de Azure tiene el rol de la instancia principal. Todos los clientes se conectan a la instancia principal. Todos los cambios en las transacciones de base de datos se almacenan localmente en el registro de transacciones de Db2. Como las entradas del registro de transacciones son persistentes localmente, los registros se transfieren a través de TCP/IP a la instancia de base de datos en el segundo servidor de base de datos, el servidor en espera o instancia en espera. La instancia en espera, actualiza la base de datos local puesta al día la transacción transferida entradas del registro. Por lo que el servidor en espera se mantiene sincronizado con el servidor principal.

HADR es sólo una funcionalidad de replicación. Tiene ninguna detección de errores y sin instalaciones de la adquisición o la conmutación por error automática. Una adquisición o transferencia en el modo de espera se debe iniciar manualmente un administrador de base de datos. Para lograr una adquisición automática y la detección de errores, puede usar la característica de clústeres de Linux Pacemaker. Pacemaker supervisa las servidores o instancias de dos bases de datos. Cuando se bloquea la base de datos principal/instancia del servidor, inicia Pacemaker una **automática** la adquisición de HADR por el servidor en espera y también garantiza que la dirección IP virtual se asigna al nuevo servidor principal.

![Introducción a IBM Db2 alta disponibilidad](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

Para SAP servidores de aplicaciones para conectarse a la base de datos principal necesitan un nombre de host virtual y una dirección IP virtual. En caso de una conmutación por error, los servidores de aplicaciones de SAP se conectarán a la nueva instancia de base de datos principal. En un entorno de Azure, un [Azure Load Balancer](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) es necesario utilizar una dirección IP virtual de la forma necesaria para HADR de IBM Db2. 

Para comprender, cómo LUW de IBM Db2 con HADR y Pacemaker se adapta a una configuración de sistema de alta disponibilidad SAP, la imagen siguiente presenta una visión general de un programa de instalación de alta disponibilidad de un sistema SAP en función de la base de datos IBM Db2. En este artículo se trata solo de IBM Db2 y referencias a otros artículos de cómo configurar otros componentes del sistema SAP.

![Introducción al entorno de IBM DB2 alta disponibilidad completa](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-steps-needed"></a>Descripción general de los pasos necesarios
Estos pasos para implementar una configuración de IBM Db2, deben abordarse:

  + Planificar el entorno
  + Implementar máquinas virtuales
  + Actualización de SUSE Linux y configurar sistemas de archivos
  + Instalación y configuración de Pacemaker
  + Instalar [NFS de alta disponibilidad][nfs-ha]
  + Instalar [ASCS/ERS en un clúster independiente][ascs-ha] 
  + Instalar la base de datos de IBM Db2 con la opción Distributed y alta disponibilidad (SWPM)
  + Instalar o crear el nodo de base de datos secundaria y la instancia y configurar HADR
  + Confirmar el funcionamiento de HADR
  + Configuración de Pacemaker se aplican al control de IBM Db2
  + Configuración de Azure Load Balancer 
  + Instalar principal + diálogo servidores de aplicaciones
  + Comprobación y adapte la configuración de servidores de aplicaciones SAP
  + Realizar la conmutación por error la adquisición de pruebas



## <a name="planning-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>Planear la infraestructura de Azure para hospedar LUW de IBM Db2 con HADR

Vaya a través de la planificación antes de ejecutar la implementación. Está creando la base para implementar una configuración de Db2 con HADR en Azure. Elementos clave que deben formar parte del planeamiento de IMB Db2 LUW (parte de la base de datos del entorno de SAP).

| Tema | Descripción breve |
| --- | --- |
| Definir grupos de recursos de Azure | Grupos de recursos donde implementar máquina virtual, red virtual, Azure Load Balancer y otros recursos. Puede ser nuevo o existente |
| Red virtual o definición de la subred | Donde las máquinas virtuales para IBM Db2 y el equilibrador de carga de Azure están obteniendo para implementarse. Puede ser existente o recién creado |
| Máquinas virtuales que hospedan LUW de IBM Db2 | Tamaño de máquina virtual, almacenamiento, redes, la dirección IP |
| Nombre de host virtual y dirección IP virtual de base de datos IBM Db2| IP/nombre de host virtual que se usa para la conexión de servidores de aplicaciones SAP. **db-virt-hostname**, **db-virt-ip** |
| Barrera de Azure | Barrera de Azure o vallado de SBD (altamente recomendado). Se impide el método para evitar situaciones de cerebro dividido |
| SBD VM | Tamaño de máquina Virtual de SBD, almacenamiento, red |
| Azure Load Balancer | Uso de Basic o Standard (recomendado), el sondeo de puerto para la base de datos de Db2 (nuestra recomendación 62500) **puerto de sondeo** |
| Resolución de nombres| Cómo funciona la resolución de nombre en el entorno. Se recomienda el servicio DNS. Se puede usar el archivo hosts local |
    
Encontrará más detalles sobre el uso de Linux Pacemaker en Azure en estos artículos:

- [Configuración de Pacemaker en SUSE Linux Enterprise Server en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)



## <a name="deployment-on-suse-linux"></a>Implementación en SUSE Linux

El agente de recursos para IBM Db2 LUW se incluye en SUSE Linux Enterprise Server para aplicaciones de SAP. Para la configuración descrita en este documento, es obligatorio usar el servidor Linux de SUSE para aplicaciones de SAP. Azure Marketplace contiene una imagen de SUSE Enterprise Server para SAP Applications 12 que puede usar para implementar nuevas máquinas virtuales de Azure. Tenga en cuenta los modelos de servicio de asistencia para diferentes ofrecidos SUSE a través de las galerías de Azure al elegir la imagen de máquina virtual en la Galería de máquina virtual de Azure. 

### <a name="hosts---dns-updates"></a>Hosts - las actualizaciones de DNS
Realice una lista de todos los nombres de host como nombres de host virtual y actualizar los servidores DNS para habilitar la dirección IP correcta para la resolución de nombre de host. En el caso, un servidor DNS no existe o no es capaz de actualizar y crear entradas DNS, deberá aprovechar los archivos hosts locales de las máquinas virtuales individuales que forman parte de este escenario. En el caso de uso de las entradas de archivos de host, deberá asegurarse de que las entradas se aplican a todas las máquinas virtuales en el entorno del sistema SAP. Sin embargo, recomendación es usar el DNS que lo ideal es que se extienden a Azure


### <a name="manual-deployment"></a>Implementación manual

Asegúrese de que el sistema operativo seleccionado sea compatible con SAP de IBM para IBM Db2 LUW. La lista de versiones de SO admitidas para máquinas virtuales de Azure y las versiones de Db2 está disponible en la nota de SAP [1928533]. La lista de las versiones de SO por cada versión de Db2 está disponible en la matriz de disponibilidad de productos de SAP. Se recomienda un mínimo de SLES 12 SP3 debido a las mejoras de rendimiento relacionados con Azure en este o versiones posteriores versiones de SUSE Linux.

1. Crear o seleccionar un grupo de recursos
2. Crear o seleccionar una red virtual y subred
3. Crear conjunto de disponibilidad de Azure o implementar en la zona de disponibilidad
    + Disponibilidad establecer; establézcalo dos dominios de actualización máximo
4. Crear la máquina Virtual 1.
    + Usar SLES para imágenes SAP en la Galería de Azure
    + Seleccione Azure conjunto de disponibilidad en creada en el paso 3, o seleccione zona de disponibilidad
5.  Crear la máquina Virtual 2.
    + Usar SLES para imágenes SAP en la Galería de Azure
    + Seleccione Azure conjunto de disponibilidad en creado en el paso 3. o seleccione la zona de disponibilidad: no es la misma zona como en el paso 3.
6. Agregar discos de datos a las máquinas virtuales: Compruebe la recomendación del programa de instalación de sistema de archivos en el artículo [implementación de DBMS de máquinas virtuales de Azure de IBM Db2 para cargas de trabajo SAP][dbms-db2]

## <a name="create-the-pacemaker-cluster"></a>Crear el clúster de Pacemaker
    
Siga los pasos de [configuración de Pacemaker en SUSE Linux Enterprise Server en Azure][sles-pacemaker] para crear un clúster de Pacemaker básico para este servidor IBM Db2. 

## <a name="install-ibm-db2-luw-and-sap-environment"></a>Instalar el entorno de SAP y IBM Db2 LUW

Antes de comenzar la instalación de un entorno SAP basado en IBM Db2 LUW, revise (vínculos que aparecen al principio del artículo):

+ Documentación de Azure
+ Documentación de SAP
+ Documentación de IBM

Consulte los manuales de instalación de SAP para instalar aplicaciones basadas en NetWeaver en IBM Db2 LUW.

Puede encontrar las guías de Ayuda de SAP portal usando la [buscador de guía de instalación de SAP][sap-instfind]

Puede filtrar la búsqueda para reducir el número de guías disponibles con la configuración de los filtros:

+ Quiero: "Instalar un nuevo sistema"
+ Mi base de datos: "IBM Db2 para Linux, Unix y Windows"
+ Filtros adicionales para las versiones de SAP Netweaver, configuración de la pila o sistema operativo.

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Sugerencias de instalación para la configuración de IBM Db2 LUW con HADR

Configurar la instancia de base de datos IBM Db2 LUW principal:

- Utilice la opción distribuida o alta disponibilidad
- Instalar la instancia de SAP ASCS/ERS y la base de datos
- Realizar una copia de seguridad de base de datos recién instalado


> [!IMPORTANT] 
> Anote el "puerto de comunicación de la base de datos" establecido durante la instalación. Debe ser el mismo número de puerto para ambas instancias de base de datos

Para configurar el servidor de base de datos en espera utilizando el procedimiento de copia del sistema homogéneo de SAP, ejecute estos pasos:

  - Utilice la opción de copia del sistema: sistemas - distribuidos - instancia de base de datos de destino.
  - Como método de copia, elija copia homogénea del sistema para que pueda usar copia de seguridad para restaurar una copia de seguridad en el servidor/instancia en espera
  - Cuando llegue al paso de salida para restaurar la base de datos para la copia del sistema homogéneo, salir del instalador. Restaurar la base de datos desde una copia de seguridad del host principal. Todas las fases de instalación posterior ya se han ejecutado en el servidor de base de datos principal
- Configuración de HADR para IBM Db2

> [!NOTE]
> Instalación y configuración específica de Azure y Pacemaker. Durante el procedimiento de instalación a través de SAP Software Provisioning Manager, hay una pregunta sobre la alta disponibilidad para IBM Db2 LUW explícita:
>+ No seleccione pureScale de IBM Db2
>+ No seleccione "instalar IBM Tivoli sistema de automatización para Multiplatforms
>+ No seleccione "Generar archivos de configuración del clúster"

> [!NOTE]
>Cuando se usa un dispositivo SBD para Linux Pacemaker, establezca los parámetros de HADR de Db2
>+ Duración (segundos) (HADR_PEER_WINDOW) de la ventana del mismo nivel HADR = 300  
>+ Valor de tiempo de espera HADR (HADR_TIMEOUT) = 60

> [!NOTE]
>Con el agente de vallado de Pacemaker de Azure:
>+ Duración (segundos) (HADR_PEER_WINDOW) de la ventana del mismo nivel HADR = 900  
>+ Valor de tiempo de espera HADR (HADR_TIMEOUT) = 60

Los parámetros se recomiendan en función de pruebas de conmutación por error/adquisición inicial. Es obligatorio para probar la funcionalidad adecuada de adquisición y la conmutación por error con estos valores de parámetro. Puesto que las configuraciones individuales pueden variar, estos parámetros pueden necesitar ajustes. 

> [!IMPORTANT]
> Específica para IBM Db2 en la configuración de HADR con inicio normal: la instancia de base de datos secundario o en espera debe estar en funcionamiento antes de poder iniciar la instancia de base de datos principal.

Para fines de demostración y los procedimientos documentados en este documento, la base de datos SID es **PTR**.

##### <a name="ibm-db2-hadr-check"></a>Comprobación HADR de IBM Db2
Una vez que configuró HADR y estado del mismo nivel y conectado en los nodos principales y en espera.

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
# Database Member 0 -- Database PTR -- Active -- Up 1 days 01:51:38 -- Date 2019-02-06-15.35.28.505451
# 
#                             <b>HADR_ROLE = PRIMARY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 1
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.170561 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6137
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 13
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000025
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223713
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 374400
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#        STANDBY_RECV_REPLAY_GAP(bytes) = 0
#                      PRIMARY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#                      STANDBY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:40:25.000000 (1549467625)
#              READS_ON_STANDBY_ENABLED = N

#Secondary output:
# Database Member 0 -- Database PTR -- Standby -- Up 1 days 01:46:43 -- Date 2019-02-06-15.38.25.644168
# 
#                             <b>HADR_ROLE = STANDBY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 0
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.205067 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6186
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 5
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000023
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223725
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 372480
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#        STANDBY_RECV_REPLAY_GAP(bytes) = 155
#                      PRIMARY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#                      STANDBY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:43:19.000000 (1549467799)
#              READS_ON_STANDBY_ENABLED = N
</code></pre>



## <a name="db2-pacemaker-configuration"></a>Configuración de Pacemaker de Db2

Cuando usas Pacemaker conmutación automática por error en el caso de un error de nodo, deberá configurar las instancias de Db2 y Pacemaker, según corresponda. En esta sección se describe este tipo de configuración.

Tienen el prefijo los siguientes elementos:

- **[A]**  ; se aplica a todos los nodos
- **[1]**  : aplicable solo al nodo 1 
- **[2]**  : aplicable solo al nodo 2.

<!-- Yast is a fixed term in Linux and not a spelling error -->

**[A]**  Requisitos previos para la configuración de Pacemaker:
1. Apagar tanto servidor de base de datos con db2 usuario\<sid > con db2stop
2. Entorno de shell de cambio para db2\<sid > usuario a "/ bin/ksh" - recomienda para usar la herramienta de Yast 


### <a name="pacemaker-configuration"></a>Configuración de pacemaker:

**[1]**  Configuración específica de Pacemaker de HADR de IBM Db2
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]**  Los recursos de la creación de IBM Db2
<pre><code># Replace **bold strings** with your instance name db2sid, database SID and virtual IP address/Azure Load Balancer

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virutal IP - same as Azure Load Balancer IP
sudo crm configure primitive rsc_ip_db2ptr_<b>PTR</b> IPaddr2 \
        op monitor interval="10s" timeout="20s" \
        params ip="<b>10.100.0.10</b>"

# Configure probe port for Azure load Balancer
sudo crm configure primitive rsc_nc_db2ptr_<b>PTR</b> anything \
        params binfile="/usr/bin/nc" cmdline_options="-l -k <b>62500</b>" \
        op monitor timeout="20s" interval="10" depth="0"

sudo crm configure group g_ip_db2ptr_<b>PTR</b> rsc_ip_db2ptr_<b>PTR</b> rsc_nc_db2ptr_<b>PTR</b>

sudo crm configure ms msl_Db2_db2ptr_<b>PTR</b> rsc_Db2_db2ptr_<b>PTR</b> \
        meta target-role="Started" notify="true"

sudo crm configure colocation col_db2_db2ptr_<b>PTR</b> inf: g_ip_db2ptr_<b>PTR</b>:Started msl_Db2_db2ptr_<b>PTR</b>:Master

sudo crm configure order ord_db2_ip_db2ptr_<b>PTR</b> inf: msl_Db2_db2ptr_<b>PTR</b>:promote g_ip_db2ptr_<b>PTR</b>:start

sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

**[1]**  Recursos iniciar IBM Db2 - librará Pacemaker en modo de mantenimiento
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]**  Asegúrese de que el estado del clúster es correcto y que todos los recursos se han iniciado. No es importante en qué nodo se ejecutan los recursos.
<pre><code>sudo crm status</code>

# <a name="2-nodes-configured"></a>2 nodos configurados
# <a name="5-resources-configured"></a>5 recursos configurados

# <a name="online--azibmdb01-azibmdb02-"></a>Online: [ azibmdb01 azibmdb02 ]

# <a name="full-list-of-resources"></a>Lista completa de recursos:

#  <a name="stonith-sbd----stonithexternalsbd-started-azibmdb02"></a>stonith-sbd    (stonith:external/sbd): Azibmdb02 iniciada
#  <a name="resource-group-gipdb2ptrptr"></a>Grupo de recursos: g_ip_db2ptr_PTR
#      <a name="rscipdb2ptrptr--ocfheartbeatipaddr2-------started-azibmdb02"></a>rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Azibmdb02 iniciada
#      <a name="rscncdb2ptrptr--ocfheartbeatanything------started-azibmdb02"></a>rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Azibmdb02 iniciada
#  <a name="masterslave-set-msldb2db2ptrptr-rscdb2db2ptrptr"></a>Conjunto maestro/esclavo: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
#      <a name="masters--azibmdb02-"></a>Masters: [ azibmdb02 ]
#      <a name="slaves--azibmdb01-"></a>Servidores subordinados: [azibmdb01]
</pre>

> [!IMPORTANT]
> Administración de Pacemaker instancia en clúster de Db2 debe realizarse a través de herramientas de Pacemaker. Mediante comandos de db2 (como db2stop) detectará Pacemaker como error de recurso. En el caso de mantenimiento, puede colocar los nodos o los recursos a modo de mantenimiento y Pacemaker suspenderá la supervisión de recursos y se pueden usar comandos de administración normal de db2.


### <a name="configure-azure-load-balancer"></a>Configuración de Azure Load Balancer
Se recomienda usar la [SKU del equilibrador de carga estándar de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

1. Configure el equilibrador de carga de Azure (a través del portal de Azure). Primero, cree un grupo de direcciones IP de front-end:

   1. En Azure portal, abra el equilibrador de carga de Azure, seleccione **grupo IP de front-end**y seleccione **agregar**.
   2. Escriba el nombre del nuevo grupo de IP front-end (por ejemplo, **conexión de Db2**).
   3. Establecer el **asignación** a **estático** y escriba la dirección IP **IP Virtual** definidas al principio.
   4. Seleccione **Aceptar**.
   5. Una vez creado el nuevo grupo de direcciones IP de front-end, anote la dirección IP del grupo.

2. Siguiente paso es crear un grupo de back-end:

   1. En Azure portal, abra el equilibrador de carga de Azure, seleccione **grupos back-end**y seleccione **agregar**.
   2. Escriba el nombre del nuevo grupo de back-end (por ejemplo, **back-end de Db2**).
   3. Seleccione **Agregar una máquina virtual**.
   4. Seleccione las máquinas virtuales/conjunto de disponibilidad hospeda la base de datos de IBM Db2 creada en el paso 3.
   5. Seleccione las máquinas virtuales del clúster de IBM Db2.
   6. Seleccione **Aceptar**.

3. Tercer paso es crear un sondeo de estado:

   1. En Azure portal, abra el equilibrador de carga de Azure, seleccione **sondeos de estado**y seleccione **agregar**.
   2. Escriba el nombre del nuevo sondeo de estado (por ejemplo, **Db2 hp**).
   3. Seleccione **TCP** como el protocolo y puerto **62500**. Mantenga el valor de **Intervalo** en 5 y el valor de **Umbral incorrecto** en 2.
   4. Seleccione **Aceptar**.

4. Crear las reglas de equilibrio de carga:

   1. En Azure portal, abra el equilibrador de carga de Azure, seleccione **reglas de equilibrio de carga**y seleccione **agregar**.
   2. Escriba el nombre de la nueva regla de equilibrador de carga (por ejemplo, **Db2 SID**).
   3. Seleccione la dirección IP de front-end, el grupo de back-end y el sondeo de estado que creó anteriormente (por ejemplo, **Db2-frontend**).
   4. Mantener la **protocolo** establecido en **TCP**y escriba el puerto *puerto de comunicación de la base de datos*.
   5. Aumente el **tiempo de espera de inactividad** a 30 minutos.
   6. Asegúrese de **habilitar la dirección IP flotante**.
   7. Seleccione **Aceptar**.


### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Realizar cambios en los perfiles de SAP para usar la dirección IP virtual para la conexión
El nivel de aplicación debe usar la dirección IP virtual definido y configurado para que el equilibrador de carga de Azure para conectarse a la instancia principal de la configuración de HADR. Los siguientes cambios son necesarios.

/sapmnt/\<SID >/perfil/de forma predeterminada. PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Instalación principal y los servidores de aplicación de cuadro de diálogo

Cuándo instalar principal y los servidores de aplicaciones de cuadro de diálogo con una configuración de HADR de Db2, deben usar el nombre de host virtual que seleccionó para la configuración. 

En caso de que ha realizado la instalación antes de crear la configuración de HADR de Db2, deberá realizar cambios como se describe en el párrafo anterior y siguiente para pilas de Java de SAP.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>Comprobación sistemas de pila de Java o ABAP + Java dirección URL de JDBC

Utilice la herramienta de configuración de J2EE para comprobar o actualizar la dirección URL de JDBC. El la herramienta de configuración de J2EE es una herramienta gráfica, como resultado debe **servidor X** instalado:
 
1. Inicie sesión en el servidor de aplicaciones principal de la instancia de J2EE y ejecute:
    <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
2. En el marco izquierdo, elija el almacén de seguridad.
2. En el marco de la derecha, elija el grupo/jdbcclave/<SAPSID>/url.
2. Cambiar el nombre de host en la dirección URL de JDBC en el nombre de host virtual <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
5. Elija Agregar.
5. Para guardar los cambios, haga clic en el icono de disco en la esquina superior izquierda.
5. Cierre la herramienta de configuración.
5. Reinicie la instancia de Java.

## <a name="configuration-of-log-archiving-for-hadr-setup"></a>Configuración de archivado de registros para la instalación de HADR
Para configurar el registro de Db2 para la configuración HADR de archivado, se recomienda que configure el servidor principal y la base de datos en espera tener capacidad de recuperación de inicio de sesión automático de todas las ubicaciones de archivo de registro. La principal y la base de datos en espera deben ser capaz de recuperar los archivos de registro de todas las ubicaciones del archivo de registro a que cualquiera de la base de datos, instancias podrían archivar los archivos de registro. 

El archivado de registros solo se realiza mediante la base de datos principal. Si cambia los roles HADR de los servidores de base de datos o si se produce un error, la nueva base de datos principal es responsable de archivado de registros. Si ha configurado las ubicaciones de archivo de registro diferente, los registros se pueden archivar dos veces y, en el caso de puesta al día local o remoto, puede que tenga que copiar manualmente los registros archivados desde el servidor principal anterior a la ubicación del registro activo del nuevo servidor principal.

Se recomienda configurar el recurso compartido NFS donde se escriben los registros de ambos nodos. NFS debe tener alta disponibilidad. 

Puede usar NFS de alta disponibilidad existente utilizado para los transportes, el directorio del perfil. Lectura:

- [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server][nfs-ha] 
- [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server con Azure Files de NetApp para SAP applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) sobre cómo usar [Azure Files de NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) para crear recursos compartidos de NFS


## <a name="test-the-cluster-setup"></a>Prueba de la configuración del clúster

En esta sección se describe cómo puede probar la configuración de HADR de Db2. **Todas las pruebas se supone que se registran como raíz del usuario** y IBM Db2 principal se está ejecutando en **azibmdb01** máquina virtual.

El estado inicial de todos los casos de prueba se explica aquí: (crm_mon - r o crm status)

- **estado de CRM** es un estado de instantánea Pacemaker en tiempo de ejecución 
- **-r crm_mon** es salida continua del estado de Pacemaker

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   Promoting azibmdb01
     Slaves: [ azibmdb02 ]
</code></pre>

El estado original en un sistema SAP se documenta en la transacción de DBACOCKPIT--> Configuración--> información general, como:

![DBACockpit - Pre migración](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>Adquisición de prueba de IBM Db2.


> [!IMPORTANT] 
> Antes de empezar la prueba, asegúrese de que Pacemaker no tiene ninguna acción con errores (estado de crm) y no hay ninguna restricción de ubicación (restos de pruebas de migración) y funciona la sincronización de HADR de IBM Db2. Póngase en contacto con el usuario db2\<sid > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Migrar el nodo que ejecuta la base de datos principal de Db2 mediante la ejecución de comando siguiente:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Una vez que se realiza la migración, la salida del estado de crm tendrá este aspecto:
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]
</code></pre>

El estado original en un sistema SAP se documenta en la transacción de DBACOCKPIT--> Configuración--> información general, como: ![DBACockpit - después de la migración](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

Migración de recursos con "migrar recursos de crm" crea restricciones de ubicación. Se deben eliminar las restricciones de ubicación. Si no se eliminan las restricciones de ubicación, el recurso no se puede conmutar por recuperación o experimenta adquisiciones no deseados. 

Migrar recursos a **azibmdb01** y desactive las restricciones de ubicación
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>


- recursos de CRM migrar < res_name > <host> : crea restricciones de ubicación y puede causar problemas con la adquisición
- recurso CRM desactive < res_name > - borra las restricciones de ubicación
- limpieza de recursos de CRM < res_name > - borra todos los errores del recurso


### <a name="test-the-fencing-agent"></a>El agente de vallado de prueba

En este caso, probamos vallado de SBD, que se recomienda para su uso con SUSE Linux.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Nodo de clúster **azibmdb01** debe reiniciarse. Rol principal de HADR de IBM Db2 que se va a mover a **azibmdb02**. Cuando **azibmdb01** es realizar una copia en línea, la instancia se va a mover en el rol de una instancia de base de datos secundaria de Db2. 

Para el caso de que el servicio Pacemaker no se inicia automáticamente en la primera reiniciada principal, asegúrese de que iniciarlo manualmente con:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Probar una adquisición manual

Puede probar una adquisición manual, detenga el servicio Pacemaker en **azibmdb01** nodo:
<pre><code>service pacemaker stop</code></pre>

estado en **azibmdb02**
<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]
</code></pre>

Después de la conmutación por error, puede iniciar el servicio nuevo en **azibmdb01**.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-db2-process-on-the-node-running-the-hadr-primary-database"></a>Terminar proceso Db2 en el nodo que ejecuta la base de datos principal de HADR

<pre><code>#Kill main db2 process - db2sysc
azibmdb01:~ # ps -ef|grep db2s
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0

azibmdb01:~ # kill -9 34598
</code></pre>

La instancia de Db2 que se va a producir un error y Pacemaker notificará el siguiente estado:

<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Slaves: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms


</code></pre>

Pacemaker se va a reiniciar la instancia de base de datos principal de Db2 en el mismo nodo o conmutación por error en el nodo que ejecuta la instancia de base de datos secundaria y se notifica un error.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms
</code></pre>


### <a name="kill-db2-process-on-node-that-runs-the-secondary-database-instance"></a>Terminar proceso Db2 en el nodo que ejecuta la instancia de base de datos secundaria

<pre><code>azibmdb02:~ # ps -ef|grep db2s
db2ptr    65250  65248  0 Feb11 ?        00:09:27 db2sysc 0

azibmdb02:~ # kill -9</code></pre>

Obtiene en error en el nodo indicado y el error notificado
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb02
     Masters: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>

Reinicia el get de la instancia de Db2 en el rol secundario de que se había asignado antes

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>



### <a name="stop-db-via-db2stop-force-on-node-running-the-hadr-primary-database-instance"></a>Detener la base de datos a través de db2stop force en el nodo que ejecuta la instancia de base de datos principal de HADR

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Como usuario db2\<sid > ejecutar force db2stop de comando:
<pre><code>azibmdb01:~ # su - db2ptr
azibmdb01:db2ptr> db2stop force</code></pre>

Se detectó un error
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb01
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=201, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:25 2019', queued=1ms, exec=150ms</code></pre>

La instancia de base de datos secundaria HADR Db2 se ha promocionado al rol principal
<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_start_0 on azibmdb01 'unknown error' (1): call=205, stat
us=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:27 2019', queued=0ms, exec=865ms</pre></code>


### <a name="crash-vm-with-restart-on-node-running-the-hadr-primary-database-instance"></a>Bloquear la máquina virtual con el reinicio en el nodo que ejecuta la instancia de base de datos principal de HADR

<pre><code>#Linux kernel panic - with OS restart
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

Pacemaker promoverá la instancia secundaria a la función de la instancia principal. La instancia principal antigua trasladará el rol secundario después de la máquina virtual y todos los servicios se restauran completamente después de reiniciar la máquina virtual:

<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>



### <a name="crash-the-vm-running-the-hadr-primary-database-instance-with-halt"></a>Bloquear la máquina virtual que ejecuta la instancia de base de datos principal de HADR con "halt"

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

En tal caso, Pacemaker detectará que el nodo que ejecuta la instancia de base de datos principal no responde.

<pre><code>2 nodes configured
5 resources configured

Node azibmdb01: UNCLEAN (online)
Online: [ azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

el paso siguiente es comprobar un **dividir cerebro** situación. Una vez que el nodo que queda es asegurarse de que el nodo que se ejecutó por última vez la instancia de base de datos principal, no está funcionando, una conmutación por error de recursos se va a ejecutar
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ] </code></pre>


En el caso de "Detener" del nodo, el nodo con errores debe reiniciarse a través de herramientas de administración de Azure (Portal, PowerShell, CLI de Azure,...). El nodo con error se va a iniciar la instancia de Db2 en el rol secundario, una vez que se vuelve a conectar.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]</code></pre>

## <a name="next-steps"></a>Pasos siguientes
Consulte esta documentación:

- [Escenarios y arquitectura de alta disponibilidad para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Configuración de Pacemaker en SUSE Linux escriba
- PRI
- (se Server en Azure]https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

     

