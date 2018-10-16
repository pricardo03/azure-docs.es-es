---
title: Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP | Microsoft Docs
description: Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/06/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e46503f8dc97f58db1cd5acfd2122e2895fb15b0
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162315"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[planning-guide]:planning-guide.md 

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Esta guía forma parte de la documentación sobre la implementación del software de SAP en Microsoft Azure. Antes de leer esta guía, consulte la [Guía de planeamiento e implementación][planning-guide]. En este documento se describen los aspectos de implementación genéricos de los sistemas DBMS relacionados con SAP en Microsoft Azure Virtual Machines (VM) mediante las funcionalidades de infraestructura como servicio (IaaS) de Azure.

El documento complementa la Documentación de instalación de SAP y las Notas de SAP, que representan los principales recursos para las instalaciones e implementaciones de software de SAP en las plataformas proporcionadas.

En este documento, se presentan las consideraciones sobre la ejecución de sistemas DBMS relacionados con SAP en máquinas virtuales de Azure. En este capítulo, se hacen algunas referencias a sistemas DBMS específicos. En su lugar, en este artículo se abordan los sistemas DBMS específicos, después de este documento.

## <a name="definitions-upfront"></a>Lista de definiciones
A lo largo del documento se usan los términos siguientes:

* IaaS: infraestructura como servicio.
* PaaS: plataforma como servicio.
* SaaS: software como servicio.
* Componente de SAP: una aplicación de SAP individual, como ECC, BW, Solution Manager o EP.  Los componentes de SAP pueden basarse en tecnologías tradicionales, como ABAP o Java, o en una aplicación no basada en NetWeaver, como Business Objects.
* Entorno de SAP: uno o varios componentes de SAP agrupados lógicamente para desempeñar una función empresarial, como desarrollo, control de calidad, aprendizaje, recuperación ante desastres o producción.
* Infraestructura de SAP: este término hace referencia a todos los recursos de SAP de la infraestructura de TI de un cliente. La infraestructura de SAP incluye todos los entornos, tanto los que son de producción como los que no.
* Sistema SAP: la combinación de la capa de DBMS y la capa de aplicación de, por ejemplo, un sistema de desarrollo SAP ERP, un sistema de prueba SAP BW, un sistema de producción SAP CRM, etc. En las implementaciones de Azure no se admite la división de estas dos capas entre la infraestructura local y de Azure. Esto significa que un sistema SAP debe implementarse de forma local o en Azure, pero no en ambos. Sin embargo, los diferentes sistemas de un entorno de SAP pueden implementarse en Azure o de forma local. Por ejemplo, pueden implementarse sistemas de pruebas y de desarrollo de SAP CRM en Azure, a la vez que se implementa el sistema de producción de forma local.
* Entre locales: describe un escenario donde se implementan máquinas virtuales en una suscripción de Azure con conexión de sitio a sitio, entre varios sitios o de ExpressRoute entre los centros de datos locales y Azure. En la documentación habitual de Azure, este tipo de implementaciones se denominan "escenarios entre locales". El motivo de la conexión es ampliar los dominios locales, Active Directory local y DNS local a Azure. La infraestructura local se extiende a los recursos de Azure de la suscripción. Con esta extensión, las máquinas virtuales pueden formar parte del dominio local. Los usuarios del dominio local pueden tener acceso a los servidores y ejecutar servicios en esas máquinas virtuales (por ejemplo, servicios de DBMS). Es posible la comunicación y resolución de nombres entre máquinas virtuales implementadas de forma local y en Azure. Este es el escenario más habitual para implementar recursos de SAP en Azure. Para obtener más información, vea [Planeamiento y diseño de VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> En sistemas de producción SAP, se admiten implementaciones entre locales de sistemas SAP donde Azure Virtual Machines que ejecuten sistemas SAP pertenezcan a un dominio local. Las configuraciones entre locales se admiten para la implementación completa o parcial de infraestructuras de SAP en Azure. Incluso la ejecución de una infraestructura completa de SAP en Azure requiere que esas máquinas virtuales formen parte del dominio local y AD/LDAP. En versiones anteriores de la documentación se mencionaban escenarios de TI híbridos, donde el término *híbrido* implica una conectividad entre locales de la infraestructura local y Azure. En este caso, *híbrido* también significa que las máquinas virtuales de Azure forman parte de Active Directory local.
> 
> 

Algunos documentos de Microsoft describen escenarios entre locales de un modo ligeramente distinto, en especial en configuraciones de DBMS de alta disponibilidad. En el caso de los documentos relacionados con SAP, el escenario entre locales se reduce a una conectividad de sitio a sitio o [ExpressRoute](https://azure.microsoft.com/services/expressroute/), y al hecho de que la infraestructura de SAP se distribuye entre medios locales y Azure.

## <a name="resources"></a>Recursos
Hay varios artículos publicados sobre la carga de trabajo de SAP en Azure.  Se recomienda empezar por [Introducción a SAP en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) y después elegir el área de interés.

Las notas de SAP siguientes están relacionadas con SAP en Azure con respecto al área que se describe en este documento:

| Número de nota | Título |
| --- | --- |
| [1928533] |SAP Applications on Azure: Supported Products and Azure VM types (Aplicaciones de SAP en Azure: tipos de máquina virtual de Azure y productos compatibles) |
| [2015553] |SAP on Microsoft Azure: Support Prerequisites (SAP en Microsoft Azure: requisitos previos de compatibilidad) |
| [1999351] |Troubleshooting Enhanced Azure Monitoring for SAP (Solución de problemas de la supervisión mejorada de Azure para SAP) |
| [2178632] |Key Monitoring Metrics for SAP on Microsoft Azure (Métricas de supervisión clave para SAP en Microsoft Azure) |
| [1409604] |Virtualization on Windows: Enhanced Monitoring (Virtualización en Windows: supervisión mejorada) |
| [2191498] |SAP on Linux with Azure: Enhanced Monitoring (SAP en Linux con Azure: supervisión mejorada) |
| [2039619] |SAP Applications on Microsoft Azure using the Oracle Database: Supported Products and Versions (Aplicaciones de SAP en Microsoft Azure con Base de datos de Oracle: versiones y productos compatibles) |
| [2233094] |DB6: SAP Applications on Azure Using IBM DB2 for Linux, UNIX, and Windows - Additional Information (DB6: aplicaciones de SAP en Azure con IBM DB2 para Linux, UNIX y Windows: información adicional) |
| [2243692] |Linux on Microsoft Azure (IaaS) VM: SAP license issues (Linux y máquinas virtuales de Microsoft Azure (IaaS): problemas de licencia de SAP) |
| [1984787] |SUSE LINUX Enterprise Server 12: Installation notes (SUSE Linux Enterprise Server 12: notas de instalación) |
| [2002167] |Red Hat Enterprise Linux 7.x: Installation and Upgrade (Red Hat Enterprise Linux 7.x: instalación y actualización) |
| [2069760] |Instalación y actualización de SAP en Oracle Linux 7.x |
| [1597355] |Recomendación de espacio de intercambio para Linux |
| [2171857] |Oracle Database 12c - compatibilidad con sistema de archivos en Linux |
| [1114181] |Oracle Database 11g - compatibilidad con sistema de archivos en Linux |


Consulte también la [wiki de SCN](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes), que contiene todas las notas de SAP para Linux.

Debe tener conocimientos prácticos sobre la arquitectura de Microsoft Azure y cómo se implementan y utilizan Microsoft Azure Virtual Machines. Puede encontrar más información en la [documentación de Azure](https://docs.microsoft.com/azure/).

Al analizar IaaS, en general, el proceso de instalación y configuración de DBMS, Windows y Linux es prácticamente el mismo en cualquier máquina virtual o máquina sin sistema operativo que se instalaría en un entorno local. Pero hay algunas decisiones de implementación de administración de sistemas y arquitecturas que difieren cuando se usa IaaS de Azure. El objetivo de este documento consiste en explicar las diferencias de administración de sistemas y arquitecturas específicas que se deben tener en cuenta a la hora de usar IaaS de Azure.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Estructura de almacenamiento de una máquina virtual para las implementaciones de RDBMS
Para seguir adecuadamente este capítulo, hay que comprender la información de [este][deployment-guide-3] capítulo de la [Guía de implementación][deployment-guide]. Antes de leer este capítulo, debe conocer la serie de máquinas virtuales y sus diferencias, así como las diferencias entre Azure Standard y [Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).

En términos de Azure Storage para máquinas virtuales de Azure, debe estar familiarizado con los artículos siguientes:

- [Acerca del almacenamiento de discos para máquinas virtuales Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Acerca del almacenamiento de discos para VM de Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds) 

En una configuración básica, normalmente se recomienda una estructura de implementación en la que el sistema operativo, el DBMS y los archivos binarios finales de SAP sean independientes de los archivos de base de datos. Por tanto, se recomienda que los sistemas SAP que se ejecutan en Azure Virtual Machines tengan instalado el disco duro virtual base (o disco) con el sistema operativo, los archivos ejecutables del sistema de administración de bases de datos y los archivos ejecutables de SAP. Los archivos de datos y de registro de DBMS se almacenan en Azure Standard Storage o Azure Premium Storage, y se asocian como discos lógicos a la máquina virtual de imagen del sistema operativo de Azure original. Especialmente en las implementaciones de Linux, puede haber distintas recomendaciones documentadas. En especial en relación a SAP HANA.  

Al planear la distribución de los discos, tendrá que encontrar el equilibrio óptimo entre los elementos siguientes:

* El número de archivos de datos
* El número de discos que contienen los archivos.
* Las cuotas de IOPS de un solo disco.
* El rendimiento por disco.
* El número de discos de datos adicionales posibles por tamaño de máquina virtual.
* El rendimiento de almacenamiento global que puede proporcionar una máquina virtual
* La latencia que pueden proporcionar los diferentes tipos de Azure Storage.
* SLA de máquina virtual.

Azure impone una cuota de IOPS por disco de datos. Estas cuotas son diferentes para los discos hospedados en Azure Storage Estándar y Azure Premium Storage. La latencia de E/S también es diferente entre los dos tipos de almacenamiento.  Premium Storage ofrece una mejor latencia de E/S. Cada uno de los distintos tipos de máquina virtual tiene un número limitado de discos de datos que se pueden conectar. Otra restricción consiste en que solo determinados tipos de máquinas virtuales pueden utilizar Azure Premium Storage. Como resultado, la elección de un tipo de máquina virtual determinado podría no estar basada exclusivamente en los requisitos de CPU y memoria, sino también en los de IOPS, latencia y rendimiento de disco que normalmente se escalan con el número de discos o el tipo de discos de Premium Storage. El tamaño de un disco también podría venir determinado por el número de IOPS y el rendimiento que se haya de obtener en cada disco, especialmente con Premium Storage.

> [!NOTE]
> Para las implementaciones de DBMS, se recomienda encarecidamente el uso de Premium Storage para los archivos de datos, registro de transacciones o fase de puesta al día. Por tanto, no importa si se quieren implementar sistemas de producción o que no sean de producción.

> [!NOTE]
> Para poder beneficiarse del [Contrato de nivel de servicio para Máquinas virtuales único](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) de Azure, todos los discos conectados deben ser del tipo Azure Premium Storage, incluido el disco duro virtual base.
>

La colocación de los archivos de base de datos y de registro o fase de puesta al día, y el tipo de Azure Storage que se use, se debe definir en función de los requisitos de IOPS, latencia y rendimiento. Con el fin de disponer de IOPS suficiente, es posible que tenga que usar varios discos o un disco de Premium Storage de mayor tamaño. En el caso de usar varios discos, crearía una sección de software entre los discos, con los archivos de datos o los de registro y fase de puesta al día. En esos casos, los contratos de nivel de servicio de IOPS y rendimiento de disco de los discos de Premium Storage subyacentes o la cantidad máxima de IOPS que se puede alcanzar de los discos de Azure Standard Storage son acumulativos para el espacio seccionado resultante. 

Como ya se ha indicado, si el requisito de IOPS es superior a lo que un solo disco duro virtual puede proporcionar, debe equilibrar el número de operaciones de IOPS necesarias para los archivos de base de datos entre varios discos duros virtuales. La forma más sencilla de distribuir la carga de IOPS entre los discos consiste en crear una sección de software por los diferentes discos. Después, coloque una serie de archivos de datos del DBMS de SAP en los LUN extraídos de la sección de software. El número de discos de la sección está controlado por las demandas de IOPS, rendimiento y volumen. 


- - -
> ![Windows][Logo_Windows] Windows
> 
> Se recomienda usar espacios de almacenamiento de Windows para crear esos espacios seccionados entre varios discos duros virtuales de Azure. Cómo mínimo, se recomienda usar Windows Server 2012 R2 o Windows Server 2016.
> 
> ![Linux][Logo_Linux] Linux
> 
> Para crear un software RAID en Linux, solo se admiten MDADM y LVM (Logical Volume Manager). Para más información, consulte los siguientes artículos:
> 
> - [Configuración del software RAID en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) con MDADM.
> - [Configuración del LVM en una máquina virtual Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) con LVM.
> 
> 

- - -
 
> [!NOTE]
> Dado que Azure Storage mantiene tres imágenes de los discos duros virtuales, no tiene sentido para configurar una redundancia al seccionar. Solo tiene que configurar las secciones, para que las operaciones de E/S se distribuyan entre los diferentes discos duros virtuales.
>

### <a name="managed-or-non-managed-disks"></a>Discos administrados o no administrados
Las cuentas de Azure Storage se usan con fines administrativos y presentan algunas limitaciones. Las limitaciones son diferentes entre cuentas de Azure Standard Storage y Azure Premium Storage. Las funcionalidades y limitaciones exactas se enumeran en el artículo [Objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

Para Azure Standard Storage, es importante recordar que hay un límite en IOPS por cuenta de almacenamiento (en la fila que contiene **Tasa de solicitud total** en el artículo [Objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)). Además, hay un límite inicial del número de cuentas de almacenamiento por suscripción de Azure. Por tanto, debe equilibrar los discos duros virtuales para las infraestructuras de SAP más grandes entre otras cuentas de almacenamiento para evitar alcanzar los límites de estas cuentas de almacenamiento. Un trabajo tedioso cuando se trata de varios cientos de máquinas virtuales con más de mil discos duros virtuales. 

Puesto que no se recomienda usar Azure Standard Storage para las implementaciones de DBMS junto con la carga de trabajo de SAP, las referencias y recomendaciones para Azure Standard Storage se limitan a este breve [artículo](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Para evitar el trabajo administrativo de la planeación e implementación de discos duros virtuales entre varias cuentas de Azure Storage, Microsoft presentó lo que se denomina [Managed Disks](https://azure.microsoft.com/services/managed-disks/) en 2017. Managed Disks están disponibles para Azure Standard Storage, así como para Azure Premium Storage. Las principales ventajas de Managed Disks en comparación con los discos no administrados son las siguientes:

- Para Managed Disks, Azure distribuye de forma automática los distintos discos duros virtuales entre las cuentas de almacenamiento en tiempo de implementación y así evita alcanzar los límites de una cuenta de Azure Storage en términos de volumen de datos, rendimiento de E/S e IOPS.
- Al usar Managed Disks, Azure Storage respeta los conceptos de los conjuntos de disponibilidad de Azure y, de ese modo, implementa el disco duro virtual base y el disco conectado de una máquina virtual en otros dominios de error y de actualización si la máquina virtual forma parte de un conjunto de disponibilidad de Azure.


> [!IMPORTANT]
> Dadas las ventajas de Azure Managed Disks, se recomienda encarecidamente usar Azure Managed Disks para las implementaciones de DBMS y las de SAP en general.
>

Para convertir de discos no administrados a discos administrados, vea estos artículos:

- [Conversión de una máquina virtual Windows con discos no administrados a discos administrados](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Conversión de una máquina virtual Linux con discos no administrados a discos administrados](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Almacenamiento en caché de máquinas virtuales y discos de datos
Al montar los discos en las máquinas virtuales, se puede elegir si almacenar en caché el tráfico de E/S entre la máquina virtual y los discos ubicados en Azure Storage. Azure Standard y Premium Storage utilizan dos tecnologías diferentes para este tipo de almacenamiento en caché. 

En las recomendaciones siguientes se asumen estas características de E/S para DBMS estándar:

- Principalmente es una carga de trabajo de lectura en los archivos de datos de una base de datos. Estas operaciones de lectura son críticas para el rendimiento del sistema DBMS.
- La escritura en los archivos de datos se produce en ráfagas en función de puntos de comprobación o un flujo constante. Pero según el promedio diario, las operaciones de escritura son menos que las de lectura. Al contrario que las operaciones de lectura de archivos de datos, estas operaciones de escritura son asincrónicas y no bloquean transacciones de usuario.
- Apenas hay lecturas del registro de transacciones o de los archivos de fase de puesta al día. Las excepciones son las operaciones de E/S de gran tamaño al realizar copias de seguridad del registro de transacciones. 
- La carga principal en los archivos de registro de transacciones o de fase de puesta al día es una operación de escritura. Según la naturaleza de la carga de trabajo, puede tener operaciones de E/S de solo 4 KB o llegar a superar 1 MB.
- Todas las operaciones de escritura se deben conservar en el disco de forma confiable.

En Azure Standard Storage, estos son los tipos posibles de caché:

* None
* Lectura
* Lectura/Escritura

Para obtener un rendimiento coherente y determinista, debe establecer el almacenamiento en caché de Azure Standard Storage en todos los discos que contienen **archivos de datos, archivos de registro o fase de puesta al día, y espacio de tablas relacionados con DBMS en "NINGUNO"**. Se puede conservar el valor de almacenamiento en caché predeterminado del disco duro virtual base.

Para Azure Premium Storage, existen las opciones de almacenamiento en caché siguientes:

* None
* Lectura 
* Lectura/escritura 
* Ninguno + Acelerador de escritura (solo para máquinas virtuales de la serie M de Azure)
* Lectura + Acelerador de escritura (solo para máquinas virtuales de la serie M de Azure)

Para Azure Premium Storage, se recomienda aprovechar el **almacenamiento en caché de lectura para archivos de datos** de la base de datos de SAP y no elegir **almacenamiento en caché para los discos de archivos de registro**.

Para las implementaciones de la serie M, se recomienda usar el Acelerador de escritura de Azure para la implementación de DBMS. Para obtener más información, las restricciones y la implementación del Acelerador de escritura de Azure, vea el documento [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator). 


### <a name="azure-non-persistent-disks"></a>Discos de Azure no persistentes
Las máquinas virtuales de Azure ofrecen discos no persistentes después de implementar una máquina virtual. En el caso de un reinicio de máquina virtual, se borrará todo el contenido de esas unidades. Por tanto, es evidente que los archivos de datos y los archivos de registro y fase de puesta al día de las bases de datos no se deben colocar bajo ninguna circunstancia en esas unidades de disco no persistentes. Es posible que haya excepciones para algunas de las bases de datos, donde estas unidades de disco no persistentes podrían ser adecuadas para tempdb y los espacios de tablas temporales. Pero evite usar esas unidades de disco para las máquinas virtuales de la serie A, ya que esas unidades no persistentes tienen un límite de rendimiento con esa familia de máquinas virtuales. Para obtener más información, lea el artículo [Understanding the temporary drive on Windows Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) (Descripción de la unidad temporal en Microsoft Azure Virtual Machines).

- - -
> ![Windows][Logo_Windows] Windows
> 
> La unidad D:\ de una máquina virtual de Azure es una unidad no persistente respaldada por algunos discos locales del nodo de proceso de Azure. Como no es persistente, significa que los cambios realizados en el contenido de la unidad D:\ se perderán cuando se reinicie la máquina virtual. Por "cambios", se refiere a los archivos guardados, los directorios creados, las aplicaciones instaladas, etc.
> 
> ![Linux][Logo_Linux] Linux
> 
> Las máquinas virtuales de Linux de Azure montan automáticamente una unidad en /mnt/resource, que es una unidad no persistente respaldada por discos locales del nodo de proceso de Azure. Como no es persistente, significa que los cambios realizados en el contenido de la unidad /mnt/resource se perderán cuando se reinicie la máquina virtual. Por cambios, se refiere a los archivos guardados, los directorios creados, las aplicaciones instaladas, etc.
> 
> 

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Resistencia de Microsoft Azure Storage
Microsoft Azure Storage almacena el disco duro virtual base (con SO) y los discos conectados o los blobs en, al menos, tres nodos de almacenamiento independientes. Este hecho se denomina Almacenamiento con redundancia local (LRS). LRS es el valor predeterminado para todos los tipos de almacenamiento en Azure. 

Existen varios métodos de redundancia más, que se describen en el artículo [Replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>A partir de Azure Premium Storage, que es el tipo de almacenamiento recomendado para máquinas virtuales de DBMS y discos en los que se almacenan los archivos de base de datos y de registro o fase de puesta al día, el único método disponible es LRS. Como resultado, deberá configurar métodos de la base de datos, como SQL Server Always On, Oracle Data Guard o replicación del sistema HANA para habilitar la replicación de datos de base de datos en otra región de Azure o en otra zona de disponibilidad de Azure.


> [!NOTE]
> Para las implementaciones de DBMS, no se recomienda el uso de almacenamiento con redundancia geográfica disponible con Azure Standard Storage, ya que tiene consecuencias graves de rendimiento y no respeta el orden de escritura en los diferentes discos duros virtuales conectados a una máquina virtual. El hecho de no respetar el orden de escritura entre los diferentes discos duros virtuales puede provocar que las bases de datos acaben siendo incoherentes en el lado de destino de replicación si los archivos de base de datos y de registro o fase de puesta al día están repartidos entre varios discos duros virtuales (como suele ser el caso) en el lado de la máquina virtual de origen.

 

## <a name="vm-node-resiliency"></a>Resistencia de nodos de máquina virtual
La plataforma Azure ofrece varios SLA diferentes para las máquinas virtuales. Los detalles exactos se pueden encontrar en la versión más reciente de [Contrato de nivel de servicio para Máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Como la capa de DBMS normalmente es una parte de disponibilidad fundamental de un sistema SAP, debe familiarizarse con los conceptos de conjuntos de disponibilidad, zonas de disponibilidad y eventos de mantenimiento. En los artículos [Administración de la disponibilidad de las máquinas virtuales Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) y [Administración de la disponibilidad de las máquinas virtuales Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability) se describen todos estos conceptos.  

La recomendación mínima para escenarios DBMS de producción con la carga de trabajo de SAP es:

- Implementar dos máquinas virtuales en un conjunto de disponibilidad independiente en la misma región de Azure.
- Estas dos máquinas virtuales se ejecutarían en la misma red virtual de Azure y tendrían adaptadores de red conectados desde las mismas subredes.
- Use los métodos de la base de datos para mantener una espera activa con la segunda máquina virtual. Los métodos pueden ser SQL Server Always On, Oracle Data Guard o Replicación del sistema de HANA.

Además, puede implementar una tercera máquina virtual en otra región de Azure y usar los mismos métodos de base de datos para proporcionar una réplica asincrónica en otra región de Azure.

En este [tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) se muestra cómo configurar los conjuntos de disponibilidad de Azure.



## <a name="azure-network-considerations"></a>Consideraciones sobre la red de Azure 
En las implementaciones de SAP a gran escala, se recomienda usar el plano técnico de [Centro de datos virtual de Azure](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) para la configuración de redes virtuales y las asignaciones de permisos y roles a las diferentes partes de la organización.

Hay varios procedimientos recomendados, fruto de cientos de implementaciones de clientes:

- Las redes virtuales en las que se implementa la aplicación de SAP no tienen acceso a Internet.
- La máquinas virtuales de base de datos se ejecutan en la misma red virtual que el nivel de aplicación.
- Las máquinas virtuales dentro de la red virtual tienen una asignación estática de la dirección IP privada. Vea el artículo [Tipos de direcciones IP y métodos de asignación en Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) como referencia.
- Las restricciones de enrutamiento a y desde las máquinas virtuales de DBMS **NO** se establecen con firewalls instalados en las máquinas virtuales de DBMS locales. En su lugar, el enrutamiento del tráfico se define mediante [Grupos de seguridad de red (NSG) de Azure](https://docs.microsoft.com/azure/virtual-network/security-overview)
- Con el fin de separar y aislar el tráfico a la máquina virtual de DBMS, asigne otros adaptadores de red a la máquina virtual. Donde cada adaptador de red tiene una dirección IP diferente y se asigna a una otra subred de red virtual lo que, de nuevo, tiene otras reglas de NSG. Tenga en cuenta que la separación o el aislamiento del tráfico de red es simplemente una medida para el enrutamiento y no permite establecer cuotas para el rendimiento de la red.

> [!NOTE]
> Debería asignar direcciones IP estáticas mediante Azure a NIC virtuales individuales. No debería asignar direcciones IP estáticas dentro del sistema operativo invitado a una NIC virtual. Algunos servicios de Azure como el servicio Azure Backup se basan en el hecho de que al menos el vNIC principal está establecido en DHCP y no en direcciones IP estáticas. Consulte también el documento [Solución de problemas de copia de seguridad de máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Si necesita asignar varias direcciones IP estáticas a una máquina virtual, tiene que asignar varias NIC virtuales a una máquina virtual.
>
>

Con dos máquinas virtuales para la implementación de DBMS de producción, dentro de un conjunto de disponibilidad de Azure además de un enrutamiento independiente para la capa de aplicación de SAP y el tráfico de administración y operaciones a las dos máquinas virtuales de DBMS, el diagrama aproximado tendría el siguiente aspecto:

![Diagrama de dos máquinas virtuales en dos subredes](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="azure-load-balancer-for-redirecting-traffic"></a>Equilibrador de carga de Azure para redirigir el tráfico
El uso de direcciones IP virtuales privadas en funcionalidades como SQL Server Always On o Replicación de sistema de HANA requiere la configuración de una instancia de Azure Load Balancer. Azure Load Balancer se habilita través de los puertos de sondeo para determinar el nodo de DBMS activo y enrutar el tráfico exclusivamente a ese nodo de base de datos activo. En caso de la conmutación por error del nodo de base de datos, no hay necesidad de volver a configurar la aplicación de SAP. En su lugar, las arquitecturas de aplicaciones de SAP más comunes se volverán a conectar con la dirección IP virtual privada. Mientras tanto, el equilibrador de carga de Azure reacciona a la conmutación por error del nodo mediante la redirección del tráfico con la dirección IP virtual privada al segundo nodo.

Azure ofrece dos [SKU de equilibrador de carga](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) diferentes. Una SKU de nivel básico y otra estándar. A menos que quiera implementar en varias Azure Availability Zones, la SKU de equilibrador de carga básica es suficiente. 

¿El tráfico entre las máquinas virtuales de DBMS y el nivel de aplicación de SAP siempre se enruta a través del equilibrador de carga de Azure? La respuesta depende de cómo se configure el equilibrador de carga. En este momento, el tráfico entrante a la máquina virtual de DBMS siempre se enrutará a través del equilibrador de carga de Azure. La ruta de tráfico saliente desde la máquina virtual de DBMS a la de la capa de aplicación depende de la configuración del equilibrador de carga de Azure. El equilibrador de carga ofrece una opción de DirectServerReturn. Si se configura esta opción, el tráfico dirigido desde la máquina virtual de DBMS al nivel de aplicación de SAP **NO** se enrutará a través del equilibrador de carga de Azure. En su lugar, irá directamente a la capa de aplicación. Si no se configura DirectServerReturn, el tráfico de retorno se enruta a la capa de aplicación de SAP a través del equilibrador de carga de Azure

Se recomienda configurar DirectServerReturn junto con los equilibradores de carga de Azure colocados entre la capa de aplicación de SAP y la capa de DBMS para reducir la latencia de red entre las dos capas

[En este artículo](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener) se publica un ejemplo de cómo establecer este tipo de configuración en torno a SQL Server Always On.

Si decide usar las plantillas JSON publicadas en GitHub como referencia para las implementaciones de infraestructura SAP en Azure, debe estudiar esta [plantilla para un sistema SAP de tres niveles](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). En esta plantilla, también puede estudiar la configuración correcta del equilibrador de carga de Azure.

### <a name="azure-accelerated-networking"></a>Redes aceleradas de Azure
Para reducir aún más la latencia de red entre las máquinas virtuales de Azure, se recomienda elegir la opción de [Redes aceleradas de Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) al implementar máquinas virtuales de Azure para la carga de trabajo de SAP. Especialmente para la capa de aplicación de SAP y la capa de DBMS de SAP. 

> [!NOTE]
> No todos los tipos de máquinas virtuales son compatibles con las redes aceleradas. En el artículo al que se hace referencia se enumeran los tipos de máquina virtual que admiten Redes aceleradas. 
>  

- - -
> ![Windows][Logo_Windows] Windows
> 
> Para Windows, vea el artículo [Creación de una máquina virtual Windows con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) para entender los conceptos y el procedimiento de implementación de máquinas virtuales con Redes aceleradas.
> 
> ![Linux][Logo_Linux] Linux
> 
> Para Linux, lea el artículo [Creación de una máquina virtual Linux con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) con el fin de obtener detalles para la distribución de Linux. 
> 
> 

- - -

> [!NOTE]
> En el caso de SUSE, Red Hat y Oracle Linux, Redes aceleradas se admite con las versiones recientes. Las versiones anteriores como SLES 12 SP2 o RHEL 7.2 no son compatibles con Redes aceleradas de Azure. 
>  


## <a name="deployment-of-host-monitoring"></a>Implementación de funcionalidades de supervisión de hosts
Para el uso en producción de las aplicaciones de SAP en Azure Virtual Machines, SAP requiere que se obtengan datos de supervisión de los hosts físicos que se ejecutan en Azure Virtual Machines. Se requiere un nivel de revisión específico del agente de host de SAP que permita esta funcionalidad en SAPOSCOL y el agente de host de SAP. El nivel de revisión exacto se menciona en la nota de SAP [1409604].

Para obtener más información sobre la implementación de componentes que proporcionen datos de host a SAPOSCOL y el agente de host de SAP, así como sobre la administración del ciclo de vida de los componentes, consulte la [Guía de implementación][deployment-guide].


## <a name="next-steps"></a>Pasos siguientes
Para obtener documentación sobre un DBMS concreto, vea estos artículos:

- [Implementación de DBMS de Azure Virtual Machines de SQL Server para una carga de trabajo de SAP](dbms_guide_sqlserver.md)
- [Implementación de DBMS de Azure Virtual Machines de Oracle para una carga de trabajo de SAP](dbms_guide_oracle.md)
- [Implementación de DBMS de Azure Virtual Machines de IBM DB2 para una carga de trabajo de SAP](dbms_guide_ibm.md)
- [Implementación de DBMS de Azure Virtual Machines de SAP ASE para una carga de trabajo de SAP](dbms_guide_sapase.md)
- [Implementación de SAP maxDB, Live Cache y del servidor de contenido en Azure](dbms_guide_maxdb.md)
- [SAP HANA en la guía de operaciones de Azure](hana-vm-operations.md)
- [Alta disponibilidad de SAP HANA para máquinas virtuales de Azure](sap-hana-availability-overview.md)
- [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md)

