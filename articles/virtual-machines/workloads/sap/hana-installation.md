---
title: Instalación de SAP HANA en SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Instalación de SAP HANA en SAP HANA en Azure (instancia grande).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dad088138fea2dd4fadc0cc9eed71245c32a8e0b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162685"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Procedimiento para instalar y configurar SAP HANA en Azure (instancias grandes)

Antes de leer este artículo, familiarícese con [términos comunes de instancias grandes de HANA](hana-know-terms.md) y las [SKU de instancias grandes de HANA](hana-available-skus.md).

La instalación de SAP HANA es su responsabilidad y puede hacerlo inmediatamente después de la entrega de un nuevo servidor de SAP HANA en Azure (instancias de grandes). Y después de establecer la conectividad entre las redes virtuales de Azure y las unidades de instancia grande de HANA. 

> [!Note]
> Según la directiva de SAP, la instalación de SAP HANA debe realizarla una persona certificada para realizar instalaciones de SAP HANA. Una persona que haya superado el examen de Certified SAP Technology Associate, el examen de certificación Instalación de SAP HANA o un integrador de sistemas (SI) certificado de SAP.

Vuelva a comprobar, en especial si tiene pensado instalar HANA 2.0, [Nota de compatibilidad de SAP n.º 2235581 - SAP HANA: Sistemas operativos admitidos](https://launchpad.support.sap.com/#/notes/2235581/E) que el sistema operativo es compatible con la versión de SAP HANA que ha decidido instalar. Comprobará que el sistema operativo compatible con HANA 2.0 tiene más restricciones que el sistema operativo compatible con HANA 1.0. 

> [!IMPORTANT] 
> Para las unidades de tipo II, en este momento solo se admite la versión del sistema operativo SLES 12 SP2. 

Antes de comenzar la instalación de HANA se debe realizar lo siguiente:
- [Validación de las unidades HLI](#validate-the-hana-large-instance-units)
- [Configuración del sistema operativo](#operating-system)
- [Configuración de la red](#networking)
- [Configuración de almacenamiento](#storage)


## <a name="validate-the-hana-large-instance-units"></a>Validación de las unidades de HANA (instancias grandes)

Después de recibir la unidad de HANA (instancias grandes) de Microsoft, valide la siguiente configuración y haga los ajustes que sean necesarios.

El **primer paso** después de recibir la instancia grande de HANA y establecer el acceso y la conectividad a las instancias consiste en registrar el sistema operativo de la instancia con el proveedor de este. Este paso incluye el registro del sistema operativo SUSE Linux en una instancia de SUSE SMT que debe haber implementado en una máquina virtual en Azure. La unidad de HANA (instancias grandes) puede conectarse a esta instancia de SMT (vea [How to setup SMT server for SUSE Linux](hana-setup-smt.md) [Procedimiento para configurar el servidor SMT para SUSE Linux]). O bien, debe registrar el sistema operativo RedHat con la instancia de RedHat Subscription Manager a la que necesite conectarse. Vea también los comentarios de este [documento](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Este paso también es necesario para poder aplicar la revisión del sistema operativo. Una tarea que es responsabilidad del cliente. Para SUSE, encuentre documentación para instalar y configurar SMT [aquí](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

El **segundo paso** es comprobar si hay nuevas revisiones y correcciones de la versión del sistema operativo específica. Compruebe si la versión de la instancia grande de HANA es la más reciente. En función del momento en el que Microsoft implemente las versiones o revisiones del sistema operativo, y los cambios de imagen, es posible que haya casos en los que no se incluyan las revisiones más recientes. Por tanto, comprobar si el proveedor particular de Linux ha publicado revisiones importantes para la seguridad, la funcionalidad, la disponibilidad y el rendimiento que necesiten implementarse es un paso obligatorio después de asumir una unidad de instancia grande de HANA.

El **tercer paso** consiste en comprobar las notas de SAP correspondientes a la instalación y configuración de SAP HANA en la versión concreta del sistema operativo. Por cambios en las recomendaciones o en las notas de SAP o las configuraciones que dependen de los distintos escenarios de instalación, Microsoft no siempre podrá tener una unidad de instancia grande de HANA configurada a la perfección. Por tanto, como cliente es obligatorio que lea las notas de SAP relacionadas con SAP HANA en la versión exacta de Linux. Compruebe también las configuraciones de la versión del sistema operativo necesaria y aplique los valores de configuración donde no se haya hecho.

Para casos particulares, compruebe los siguientes parámetros y, si es necesario, ajústelos a:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

A partir de SP1 SLES12 y RHEL 7.2, se deben establecer estos parámetros en un archivo de configuración en el directorio /etc/sysctl.d. Por ejemplo, se debe crear un archivo de configuración con el nombre 91-NetApp-HANA.conf. Para las versiones anteriores de SLES y RHEL, estos parámetros deben establecerse en in/etc/sysctl.conf.

Para todas las versiones de RHEL y a partir de SLES12, el parámetro 
- sunrpc.tcp_slot_table_entries = 128

se debe establecer en in/etc/modprobe.d/sunrpc-local.conf. Si el archivo no existe, primero debe crearse; para ello, agregue la entrada siguiente: 
- options sunrpc tcp_max_slot_table_entries=128

El **cuarto paso** consiste en comprobar la hora del sistema de la unidad de instancia grande de HANA. Las instancias se implementan con una zona horaria del sistema que represente la ubicación de la región de Azure donde se encuentre el sello de la instancia grande de HANA. Es libre de cambiar la hora de su sistema o la zona horaria de sus instancias. Si lo hace, al pedir más instancias para su inquilino, necesitará adaptar la zona horaria de las instancias recién entregadas. Las operaciones de Microsoft no tienen ninguna información sobre la zona horaria del sistema donde instale las instancias tras la entrega. Por lo tanto, las instancias recién implementadas pueden no tener la misma zona horaria que a las que cambió. Como resultado, es su responsabilidad como cliente comprobarlo y, si es necesario, adaptar la zona horaria de las instancias entregadas. 

El **quinto paso** consiste en comprobar etc/hosts. A medida que van entregando las hojas, se les asignan direcciones IP diferentes con fines distintos (consulte la sección siguiente). Compruebe el archivo /etc/hosts. Cuando se agreguen unidades a un inquilino existente, no espere que etc/hosts en los sistemas recién implementados se mantenga correctamente con las direcciones IP de los sistemas entregados anteriormente. Depende de usted como cliente comprobar que la configuración sea la correcta, de manera que una instancia recién implementada pueda interactuar y resolver los nombres de las unidades implementadas anteriormente en el inquilino. 

## <a name="operating-system"></a>Sistema operativo

> [!IMPORTANT] 
> Para las unidades de tipo II, en este momento solo se admite la versión del sistema operativo SLES 12 SP2. 

El espacio de intercambio de la imagen del sistema operativo proporcionada está establecido en 2 GB de acuerdo con la [SAP Support Note #1999997 - FAQ: SAP HANA Memory](https://launchpad.support.sap.com/#/notes/1999997/E) (Nota de compatibilidad de SAP 1999997: Preguntas más frecuentes sobre la memoria de SAP HANA). Cualquier valor diferente deseado debe establecerlo el cliente.

[SUSE Linux Enterprise Server 12 SP1 for SAP Applications](https://www.suse.com/products/sles-for-sap/hana) es la distribución de Linux instalada para SAP HANA en Azure (instancias grandes). Esta distribución particular proporciona funcionalidades específicas de SAP &quot;listas para usar&quot; (incluidos los parámetros predefinidos para ejecutar SAP en SLES de forma eficaz).

Vea [Biblioteca de recursos/Informes oficiales](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) en el sitio web de SUSE y [SAP on SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) (SAP en SUSE) en la red de la comunidad de SAP (SCN) para varios recursos útiles relacionados con la implementación de SAP HANA en SLES (incluida la configuración de alta disponibilidad, el endurecimiento de la seguridad específico de las operaciones de SAP etc.).

Vínculos adicionales y útiles relacionados sobre SAP en SUSE:

- [SAP HANA on SUSE Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) (Sitio de SAP HANA en SUSE Linux)
- [Best Practice for SAP: Enqueue Replication – SAP NetWeaver on SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113) (Procedimiento recomendado para SAP: poner en cola la replicación - SAP NetWeaver en SUSE Linux Enterprise 12)
- [ClamSAP – SLES Virus Protection for SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (ClamSAP: protección contra virus de SLES para SAP), incluido SLES 12 para SAP Applications

Notas de compatibilidad de SAP aplicables a la implementación de SAP HANA en SLES 12:

- [Nota de compatibilidad de SAP n.º 1944799: Instrucciones de SAP HANA para la instalación de sistema operativo](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
- [Nota de compatibilidad de SAP n.º 2205917: Configuración recomendada del sistema operativo de SAP HANA DB para SLES 12 en aplicaciones SAP](https://launchpad.support.sap.com/#/notes/2205917/E)
- [Nota de compatibilidad de SAP n.º 1984787: Notas de instalación de SUSE Linux Enterprise Server 12](https://launchpad.support.sap.com/#/notes/1984787)
- [Nota de compatibilidad de SAP n.º 171356: Información general del software SAP en Linux](https://launchpad.support.sap.com/#/notes/1984787)
- [Nota de compatibilidad de SAP n.º 1391070: Soluciones UUID de Linux](https://launchpad.support.sap.com/#/notes/1391070)

[Red Hat Enterprise Linux para SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) es otra oferta para ejecutar SAP HANA en instancias grandes de HANA. Ya están disponibles las versiones de RHEL 6.7 y 7.2. Tenga en cuenta que en máquinas virtuales de Azure no nativas en las que solo se admite RHEL 7.2 y versiones posteriores, las instancias grandes de HANA también admiten RHEL 6.7. Sin embargo, se recomienda utilizar una versión RHEL 7.x.

Vínculos adicionales y útiles relacionados sobre SAP en Red Hat:
- [Sitio de SAP HANA en Red Hat Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

Notas de compatibilidad de SAP aplicables a la implementación de SAP HANA en Red Hat:

- [Nota de compatibilidad de SAP n.º 2009879: Directrices de SAP HANA para el sistema operativo Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879/E)
- [2292690 - SAP HANA DB: Configuración recomendada del sistema operativo para RHEL 7](https://launchpad.support.sap.com/#/notes/2292690)
- [Nota de compatibilidad de SAP n.º 2247020: Configuración recomendada del sistema operativo para RHEL 6.7 en SAP HANA DB](https://launchpad.support.sap.com/#/notes/2247020)
- [Nota de compatibilidad de SAP n.º 1391070: Soluciones UUID de Linux](https://launchpad.support.sap.com/#/notes/1391070)
- [Nota de compatibilidad de SAP n.º 2228351: Linux: Revisión 110 (o superior) de SAP HANA Database SPS 11 en RHEL 6 o SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [Nota de compatibilidad de SAP n.º2397039: P+F sobre SAP en RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [Nota de compatibilidad de SAP n.º 1496410: Instalación y actualización de Red Hat Enterprise Linux 6.x](https://launchpad.support.sap.com/#/notes/1496410).
- [Nota de compatibilidad de SAP n.º 2002167: Instalación y actualización de Red Hat Enterprise Linux 7.x](https://launchpad.support.sap.com/#/notes/2002167).

### <a name="time-synchronization"></a>Sincronización de la hora

Las aplicaciones de SAP basadas en la arquitectura de SAP NetWeaver son sensibles a las diferencias de hora para los distintos componentes que conforman el sistema SAP. Los volcados cortos de ABAP de SAP con el título de error de ZDATE\_LARGE\_TIME\_DIFF probablemente le resulten familiares, ya que estos volcados cortos aparecen cuando la hora del sistema de diferentes servidores o máquinas virtuales se distancia demasiado.

Para SAP HANA en Azure (instancias grandes), la sincronización de la hora que se realiza en Azure no se aplica a las unidades de proceso en las marcas de instancia grande. Esta sincronización no se aplica para ejecutar aplicaciones de SAP en máquinas virtuales nativas de Azure, ya que Azure garantiza que la hora del sistema está correctamente sincronizada. Como consecuencia de esto, se debe configurar un servidor horario independiente, que lo puedan usar servidores de aplicaciones SAP que se ejecutan en VM de Azure y las instancias de base de datos de SAP HANA que se ejecutan en instancias grandes de HANA. La infraestructura de almacenamiento en marcas de instancias grandes se sincronizado con los servidores NTP.


## <a name="networking"></a>Redes
Se supone que ha seguido las recomendaciones de diseño de las redes virtuales de Azure y de conexión de esas redes a las instancias grandes de HANA tal y como se describe en estos documentos:

- [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Hay algunos detalles que vale la pena mencionar acerca de las funciones de red de las unidades individuales. Cada unidad de instancia grande de HANA viene con dos o tres direcciones IP asignadas a dos o tres puertos NIC de la unidad. En las configuraciones de escalado horizontal de HANA y el escenario de replicación del sistema de HANA se utilizan tres direcciones IP. Una de las direcciones IP asignadas a la NIC de la unidad está fuera del grupo de direcciones IP del servidor que se describió en la [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Consulte los [escenarios admitidos por HLI](hana-supported-scenario.md) para conocer los detalles de Ethernet correspondiente a su arquitectura.

## <a name="storage"></a>Storage

El diseño de almacenamiento para SAP HANA en Azure (instancias grandes) lo configura SAP HANA en Azure Service Management a través de directrices recomendadas de SAP, como se documenta en las notas del producto [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (Requisitos de almacenamiento de SAP HANA). Los tamaños aproximados de los diferentes volúmenes con las diferentes SKU de grandes instancias de HANA se documentan en [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Las convenciones de nomenclatura de los volúmenes de almacenamiento se muestran en la tabla siguiente:

| Uso del almacenamiento | Nombre de montaje | Nombre del volumen | 
| --- | --- | ---|
| Datos de HANA | /hana/data/SID/mnt0000<m> | Almacenamiento IP: /hana_data_SID_mnt00001_tenant_vol |
| Registro HANA | /hana/log/SID/mnt0000<m> | Almacenamiento IP: /hana_log_SID_mnt00001_tenant_vol |
| Copia de seguridad del registro de HANA | /hana/log/backups | Almacenamiento IP: /hana_log_backups_SID_mnt00001_tenant_vol |
| HANA compartido | /hana/shared/SID | Almacenamiento IP: IP:/hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/sap/SID | Almacenamiento IP: /hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Donde "SID" = el identificador de sistema de la instancia de HANA 

Y "tenant" = una enumeración interna de operaciones al implementar un inquilino.

Como puede ver, HANA compartido y usr/sap están compartiendo el mismo volumen. La nomenclatura de los puntos de montaje incluye el identificador de sistema de las instancias de HANA así como el número de montaje. En las implementaciones de escalado vertical solo hay un montaje, como mnt00001. Al mismo tiempo, en la implementación de escalado horizontal puede que vea tantos montajes como nodos de trabajo y principales tiene. Para el entorno de escalado horizontal, los datos, el registro y los volúmenes de copia de seguridad del registro se comparten y se adjuntan a cada nodo en la configuración de escalado horizontal. Para las configuraciones que ejecutan varias instancias de SAP, se crea un conjunto diferente de volúmenes y se adjunta a la unidad de instancia grande de HANA. Consulte los [escenarios admitidos por HLI](hana-supported-scenario.md) para información detallada sobre la distribución de almacenamiento en su caso.

Según vaya leyendo el documento y buscando una unidad de instancia grande de HANA, se dará cuenta de que las unidades vienen con un volumen de disco bastante amplio para HANA/data y que tenemos un volumen HANA/log/backup. El motivo por el que el tamaño de HANA/data es tan grande es que las instantáneas de almacenamiento que le ofrecemos como cliente usan el mismo volumen de disco. Esto significa que cuantas más instantáneas de almacenamiento realice, más espacio consumen las instantáneas en los volúmenes de almacenamiento asignados. HANA/log/backup no se considera el volumen en el que colocar las copias de seguridad de base de datos. Su tamaño se ajusta para usarlo como volumen de copia de seguridad para las copias de seguridad del registro de transacciones de HANA. Vea los detalles en [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Además del almacenamiento proporcionado, puede adquirir capacidad de almacenamiento adicional en incrementos de 1 TB. Este almacenamiento adicional se puede agregar como nuevos volúmenes a instancias grandes de HANA.

Durante la incorporación de SAP HANA en Azure Service Management, el cliente especifica un identificador de usuario (UID) e Id. de grupo (GID) para el usuario sidadm y el grupo sapsys (p. ej.: 1000,500). Es necesario que durante la instalación del sistema SAP HANA se usen estos mismos valores. Cuando quiera implementar varias instancias de HANA en una unidad, obtendrá varios conjuntos de volúmenes (un conjunto por cada instancia). Como resultado, durante la implementación tendrá que definir:

- El SID de las distintas instancias de HANA (del que se deriva sidadm).
- Tamaños de memoria de las distintas instancias de HANA. Dado que el tamaño de memoria por instancia define el tamaño de los volúmenes en cada conjunto de volúmenes individuales.

En función de las recomendaciones del proveedor de almacenamiento se configuran las siguientes opciones de montaje para todos los volúmenes montados (se excluyen los LUN de inicio):

- nfs    rw, vers=4, hard, timeo=600, rsize=1048576, wsize=1048576, intr, noatime, lock 0 0

Estos puntos de montaje se configuran en /etc/fstab como se muestra en los gráficos siguientes:

![fstab de volúmenes montados en la unidad de instancia grande de HANA](./media/hana-installation/image1_fstab.PNG)

La salida del comando df -h en una unidad de instancia grande de HANA S72m tendría este aspecto:

![fstab de volúmenes montados en la unidad de instancia grande de HANA](./media/hana-installation/image2_df_output.PNG)


El controlador de almacenamiento y los nodos de las marcas de instancia grandes se sincronizan con los servidores NTP. Mediante la sincronización de SAP HANA en unidades de Azure (instancias grandes) y Azure Virtual Machines con un servidor NTP, no debería haber un desfase de tiempo significativo entre la infraestructura y las unidades de proceso de Azure o las marcas de instancias grandes.

Con el fin de optimizar SAP HANA para el almacenamiento subyacente, también debe establecer los parámetros de configuración de SAP HANA siguientes:

- max_parallel_io_requests 128
- async_read_submit on
- async_write_submit_active on
- async_write_submit_blocks all
 
En las versiones de SAP HANA 1.0 hasta SPS12, estos parámetros se pueden establecer durante la instalación de la base de datos de SAP HANA, tal y como se describe en la [nota de SAP n.º 2267798: Configuración de la base de datos de SAP HANA](https://launchpad.support.sap.com/#/notes/2267798)

También puede configurar los parámetros después de la instalación de la base de datos de SAP HANA mediante el marco de hdbparam. 

Con SAP HANA 2.0, el marco de hdbparam está en desuso. Como resultado, los parámetros deben establecerse mediante comandos SQL. Para más información, consulte la [nota de SAP n.º 2399079: Eliminación de hdbparam en HANA 2](https://launchpad.support.sap.com/#/notes/2399079).

Consulte los [escenarios admitidos por HLI](hana-supported-scenario.md) para conocer la distribución de almacenamiento correspondiente a su arquitectura.


**Pasos siguientes**

- Consulte [Instalación de HANA en HLI](hana-example-installation.md).










































 







 




