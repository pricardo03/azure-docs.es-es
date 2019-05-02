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
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b19c0fd8af2792a4ffb877e5c6a7fc6b3f94511
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60836131"
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

Esta guía forma parte de la documentación sobre cómo implementar y distribuir software de SAP en Microsoft Azure. Antes de leer esta guía, lea el [Guía de planeación e implementación][planning-guide]. Este documento cubre los aspectos de la implementación genérica de los sistemas DBMS relacionados con SAP en Microsoft Azure virtual machines (VM) mediante el uso de la infraestructura de Azure como las capacidades de un servicio (IaaS).

El documento complementa la documentación de instalación de SAP y las notas de SAP, que representan los principales recursos para las instalaciones e implementaciones de software de SAP en las plataformas proporcionadas.

En este documento, se presentan las consideraciones sobre la ejecución de sistemas DBMS relacionados con SAP en máquinas virtuales de Azure. En este capítulo, se hacen algunas referencias a sistemas DBMS específicos. En su lugar, se administran los sistemas DBMS específicos dentro de este documento, después de este documento.

## <a name="definitions"></a>Definiciones
En todo el documento, se utilizan estos términos:

* **IaaS**: Infraestructura como servicio.
* **PaaS**: Plataforma como servicio.
* **SaaS**: Software como servicio.
* **Componente de SAP**: Una aplicación de SAP individual, como ERP Central Component (ECC), Business Warehouse (BW), Solution Manager o Enterprise Portal (EP). Los componentes de SAP pueden basarse en tecnologías ABAP o Java tradicionales o en una aplicación no basada en NetWeaver, como Business Objects.
* **Entorno de SAP**: Uno o más componentes SAP agrupan lógicamente para desempeñar una función empresarial, como desarrollo, control de calidad, aprendizaje, recuperación ante desastres o producción.
* **Infraestructura de SAP**: este término hace referencia a todos los recursos de SAP de la infraestructura de TI de un cliente. La infraestructura de SAP incluye todos los entornos de producción y no sea de producción.
* **Sistema SAP**: La combinación de una capa DBMS y una aplicación de, por ejemplo, un sistema de desarrollo SAP ERP, de una instancia de SAP Business Warehouse pruebas del sistema o un sistema de producción SAP CRM. En las implementaciones de Azure no se admite la división de estas dos capas entre local y Azure. Como resultado, un sistema SAP está implementado en local o de TI implementadas en Azure. Puede implementar los diferentes sistemas de un entorno de SAP en Azure o en el entorno local. Por ejemplo, podría implementar el desarrollo de SAP CRM y los sistemas de prueba en Azure pero implementar SAP CRM producción del sistema local.
* **Entre locales**: Describe un escenario donde las máquinas virtuales se implementan en una suscripción de Azure que tenga el sitio a sitio, multisitio o centros de Azure expressroute entre los datos locales y Azure. En la documentación habitual de Azure, este tipo de implementaciones se denominan "escenarios entre locales". 

    El motivo de la conexión es ampliar los dominios locales, Active Directory local y DNS local a Azure. La infraestructura local se extiende a los recursos de Azure de la suscripción. Con esta extensión, las máquinas virtuales pueden formar parte del dominio local. Los usuarios del dominio del dominio local pueden tener acceso a los servidores y ejecutar servicios en esas máquinas virtuales, como servicios de DBMS. Es posible la comunicación y resolución de nombres entre máquinas virtuales implementadas de forma local y en Azure. Este escenario es el escenario más común en uso para implementar activos de SAP en Azure. Para obtener más información, vea [Planeamiento y diseño de VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Implementaciones entre locales de sistemas SAP donde Azure virtual machines que ejecutan sistemas SAP son miembros de un dominio local y se admiten para los sistemas SAP de producción. Las configuraciones entre locales se admiten para la implementación completa o parcial de infraestructuras de SAP en Azure. Incluso al ejecutar la infraestructura completa de SAP en Azure requiere que esas máquinas virtuales formen parte de un dominio local y Active Directory y LDAP. 
>
> En versiones anteriores de la documentación, se mencionaron los escenarios de TI híbrida. El término *híbrida* se basa en el hecho de que hay una conectividad entre locales entre local y Azure. En este caso, híbrida también significa que las máquinas virtuales de Azure forman parte de Active Directory local.
>
>

Algunos documentos de Microsoft describen escenarios entre locales forma un poco diferente, especialmente para configuraciones de alta disponibilidad DBMS. En el caso de los documentos relacionados con SAP, el escenario entre locales se reduce a sitio a sitio o privada [ExpressRoute](https://azure.microsoft.com/services/expressroute/) conectividad y un entorno de SAP se distribuye entre local y Azure.

## <a name="resources"></a>Recursos
Hay otros artículos disponibles en la carga de trabajo SAP en Azure. Comience con [carga de trabajo SAP en Azure: Introducción a](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) y, a continuación, elija el área de interés.

Las siguientes notas de SAP están relacionadas con SAP en Azure en relación con el área cubierta en este documento.

| Número de nota | Título |
| --- | --- |
| [1928533] |Aplicaciones de SAP en Azure: Supported Products and Azure VM types (Nota de SAP 1928533: Aplicaciones SAP en Azure: productos admitidos y tipos de máquina virtual de Azure) |
| [2015553] |SAP on Microsoft Azure: Support Prerequisites (Nota de soporte técnico 2015553 de SAP en Microsoft Azure: requisitos previos de soporte técnico) |
| [1999351] |Troubleshooting Enhanced Azure Monitoring for SAP (Solución de problemas de la supervisión mejorada de Azure para SAP) |
| [2178632] |Key Monitoring Metrics for SAP on Microsoft Azure (Métricas de supervisión clave para SAP en Microsoft Azure) |
| [1409604] |Virtualization on Windows: Enhanced Monitoring (SAP en Linux con Azure: supervisión mejorada) |
| [2191498] |SAP on Linux with Azure: Enhanced Monitoring (SAP en Linux con Azure: supervisión mejorada) |
| [2039619] |SAP Applications on Microsoft Azure using the Oracle Database: Supported products and versions (Aplicaciones de SAP en Microsoft Azure con Base de datos de Oracle: versiones y productos compatibles) |
| [2233094] |DB6: Aplicaciones de SAP en Azure con IBM DB2 para Linux, UNIX y Windows: Información adicional |
| [2243692] |Linux on Microsoft Azure (IaaS) VM: SAP license issues (Linux y máquinas virtuales de Microsoft Azure (IaaS): problemas de licencia de SAP) |
| [1984787] |SUSE LINUX Enterprise Server 12: Notas de instalación |
| [2002167] |Red Hat Enterprise Linux 7.x: Instalación y actualización |
| [2069760] |Instalación y actualización de SAP en Oracle Linux 7.x |
| [1597355] |Recomendación de espacio de intercambio para Linux |
| [2171857] |Oracle Database 12c: Compatibilidad con sistema de archivos en Linux |
| [1114181] |Oracle Database 11g: Compatibilidad con sistema de archivos en Linux |


Para obtener información sobre todas las notas de SAP para Linux, consulte el [wiki de la Comunidad SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes).

Necesita conocimientos prácticos de arquitectura de Microsoft Azure y cómo se implementan y opera las máquinas virtuales de Microsoft Azure. Para obtener más información, consulte [documentación de Azure](https://docs.microsoft.com/azure/).

En general, la instalación de Windows, Linux y DBMS y la configuración están prácticamente igual que cualquier máquina virtual o máquina sin sistema operativo, instalar de forma local. Hay algunas decisiones de implementación de administración de sistemas y arquitecturas que son diferentes al utilizar IaaS de Azure. Este documento explica las diferencias de administración del sistema para estar preparado para cuando se usa Azure IaaS y arquitecturas específicas.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Estructura de almacenamiento de una máquina virtual para las implementaciones de RDBMS
Para seguir este capítulo, leer y comprender la información presentada en [este capítulo] [ deployment-guide-3] de la [Guía de implementación][deployment-guide]. Debe entender y conocer las distintas Series de máquinas virtuales y las diferencias entre estándar y premium storage antes de leer este capítulo. 

Para obtener información sobre el almacenamiento de Azure para máquinas virtuales de Azure, consulte:

- [Introducción a managed disks para máquinas virtuales de Windows Azure](../../windows/managed-disks-overview.md).
- [Introducción a managed disks para máquinas virtuales Linux](../../linux/managed-disks-overview.md).

En una configuración básica, por lo general se recomienda una estructura de implementación donde el sistema operativo, los archivos binarios finales de SAP y DBMS son independientes de los archivos de base de datos. Se recomienda que los sistemas SAP que se ejecutan en máquinas virtuales de Azure tienen el disco duro virtual base, o el disco, instalado con el sistema operativo, los ejecutables del sistema de administración de base de datos y los ejecutables de SAP. 

Los archivos de datos y registro DBMS se almacenan en almacenamiento estándar o premium storage. Que se almacenen en discos separados y conectados como discos lógicos a la imagen de sistema operativo Azure VM original. Para las implementaciones de Linux, se documentan distintas recomendaciones, especialmente para SAP HANA.

Al planear el diseño del disco, encontrar el mejor equilibrio entre estos elementos:

* El número de archivos de datos
* El número de discos que contienen los archivos.
* Las cuotas de IOPS de un solo disco.
* El rendimiento por disco.
* El número de discos de datos adicionales posibles por tamaño de máquina virtual.
* El rendimiento de almacenamiento global que puede proporcionar una máquina virtual
* La latencia que pueden proporcionar los diferentes tipos de Azure Storage.
* SLA de máquina virtual.

Azure impone una cuota de IOPS por disco de datos. Estas cuotas son diferentes para los discos hospedados en almacenamiento estándar y premium storage. La latencia de E/S también es diferente entre los dos tipos de almacenamiento. Premium storage ofrece mejor latencia de E/S. 

Cada uno de los distintos tipos de máquina virtual tiene un número limitado de discos de datos que puede adjuntar. Otra restricción es que solo determinados tipos de máquina virtual pueden usar premium storage. Normalmente, se decide usar un determinado tipo de máquina virtual según los requisitos de CPU y memoria. También puede considerar la IOPS, latencia y los requisitos de rendimiento de disco que normalmente se escalan con el número de discos o el tipo de discos de premium storage. El número de IOPS y el rendimiento se logra por cada disco podría indicar el tamaño del disco, especialmente con premium storage.

> [!NOTE]
> Para las implementaciones de DBMS, se recomienda el uso de premium storage para los datos y registro de transacciones, o archivos de puesta al día. No importa si desea implementar los sistemas de producción o de no producción.

> [!NOTE]
> Para beneficiarse de Azure único del [único SLA de máquina virtual](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), todos los discos que están asociados deben ser el tipo de almacenamiento premium, que incluye el disco duro virtual base.

> [!NOTE]
> Hospedaje de archivos de base de datos principal, como archivos de registro y datos de bases de datos SAP en hardware de almacenamiento que se encuentra en los centros de datos de otros fabricantes colocada junto a los centros de datos de Azure no se admite. Para cargas de trabajo SAP, se admite solo almacenamiento que se representa como servicio de Azure nativa para los archivos de registro de transacciones y datos de bases de datos SAP.

La colocación de los archivos de base de datos y los archivos de registro y de puesta al día y el tipo de almacenamiento de Azure que usa se define mediante los requisitos de IOPS, latencia y rendimiento. Para tener suficientes IOPS, podría verse obligado a usar varios discos o usar un disco de premium storage mayor. Si usa varios discos, crear una banda de software en todos los discos que contienen los archivos de datos o en el registro y archivos de puesta al día. En tales casos, el número de IOPS y el rendimiento de disco SLA del almacenamiento premium subyacente el IOPS máximo factible de discos de almacenamiento estándar o discos son acumulativos para el conjunto resultante de la franja.

Como ya se ha indicado, si los requisitos de IOPS es superior a lo que puede proporcionar un solo VHD, equilibrar el número de IOPS que son necesarios para los archivos de base de datos a través de un número de discos duros virtuales. Es la manera más fácil de distribuir la carga de IOPS en varios discos crear una banda de software a través de los diferentes discos. A continuación, colocar una cantidad de archivos de datos del DBMS de SAP en los LUN extraídas de la franja de software. El número de discos en la banda está controlado por volumen, demandas de rendimiento de disco y demandas IOPs demandas.


- - -
> ![ Windows][Logo_Windows]  Windows
>
> Se recomienda usar espacios de almacenamiento de Windows para crear conjuntos de bandas en varios VHD de Azure. Utilice al menos Windows Server 2012 R2 o Windows Server 2016.
>
> ![Linux][Logo_Linux] Linux
>
> Se admiten solo MDADM y Administrador de volúmenes lógicos (LVM) para crear un software RAID en Linux. Para más información, consulte:
>
> - [Configuración del software RAID en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) mediante MDADM
> - [Configuración del LVM en una máquina virtual Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) con LVM.
>
>

- - -

> [!NOTE]
> Dado que Azure Storage mantiene tres imágenes de los discos duros virtuales, no tiene sentido para configurar una redundancia al secciona. Sólo debe configurar la creación de bandas para que las operaciones de E/s se distribuyen a través de los diferentes discos duros virtuales.
>

### <a name="managed-or-nonmanaged-disks"></a>Discos administrados o no gestionados
Una cuenta de almacenamiento de Azure es una construcción administrativa y también presentan algunas limitaciones. Limitaciones difieren entre cuentas de almacenamiento estándar y premium storage. Para obtener información sobre las capacidades y limitaciones, consulte [objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

Para el almacenamiento estándar, recuerde que hay un límite en el número de IOPS por cuenta de almacenamiento. Consulte la fila que contiene **velocidad Total de solicitudes** en el artículo [objetivos de escalabilidad y rendimiento de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets). También hay un límite inicial en el número de cuentas de almacenamiento por suscripción de Azure. Equilibrar los discos duros virtuales para el entorno SAP más grande a través de diferentes cuentas de almacenamiento para evitar alcanzar los límites de estas cuentas de almacenamiento. Se trata de un trabajo tedioso cuando usted se refiere a unos cientos de equipos virtuales con discos duros virtuales de más de mil.

Dado que no se recomienda el uso de almacenamiento estándar para las implementaciones de DBMS junto con una carga de trabajo SAP, referencias y recomendaciones para almacenamiento estándar se limitan a este breve [artículo](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Para evitar el trabajo administrativo de planeación e implementación de los discos duros virtuales entre cuentas de almacenamiento de Azure diferentes, Microsoft presentó [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) en 2017. Discos administrados están disponibles para almacenamiento estándar y premium storage. Las principales ventajas de los discos administrados en comparación con discos no gestionados son:

- Para los discos administrados, Azure distribuye los diferentes discos duros virtuales en diferentes cuentas de almacenamiento automáticamente durante la implementación. De esta manera, límites de cuenta de almacenamiento para el volumen de datos, el rendimiento de E/S, y no se alcanza la e/s por segundo.
- Uso de discos administrados, el almacenamiento de Azure respeta los conceptos de conjuntos de disponibilidad de Azure. Si la máquina virtual forma parte de un conjunto de disponibilidad de Azure, el VHD base y el disco conectado de una máquina virtual se implementan en diferentes dominios de error y actualización.


> [!IMPORTANT]
> Dadas las ventajas de Azure Managed Disks, se recomienda usar Azure Managed Disks para las implementaciones de SAP y las implementaciones de DBMS en general.
>

Para convertir en no administrados a discos administrados, consulte:

- [Convertir una máquina virtual de Windows de unmanaged disks a managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks).
- [Convertir una máquina virtual Linux de unmanaged disks a managed disks](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks).


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Almacenamiento en caché de máquinas virtuales y discos de datos
Si se monta discos en máquinas virtuales, puede elegir si el tráfico de E/S entre la máquina virtual y los discos ubicados en almacenamiento de Azure se almacena en caché. Almacenamiento estándar y premium se utilizan dos tecnologías diferentes para este tipo de caché.

Las siguientes recomendaciones suponen estas características de E/S para DBMS estándar:

- Es principalmente una carga de trabajo lectura frente a los archivos de datos de una base de datos. Estas lecturas son críticos para el sistema DBMS de rendimiento.
- Escribir en los archivos de datos se produce en ráfagas en función de los puntos de control o un flujo constante. Promedio de un día, hay menos escrituras más lecturas. Opuesto a las lecturas de archivos de datos, estas escrituras son asincrónicas y no contienen hasta cualquier transacción de usuario.
- Apenas hay lecturas del registro de transacciones o de los archivos de fase de puesta al día. Las excepciones son las operaciones de E/s grandes al realizar copias de seguridad de registro de transacciones.
- La carga principal con los archivos de registro de transacciones o de rehacer es escrituras. Según la naturaleza de la carga de trabajo, puede hacer que las operaciones de E/s tan pequeña como 4 KB o, en otros casos, los tamaños de E/S de 1 MB o más.
- Todas las escrituras deben conservarse en el disco de forma confiable.

Para el almacenamiento estándar, los tipos posibles de la caché son:

* None
* Lectura
* Lectura/Escritura

Para obtener un rendimiento coherente y determinista, establezca el almacenamiento en caché en el almacenamiento estándar para todos los discos que contienen archivos de datos relacionados con el DBMS, inicie sesión y los archivos y espacio de tablas de puesta al día **NONE**. Se puede conservar el valor de almacenamiento en caché predeterminado del disco duro virtual base.

Premium Storage, existen las siguientes opciones de almacenamiento en caché:

* None
* Lectura
* Lectura/escritura
* Ninguno + Acelerador de escritura, que es solo para máquinas virtuales de la serie M de Azure
* Lectura + Acelerador de escritura, que es solo para máquinas virtuales de la serie M de Azure

Premium Storage, se recomienda que use **leer archivos de datos de almacenamiento en caché** de SAP de base de datos y elija **sin almacenamiento en caché para los discos de archivos de registro**.

Para las implementaciones de la serie M, se recomienda que utilice el Acelerador de escritura de Azure para la implementación de DBMS. Para obtener información detallada, restricciones y la implementación del Acelerador de escritura de Azure, consulte [habilitar el Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


### <a name="azure-nonpersistent-disks"></a>Discos de Azure no persistentes
Máquinas virtuales de Azure ofrecen no persistentes discos una vez implementada una máquina virtual. En el caso de un reinicio de máquina virtual, se borra todo el contenido de esas unidades. Es un dado que los archivos de datos y archivos de registro y de puesta al día de bases de datos deben bajo ninguna circunstancia se encuentra en esas unidades no persistentes. Puede haber excepciones para algunas bases de datos, donde estas unidades no persistentes podrían ser adecuadas para tempdb y espacios de tabla temporal. Evite usar esas unidades de disco para máquinas virtuales de serie porque esas unidades no persistentes tienen un límite de rendimiento con esta familia de máquina virtual. 

Para obtener más información, consulte [comprender la unidad temporal en máquinas virtuales Windows en Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).

- - -
> ![ Windows][Logo_Windows]  Windows
>
> Unidad D en una máquina virtual de Azure es una unidad no persistente, que está respaldada por algunos discos locales en el nodo de proceso de Azure. Porque es persistente, se pierden los cambios realizados en el contenido en la unidad D cuando se reinicia la máquina virtual. Los cambios incluyen los archivos que se almacenaron, los directorios que se crearon y las aplicaciones que se instalaron.
>
> ![Linux][Logo_Linux] Linux
>
> Máquinas virtuales Linux de Azure montan automáticamente una unidad en/mnt/Resource que es una unidad no persistente respaldada por discos locales en el nodo de proceso de Azure. Porque es no persistentes, los cambios realizados en el contenido en/mnt/resource se perderán cuando se reinicia la máquina virtual. Los cambios incluyen los archivos que se almacenaron, los directorios que se crearon y las aplicaciones que se instalaron.
>
>

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Resistencia de Microsoft Azure Storage
Microsoft Azure Storage almacena el VHD base, con el sistema operativo y discos conectados o blobs en al menos tres nodos de almacenamiento independientes. Este tipo de almacenamiento se denomina almacenamiento con redundancia local (LRS). LRS es el valor predeterminado para todos los tipos de almacenamiento en Azure.

Hay otros métodos de redundancia. Para más información, consulte [Replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Premium storage es el tipo de almacenamiento para máquinas virtuales de DBMS y los discos para almacenan la base de datos y de registro y archivos de puesta al día recomendado. El método de redundancia solo está disponible para el almacenamiento premium es LRS. Como resultado, deberá configurar métodos de la base de datos para habilitar la replicación de datos de base de datos en otra zona de disponibilidad o de región de Azure. Métodos de la base de datos incluyen SQL Server AlwaysOn, Oracle Data Guard y replicación del sistema de HANA.


> [!NOTE]
> Para las implementaciones de DBMS, no se recomienda el uso de almacenamiento con redundancia geográfica (GRS) para el almacenamiento estándar. GRS gravemente afecta al rendimiento y no aceptan el orden de escritura en diferentes discos duros virtuales que están conectados a una máquina virtual. No respeta el orden de escritura en diferentes discos duros virtuales potencialmente conduce a las bases de datos incoherentes en el lado de destino de replicación. Esta situación se produce si los archivos de base de datos y de registro y de puesta al día se reparten entre varios discos duros virtuales, como suele ser el caso, en el lado de la máquina virtual de origen.



## <a name="vm-node-resiliency"></a>Resistencia de nodos de máquina virtual
Azure ofrece varias distintos SLA para las máquinas virtuales. Para obtener más información, consulte la versión más reciente de [SLA para las máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Dado que la capa de DBMS es normalmente fundamental para disponibilidad en un sistema SAP, deberá comprender los eventos de mantenimiento, las zonas de disponibilidad y conjuntos de disponibilidad. Para obtener más información sobre estos conceptos, consulte [administrar la disponibilidad de las máquinas virtuales de Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) y [administrar la disponibilidad de máquinas virtuales Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

Es la recomendación mínima para escenarios DBMS de producción con una carga de trabajo SAP:

- Implemente dos máquinas virtuales en un conjunto en la misma región de Azure de disponibilidad independiente.
- Ejecute estas dos máquinas virtuales en la misma red virtual de Azure y tener NIC asociadas fuera de las mismas subredes.
- Use los métodos de la base de datos para mantener una espera activa con la segunda máquina virtual. Los métodos pueden ser SQL Server Always On, Oracle Data Guard o Replicación del sistema de HANA.

También puede implementar una tercera máquina virtual en otra región de Azure y use los mismos métodos de la base de datos para proporcionar una réplica asincrónica en otra región de Azure.

Para obtener información sobre cómo configurar conjuntos de disponibilidad de Azure, consulte [este tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Consideraciones de red de Azure
En las implementaciones de SAP a gran escala, usar el proyecto de [Azure Virtual Datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter). Se utiliza para las asignaciones de configuración y los permisos y roles de red virtual a diferentes partes de su organización.

Estos procedimientos recomendados son el resultado de cientos de implementaciones de clientes:

- Las redes virtuales en que se implementa la aplicación de SAP no tienen acceso a internet.
- La base de datos de las máquinas virtuales se ejecutan en la misma red virtual que la capa de aplicación.
- Las máquinas virtuales dentro de la red virtual tienen una asignación de la dirección IP privada estática. Para obtener más información, consulte [tipos y métodos de asignación de Azure de direcciones IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm).
- Restricciones en el enrutamiento a y desde las máquinas virtuales de DBMS son *no* establecido con firewalls instalados en las máquinas virtuales de DBMS local. En su lugar, el enrutamiento de tráfico se define con [grupos de seguridad (NSG) de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
- Para separar y aislar el tráfico a la VM de DBMS, asignar diferentes interfaces de red a la máquina virtual. Cada NIC Obtiene una dirección IP diferente y cada NIC se asigna a una subred de red virtual diferente. Cada subred tiene diferentes reglas NSG. La separación del tráfico de red o de aislamiento es una medida para el enrutamiento. No se usa para establecer cuotas para el rendimiento de la red.

> [!NOTE]
> Asignar direcciones IP estáticas a través de Azure significa que los asigne a cada NIC virtuales. No asignar direcciones IP estáticas en el sistema operativo invitado a una NIC virtual. Algunos servicios de Azure como copia de seguridad de Azure se basan en el hecho de que al menos la NIC virtual principal se establece en DHCP y no a las direcciones IP estáticas. Para obtener más información, consulte [copia de seguridad de máquina virtual de solución de problemas de Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Para asignar varias direcciones IP estáticas a una máquina virtual, asigne varias NIC virtuales a una máquina virtual.
>


> [!IMPORTANT]
> Configurar [aplicaciones virtuales de red](https://azure.microsoft.com/solutions/network-appliances/) en la ruta de comunicación entre la aplicación de SAP y la capa de DBMS de un SAP NetWeaver-, Hybris o del sistema basado en S/4HANA SAP no se admite. Esta restricción es por motivos de rendimiento y funcionalidad. La ruta de comunicación entre la capa de aplicación de SAP y la capa de DBMS debe ser un directa. No incluye la restricción [el grupo de seguridad de aplicaciones (ASG) y las reglas de NSG](https://docs.microsoft.com/azure/virtual-network/security-overview) si esas reglas ASG y NSG permiten una ruta de acceso de comunicación directa. 
>
> Otros escenarios donde no se admiten las aplicaciones virtuales de red se encuentran en:
>
> * Rutas de comunicación entre máquinas virtuales de Azure que representan Linux Pacemaker clúster nodos y dispositivos SBD como se describe en [alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server para SAP Applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> * Conjunto de rutas de comunicación entre máquinas virtuales de Azure y el servidor de archivos de escalabilidad horizontal (SOFS) de Windows Server como se describe en [agrupar una instancia ASCS/SCS de SAP en un clúster de conmutación por error de Windows mediante el uso de un recurso compartido de archivos en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Aplicaciones virtuales de red en las rutas de comunicación pueden duplicar fácilmente la latencia de red entre los asociados de comunicación de dos. También puede restringir el rendimiento en las rutas críticas entre la capa de aplicación de SAP y la capa de DBMS. En algunos escenarios de clientes, aplicaciones virtuales de red pueden provocar un error en los clústeres de Linux Pacemaker. Estos son los casos donde las comunicaciones entre los nodos del clúster de Linux Pacemaker comunican con su dispositivo SBD a través de una aplicación virtual de red.
>

> [!IMPORTANT]
> Otro diseño del *no* admite es la segregación de la capa de aplicación de SAP y la capa DBMS en diferentes redes virtuales de Azure que no son [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre sí. Se recomienda separar las capas de aplicación de SAP y DBMS mediante subredes dentro de una red virtual de Azure en lugar de a través de diferentes redes virtuales de Azure. 
>
> Si decide no sigue la recomendación y en su lugar, separar las dos capas en diferentes redes virtuales, las dos redes virtuales deben ser [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Tenga en cuenta que el tráfico entre dos de red [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) redes virtuales de Azure está sujeto a los costes de transferencia. Volumen de datos enorme que consta de muchos terabytes se intercambia entre la capa de aplicación de SAP y la capa de DBMS. Puede acumular los costos sustanciales si las capas de aplicación de SAP y DBMS se segregan entre dos redes virtuales emparejadas Azure.

Establecer dos máquinas virtuales de uso para la implementación de DBMS dentro de una disponibilidad de Azure de producción. Use también el enrutamiento independiente para la capa de aplicación de SAP y el tráfico de administración y operaciones a las dos máquinas virtuales de DBMS. Consulte la siguiente imagen:

![Diagrama de dos máquinas virtuales en dos subredes](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="use-azure-load-balancer-to-redirect-traffic"></a>Usar Azure Load Balancer para redirigir el tráfico
El uso de privada direcciones IP virtuales utilizados en las funcionalidades, como SQL Server Always On o replicación del sistema de HANA requiere la configuración de un equilibrador de carga. El equilibrador de carga utiliza los puertos de sondeo para determinar el nodo activo de DBMS y enrutar el tráfico exclusivamente para ese nodo de base de datos activa. 

Si se produce una conmutación por error del nodo de base de datos, no hay ninguna necesidad de volver a configurar la aplicación de SAP. En su lugar, las arquitecturas de aplicación más comunes de SAP volverá a conectar con la dirección IP virtual privada. Mientras tanto, el equilibrador de carga reacciona a la conmutación por error de nodo al redirigir el tráfico con la dirección IP virtual privada en el segundo nodo.

Azure ofrece dos diferentes [SKU del equilibrador de carga](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview): una SKU básica y una SKU estándar. A menos que desee implementar en las zonas de disponibilidad de Azure, el equilibrador de carga básico SKU se realiza correctamente.

¿Es el tráfico entre las máquinas virtuales de DBMS y el nivel de aplicación siempre se enrutan a través del equilibrador de carga todo el tiempo? La respuesta depende de cómo se configure el equilibrador de carga. 

En este momento, el tráfico entrante a la VM de DBMS siempre se enruta a través del equilibrador de carga. Ruta de tráfico saliente desde la VM de DBMS para la máquina virtual depende de la configuración del equilibrador de carga de capa de aplicación. 

El equilibrador de carga ofrece una opción de DirectServerReturn. Si se configura esta opción, el tráfico dirigido de la VM de DBMS al nivel de aplicación de SAP es *no* enruta a través del equilibrador de carga. En su lugar, pasa directamente a la capa de aplicación. Si no está configurado DirectServerReturn, se enruta el tráfico de retorno a la capa de aplicación de SAP a través del equilibrador de carga.

Se recomienda que configure DirectServerReturn en combinación con equilibradores de carga que se colocan entre la capa de aplicación de SAP y la capa de DBMS. Esta configuración reduce la latencia de red entre las dos capas.

Para obtener un ejemplo de cómo establecer esta configuración con SQL Server Always On, vea [configurar un agente de escucha ILB para grupos de disponibilidad AlwaysOn en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Si usa las plantillas publicadas de JSON de GitHub como referencia para las implementaciones de infraestructura SAP en Azure, estudie esta [plantilla para un sistema de 3 niveles SAP](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). En esta plantilla, también puede ver la configuración correcta para el equilibrador de carga.

### <a name="azure-accelerated-networking"></a>Redes aceleradas de Azure
Para reducir aún más la latencia de red entre máquinas virtuales de Azure, se recomienda que elija [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Puede usarlo al implementar máquinas virtuales de Azure para una carga de trabajo SAP, especialmente para la capa de aplicación de SAP y la capa de DBMS de SAP.

> [!NOTE]
> No todos los tipos de máquina virtual admiten Accelerated Networking. El artículo anterior enumera los tipos de máquina virtual que admite Accelerated Networking.
>

- - -
> ![ Windows][Logo_Windows]  Windows
>
> Para obtener información sobre cómo implementar máquinas virtuales con Accelerated Networking para Windows, consulte [crear una máquina virtual de Windows con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).
>
> ![Linux][Logo_Linux] Linux
>
> Para obtener más información sobre la distribución de Linux, consulte [crear una máquina virtual Linux con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).
>
>

- - -

> [!NOTE]
> En el caso de SUSE, Red Hat y Oracle Linux, Redes aceleradas se admite con las versiones recientes. No son compatibles con versiones anteriores como SLES 12 SP2 o RHEL 7.2 Azure Accelerated Networking.
>


## <a name="deployment-of-host-monitoring"></a>Implementación de host de supervisión
Para su uso en producción de aplicaciones de SAP en Azure virtual machines, SAP requiere la capacidad de obtener datos de supervisión de los hosts físicos que se ejecutan las máquinas virtuales del host. Se requiere un nivel de revisión específico del agente de host de SAP que permita esta funcionalidad en SAPOSCOL y el agente de host de SAP. El nivel de revisión exacto se menciona en la nota de SAP [1409604].

Para obtener más información sobre la implementación de componentes que proporcionen datos de host a SAPOSCOL y el agente de Host de SAP y la administración del ciclo de vida de los componentes, consulte el [Guía de implementación][deployment-guide].


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre un DBMS concreto, consulte:

- [Implementación de DBMS de Azure Virtual Machines de SQL Server para una carga de trabajo de SAP](dbms_guide_sqlserver.md)
- [Implementación de DBMS de Azure Virtual Machines de Oracle para una carga de trabajo de SAP](dbms_guide_oracle.md)
- [Implementación de DBMS de Azure Virtual Machines de IBM DB2 para una carga de trabajo de SAP](dbms_guide_ibm.md)
- [Implementación de DBMS de Azure Virtual Machines de SAP ASE para una carga de trabajo de SAP](dbms_guide_sapase.md)
- [Implementación de SAP maxDB, Live Cache y del servidor de contenido en Azure](dbms_guide_maxdb.md)
- [SAP HANA en la guía de operaciones de Azure](hana-vm-operations.md)
- [Alta disponibilidad de SAP HANA para máquinas virtuales de Azure](sap-hana-availability-overview.md)
- [Guía de copia de seguridad de SAP HANA en Azure virtual machines](sap-hana-backup-guide.md)

