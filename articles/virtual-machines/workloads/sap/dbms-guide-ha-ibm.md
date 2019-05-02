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
ms.openlocfilehash: a74dd1a932cac41081786f76938a5b35de62d878
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689711"
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

IBM Db2 para Linux, UNIX y Windows (LUW) en [configuración de (HADR) de la recuperación ante desastres y disponibilidad alta](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) consta de un nodo que ejecuta una instancia de base de datos principal y al menos un nodo que ejecuta una instancia de base de datos secundaria. Los cambios realizados en la instancia de base de datos principal se replican en una instancia de base de datos secundaria sincrónica o asincrónicamente, dependiendo de la configuración. 

En este artículo se describe cómo implementar y configurar las máquinas virtuales (VM) de Azure, instalar la plataforma del clúster, e instalar el LUW de IBM Db2 con configuración de HADR. 

El artículo no abarca cómo instalar y configurar LUW de IBM Db2 con la instalación de software SAP o de HADR. Para ayudarle a realizar estas tareas, se proporcionan referencias a los manuales de instalación de SAP e IBM. En este artículo se centra en los elementos que son específicos para el entorno de Azure. 

Las versiones de IBM Db2 admitidas son 10.5 y posterior, como se documenta en la nota de SAP [1928533].

Antes de comenzar una instalación, consulte la documentación y notas SAP siguientes:

| Nota de SAP | DESCRIPCIÓN |
| --- | --- |
| [1928533] | Aplicaciones de SAP en Azure: Supported Products and Azure VM types (Nota de SAP 1928533: Aplicaciones SAP en Azure: productos admitidos y tipos de máquina virtual de Azure) |
| [2015553] | SAP en Azure: Support Prerequisites (Nota de soporte técnico 2015553 de SAP en Microsoft Azure: requisitos previos de soporte técnico) |
| [2178632] | Clave de las métricas de supervisión para SAP en Azure |
| [2191498] | SAP on Linux with Azure: Enhanced Monitoring (SAP en Linux con Azure: supervisión mejorada) |
| [2243692] | Linux en Azure (IaaS) VM: SAP license issues (Linux en máquinas virtuales de Microsoft Azure (IaaS): problemas de licencia de SAP) |
| [1984787] | SUSE LINUX Enterprise Server 12: Notas de instalación |
| [1999351] | Troubleshooting Enhanced Azure Monitoring for SAP (Solución de problemas de la supervisión mejorada de Azure para SAP) |
| [2233094] | DB6: Aplicaciones de SAP en Azure que usan IBM Db2 para Linux, UNIX y Windows - información adicional |
| [1612105] | DB6: Preguntas más frecuentes sobre Db2 con HADR |


| Documentación | 
| --- |
| [Wiki de la Comunidad SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes): Tiene todas las notas de SAP necesaria para Linux |
| [Máquinas virtuales planeación e implementación de SAP en Linux de Azure] [ planning-guide] guía |
| [Implementación de máquinas virtuales de Azure para SAP en Linux] [ deployment-guide] (este artículo) |
| [Implementación de administración system(DBMS) para SAP en Linux de base de datos de máquinas virtuales de Azure] [ dbms-guide] guía |
| [Carga de trabajo SAP en la lista de comprobación de planeación e implementación de Azure][azr-sap-plancheck] |
| [Guías de procedimientos recomendados de SUSE Linux Enterprise Server para SAP Applications 12 SP3][sles-for-sap-bp] |
| [Alta disponibilidad SUSE Linux Enterprise extensión 12 SP3][sles-ha-guide] |
| [Implementación de DBMS de máquinas virtuales de Azure de IBM Db2 para cargas de trabajo SAP][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR R 10.5][db2-hadr-10.5] |

## <a name="overview"></a>Información general
Para lograr una alta disponibilidad, IBM Db2 LUW con HADR está instalado en al menos dos máquinas virtuales de Azure, que se implementan en un [conjunto de disponibilidad de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) o en [zonas de disponibilidad de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). 

Los gráficos siguientes muestran un programa de instalación del servidor de base de datos de dos máquinas virtuales de Azure. Ambas máquinas virtuales de Azure del servidor de base de datos tiene su propio almacenamiento conectado y estén en funcionamiento. En HADR, una instancia de base de datos en una de las máquinas virtuales de Azure tiene el rol de la instancia principal. Todos los clientes se conectan a la instancia principal. Todos los cambios en las transacciones de base de datos se almacenan localmente en el registro de transacciones de Db2. Como las entradas del registro de transacciones son persistentes localmente, los registros se transfieren a través de TCP/IP a la instancia de base de datos en el segundo servidor de base de datos, el servidor en espera o instancia en espera. La instancia en espera, actualiza la base de datos local puesta al día la transacción transferida entradas del registro. De este modo, el servidor en espera se mantiene sincronizado con el servidor principal.

HADR es sólo una funcionalidad de replicación. Tiene ninguna detección de errores y sin instalaciones de la adquisición o la conmutación por error automática. Una adquisición o la transferencia al servidor de reserva se debe iniciar manualmente un administrador de base de datos. Para lograr una adquisición automática y la detección de errores, puede usar la característica de agrupación en clústeres de Linux Pacemaker. Pacemaker supervisa las instancias del servidor de dos bases de datos. Cuando se bloquea la instancia del servidor de base de datos principal, inicia Pacemaker una *automática* la adquisición de HADR por el servidor en espera. Pacemaker también garantiza que la dirección IP virtual se asigna al nuevo servidor principal.

![Información general de alta disponibilidad de IBM Db2](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

Para que SAP conectan los servidores de aplicaciones para la base de datos principal, necesita un nombre de host virtual y una dirección IP virtual. Si se produce una conmutación por error, los servidores de aplicaciones de SAP se conectarán a la nueva instancia de base de datos principal. En un entorno de Azure, un [equilibrador de carga Azure](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) es necesario usar una dirección IP virtual de la manera en que se necesita para HADR de IBM Db2. 

Para ayudarle a comprender cómo LUW de IBM Db2 con HADR y Pacemaker se adapta a una configuración alta disponibilidad de sistema SAP, la imagen siguiente presenta una visión general de un programa de instalación de alta disponibilidad de un sistema SAP en función de la base de datos IBM Db2. Este artículo trata solo de IBM Db2, sino que proporciona referencias a otros artículos acerca de cómo configurar otros componentes de un sistema SAP.

![Información general de todo el entorno de alta disponibilidad de IBM DB2](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-the-required-steps"></a>Descripción general de los pasos necesarios
Para implementar una configuración de IBM Db2, deberá seguir estos pasos:

  + Planear su entorno.
  + Implemente las máquinas virtuales.
  + Actualización de SUSE Linux y configurar sistemas de archivos.
  + Instale y configure Pacemaker.
  + Instalar [NFS de alta disponibilidad][nfs-ha].
  + Instalar [ASCS/ERS en un clúster independiente][ascs-ha].
  + Instalar la base de datos de IBM Db2 con la opción Distributed y alta disponibilidad (SWPM).
  + Instalar y crear un nodo de la base de datos secundaria y una instancia y configurar HADR.
  + Confirmar el funcionamiento de HADR.
  + Aplicar la configuración de Pacemaker al control de IBM Db2.
  + Configure el equilibrador de carga de Azure.
  + Instalación principal y los servidores de aplicaciones del cuadro de diálogo.
  + Compruebe y adaptar la configuración de servidores de aplicaciones SAP.
  + Realizar pruebas de adquisición y la conmutación por error.



## <a name="plan-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>Planear la infraestructura de Azure para hospedar LUW de IBM Db2 con HADR

Complete el proceso de planeamiento antes de ejecutar la implementación. Planeación, crea la base para la implementación de una configuración de Db2 con HADR en Azure. Elementos clave que deben formar parte del planeamiento de IMB Db2 LUW (parte de la base de datos del entorno de SAP) se muestran en la tabla siguiente:

| Tema | Descripción breve |
| --- | --- |
| Definir grupos de recursos de Azure | Grupos de recursos donde implementar máquina virtual, red virtual, Azure Load Balancer y otros recursos. Puede ser nuevo o existente. |
| Red virtual o definición de la subred | Donde se implementan máquinas virtuales para IBM Db2 y el equilibrador de carga de Azure. Puede ser existente o recién creado. |
| Máquinas virtuales que hospedan LUW de IBM Db2 | Tamaño de máquina virtual, almacenamiento, redes, la dirección IP. |
| Nombre de host virtual y dirección IP virtual de base de datos IBM Db2| La virtual IP o nombre de host que se usa para la conexión de servidores de aplicaciones SAP. **db-virt-hostname**, **db-virt-ip**. |
| Barrera de Azure | Barrera de Azure o vallado de SBD (altamente recomendado). Se impide el método para evitar situaciones de cerebro dividido. |
| SBD VM | Tamaño de máquina virtual SBD, almacenamiento, red. |
| Azure Load Balancer | Uso de Basic o Standard (recomendado), el sondeo de puerto para la base de datos de Db2 (nuestra recomendación 62500) **puerto de sondeo**. |
| Resolución de nombres| Cómo funciona la resolución de nombre en el entorno. Se recomienda el servicio DNS. Archivo de hosts local se puede usar. |
    
Para obtener más información sobre Linux Pacemaker en Azure, consulte [configuración de Pacemaker en SUSE Linux Enterprise Server en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker).

## <a name="deployment-on-suse-linux"></a>Implementación en SUSE Linux

El agente de recursos para IBM Db2 LUW se incluye en SUSE Linux Enterprise Server para aplicaciones de SAP. Para la configuración que se describe en este documento, debe usar el servidor Linux de SUSE para aplicaciones de SAP. Azure Marketplace contiene una imagen de SUSE Enterprise Server para SAP Applications 12 que puede usar para implementar nuevas máquinas virtuales de Azure. Tenga en cuenta los distintos modelos de soporte técnico o el servicio que ofrecen SUSE en Azure Marketplace al elegir una imagen de máquina virtual en la máquina virtual de Azure Marketplace. 

### <a name="hosts-dns-updates"></a>Hosts: Actualizaciones del servicio de nombres de dominio
Realice una lista de todos los nombres de host, incluidos los nombres de host virtual y actualizar los servidores DNS para habilitar la dirección IP correcta para la resolución de nombres de host. Si un servidor DNS no existe o no se puede actualizar y crear entradas DNS, deberá usar los archivos de host local de las máquinas virtuales individuales que forman parte de este escenario. Si usa las entradas de archivos de host, asegúrese de que las entradas se aplican a todas las máquinas virtuales en el entorno de sistema SAP. Sin embargo, se recomienda que utilice DNS que, idealmente, se extiende a Azure


### <a name="manual-deployment"></a>Implementación manual

Asegúrese de que el sistema operativo seleccionado sea compatible con SAP de IBM para IBM Db2 LUW. La lista de versiones de SO admitidas para máquinas virtuales de Azure y las versiones de Db2 está disponible en la nota de SAP [1928533]. La lista de las versiones del sistema operativo por versión de Db2 individual está disponible en la matriz de disponibilidad de productos de SAP. Se recomienda un mínimo de SLES 12 SP3 debido a las mejoras de rendimiento relacionados con Azure en este o versiones posteriores versiones de SUSE Linux.

1. Cree o seleccione un grupo de recursos.
1. Cree o seleccione una red virtual y subred.
1. Crear un conjunto de disponibilidad de Azure o implementar una zona de disponibilidad.
    + Para el conjunto de disponibilidad, establezca los dominios de actualización máximo en 2.
1. Crear la máquina Virtual 1.
    + Utilice SLES para imágenes SAP en Azure Marketplace.
    + Seleccione el conjunto de disponibilidad de Azure que creó en el paso 3, o seleccione la zona de disponibilidad.
1.  Crear la máquina Virtual 2.
    + Utilice SLES para imágenes SAP en Azure Marketplace.
    + Seleccione el conjunto de disponibilidad de Azure en creado en el paso 3, o seleccione la zona de disponibilidad (no la misma zona como en el paso 3).
1. Agregar discos de datos a las máquinas virtuales y, a continuación, compruebe la recomendación de una configuración de sistema de archivos en el artículo [implementación de DBMS de máquinas virtuales de Azure de IBM Db2 para la carga de trabajo SAP][dbms-db2].

## <a name="create-the-pacemaker-cluster"></a>Crear el clúster de Pacemaker
    
Para crear un clúster de Pacemaker básico para este servidor IBM Db2, vea [configuración de Pacemaker en SUSE Linux Enterprise Server en Azure][sles-pacemaker]. 

## <a name="install-the-ibm-db2-luw-and-sap-environment"></a>Instalar el entorno de SAP y IBM Db2 LUW

Antes de comenzar la instalación de un entorno SAP basado en IBM Db2 LUW, revise la siguiente documentación:

+ Documentación de Azure
+ Documentación de SAP
+ Documentación de IBM

En la sección Introducción de este artículo se proporcionan vínculos a esta documentación.

Consulte los manuales de instalación de SAP sobre la instalación de aplicaciones basadas en NetWeaver en IBM Db2 LUW.

Puede encontrar las guías en el portal de Ayuda de SAP mediante la [buscador de guía de instalación de SAP][sap-instfind].

Puede reducir el número de guías que se muestran en el portal mediante el establecimiento de los siguientes filtros:

- Quiero: "Instalar un nuevo sistema"
- Mi base de datos: "IBM Db2 para Linux, Unix y Windows"
- Filtros adicionales para las versiones de SAP NetWeaver, configuración de la pila o sistema operativo

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Sugerencias de instalación para la configuración de IBM Db2 LUW con HADR

Para configurar la instancia de base de datos IBM Db2 LUW principal:

- Utilice la opción distribuida o alta disponibilidad.
- Instale la instancia de SAP ASCS/ERS y la base de datos.
- Realizar una copia de seguridad de la base de datos recién instalado.


> [!IMPORTANT] 
> Anote el "puerto de comunicación de la base de datos" que se establece durante la instalación. Debe ser el mismo número de puerto para ambas instancias de base de datos

Para configurar el servidor de base de datos en espera utilizando el procedimiento de copia del sistema homogéneo de SAP, ejecute estos pasos:

1. Seleccione el **copia del sistema** opción > **elaborar sistemas** > **distribuidas** > **instancia de base de datos**.
1. Como un método de copia, seleccione **sistema homogéneo** para que pueda usar copia de seguridad para restaurar una copia de seguridad en la instancia de servidor en espera.
1. Cuando llegue al paso de salida para restaurar la base de datos para la copia del sistema homogéneo, salir del instalador. Restaurar la base de datos desde una copia de seguridad del host principal. Ya se han ejecutado todas las fases de instalación posterior en el servidor de base de datos principal.
1. Configuración de HADR para IBM Db2.

   > [!NOTE]
   > Para la instalación y configuración que es específico de Azure y Pacemaker: Durante el procedimiento de instalación a través de SAP Software Provisioning Manager, hay una pregunta sobre la alta disponibilidad para IBM Db2 LUW explícita:
   >+ No seleccione **pureScale de IBM Db2**.
   >+ No seleccione **instalar IBM Tivoli System Automation para Multiplatforms**.
   >+ No seleccione **generar archivos de configuración de clúster**.

   Cuando se usa un dispositivo SBD para Linux Pacemaker, establezca los siguientes parámetros de HADR de Db2:
   + Duración (segundos) (HADR_PEER_WINDOW) de la ventana del mismo nivel HADR = 300  
   + Valor de tiempo de espera HADR (HADR_TIMEOUT) = 60

   Cuando se usa un agente de barrera de Azure Pacemaker, establezca los parámetros siguientes:
   + Duración (segundos) (HADR_PEER_WINDOW) de la ventana del mismo nivel HADR = 900  
   + Valor de tiempo de espera HADR (HADR_TIMEOUT) = 60

Se recomienda que los parámetros anteriores según las pruebas iniciales de conmutación por error o adquisición. Es obligatorio de prueba para el correcto funcionamiento de la conmutación por error y adquisición con estos valores de parámetro. Dado que las configuraciones individuales pueden variar, los parámetros pueden necesitar ajustes. 

> [!IMPORTANT]
> Específicas de IBM Db2 con la configuración de HADR con inicio normal: La instancia de base de datos secundario o en espera debe ser en marcha antes de iniciar la instancia de base de datos principal.

Para fines de demostración y los procedimientos descritos en este artículo, la base de datos SID es **PTR**.

#### <a name="ibm-db2-hadr-check"></a>Comprobación de HADR de IBM Db2
Una vez que haya configurado HADR y el estado es del mismo nivel y conectado en los nodos principales y en espera, realizar la comprobación siguiente:

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

Cuando usas Pacemaker conmutación automática por error si se produce un error de nodo, deberá configurar las instancias de Db2 y Pacemaker, según corresponda. En esta sección se describe este tipo de configuración.

Tienen el prefijo los siguientes elementos:

- **[A]**: Aplicable a todos los nodos
- **[1]**: Aplicable solo al nodo 1 
- **[2]**: Aplicable solo al nodo 2

**[A]**  Requisitos previos para la configuración de Pacemaker:
1. Apague ambos servidores de base de datos con db2 usuario\<sid > con db2stop.
1. Cambie el entorno de shell para db2\<sid > usuario */bin/ksh*. Se recomienda que utilice la herramienta de Yast. 


### <a name="pacemaker-configuration"></a>Configuración de pacemaker

**[1]**  Configuración de Pacemaker HADR específicos de IBM Db2:
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]**  Los recursos de la creación de IBM Db2:
<pre><code># Replace **bold strings** with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virtual IP - same as Azure Load Balancer IP
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

**[1]**  Recursos iniciar IBM Db2:
* Coloque a Pacemaker del modo de mantenimiento.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]**  Asegúrese de que el estado del clúster es correcto y que todos los recursos se han iniciado. No es importante qué nodo que se ejecutan en los recursos.
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
> Debe administrar la Pacemaker instancia en clúster de Db2 mediante herramientas de Pacemaker. Si usa comandos de db2 como db2stop, Pacemaker detecta la acción como un error de recurso. Si va a realizar mantenimiento, puede colocar los nodos en modo de mantenimiento. Pacemaker suspende la supervisión de recursos y, a continuación, puede usar los comandos de administración normal de db2.


### <a name="configure-azure-load-balancer"></a>Configuración de Azure Load Balancer
Para configurar el equilibrador de carga de Azure, se recomienda que use el [SKU del equilibrador de carga estándar de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) y, a continuación, realice lo siguiente:

1. Crear un grupo IP front-end:

    a. En el portal de Azure, abra el equilibrador de carga de Azure, seleccione **grupo IP de front-end**y, a continuación, seleccione **agregar**.

   b. Escriba el nombre del nuevo grupo de IP front-end (por ejemplo, **conexión de Db2**).

   c. Establecer el **asignación** a **estático**y escriba la dirección IP **IP Virtual** definidas al principio.

   d. Seleccione **Aceptar**.

   e. Una vez creado el nuevo grupo de direcciones IP de front-end, anote la dirección IP del grupo.

1. Cree un grupo de back-end:

    a. En el portal de Azure, abra el equilibrador de carga de Azure, seleccione **grupos back-end**y, a continuación, seleccione **agregar**.

   b. Escriba el nombre del nuevo grupo de back-end (por ejemplo, **back-end de Db2**).

   c. Seleccione **Agregar una máquina virtual**.

   d. Seleccione el conjunto de disponibilidad o las máquinas virtuales que hospeda la base de datos IBM Db2 en el paso anterior.

   e. Seleccione las máquinas virtuales del clúster de IBM Db2.

   f. Seleccione **Aceptar**.

1. Cree un sondeo de estado:

    a. En el portal de Azure, abra el equilibrador de carga de Azure, seleccione **sondeos de estado**y seleccione **agregar**.

   b. Escriba el nombre del nuevo sondeo de estado (por ejemplo, **Db2 hp**).

   c. Seleccione **TCP** como el protocolo y puerto **62500**. Mantener la **intervalo** valor establecido en **5**y mantener la **umbral incorrecto** valor establecido en **2**.

   d. Seleccione **Aceptar**.

1. Crear las reglas de equilibrio de carga:

    a. En el portal de Azure, abra el equilibrador de carga de Azure, seleccione **reglas de equilibrio de carga**y, a continuación, seleccione **agregar**.

   b. Escriba el nombre de la nueva regla de equilibrador de carga (por ejemplo, **Db2 SID**).

   c. Seleccione la dirección IP de front-end, el grupo de back-end y el sondeo de estado que creó anteriormente (por ejemplo, **Db2-frontend**).

   d. Mantener la **protocolo** establecido en **TCP**y escriba el puerto *puerto de comunicación de la base de datos*.

   e. Aumente el **tiempo de espera de inactividad** a 30 minutos.

   f. Asegúrese de **habilitar la dirección IP flotante**.

   g. Seleccione **Aceptar**.


### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Realizar cambios en los perfiles de SAP para usar la dirección IP virtual para la conexión
Para conectarse a la instancia principal de la configuración de HADR, SAP capa de aplicación debe usar la dirección IP virtual que definen y configuran para el equilibrador de carga de Azure. Se requieren los siguientes cambios:

/sapmnt/\<SID >/perfil/de forma predeterminada. PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Instalación principal y los servidores de aplicaciones del cuadro de diálogo

Al instalar principal y servidores de aplicaciones de cuadro de diálogo con una configuración de HADR de Db2, use el host virtual el nombre que eligió para la configuración. 

Si ha realizado la instalación antes de que ha creado la configuración de HADR de Db2, realice los cambios como se describe en la sección anterior y siguiente para pilas de Java de SAP.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>Comprobación sistemas de pila de Java o ABAP + Java dirección URL de JDBC

Utilice la herramienta de configuración de J2EE para comprobar o actualizar la dirección URL de JDBC. Dado que la herramienta de configuración de J2EE es una herramienta gráfica, deberá tener X server instalado:
 
1. Inicie sesión en el servidor de aplicaciones principal de la instancia de J2EE y ejecute:
     <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
1. En el marco izquierdo, elija **almacén seguridad**.
1. En el marco de la derecha, elija el grupo de jdbc clave / / \<SAPSID>/url.
1. Cambiar el nombre de host en la dirección URL de JDBC en el nombre de host virtual.
     <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
1. Seleccione **agregar**.
1. Para guardar los cambios, seleccione el icono de disco en la parte superior izquierda.
1. Cierre la herramienta de configuración.
1. Reinicie la instancia de Java.

## <a name="configure-log-archiving-for-hadr-setup"></a>Configurar el archivado de registros para la instalación HADR
Para configurar el registro de Db2 para la configuración HADR de archivado, se recomienda que configure el servidor principal y la base de datos en espera tener capacidad de recuperación de inicio de sesión automático de todas las ubicaciones de archivo de registro. La base de datos principal y en espera debe ser capaz de recuperar los archivos de registro de todas las ubicaciones del archivo de registro a que cualquiera de la base de datos, instancias podrían archivar los archivos de registro. 

El archivado de registros se realiza solo mediante la base de datos principal. Si cambia los roles HADR de los servidores de base de datos o si se produce un error, la nueva base de datos principal es responsable de archivado de registros. Si ha configurado varias ubicaciones del archivo de registro, pueden archivar los registros de dos veces. Si se produce una actualización local o remota, también tendrá que copiar manualmente los registros archivados desde el servidor principal anterior a la ubicación del registro activo del nuevo servidor principal.

Se recomienda configurar un recurso compartido NFS, donde se escriben los registros de ambos nodos. El recurso compartido NFS debe tener alta disponibilidad. 

Puede usar recursos compartidos NFS de alta disponibilidad existentes para los transportes o un directorio del perfil. Para más información, consulte:

- [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server][nfs-ha] 
- [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server con Azure Files de NetApp para las aplicaciones de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
- [Los archivos de NetApp Azure](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (para crear recursos compartidos de NFS)


## <a name="test-the-cluster-setup"></a>Prueba de la configuración del clúster

En esta sección se describe cómo puede probar la configuración de HADR de Db2. *Todas las pruebas se supone que se registran como raíz del usuario* y se ejecuta el servidor principal de IBM Db2 en el *azibmdb01* máquina virtual.

El estado inicial de todos los casos de prueba se explica aquí: (crm_mon - r o crm status)

- **estado de CRM** es una instantánea del estado de Pacemaker en tiempo de ejecución 
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

El estado original en un sistema SAP se documenta en la transacción de DBACOCKPIT > Configuración > información general, tal como se muestra en la siguiente imagen:

![DBACockpit - Pre migración](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>Adquisición de prueba de IBM Db2


> [!IMPORTANT] 
> Antes de empezar la prueba, asegúrese de que:
> * Pacemaker no tiene ninguna acción con errores (estado de crm).
> * No hay ninguna restricción de ubicación (restos de pruebas de migración)
> * La sincronización de HADR de IBM Db2 está trabajando. Póngase en contacto con el usuario db2\<sid > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Migrar el nodo que se está ejecutando la base de datos principal de Db2 mediante la ejecución de comando siguiente:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Después de realiza la migración, la salida del estado de crm tendrá este aspecto:
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

El estado original en un sistema SAP se documenta en la transacción de DBACOCKPIT > Configuración > información general, tal como se muestra en la siguiente imagen:

![DBACockpit - después de la migración](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

Migración de recursos con "migrar recursos de crm" crea restricciones de ubicación. Se deben eliminar las restricciones de ubicación. Si no se eliminan las restricciones de ubicación, el recurso no se puede conmutar por recuperación o experimenta adquisiciones no deseados. 

Migrar el recurso a *azibmdb01* y desactive las restricciones de ubicación
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>

- **migrar recursos de CRM \<res_name > <host>:** Crea restricciones de ubicación y puede causar problemas con la adquisición
- **recursos de CRM claro \<res_name >**: Borra las restricciones de ubicación
- **limpieza de recursos de CRM \<res_name >**: Borra todos los errores del recurso

### <a name="test-the-fencing-agent"></a>El agente de vallado de prueba

En este caso, probamos vallado de SBD, que le recomendamos que realice cuando use SUSE Linux.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Nodo de clúster *azibmdb01* debe reiniciarse. El rol HADR IBM Db2 principal se va a mover a *azibmdb02*. Cuando *azibmdb01* es realizar una copia en línea, la instancia se va a mover en el rol de una instancia de base de datos secundaria de Db2. 

Si no se inicia automáticamente el servicio Pacemaker en el primero reiniciado principal, asegúrese de iniciarlo manualmente con:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Probar una adquisición manual

Puede probar una adquisición manual, detenga el servicio Pacemaker en *azibmdb01* nodo:
<pre><code>service pacemaker stop</code></pre>

estado en *azibmdb02*
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

Después de la conmutación por error, puede iniciar el servicio nuevo en *azibmdb01*.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>Eliminar el proceso de Db2 en el nodo que se ejecuta la base de datos principal de HADR

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

Pacemaker reiniciará la instancia de base de datos principal de Db2 en el mismo nodo, o se producirá un error a través en el nodo que se está ejecutando la instancia de base de datos secundaria y se notifica un error.

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


### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>Eliminar el proceso de Db2 en el nodo que ejecuta la instancia de base de datos secundaria

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

Reinicia el rol secundario de que se había asignado antes la instancia de Db2.

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



### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Detener la base de datos a través de db2stop force en el nodo que ejecuta la instancia de base de datos principal de HADR

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


### <a name="crash-vm-with-restart-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Bloquear la máquina virtual con el reinicio en el nodo que ejecuta la instancia de base de datos principal de HADR

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



### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>Bloquear la máquina virtual que ejecuta la instancia de base de datos principal de HADR con "halt"

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

En tal caso, Pacemaker detectará que el nodo que se está ejecutando la instancia de base de datos principal no responde.

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

el paso siguiente es comprobar un *dividir cerebro* situación. Una vez que el nodo superviviente determinó que el nodo que se ejecutó por última vez la instancia de base de datos principal está inactivo, se ejecuta una conmutación por error de recursos.
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


En el caso de un "Detener" del nodo, el nodo con errores debe reiniciarse a través de las herramientas de administración de Azure (en Azure portal, PowerShell o la CLI de Azure). Cuando se vuelve a conectar el nodo con error, inicia la instancia de Db2 en el rol secundario.

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
- [Escenarios y arquitectura de alta disponibilidad para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Configuración de Pacemaker en SUSE Linux Enterprise Server en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

     

