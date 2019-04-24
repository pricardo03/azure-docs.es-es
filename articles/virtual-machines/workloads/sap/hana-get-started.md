---
title: 'Inicio rápido: instalación manual de una única instancia de SAP HANA en Azure Virtual Machines | Microsoft Docs'
description: Guía de inicio rápido para la instalación manual de una única instancia de SAP HANA en Azure Virtual Machines
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: hermannd
ms.openlocfilehash: 5091932989849943f00cb71f72378dd17af23a4a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60205052"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-virtual-machines"></a>Inicio rápido: Instalación manual de única instancia de SAP HANA en Azure Virtual Machines
## <a name="introduction"></a>Introducción
Esta guía le ayuda a configurar una única instancia de SAP HANA en Azure Virtual Machines cuando instala SAP NetWeaver 7.5 y SAP HANA 1.0 SP12 manualmente. El objetivo de esta guía es para implementar SAP HANA en Azure. No reemplaza la documentación de SAP. 

> [!NOTE]
> En ella se describen las implementaciones de SAP HANA en máquinas virtuales de Azure. Para obtener información sobre cómo implementar SAP HANA en instancias grandes de HANA, consulte [usar SAP en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Requisitos previos
Esta guía se da por supuesto que está familiarizado con este tipo de infraestructura como un servicio (IaaS) los conceptos básicos:
 * Cómo implementar máquinas virtuales (VM) o redes virtuales a través del portal de Azure o PowerShell.
 * El Azure la línea de comandos interfaz multiplataforma (CLI), que incluye la opción para usar las plantillas de JavaScript Object Notation (JSON).

Esta guía también se da por supuesto que está familiarizado con:
* SAP HANA y SAP NetWeaver y cómo instalarlos en el entorno local.
* Cómo instalar y trabajar con instancias de aplicaciones de SAP en Azure y SAP HANA.
* Los siguientes conceptos y procedimientos:
   * Planear la implementación de SAP en Azure, que incluye la planificación de la red Virtual de Azure y el uso de almacenamiento de Azure. Consulte [SAP NetWeaver en Azure Virtual Machines: Guía de planeación e implementación](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Principios de implementación y maneras de implementar máquinas virtuales en Azure. Consulte [Implementación de Azure Virtual Machines para SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Alta disponibilidad para SAP NetWeaver ABAP SAP Central Services (ASCS) /SAP Central Services (SCS) y Enqueue Replication Server (ERS) en Azure. Consulte [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Obtener más información sobre cómo mejorar la eficacia en una instalación de varios SID de ASCS/SCS en Azure. Consulte [Creación de una configuración de varios SID de SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principios de ejecución de SAP NetWeaver basado en máquinas virtuales orientadas a Linux en Azure. Consulte [ejecutar SAP NetWeaver en máquinas virtuales de Microsoft Azure SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Esta guía proporcionan configuraciones específicas para Linux en máquinas virtuales de Azure. También se proporciona información acerca de cómo conectar correctamente discos de almacenamiento de Azure para máquinas virtuales Linux.

Los tipos de máquina virtual de Azure que pueden utilizarse para escenarios de producción se muestran en la [documentación de SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Para escenarios de producción, una mayor variedad de tipos nativos de máquina virtual de Azure está disponible.
Para obtener más información sobre la configuración de máquina virtual y las operaciones, consulte [operaciones en Azure y las configuraciones de infraestructura de SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
Para lograr alta disponibilidad de SAP HANA, consulte [alta disponibilidad de SAP HANA para Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

Si desea obtener una instancia de SAP HANA o S/4HANA o BW/4HANA sistema implementar rápidamente, considere el uso de [SAP Cloud Appliance Library](https://cal.sap.com). Puede encontrar documentación acerca de cómo implementar un sistema S/4HANA mediante SAP Cloud Appliance Library en Azure, por ejemplo, en [esta guía](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Todo lo que necesita es una suscripción de Azure y un usuario SAP que se puede registrar con SAP Cloud Appliance Library.

## <a name="additional-resources"></a>Recursos adicionales
### <a name="sap-hana-backup"></a>Copia de seguridad de SAP HANA
Para obtener información sobre cómo realizar una copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure, consulte:
* [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide).
* [Azure Backup de SAP HANA en el nivel de archivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level).
* [Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots).

### <a name="sap-cloud-appliance-library"></a>SAP Cloud Appliance Library
Para obtener información sobre cómo usar SAP Cloud Appliance Library para implementar S/4HANA o BW/4HANA, consulte [implementación de SAP S/4HANA o BW/4HANA en Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>Sistemas operativos compatibles con SAP HANA
Para obtener información sobre los sistemas operativos compatibles con SAP HANA, consulte [nota de SAP 2235581 - SAP HANA: Sistemas operativos compatibles con](https://launchpad.support.sap.com/#/notes/2235581/E). Las máquinas virtuales de Azure solo admiten un subconjunto de estos sistemas operativos. Se admiten los siguientes sistemas operativos para la implementación de SAP HANA en Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Para obtener documentación de SAP adicional sobre SAP HANA y sistemas operativos Linux diferentes, consulte:

* [Nota de SAP 171356: Software de SAP en Linux: Información general](https://launchpad.support.sap.com/#/notes/1984787).
* [Nota de SAP 1944799: Directrices de SAP HANA para la instalación del sistema operativo SLES](https://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
* [Nota de SAP 2205917: SAP HANA DB recommended OS settings recomendada para SLES 12 para SAP applications](https://launchpad.support.sap.com/#/notes/2205917/E).
* [Nota de SAP 1391070: Soluciones UUID de Linux](https://launchpad.support.sap.com/#/notes/1391070).
* [Nota de SAP 2009879: Directrices de SAP HANA para el sistema operativo de Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879).
* [2292690 la nota de SAP: SAP HANA DB: Configuración del sistema operativo para RHEL 7 recomendada](https://launchpad.support.sap.com/#/notes/2292690/E).

### <a name="sap-monitoring-in-azure"></a>Supervisión de SAP en Azure
Para obtener información sobre la supervisión de SAP en Azure:

* [SAP 2191498 Nota](https://launchpad.support.sap.com/#/notes/2191498/E) trata SAP enhanced monitoring con máquinas virtuales Linux en Azure. 
* [SAP 1102124 Nota](https://launchpad.support.sap.com/#/notes/1102124/E) se describe información sobre SAPOSCOL en Linux. 
* [SAP 2178632 Nota](https://launchpad.support.sap.com/#/notes/2178632/E) se tratan las métricas claves de supervisión para SAP en Microsoft Azure.

### <a name="azure-vm-types"></a>Tipos de máquina virtual de Azure
Tipos de máquina virtual de Azure y escenarios de carga de trabajo compatibles con SAP que se usan con SAP HANA se documentan en [SAP certified IaaS plataformas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Tipos de máquina virtual de Azure que están certificados por SAP para SAP NetWeaver o la capa de aplicación de S/4HANA están documentados en [nota de SAP 1928533: Aplicaciones de SAP en Azure: Tipos de máquina virtual de Azure y productos admitidos](https://launchpad.support.sap.com/#/notes/1928533/E).

> [!NOTE]
> La integración SAP-Linux-Azure no se admite en el modelo de implementación clásica, solo en Azure Resource Manager. 

## <a name="manual-installation-of-sap-hana"></a>Instalación manual de SAP HANA

> [!IMPORTANT]
> Asegúrese de que el sistema operativo que seleccionó es SAP certificada para SAP HANA en los tipos específicos de máquina virtual que use. La lista de SAP HANA certified tipos de máquina virtual y el sistema operativo libera para esos tipos de máquinas virtuales se pueden buscar [plataformas IaaS certificadas para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Asegúrese de hacer clic en los detalles de los tipos de máquina virtual aparecen para obtener una lista completa de las versiones de SO compatibles con SAP HANA para el tipo específico de la máquina virtual. Por ejemplo, en este documento, se usa una versión de SUSE Linux Enterprise Server (SLES) del sistema operativo que no es compatible con SAP para SAP HANA en máquinas virtuales de la serie M.
>

En esta guía se describen dos formas diferentes de instalar manualmente SAP HANA en máquinas virtuales de Azure:

* Usar SAP Software Provisioning Manager (SWPM) como parte de una instalación distribuida de NetWeaver en el paso "instalar instancia de base de datos".
* Utilice la herramienta de administrador (HDBLCM) de ciclo de vida de base de datos de SAP HANA y después instalar NetWeaver.

También puede usar SWPM para instalar todos los componentes, como la instancia de ASCS, el servidor de aplicaciones de SAP y SAP HANA en una sola máquina virtual. Los pasos se describen en este [anuncio del blog de SAP HANA](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Esta opción no se describe en esta guía de inicio rápido, pero los problemas que debe tener en cuenta son los mismos.

Antes de iniciar una instalación, le recomendamos que lea el "Preparar máquinas virtuales de Azure para la instalación manual de SAP HANA" sección más adelante en esta guía. Si lo hace, puede ayudar a evitar varios errores básicos que se pueden producir cuando se usa solo una configuración predeterminada de máquina virtual de Azure.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Pasos clave para la instalación de SAP HANA cuando se usa SAP SWPM
Esta sección enumera los pasos principales de una instalación manual de SAP HANA de instancia única cuando usa SAP SWPM para realizar una instalación distribuida de SAP NetWeaver 7.5. Los pasos individuales se explican con más detalle en las capturas de pantalla más adelante en esta guía.

1. Cree una red virtual de Azure que incluya dos máquinas virtuales de prueba.
2. Implemente las dos máquinas virtuales de Azure con sistemas operativos según el modelo de Azure Resource Manager. En este ejemplo usa SUSE Linux Enterprise Server y SLES para SAP Applications 12 SP1. 
3. Conecte dos Azure estándar o discos de premium storage, por ejemplo, discos 75 GB y 500 GB, a la máquina virtual del servidor de aplicaciones.
4. Conecte discos de Premium Storage a la máquina virtual del servidor de base de datos de HANA. Para obtener más información, consulte la sección "Configuración de disco" más adelante en esta guía.
5. Según los requisitos de tamaño o rendimiento, conecte varios discos. A continuación, cree volúmenes seccionados. Use Administración de volúmenes lógicos (LVM) o una herramienta de administración (mdadm) de varios dispositivos en el nivel de sistema operativo dentro de la máquina virtual.
6. Cree sistemas de archivos XFS en los discos conectados o los volúmenes lógicos.
7. Monte los nuevos sistemas de archivos XFS en el nivel de sistema operativo. Use un sistema de archivos para todo el software SAP. Use el otro sistema de archivos para el directorio /sapmnt y las copias de seguridad, por ejemplo. En el servidor de SAP HANA DB, monte los sistemas de archivos XFS en los discos de almacenamiento premium como /hana y /usr/sap. Este proceso es necesario para impedir que el sistema de archivos raíz se llene. El sistema de archivos raíz no es grande en máquinas virtuales de Azure de Linux. 
8. Escriba las direcciones IP locales de las máquinas virtuales de prueba en el archivo /etc/hosts.
9. Especifique el parámetro **nofail** en el archivo /etc/fstab.
10. Establezca los parámetros de kernel de Linux según la versión de sistema operativo Linux que use. Para obtener más información, consulte las notas de SAP que describen HANA y la sección "Parámetros de Kernel" de esta guía.
11. Adición de espacio de intercambio.
12. Opcionalmente, instale un escritorio gráfico en las máquinas virtuales de prueba. De lo contrario, use una instalación remota con SAPinst.
13. Descargue el software SAP de SAP Service Marketplace.
14. Instale la instancia de ASCS de SAP en la máquina virtual del servidor de aplicaciones.
15. Comparta el directorio /sapmnt entre las máquinas virtuales de prueba mediante el uso de NFS. La máquina virtual del servidor de aplicaciones es el servidor NFS.
16. Instale la instancia de base de datos, que incluye HANA, por medio de SWPM en la máquina virtual del servidor de base de datos.
17. Instale al servidor principal de la aplicación (PAS) en la máquina virtual del servidor de aplicaciones.
18. Inicie SAP Management Console (SAP MC). Conéctese con SAP GUI o HANA Studio, por ejemplo.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Pasos clave para la instalación de SAP HANA cuando se usa HDBLCM
En esta sección se enumeran los pasos principales de una instalación manual de SAP HANA de una única instancia cuando se usa SAP HDBLCM para realizar una instalación distribuida de SAP NetWeaver 7.5. Los pasos individuales se explican con más detalle en las capturas de pantalla que se incluyen en esta guía.

1. Cree una red virtual de Azure que incluya dos máquinas virtuales de prueba.
2. Implemente dos máquinas virtuales de Azure con sistemas operativos según el modelo de Azure Resource Manager. Este ejemplo usa SLES y SLES para SAP Applications 12 SP1.
3. Conecte dos Azure estándar o discos de premium storage, por ejemplo, discos 75 GB y 500 GB, a la máquina virtual del servidor de aplicaciones.
4. Conecte discos de Premium Storage a la máquina virtual del servidor de base de datos de HANA. Para obtener más información, consulte la sección "Configuración de disco" más adelante en esta guía.
5. Según los requisitos de tamaño o rendimiento, conecte varios discos. Cree volúmenes seccionados mediante administración de volúmenes lógicos o una herramienta mdadm en el nivel de sistema operativo dentro de la máquina virtual.
6. Cree sistemas de archivos XFS en los discos conectados o los volúmenes lógicos.
7. Monte los nuevos sistemas de archivos XFS en el nivel de sistema operativo. Use un sistema de archivos para todo el software SAP. Use el otro sistema de archivos para el directorio /sapmnt y las copias de seguridad, por ejemplo. En el servidor de SAP HANA DB, monte los sistemas de archivos XFS en los discos de almacenamiento premium como /hana y /usr/sap. Este proceso es necesario para ayudar a impedir que el sistema de archivos raíz se llene. El sistema de archivos raíz no es grande en máquinas virtuales de Azure de Linux.
8. Escriba las direcciones IP locales de las máquinas virtuales de prueba en el archivo /etc/hosts.
9. Especifique el parámetro **nofail** en el archivo /etc/fstab.
10. Establezca los parámetros de kernel según la versión del sistema operativo Linux que use. Para obtener más información, consulte las notas de SAP que describen HANA y la sección "Parámetros de Kernel" de esta guía.
11. Adición de espacio de intercambio.
12. Opcionalmente, instale un escritorio gráfico en las máquinas virtuales de prueba. De lo contrario, use una instalación remota con SAPinst.
13. Descargue el software SAP de SAP Service Marketplace.
14. Cree un grupo "sapsys" con el identificador de grupo 1001 en la máquina virtual del servidor de HANA DB.
15. Instalar SAP HANA en la máquina virtual del servidor de base de datos mediante el Administrador de ciclo de vida de base de datos HANA.
16. Instale la instancia de ASCS de SAP en la máquina virtual del servidor de aplicaciones.
17. Comparta el directorio /sapmnt entre las máquinas virtuales de prueba mediante el uso de NFS. La máquina virtual del servidor de aplicaciones es el servidor NFS.
18. Instale la instancia de base de datos, que incluye HANA, por medio de SWPM en la máquina virtual del servidor de HANA DB.
19. Instale al servidor de aplicaciones principal en la máquina virtual del servidor de aplicaciones.
20. Inicie SAP MC. Conectarse mediante SAP GUI o HANA Studio.

## <a name="prepare-azure-vms-for-a-manual-installation-of-sap-hana"></a>Preparación de las máquinas virtuales de Azure para una instalación manual de SAP HANA
En esta sección se describen los temas siguientes:

* Actualizaciones del sistema operativo
* Configuración de disco
* Parámetros de kernel
* Sistemas de archivos
* El archivo /etc/hosts
* El archivo /etc/fstab

### <a name="os-updates"></a>Actualizaciones del sistema operativo
Compruebe las actualizaciones de sistema operativo Linux y correcciones antes de instalar software adicional. Al instalar una revisión, podría evitar una llamada al servicio de soporte técnico.

Asegúrese de que usa:
* SUSE Linux Enterprise Server for SAP Applications.
* Red Hat Enterprise Linux for SAP Applications o Red Hat Enterprise Linux for SAP HANA. 

Si aún no lo ha hecho, registre la implementación de sistema operativo con la suscripción de Linux del proveedor de Linux. SUSE tiene imágenes de sistema operativo para las aplicaciones de SAP que ya incluyen servicios y que se registran automáticamente.

Este es un ejemplo de cómo comprobar las revisiones disponibles para SUSE Linux mediante el **zypper** comando:

 `sudo zypper list-patches`

Según el tipo de problema, las revisiones se clasifican por categoría y gravedad. Los valores usados normalmente para categoría son: 
- Seguridad
- Recomendado
- Opcional
- Característica
- Documento
- YaST

Los valores usados normalmente para gravedad son:

- Crítico
- Importante
- Moderado
- Bajo
- No especificada

El comando **zypper** busca solo las actualizaciones que necesitan los paquetes instalados. Por ejemplo, puede usar este comando:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Puede agregar el parámetro `--dry-run` para probar la actualización sin actualizar realmente el sistema.


### <a name="disk-setup"></a>Configuración de disco
El sistema de archivos raíz en una máquina virtual Linux en Azure tiene un límite de tamaño. Por lo tanto, deberá agregar espacio en disco adicional a una máquina virtual de Azure para ejecutar SAP. Para servidor de aplicaciones de SAP máquinas virtuales de Azure, el uso de discos de almacenamiento estándar de Azure podría ser suficiente. Para máquinas virtuales de Azure de DBMS de SAP HANA, es obligatorio el uso de discos de almacenamiento premium de Azure para implementaciones de producción y no sea de producción.

Según el [los requisitos de almacenamiento de SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), se sugiere la siguiente configuración de almacenamiento premium de Azure: 

| SKU de la máquina virtual | RAM |  /hana/data y /hana/log <br /> seccionado con LVM o mdaadm | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

En la configuración de disco sugerida, el volumen de datos HANA y el volumen de registro se colocan en el mismo conjunto de discos de almacenamiento premium de Azure que están seccionados con LVM o mdaadm. No es necesario definir ningún nivel de redundancia RAID ya que Azure premium storage conserva tres imágenes de los discos para la redundancia. 

Para asegurarse de que ha configurado almacenamiento suficiente, consulte [los requisitos de almacenamiento de SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) y [Guía de instalación y actualización del servidor de SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Considere también los volúmenes de rendimiento de disco duro virtual (VHD) diferente de los discos de almacenamiento premium de Azure diferentes como se documenta en [discos administrados para máquinas virtuales y almacenamiento premium de alto rendimiento](../../windows/disks-types.md). 

Puede agregar más discos de premium storage para las máquinas virtuales de DBMS para almacenar copias de seguridad de registros de transacciones o base de datos.

Para obtener más información acerca de las dos herramientas principales que se usa para configurar el seccionamiento, consulte:

* [Configuración del software RAID en Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Configuración del LVM en una máquina virtual Linux en Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para obtener más información sobre cómo conectar discos a máquinas virtuales de Azure que ejecutan Linux como sistema operativo invitado, consulte [agregar un disco a una VM de Linux](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Con Azure premium SSD, puede definir los modos de almacenamiento en caché de disco. Para los conjuntos seccionados que contiene/Hana/Data y/Hana/log, deshabilite el almacenamiento en caché de disco. Para los otros volúmenes, es decir, discos, establezca el modo de almacenamiento en caché en **ReadOnly**.

Para buscar plantillas de JSON de ejemplo se usa para crear máquinas virtuales, consulte el [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates).
La plantilla vm-simple-sles es una plantilla básica. Incluye una sección de almacenamiento, con un disco de datos adicional de 100 GB. Use esta plantilla como base. La plantilla se puede adaptar a su configuración específica.

> [!NOTE]
> Es importante conectar el disco de almacenamiento de Azure mediante un UUID, como se documenta en [ejecutar SAP NetWeaver en máquinas virtuales de Microsoft Azure SUSE Linux](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

En el entorno de prueba, discos de almacenamiento estándar de Azure dos están conectados al servidor de aplicaciones SAP de máquina virtual, como se muestra en la siguiente captura de pantalla. Un disco almacena todo el software SAP, por ejemplo, NetWeaver 7.5, GUI de SAP y SAP HANA, para la instalación. El segundo disco garantiza que haya suficiente espacio libre está disponible para requisitos adicionales. Por ejemplo, los datos de copia de seguridad y prueba y el directorio sapmnt, es decir, los perfiles de SAP, que se comparte entre todas las máquinas virtuales que pertenecen a la misma infraestructura de SAP.

![Ventana de discos de servidor de aplicaciones de SAP HANA que muestra dos discos de datos y sus tamaños](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Parámetros de kernel
SAP HANA requiere configuraciones específicas de kernel de Linux. Estas configuraciones de kernel no forman parte de las imágenes de la Galería de Azure estándar y se deben establecer manualmente. Dependiendo de si usa SUSE o Red Hat, los parámetros pueden ser diferentes. Notas de SAP enumeradas anteriormente proporcionan información acerca de esos parámetros. En las capturas de pantalla mostradas, se ha utilizado SUSE Linux 12 SP1. 

SLES para SAP Applications 12 de la disponibilidad general y SLES para SAP Applications 12 SP1 tienen una nueva herramienta, **tuned-adm**, que reemplaza al antiguo **sapconf** herramienta. Hay un perfil de SAP HANA especial disponible para **tuned-adm**. Para ajustar el sistema de SAP HANA, escriba el siguiente perfil como un usuario raíz:

   `tuned-adm profile sap-hana`

Para más información sobre **tuned-adm**, consulte la [documentación de SUSE](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

En la siguiente captura de pantalla, puede ver cómo **tuned-adm** cambiado el `transparent_hugepage` y `numa_balancing` valores, según la configuración necesaria de SAP HANA:

![La herramienta tuned-adm cambia los valores en función de la configuración necesaria de SAP HANA](./media/hana-get-started/image005.jpg)

Para convertir en permanentes las configuraciones de kernel de SAP HANA, se debe usar **grub2** en SLES 12. Para obtener más información acerca de **grub2**, consulte el [estructura del archivo de configuración](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) sección de la documentación de SUSE.

La siguiente captura de pantalla muestra cómo se han cambiado las configuraciones del kernel en el archivo de configuración y después se han compilado mediante **grub2-mkconfig**:

![Configuraciones de kernel cambiadas en el archivo de configuración y compiladas mediante grub2 mkconfig](./media/hana-get-started/image006.jpg)

Otra opción es cambiar las configuraciones por medio de YaST y la configuración de  > **parámetros de kernel** del **cargador de arranque**:

![Pestaña de configuración de parámetros de kernel en el cargador de arranque de YaST](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Sistemas de archivos
La siguiente captura de pantalla muestra los dos sistemas de archivos que se crearon en la máquina virtual del servidor de aplicaciones SAP usando los dos discos de almacenamiento estándar de Azure conectados. Ambos sistemas de archivos son del tipo XFS y están montados en /sapdata y/sapsoftware.

No es obligatorio para estructurar los sistemas de archivos de este modo. Tiene otras opciones de cómo estructurar el espacio en disco. La consideración más importante que debe tener es la de evitar que el sistema de archivos raíz se quede sin espacio libre.

![Se crearon dos sistemas de archivos en la máquina virtual del servidor de aplicaciones SAP](./media/hana-get-started/image008.jpg)

Para la VM de SAP HANA DB, durante una instalación de la base de datos, cuando se usa SAPinst con SWPM y **típico** opción de instalación, todo lo que se instala en /hana y/usr/SAP. La ubicación predeterminada de la copia de seguridad del registro de SAP HANA se encuentra en /usr/sap. Nuevamente, es importante evitar que el sistema de archivos raíz quedarse sin espacio de almacenamiento. Asegúrese de que hay suficiente espacio libre en /hana y/usr/SAP antes de instalar SAP HANA mediante SWPM.

Para obtener una descripción del diseño del sistema de archivos estándar de SAP HANA, consulte el [Guía de instalación y actualización del servidor de SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Dos sistemas de archivos adicionales creados en la máquina virtual del servidor de aplicaciones de SAP](./media/hana-get-started/image009.jpg)

Cuando instala SAP NetWeaver en un SLES/SLES estándar para la imagen de la Galería de Azure de 12 de las aplicaciones de SAP, aparece un mensaje que indica que no hay ningún espacio de intercambio, como se muestra en la captura de pantalla siguiente. Para descartar este mensaje, puede agregar manualmente un archivo de intercambio mediante **dd**, **mkswap** y **swapon**. Para obtener información sobre cómo hacerlo, busque "Agregar manualmente un archivo de intercambio" en el [con el particionador YaST](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) sección de la documentación de SUSE.

Otra opción consiste en configurar el espacio de intercambio mediante el agente de la máquina virtual Linux. Consulte la [Guía de usuario del Agente de Linux de Azure](../../extensions/agent-linux.md) para más información.

![Mensaje emergente que avisa de que no hay espacio de intercambio suficiente](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>El archivo /etc/hosts
Antes de iniciar la instalación de SAP, asegúrese de que incluir los nombres de host y las direcciones IP de las máquinas virtuales de SAP en el archivo/etc/hosts. Implementar todas las máquinas virtuales de SAP en una red virtual de Azure. A continuación, use las direcciones IP internas, como se muestra aquí:

![Nombres de host y direcciones IP de las máquinas virtuales de SAP enumeradas en el archivo /etc/hosts](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>El archivo /etc/fstab

Resulta útil agregar la **nofail** parámetro al archivo fstab. De este modo, si algo va mal con los discos, la máquina virtual no deja de responder en el proceso de arranque. Pero recuerde que es posible que no haya más espacio en disco disponible y que los procesos llenen el sistema de archivos raíz. Si falta /hana, SAP HANA no se iniciará.

![Adición del parámetro nofail al archivo fstab](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Escritorio GNOME gráfico en SLES 12/SLES for SAP Applications 12
En esta sección se explica cómo:

* Instale el escritorio GNOME y xrdp en SLES 12/SLES para SAP Applications 12.
* Ejecute consola de administración de SAP basada en Java mediante Firefox en SLES 12/SLES para SAP Applications 12.

También puede usar alternativas como Xterminal o VNC, que no se describen en esta guía.

### <a name="install-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Instalar el escritorio GNOME y xrdp en SLES 12/SLES para SAP Applications 12
Si tiene un fondo de Windows, puede usar fácilmente un escritorio gráfico directamente dentro de las máquinas virtuales Linux de SAP para ejecutar Firefox, SAPinst, GUI de SAP, SAP MC o HANA Studio. A continuación, puede conectarse a la máquina virtual mediante el protocolo de escritorio remoto (RDP) desde un equipo Windows. Dependiendo de las directivas de la empresa acerca de cómo agregar interfaces gráficas de usuario a los sistemas de producción y de no producción basado en Linux, es posible que desea instalar a GNOME en el servidor. Siga estos pasos para instalar el escritorio GNOME en una Azure SLES 12/SLES para VM de 12 de las aplicaciones de SAP.

1. Instale el escritorio GNOME escribiendo el siguiente comando, por ejemplo, en una ventana de PuTTY:

   `zypper in -t pattern gnome-basic`

2. Instale xrdp para permitir una conexión a la máquina virtual a través de RDP:

   `zypper in xrdp`

3. Edite /etc/sysconfig/windowmanager y establezca el administrador de ventanas predeterminado en GNOME:

   `DEFAULT_WM="gnome"`

4. Ejecute **chkconfig** para asegurarse de que xrdp se inicie automáticamente después de un reinicio:

   `chkconfig -level 3 xrdp on`

5. Si tiene un problema con la conexión RDP, pruebe a reiniciar, por ejemplo, desde una ventana de PuTTY:

   `/etc/xrdp/xrdp.sh restart`

6. Si el reinicio de xrdp mencionado en el paso anterior no funciona, busque un archivo .pid:

   `check /var/run` 

   Busque `xrdp.pid`. Si lo encuentra, quítelo y pruebe a reiniciar de nuevo.

### <a name="start-sap-mc"></a>Start SAP MC
Después de instalar el escritorio GNOME, inicie el MC de SAP gráfica basada en Java desde Firefox. Si se ejecuta en un Azure SLES 12/SLES para VM de 12 de las aplicaciones de SAP, es posible que aparezca un error. El error se produce porque el complemento del explorador Java que falta.

La dirección URL para iniciar SAP MC es `<server>:5<instance_number>13`.

Para obtener más información, consulte [a partir de la consola de administración de SAP basada en web](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

Captura de pantalla siguiente muestra el mensaje de error que se muestra cuando falta el complemento del explorador de Java:

![Mensaje de error que indica que falta el complemento del explorador de Java](./media/hana-get-started/image013.jpg)

Una manera de resolver el problema es instalar el complemento que falta mediante YaST, tal y como se muestra en la captura de pantalla siguiente:

![Uso de YaST para instalar el complemento que falta](./media/hana-get-started/image014.jpg)

Cuando se vuelva a escribir la URL de consola de administración de SAP, se le pide que Active el complemento:

![Cuadro de diálogo que solicita la activación del complemento](./media/hana-get-started/image015.jpg)

Puede que también reciba un mensaje de error sobre un archivo que falta, javafx.properties. Se relaciona con el requisito de Oracle Java 1.8 para SAP GUI 7.4. Para obtener más información, consulte [nota de SAP de SAP 2059429](https://launchpad.support.sap.com/#/notes/2059424).
La versión de IBM Java y el paquete openjdk que se entrega con SLES/SLES para SAP Applications 12 no incluyen el archivo javafx necesario. La solución es descargar e instalar Java SE8 desde Oracle.

Para información sobre un problema similar con openjdk openSUSE, consulte el hilo de discusión [SAPGui 7.4 Java for openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Instalación manual de SAP HANA: SWPM
La serie de capturas de pantalla en esta sección muestra los pasos principales para instalar SAP NetWeaver 7.5 y SAP HANA SP12 cuando usa SWPM con SAPinst. Como parte de una instalación de NetWeaver 7.5, SWPM también puede instalar la base de datos HANA como una sola instancia.

En un entorno de prueba de ejemplo, se instala a un servidor de aplicaciones de Advanced Business Application Programming (ABAP). Como se muestra en la siguiente captura de pantalla, hemos usado el **sistema distribuido** opción para instalar ASCS instancias del servidor principal de la aplicación en una máquina virtual de Azure. SAP HANA se utiliza como el sistema de base de datos en otra máquina virtual de Azure.

![Instancias de ASCS y del servidor de aplicaciones principal instaladas con la opción de "Distributed System" (Sistema distribuido)](./media/hana-get-started/image012.jpg)

Después de la instancia de ASCS está instalada en la máquina virtual del servidor de aplicaciones, se identifica mediante un icono verde en la consola de administración de SAP. El directorio sapmnt, que incluye el directorio del perfil SAP, debe compartirse con la máquina virtual del servidor de base de datos de SAP HANA. El paso de instalación de la base de datos necesita acceso a esta información. La mejor manera de proporcionar acceso es utilizar NFS, que se puede configurar mediante YaST.

![Consola de administración de SAP que muestra la instancia ASCS instalada en la máquina virtual mediante un icono verde del servidor de aplicaciones](./media/hana-get-started/image016.jpg)

En la máquina virtual del servidor de aplicaciones, se comparte el directorio sapmnt mediante NFS mediante el **rw** y **no_root_squash** opciones. Los valores predeterminados son **ro** y **root_squash**, lo que podría producir problemas al instalar la instancia de base de datos.

![Uso compartido del directorio /sapmnt mediante NFS mediante el uso de las opciones rw y no_root_squash](./media/hana-get-started/image017b.jpg)

Como se muestra en la siguiente captura de pantalla, el recurso compartido /sapmnt de la máquina virtual del servidor de aplicaciones debe configurarse en la máquina virtual del servidor de base de datos de SAP HANA mediante el uso de **cliente NFS** y YaST:

![El recurso compartido /sapmnt configurado mediante el uso de cliente para NFS](./media/hana-get-started/image018b.jpg)

Para realizar una instalación distribuida de NetWeaver 7.5, es decir, un **instancia de base de datos**, inicie sesión en la máquina virtual del servidor de SAP HANA DB e inicie SWPM:

![Instalación de una instancia de base de datos mediante el inicio de sesión en la máquina virtual del servidor de SAP HANA DB y el inicio de SWPM](./media/hana-get-started/image019.jpg)

Después de seleccionar **típico** instalación y la ruta de acceso a los medios de instalación, escriba un SID de base de datos, el nombre de host, el número de instancia y la contraseña de administrador del sistema de base de datos:

![La página de inicio de sesión del administrador del sistema de base de datos de SAP HANA](./media/hana-get-started/image035b.jpg)

Escriba la contraseña para el esquema DBACOCKPIT:

![El cuadro para escribir la contraseña para el esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Escriba una pregunta para la contraseña del esquema SAPABAP1:

![Escriba una pregunta para la contraseña del esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Después de que se ha completado cada tarea, aparece una marca de verificación verde junto a cada fase del proceso de instalación de la base de datos. Se muestra el mensaje "Execution of... Database Instance has completed" (Ejecución de la instancia de base de datos… completada).

![Ventana de tarea completada con el mensaje de confirmación](./media/hana-get-started/image023.jpg)

Tras una instalación correcta, la consola de administración de SAP también se muestra la instancia de base de datos con un icono verde. Muestra la lista completa de los procesos de SAP HANA, como hdbindexserver, hdbcompileserver.

![Ventana de la consola de administración de SAP con la lista de procesos de SAP HANA](./media/hana-get-started/image024.jpg)

La siguiente captura de pantalla muestra las partes de la estructura de archivos en el directorio /hana/shared que SWPM creó durante la instalación de HANA. Dado que no hay ninguna opción para especificar una ruta de acceso diferente, es importante montar espacio en disco adicional en el directorio /hana antes de la instalación de SAP HANA mediante SWPM. Este paso impide que el sistema de archivos raíz se quede sin espacio libre.

![La estructura de archivos del directorio /hana/shared creada durante la instalación de HANA](./media/hana-get-started/image025.jpg)

Esta captura de pantalla muestra la estructura de archivos del directorio /usr/sap:

![La estructura de archivos del directorio /usr/sap](./media/hana-get-started/image026.jpg)

El último paso de la instalación distribuida de ABAP es instalar la instancia del servidor de aplicaciones principal:

![Instalación de ABAP que muestra la instancia del servidor de aplicaciones principal como último paso](./media/hana-get-started/image027b.jpg)

Después de instalar la instancia del servidor principal de la aplicación y la GUI de SAP, use el **DBA Cockpit** transaction para confirmar que la instalación de SAP HANA se ha completado correctamente:

![Ventana DBA Cockpit que confirma la instalación correcta](./media/hana-get-started/image028b.jpg)

Como paso final, es posible que desee instalar primero HANA Studio en la máquina virtual del servidor de aplicaciones SAP. A continuación, conéctese a la instancia de SAP HANA que se ejecuta en la máquina virtual del servidor de base de datos.

![Instalación de SAP HANA Studio en la máquina virtual del servidor de aplicaciones SAP](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Instalación manual de SAP HANA: HDBLCM
Además de instalar SAP HANA como parte de una instalación distribuida mediante SWPM, puede instalar primero HANA de forma independiente por medio de HDBLCM. A continuación, puede instalar, por ejemplo, SAP NetWeaver 7.5. Las capturas de pantalla de esta sección muestran cómo funciona este proceso.

Para más información sobre la herramienta HDBLCM de HANA, consulte:

* [Elija el SAP HANA HDBLCM correcto para la tarea](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm).
* [Herramientas de administración del ciclo de vida de SAP HANA](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm).
* [Guía de instalación y actualización del servidor de SAP HANA](https://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf).

Para evitar problemas con un valor de Id. de grupo predeterminado para el `\<HANA SID\>adm user`, que se crea mediante la herramienta HDBLCM. Antes de instalar SAP HANA mediante HDBLCM, definir un nuevo grupo denominado `sapsys` mediante el uso de Id. de grupo `1001`:

![Nuevo grupo "sapsys" definido mediante el uso del identificador de grupo 1001](./media/hana-get-started/image030.jpg)

Al iniciar HDBLCM por primera vez, un simple inicio muestra de menú. Seleccione el elemento 1, **instalar nuevo sistema**:

![Opción "Install new system" (Instalar nuevo sistema) en la ventana de inicio de HDBLCM](./media/hana-get-started/image031.jpg)

La captura de pantalla siguiente muestra todas las opciones principales que seleccionó anteriormente.

> [!IMPORTANT]
> Los directorios con nombre para el registro de HANA y volúmenes de datos y la ruta de instalación, que es/Hana/Shared en este ejemplo y/usr/SAP no deben formar parte del sistema de archivos raíz. Estos directorios pertenecen a los discos de datos de Azure que se conectaron a la máquina virtual. Para obtener más información, consulte la sección "Configuración de disco". 

Este enfoque ayuda a evitar que el sistema de archivos raíz se quede sin espacio. Observe que el administrador del sistema HANA tiene el Id. de usuario `1005` y forma parte de la `sapsys` grupo con el identificador `1001`, que se definió antes de la instalación.

![Lista de todos los componentes de SAP HANA principales seleccionados anteriormente](./media/hana-get-started/image032.jpg)

Compruebe el `\<HANA SID\>adm user` detalles en el directorio/etcetera/passwd. Busque `azdadm`, tal y como se muestra en la captura de pantalla siguiente:

![Detalles del usuario \<HANA SID\>adm de HANA mostrados en el directorio /etc/passwd](./media/hana-get-started/image033.jpg)

Después de instalar SAP HANA mediante HDBLCM, puede ver la estructura de archivos en SAP HANA Studio, como se muestra en la siguiente captura de pantalla. El esquema SAPABAP1 que incluye todas las tablas de SAP NetWeaver aún no está disponible.

![Estructura de archivos de SAP HANA en SAP HANA Studio](./media/hana-get-started/image034.jpg)

Después de instalar SAP HANA, puede instalar SAP NetWeaver sobre él. Como se muestra en la siguiente captura de pantalla, la instalación se realizó como una instalación distribuida mediante SWPM. Este proceso se describe en la sección anterior. Cuando se instala la instancia de base de datos mediante SWPM, escriba los mismos datos mediante HDBLCM. Por ejemplo, escribir el nombre de host, SID de HANA y número de instancia. Posteriormente, SWPM usa la instalación existente de HANA y agrega más esquemas.

![Una instalación distribuida que se ha realizado mediante SWPM](./media/hana-get-started/image035b.jpg)

La siguiente captura de pantalla muestra el paso de instalación de SWPM donde escribe los datos acerca del esquema DBACOCKPIT:

![El paso de instalación de SWPM donde se escriben los datos del esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Escriba los datos acerca del esquema SAPABAP1:

![Escritura de los datos acerca del esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Una vez finalizada la instalación de instancia de base de datos SWPM, puede ver el esquema SAPABAP1 en SAP HANA Studio:

![El esquema de SAPABAP1 en SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Por último, una vez terminado el servidor de aplicaciones SAP y SAP GUI, puede comprobar la instancia de HANA DB mediante el **DBA Cockpit** transacciones:

![La instancia de base de datos de HANA comprobada con la transacción DBA Cockpit](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>Descargas de software de SAP
Puede descargar el software desde SAP Service Marketplace, como se muestra en las siguientes capturas de pantalla.

Descargue NetWeaver 7.5 para Linux/HANA:

 ![Ventana de instalación y actualización del servicio SAP para la descarga de NetWeaver 7.5](./media/hana-get-started/image001.jpg)

Descargue HANA SP12 Platform Edition:

 ![Ventana de instalación y actualización del servicio SAP para la descarga de HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)

