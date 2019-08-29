---
title: Escenarios admitidos de SAP HANA en Azure (instancias grandes)| Microsoft Docs
description: Escenarios admitidos y sus detalles de arquitectura de SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f17e447f26ae4f7573941fc0c578a918ff45a145
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70101232"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>Escenarios admitidos para instancias grandes de HANA
En este documento se describen los escenarios admitidos junto con sus detalles de arquitectura para instancias grandes de HANA (HLI).

>[!NOTE]
>Si el escenario que necesita no se menciona aquí, póngase en contacto con el equipo de Microsoft Service Management para que evalúe sus requisitos.
Antes de continuar con el aprovisionamiento de unidades HLI, valide el diseño con SAP o con su asociado de implementación de servicios.

## <a name="terms-and-definitions"></a>Términos y definiciones
Vamos a describir los términos y definiciones usados en el documento.

- SID: identificador del sistema para sistemas HANA.
- HLI: Instancias grandes de HANA.
- Recuperación ante desastres: sitio de recuperación ante desastres.
- Recuperación ante desastres normal: una configuración del sistema con un recurso dedicado usado únicamente con fines de recuperación ante desastres.
- Recuperación ante desastres multipropósito: un sistema en el sitio de recuperación ante desastres configurado para usar un entorno que no es de producción junto con una instancia de producción configurada para usarse con el evento de recuperación ante desastres. 
- SID único:  un sistema con una instancia instalada.
- Varios SID: un sistema con varias instancias configuradas. También se conoce como entorno MCOS.


## <a name="overview"></a>Información general
Las instancias grandes de HANA admiten una diversidad de arquitecturas para cumplir los requisitos empresariales. En la lista siguiente se tratan los escenarios y sus detalles de configuración. 

El diseño de arquitectura obtenido es simplemente desde la perspectiva de infraestructura, así que debe consultar con SAP o con sus asociados de implementación todo lo relativo a la implementación de HANA. Si su escenario no se menciona, póngase en contacto con el equipo de cuentas Microsoft para que revisen la arquitectura y obtengan una solución para usted.

**Estas arquitecturas son totalmente compatibles con el diseño TDI (integración de datos personalizado) y se admiten en SAP.**

En este documento se describen los detalles de los dos componentes de cada arquitectura admitida:

- Ethernet
- Storage

### <a name="ethernet"></a>Ethernet

Cada servidor aprovisionado viene preconfigurado con los conjuntos de interfaces Ethernet. Estos son los detalles de las interfaces Ethernet configuradas en cada unidad HLI.

- **R.** : esta interfaz se usa en el acceso del cliente.
- **B**: esta interfaz se usa para la comunicación nodo a nodo. Esta interfaz es común a todos los servidores (sin importar la topología solicitada), pero solo se usa en los 
- escenarios de escalabilidad horizontal.
- **C**: esta interfaz se usa para la conectividad del nodo con el almacenamiento.
- **D**: esta interfaz se usa para la conexión del nodo con el dispositivo ISCSI para la configuración de STONITH. Esta interfaz solo se configura cuando se solicita la configuración de HSR.  

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | STONITH |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Nodo a nodo |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | STONITH |

Usará las interfaces en función de la topología configurada en la unidad HLI. Por ejemplo, la interfaz "B" está configurada para la comunicación nodo a nodo, lo que resulta útil si tiene configurada una topología de escalabilidad horizontal. En el caso de una configuración de escalabilidad horizontal de un único nodo, esta interfaz no se usa. Revise los requisitos de su escenario (más adelante en este documento) para más información sobre el uso de la interfaz. 

Si es necesario, puede definir tarjetas NIC adicionales por su cuenta. Sin embargo, la configuración de las NIC existentes no se puede cambiar.

>[!NOTE]
>Pero puede encontrar interfaces adicionales que sean interfaces físicas o uniones. Tenga en cuenta las interfaces mencionadas anteriormente para su caso de uso; el resto puede pasarlas por alto, o no se pueden alterar.

La distribución para las unidades con dos direcciones IP asignadas debería ser como lo siguiente:

- La Ethernet "A" debe tener asignada una dirección IP que esté fuera del intervalo de direcciones del grupo de direcciones IP de servidor que envió a Microsoft. Esta dirección IP se usará para el mantenimiento en /etc/hosts del sistema operativo.

- La Ethernet "C" debe tener asignada una dirección IP que se usa para la comunicación con NFS. Por lo tanto, estas direcciones **NO** necesitan permanecer en etc/hosts para permitir el tráfico de instancia a instancia dentro del inquilino.

Para los casos de implementación de la replicación del sistema de HANA o el escalado horizontal de HANA, una configuración de hoja con dos direcciones IP asignadas no es adecuada. Si solo tiene dos direcciones IP asignadas y quiere implementar este tipo de configuración, póngase en contacto con SAP HANA en Azure Service Management para obtener una tercera dirección IP en otra red VLAN asignada. Para las unidades de instancia grande de HANA con tres direcciones IP asignadas en tres puertos de NIC, se aplican las reglas de uso siguientes:

- La Ethernet "A" debe tener asignada una dirección IP que esté fuera del intervalo de direcciones del grupo de direcciones IP de servidor que envió a Microsoft. Por lo tanto, esta dirección IP no se usará para el mantenimiento en /etc/hosts del sistema operativo.

- La Ethernet "B" debe usarse exclusivamente para el mantenimiento en etc/hosts, para la comunicación entre las distintas instancias. Estas direcciones también serían las direcciones IP que deben mantenerse en las configuraciones de escalado horizontal de HANA como las direcciones IP que HANA usa para la configuración entre nodos.

- La Ethernet "C" debe tener asignada una dirección IP que se usa para la comunicación con el almacenamiento NFS. Por lo tanto, este tipo de direcciones no debe permanecer en etc/hosts.

- La Ethernet "D" debe usarse exclusivamente para el acceso al dispositivo STONITH para Pacemaker. Esta interfaz es necesaria cuando configura la replicación del sistema HANA (HSR) y quiere conseguir la conmutación por error automática en el sistema operativo con un dispositivo basado en SBD.


### <a name="storage"></a>Storage
El almacenamiento está preconfigurado en función de la topología solicitada. Los tamaños de volumen y el punto de montaje varían en función del número de servidores, las SKU y la topología configurados. Revise los requisitos de su escenario (más adelante en este documento) para más información. Si se requiere más espacio de almacenamiento, puede adquirirlo en incrementos de un TB.

>[!NOTE]
>El punto de montaje /usr/sap/\<SID> es un vínculo simbólico al punto de montaje /hana/shared.


## <a name="supported-scenarios"></a>Escenarios admitidos

En los diagramas de arquitectura, se usan las siguientes notaciones para los gráficos:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

La siguiente lista muestra los escenarios admitidos:

1. Nodo único con un SID
2. MCOS de nodo único
3. Nodo único con recuperación ante desastres (normal)
4. Nodo único con recuperación ante desastres (multipropósito)
5. HSR con STONITH
6. HSR con recuperación ante desastres (normal/multipropósito) 
7. Conmutación por error automática de host (1+1) 
8. Escalabilidad horizontal con espera
9. Escalabilidad horizontal sin espera
10. Escalabilidad horizontal con recuperación ante desastres



## <a name="1-single-node-with-one-sid"></a>1. Nodo único con un SID

Esta topología admite un nodo en una configuración de escalabilidad horizontal con un SID.

### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![Single-node-with-one-SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|/hana/shared/SID | Instalación de HANA | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro | 
|/hana/logbackups/SID | Registros de rehacer |

### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.

## <a name="2-single-node-mcos"></a>2. MCOS de nodo único

Esta topología admite un nodo en una configuración de escalabilidad horizontal con varios SID.

### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![single-node-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|/hana/shared/SID1 | Instalación de HANA para SID1 | 
|/hana/data/SID1/mnt00001 | Instalación de archivos de datos para SID1 | 
|/hana/log/SID1/mnt00001 | Instalación de archivos de registro para SID1 | 
|/hana/logbackups/SID1 | Registros de rehacer para SID1 |
|/hana/shared/SID2 | Instalación de HANA para SID2 | 
|/hana/data/SID2/mnt00001 | Instalación de archivos de datos para SID2 | 
|/hana/log/SID2/mnt00001 | Instalación de archivos de registro para SID2 | 
|/hana/logbackups/SID2 | Registros de rehacer para SID2 |

### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- La distribución de tamaño de volumen se basa en el tamaño de la base de datos en memoria. Consulte la sección [Introducción y arquitectura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) para saber qué tamaños de base de datos en memoria se admiten con entornos de varios SID.

## <a name="3-single-node-with-dr-normal"></a>3. Nodo único con recuperación ante desastres (normal)
 
Esta topología admite un nodo en una configuración de escalabilidad horizontal con uno o varios SID con la replicación basada en almacenamiento en el sitio de recuperación ante desastres para un SID principal. En el diagrama, solo se representa un SID en el sitio principal, pero también se admiten varios SID (MCOS).

### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![Single-node-with-dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|/hana/shared/SID | Instalación de HANA para SID | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para SID | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para SID | 
|/hana/logbackups/SID | Registros de rehacer para SID |


### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- En MCOS: La distribución de tamaño de volumen se basa en el tamaño de la base de datos en memoria. Consulte la sección [Introducción y arquitectura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) para saber qué tamaños de base de datos en memoria se admiten con entornos de varios SID.
- En la recuperación ante desastres: los volúmenes y los puntos de montaje están configurados (marcados como "obligatorios para la instalación de HANA") para la instalación de instancias de HANA de producción en la unidad HLI de recuperación ante desastres. 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros y los volúmenes compartidos (marcados como "replicación de almacenamiento") se replican mediante una instantánea desde el sitio de producción. Estos volúmenes se montan solo durante el tiempo de conmutación por error. Para una información más detallada, lea el documento [Procedimiento de conmutación por error de recuperación ante desastres](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery).
- El volumen de arranque de **clase SKU tipo I** se replica en el nodo de recuperación ante desastres.


## <a name="4-single-node-with-dr-multipurpose"></a>4. Nodo único con recuperación ante desastres (multipropósito)
 
Esta topología admite un nodo en una configuración de escalabilidad horizontal con uno o varios SID con la replicación basada en almacenamiento en el sitio de recuperación ante desastres para un SID principal. En el diagrama, solo se representa un SID en el sitio principal, pero también se admiten varios SID (MCOS). En el sitio de recuperación ante desastres, la unidad HLI se usa para la instancia de QA mientras las operaciones de producción se ejecutan desde el sitio principal. En el momento de la conmutación por error de recuperación ante desastres (o prueba de conmutación por error), se retira la instancia de QA en el sitio de recuperación ante desastres.

### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![single-node-with-dr-multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En el sitio principal**|
|/hana/shared/SID | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |
|**En el sitio de recuperación ante desastres**|
|/hana/shared/SID | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/shared/QA-SID | Instalación de HANA para el SID de QA | 
|/hana/data/QA-SID/mnt00001 | Instalación de archivos de datos para el SID de QA | 
|/hana/log/QA-SID/mnt00001 | Instalación de archivos de registro para el SID de QA |
|/hana/logbackups/QA-SID | Registros de rehacer para el SID de QA |

### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- En MCOS: La distribución de tamaño de volumen se basa en el tamaño de la base de datos en memoria. Consulte la sección [Introducción y arquitectura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) para saber qué tamaños de base de datos en memoria se admiten con entornos de varios SID.
- En la recuperación ante desastres: los volúmenes y los puntos de montaje están configurados (marcados como "obligatorios para la instalación de HANA") para la instalación de instancias de HANA de producción en la unidad HLI de recuperación ante desastres. 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros y los volúmenes compartidos (marcados como "replicación de almacenamiento") se replican mediante una instantánea desde el sitio de producción. Estos volúmenes se montan solo durante el tiempo de conmutación por error. Para una información más detallada, lea el documento [Procedimiento de conmutación por error de recuperación ante desastres](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros, los registros y los volúmenes compartidos para QA (marcados como "instalación de instancia de QA") se configuran para la instalación de la instancia de QA.
- El volumen de arranque de **clase SKU tipo I** se replica en el nodo de recuperación ante desastres.

## <a name="5-hsr-with-stonith"></a>5. HSR con STONITH
 
Esta topología admite dos nodos para la configuración de replicación de sistema de HANA (HSR). Esta configuración solo se admite para instancias únicas de HANA en un nodo. Esto quiere decir que los escenarios MCOS NO son compatibles.

**A partir de ahora, esta arquitectura se admite solo para el sistema operativo SUSE.**


### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![HSR-with-STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Usado para STONITH |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Usado para STONITH |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En el nodo principal**|
|/hana/shared/SID | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |
|**En el nodo secundario**|
|/hana/shared/SID | Instalación de HANA para el SID secundario | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID secundario | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID secundario | 
|/hana/logbackups/SID | Registros de rehacer para el SID secundario |

### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- En MCOS: La distribución de tamaño de volumen se basa en el tamaño de la base de datos en memoria. Consulte la sección [Introducción y arquitectura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) para saber qué tamaños de base de datos en memoria se admiten con entornos de varios SID.
- STONITH: se configura un SBD para la configuración de STONITH. Sin embargo, un uso de STONITH es opcional.


## <a name="6-hsr-with-dr"></a>6. HSR con recuperación ante desastres
 
Esta topología admite dos nodos para la configuración de replicación de sistema de HANA (HSR). Se admite tanto la recuperación ante desastres normal como multipropósito. Estas configuraciones solo se admite para instancias únicas de HANA en un nodo. Esto quiere decir que los escenarios MCOS NO son compatibles con estas configuraciones.

En el diagrama, el escenario multipropósito se representa donde, en el sitio de recuperación ante desastres, la unidad HLI se usa para la instancia de QA mientras las operaciones de producción se ejecutan desde el sitio principal. En el momento de la conmutación por error de recuperación ante desastres (o prueba de conmutación por error), se retira la instancia de QA en el sitio de recuperación ante desastres. 



### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![HSR-with-DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurado pero no en uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Usado para STONITH |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Configurado pero no en uso |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Usado para STONITH |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En el nodo principal en el sitio principal**|
|/hana/shared/SID | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |
|**En el nodo secundario en el sitio principal**|
|/hana/shared/SID | Instalación de HANA para el SID secundario | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID secundario | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID secundario | 
|/hana/logbackups/SID | Registros de rehacer para el SID secundario |
|**En el sitio de recuperación ante desastres**|
|/hana/shared/SID | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/shared/QA-SID | Instalación de HANA para el SID de QA | 
|/hana/data/QA-SID/mnt00001 | Instalación de archivos de datos para el SID de QA | 
|/hana/log/QA-SID/mnt00001 | Instalación de archivos de registro para el SID de QA |
|/hana/logbackups/QA-SID | Registros de rehacer para el SID de QA |

### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- En MCOS: La distribución de tamaño de volumen se basa en el tamaño de la base de datos en memoria. Consulte la sección [Introducción y arquitectura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) para saber qué tamaños de base de datos en memoria se admiten con entornos de varios SID.
- STONITH: se configura un SBD para la configuración de STONITH. Sin embargo, un uso de STONITH es opcional.
- En la recuperación ante desastres: **se necesitan dos conjuntos de volúmenes de almacenamiento** para la replicación de nodo principal y secundaria.
- En la recuperación ante desastres: los volúmenes y los puntos de montaje están configurados (marcados como "obligatorios para la instalación de HANA") para la instalación de instancias de HANA de producción en la unidad HLI de recuperación ante desastres. 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros y los volúmenes compartidos (marcados como "replicación de almacenamiento") se replican mediante una instantánea desde el sitio de producción. Estos volúmenes se montan solo durante el tiempo de conmutación por error. Para una información más detallada, lea el documento [Procedimiento de conmutación por error de recuperación ante desastres](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros, los registros y los volúmenes compartidos para QA (marcados como "instalación de instancia de QA") se configuran para la instalación de la instancia de QA.
- El volumen de arranque de **clase SKU tipo I** se replica en el nodo de recuperación ante desastres.


## <a name="7-host-auto-failover-11"></a>7. Conmutación por error automática de host (1+1)
 
Esta topología admite dos nodos en una configuración de conmutación por error automática del host. Hay un nodo con un rol maestro o de trabajo y otro como espera. **SAP admite este escenario solo para S/4 HANA.** Para más información, consulte la nota de OSS "[2408419 - SAP S/4HANA - Multi-Node Support](https://launchpad.support.sap.com/#/notes/2408419)" (2408419: compatibilidad con varios nodos de SAP S/4HANA).



### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![sca](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicación nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Comunicación nodo a nodo |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En los nodos principales y de espera**|
|/hana/shared | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |



### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
- En espera: los volúmenes y los puntos de montaje están configurados (marcados como "obligatorios para la instalación de HANA") para la instalación de instancias de HANA en la unidad de espera.
 

## <a name="8-scale-out-with-standby"></a>8. Escalabilidad horizontal con espera
 
Esta topología es compatible con varios nodos en una configuración de escalabilidad horizontal. Hay un nodo con el rol principal, uno o más nodos con el rol de trabajo y uno o varios nodos como espera. Sin embargo, en cualquier momento dado puede haber un único nodo principal.


### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![scaleout-nm-standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicación nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Comunicación nodo a nodo |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En los nodos principal, de trabajo y de espera**|
|/hana/shared | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |


## <a name="9-scale-out-without-standby"></a>9. Escalabilidad horizontal sin espera
 
Esta topología es compatible con varios nodos en una configuración de escalabilidad horizontal. Hay un nodo con el rol principal y uno o varios nodos con el rol de trabajo. Sin embargo, en cualquier momento dado puede haber un único nodo principal.


### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![scaleout-nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicación nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Comunicación nodo a nodo |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En los nodos principal y de trabajo**|
|/hana/shared | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |


### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.

## <a name="10-scale-out-with-dr"></a>10. Escalabilidad horizontal con recuperación ante desastres
 
Esta topología es compatible con varios nodos en una configuración de escalabilidad horizontal con recuperación ante desastres. Se admite tanto la recuperación ante desastres normal como multipropósito. En el diagrama solo se representa la recuperación ante desastres de un propósito. Puede solicitar esta topología con o sin el nodo de espera.


### <a name="architecture-diagram"></a>Diagrama de la arquitectura  

![scaleout-with-dr.png](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
Las siguientes interfaces de red están preconfiguradas:

| INTERFACES LÓGICAS DE NIC | TIPO DE SKU | Nombre con sistema operativo SUSE | Nombre con sistema operativo RHEL | Caso de uso|
| --- | --- | --- | --- | --- |
| Una | TIPO I | eth0.tenant | eno1.tenant | Cliente a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicación nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Nodo a almacenamiento |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurado pero no en uso |
| Una | TIPO II | vlan\<tenantNo> | team0.tenant | Cliente a HLI |
| b | TIPO II | vlan\<tenantNo+2> | team0.tenant+2 | Comunicación nodo a nodo |
| C | TIPO II | vlan\<tenantNo+1> | team0.tenant+1 | Nodo a almacenamiento |
| D | TIPO II | vlan\<tenantNo+3> | team0.tenant+3 | Configurado pero no en uso |

### <a name="storage"></a>Storage
Los puntos de montaje siguientes están preconfigurados:

| Punto de montaje | Caso de uso | 
| --- | --- |
|**En el nodo principal**|
|/hana/shared | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 
|/hana/logbackups/SID | Registros de rehacer para el SID de producción |
|**En el nodo de recuperación ante desastres**|
|/hana/shared | Instalación de HANA para el SID de producción | 
|/hana/data/SID/mnt00001 | Instalación de archivos de datos para el SID de producción | 
|/hana/log/SID/mnt00001 | Instalación de archivos de registro para el SID de producción | 


### <a name="key-considerations"></a>Consideraciones clave
- /usr/sap/SID es un vínculo simbólico a /hana/shared/SID.
-  En la recuperación ante desastres: los volúmenes y los puntos de montaje están configurados (marcados como "obligatorios para la instalación de HANA") para la instalación de instancias de HANA de producción en la unidad HLI de recuperación ante desastres. 
- En la recuperación ante desastres: los datos, las copias de seguridad de registros y los volúmenes compartidos (marcados como "replicación de almacenamiento") se replican mediante una instantánea desde el sitio de producción. Estos volúmenes se montan solo durante el tiempo de conmutación por error. Para una información más detallada, lea el documento [Procedimiento de conmutación por error de recuperación ante desastres](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- El volumen de arranque de **clase SKU tipo I** se replica en el nodo de recuperación ante desastres.


## <a name="next-steps"></a>Pasos siguientes
- Consulte [Infraestructura y conectividad](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) para HLI.
- [Alta disponibilidad y recuperación ante desastres](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) para HLI
