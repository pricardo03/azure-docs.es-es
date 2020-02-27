---
title: 'Inicio rápido: instalación manual de una única instancia de SAP HANA en Azure Virtual Machines | Microsoft Docs'
description: Guía de inicio rápido para la instalación manual de una única instancia de SAP HANA en Azure Virtual Machines
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: hermannd
ms.openlocfilehash: 0090ffe977dee3e493d726c9eb4d151bcbeb503f
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617244"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-virtual-machines"></a>Inicio rápido: Instalación manual de una única instancia de SAP HANA en Azure Virtual Machines
## <a name="introduction"></a>Introducción
Esta guía de inicio rápido le ayuda a configurar una instancia única de SAP HANA en Azure Virtual Machines al instalar SAP NetWeaver 7.5 y SAP HANA 1.0 SP12 manualmente. El objetivo de esta guía es cómo implementar SAP HANA en Azure. No sustituye a la documentación de SAP. 

> [!NOTE]
> En ella se describen las implementaciones de SAP HANA en máquinas virtuales de Azure. Para más información sobre cómo implementar SAP HANA en instancias grandes de HANA, consulte [Uso de SAP en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Prerrequisitos
En esta guía se da por supuesto que está familiarizado con los conceptos básicos de la infraestructura como servicio (IaaS), por ejemplo:
 * Cómo implementar máquinas virtuales (VM) o redes virtuales mediante Azure Portal o PowerShell.
 * La interfaz de línea de comandos (CLI) multiplataforma de Azure, que incluye la opción para usar plantillas de notificación de objetos JavaScript (JSON).

También se da por supuesto que está familiarizado con:
* SAP HANA y SAP NetWeaver y cómo instalarlos en el entorno local.
* Cómo instalar y usar SAP HANA y las instancias de aplicación SAP en Azure.
* Los siguientes conceptos y procedimientos:
   * Planeamiento de la implementación de SAP en Azure, que incluye el planeamiento de Azure Virtual Network y el uso de Azure Storage. Consulte [SAP NetWeaver en Azure Virtual Machines: guía de planeamiento e implementación](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Principios de implementación y maneras de implementar máquinas virtuales en Azure. Consulte [Implementación de Azure Virtual Machines para SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Alta disponibilidad para SAP NetWeaver ABAP SAP Central Services (ASCS), SAP Central Services (SCS) y Enqueue Replication Server (ERS) en Azure. Consulte [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Información detallada sobre cómo mejorar la eficacia en una instalación de varios SID de ASCS/SCS en Azure. Consulte [Creación de una configuración de varios SID de SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principios de ejecución de SAP NetWeaver basado en máquinas virtuales orientadas a Linux en Azure. Consulte [Ejecución de SAP NetWeaver en máquinas virtuales de SUSE Linux de Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). En esta guía se proporcionan configuraciones específicas para Linux en Azure Virtual Machines. También se proporciona información sobre cómo conectar correctamente discos de almacenamiento de Azure con máquinas virtuales Linux.

Los tipos de máquina virtual de Azure que pueden utilizarse para escenarios de producción se muestran en la [documentación de SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). En el caso de los escenarios que no son de producción, existe una variedad más amplia de tipos de máquinas virtuales de Azure disponible.
Para más información sobre la configuración y las operaciones de máquinas virtuales, consulte [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
Para más información sobre la alta disponibilidad de SAP HANA, consulte [Alta disponibilidad de SAP HANA para Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

Si desea obtener una instancia de SAP HANA o el sistema S/4HANA o BW/4HANA rápidamente implementado, considere el uso de [SAP Cloud Appliance Library](https://cal.sap.com). Puede encontrar documentación sobre cómo implementar un sistema S/4HANA a través de SAP Cloud Appliance Library en Azure, por ejemplo, en [esta guía](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Todo lo que necesita es una suscripción de Azure y un usuario de SAP que se pueda registrar en SAP Cloud Appliance Library.

## <a name="additional-resources"></a>Recursos adicionales
### <a name="sap-hana-backup"></a>Copia de seguridad de SAP HANA
Para más información sobre cómo hacer una copia de seguridad de bases de datos SAP HANA en máquinas virtuales de Azure, consulte:
* [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide).
* [Azure Backup de SAP HANA en el nivel de archivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level).
* [Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots).

### <a name="sap-cloud-appliance-library"></a>SAP Cloud Appliance Library
Para más información sobre cómo usar SAP Cloud Appliance Library para implementar S/4HANA o BW/4HANA, consulte [Implementación de SAP S/4HANA o BW/4HANA en Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>Sistemas operativos compatibles con SAP HANA
Para más información sobre los sistemas operativos compatibles con SAP HANA, consulte [SAP Note 2235581 - SAP HANA: Supported operating systems](https://launchpad.support.sap.com/#/notes/2235581/E) (Nota de SAP 2235581: sistemas operativos compatibles). Las máquinas virtuales de Azure solo admiten un subconjunto de estos sistemas operativos. Se admiten los siguientes sistemas operativos para la implementación de SAP HANA en Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Para obtener documentación de SAP adicional sobre SAP HANA y sistemas operativos Linux diferentes, consulte:

* [SAP Note 171356: SAP Software on Linux: General information](https://launchpad.support.sap.com/#/notes/1984787) (Nota de 171356: Software de SAP en Linux: información general).
* [SAP Note 1944799: SAP HANA guidelines for SLES operating system installation](http://service.sap.com/sap/support/notes/1944799) (Nota de SAP 1944799: Instrucciones de SAP HANA para la instalación del sistema operativo SLES).
* [SAP Note 2205917: SAP HANA DB recommended OS settings for SLES 12 for SAP applications](https://launchpad.support.sap.com/#/notes/2205917/E) (Nota de SAP 2205917: Configuración recomendada del sistema operativo de SAP HANA DB para SLES 12 en aplicaciones SAP).
* [SAP Note 1391070: Linux UUID solutions](https://launchpad.support.sap.com/#/notes/1391070) (Nota SAP 1391070: Soluciones UUID de Linux).
* [SAP Note 2009879: SAP HANA guidelines for Red Hat Enterprise Linux (RHEL) operating system](https://launchpad.support.sap.com/#/notes/2009879) (Nota de SAP 2009879: Directrices de SAP HANA para el sistema operativo Red Hat Enterprise Linux [RHEL]).
* [SAP Note 2292690: SAP HANA DB: Recommended OS settings for RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E) (Nota de SAP 2292690: SAP HANA DB: configuraciones de sistema operativo recomendadas para RHEL 7).

### <a name="sap-monitoring-in-azure"></a>Supervisión de SAP en Azure
Para más información sobre la supervisión de SAP en Azure:

* En [SAP Note 2191498](https://launchpad.support.sap.com/#/notes/2191498/E) (Nota de SAP 2191498) se describe la supervisión mejorada de SAP con máquinas virtuales Linux en Azure. 
* En [SAP Note 1102124](https://launchpad.support.sap.com/#/notes/1102124/E) (Nota de SAP 1102124) se describe información sobre SAPOSCOL en Linux. 
* En [SAP Note 2178632](https://launchpad.support.sap.com/#/notes/2178632/E) (Nota de SAP 2178632) se describen las métricas de supervisión clave para SAP en Microsoft Azure.

### <a name="azure-vm-types"></a>Tipos de máquina virtual de Azure
Los tipos de máquina virtual de Azure y los escenarios de carga de trabajo admitidos por SAP y que se usan con SAP HANA se documentan en [SAP certified IaaS platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html) (Plataformas IaaS certificadas por SAP). 

Los tipos de máquina virtual de Azure que están certificadas por SAP para SAP NetWeaver o la capa de aplicación de S/4HANA se documentan en [SAP Note 1928533: SAP applications on Azure: Supported products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533/E). (Nota de SAP 1928533: Aplicaciones de SAP en Azure: productos y tipos de máquina virtual de Azure admitidos)

> [!NOTE]
> La integración SAP-Linux-Azure no se admite en el modelo de implementación clásica, solo en Azure Resource Manager. 

## <a name="manual-installation-of-sap-hana"></a>Instalación manual de SAP HANA

> [!IMPORTANT]
> Asegúrese de que el sistema operativo seleccionado está certificado por SAP para SAP HANA en los tipos específicos de máquina virtual que usa. La lista de tipos de máquina virtual certificados para SAP HANA y las versiones de sistema operativo correspondientes se puede consultar en [SAP HANA certified IaaS platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) (Plataformas IaaS certificadas por SAP). No olvide consultar la información del tipo de máquina virtual mostrado para obtener una lista completa de las versiones de sistema operativo que SAP HANA admite para ese tipo específico de máquina virtual. En el ejemplo de este documento, hemos usado una versión de sistema operativo SUSE Linux Enterprise Server (SLES) que SAP no admite para SAP HANA en máquinas virtuales de la serie M.
>

En esta guía se describen dos formas diferentes de instalar manualmente SAP HANA en máquinas virtuales de Azure:

* Use SAP Software Provisioning Manager (SWPM) como parte de una instalación distribuida de NetWeaver en el paso de instalación de una instancia de base de datos.
* Utilice la herramienta de administración de ciclos de vida de base de datos SAP HANA (HDBLCM) y luego instale NetWeaver.

También puede utilizar SWPM e instalar todos los componentes, como SAP HANA, el servidor de aplicaciones SAP y la instancia de ASCS, en una sola máquina virtual. Los pasos se describen en este [anuncio del blog de SAP HANA](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Esta opción no se describe en este guía de inicio rápido, pero los problemas que debe tener en cuenta son los mismos.

Antes de iniciar una instalación, le recomendamos que lea la sección "Preparación de las máquinas virtuales de Azure para la instalación manual de SAP HANA", más adelante en esta guía. Si lo hace, puede ayudar a evitar varios errores básicos que se pueden producir cuando se usa solo una configuración predeterminada de máquina virtual de Azure.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Pasos clave para la instalación de SAP HANA cuando se usa SAP SWPM
Esta sección enumera los pasos principales de una instalación manual de SAP HANA de instancia única cuando usa SAP SWPM para realizar una instalación distribuida de SAP NetWeaver 7.5. Los pasos individuales se explican con más detalle en las capturas de pantalla más adelante en esta guía.

1. Cree una red virtual de Azure que incluya dos máquinas virtuales de prueba.
2. Implemente las dos máquinas virtuales de Azure con los sistemas operativos según el modelo de Azure Resource Manager. Este ejemplo usa SUSE Linux Enterprise Server y SLES para SAP Applications 12 SP1. 
3. Conecte dos discos de almacenamiento Azure Estándar o Premium, por ejemplo, discos de 75 GB y 500 GB, a la máquina virtual del servidor de aplicaciones.
4. Conecte discos de Premium Storage a la máquina virtual del servidor de base de datos de HANA. Para más información, consulte la sección "Configuración de disco" más adelante en esta guía.
5. En función de los requisitos de tamaño o procesamiento, conecte varios discos. Después, cree volúmenes seccionados. Use la administración de volúmenes lógicos (LVM) o una herramienta de administración de varios dispositivos (mdadm) en el nivel de sistema operativo dentro de la máquina virtual.
6. Cree sistemas de archivos XFS en los discos conectados o los volúmenes lógicos.
7. Monte los nuevos sistemas de archivos XFS en el nivel de sistema operativo. Use un sistema de archivos para todo el software SAP. Use el otro sistema de archivos para el directorio /sapmnt y las copias de seguridad, por ejemplo. En el servidor de SAP HANA DB, monte los sistemas de archivos XFS en los discos de almacenamiento premium como /hana y /usr/sap. Este proceso es necesario para impedir que el sistema de archivos raíz se llene. El sistema de archivos raíz no es grande en máquinas virtuales de Azure de Linux. 
8. Escriba las direcciones IP locales de las máquinas virtuales de prueba en el archivo /etc/hosts.
9. Especifique el parámetro **nofail** en el archivo /etc/fstab.
10. Establezca los parámetros del kernel de Linux según la versión de sistema operativo Linux que usa. Para más información, consulte las notas de SAP que describen HANA y la sección "Parámetros de kernel" de esta guía.
11. Adición de espacio de intercambio.
12. Opcionalmente, instale un escritorio gráfico en las máquinas virtuales de prueba. De lo contrario, use una instalación remota con SAPinst.
13. Descargue el software SAP de SAP Service Marketplace.
14. Instale la instancia de ASCS de SAP en la máquina virtual del servidor de aplicaciones.
15. Comparta el directorio /sapmnt entre las máquinas virtuales de prueba mediante el uso de NFS. La máquina virtual del servidor de aplicaciones es el servidor NFS.
16. Instale la instancia de base de datos, que incluye HANA, por medio de SWPM en la máquina virtual del servidor de bases de datos.
17. Instale al servidor principal de la aplicación (PAS) en la máquina virtual del servidor de aplicaciones.
18. Inicie SAP Management Console (SAP MC). Conéctese con SAP GUI o HANA Studio, por ejemplo.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Pasos clave para la instalación de SAP HANA cuando se usa HDBLCM
En esta sección se enumeran los pasos principales de una instalación manual de SAP HANA de una única instancia cuando se usa SAP HDBLCM para realizar una instalación distribuida de SAP NetWeaver 7.5. Los pasos individuales se explican con más detalle en las capturas de pantalla que se incluyen en esta guía.

1. Cree una red virtual de Azure que incluya dos máquinas virtuales de prueba.
2. Implemente las dos máquinas virtuales de Azure con los sistemas operativos según el modelo de Azure Resource Manager. Este ejemplo usa SLES y SLES para SAP Applications 12 SP1.
3. Conecte dos discos de almacenamiento Azure Estándar o Premium, por ejemplo, discos de 75 GB y 500 GB, a la máquina virtual del servidor de aplicaciones.
4. Conecte discos de Premium Storage a la máquina virtual del servidor de base de datos de HANA. Para más información, consulte la sección "Configuración de disco" más adelante en esta guía.
5. En función de los requisitos de tamaño o procesamiento, conecte varios discos. Cree volúmenes seccionados mediante la administración de volúmenes lógicos o una herramienta mdadm en el nivel de sistema operativo dentro de la máquina virtual.
6. Cree sistemas de archivos XFS en los discos conectados o los volúmenes lógicos.
7. Monte los nuevos sistemas de archivos XFS en el nivel de sistema operativo. Use un sistema de archivos para todo el software SAP. Use el otro sistema de archivos para el directorio /sapmnt y las copias de seguridad, por ejemplo. En el servidor de SAP HANA DB, monte los sistemas de archivos XFS en los discos de almacenamiento premium como /hana y /usr/sap. Este proceso es necesario para ayudar a impedir que el sistema de archivos raíz se llene. El sistema de archivos raíz no es grande en máquinas virtuales de Azure de Linux.
8. Escriba las direcciones IP locales de las máquinas virtuales de prueba en el archivo /etc/hosts.
9. Especifique el parámetro **nofail** en el archivo /etc/fstab.
10. Establezca los parámetros del kernel según la versión de sistema operativo Linux que usa. Para más información, consulte las notas de SAP que describen HANA y la sección "Parámetros de kernel" de esta guía.
11. Adición de espacio de intercambio.
12. Opcionalmente, instale un escritorio gráfico en las máquinas virtuales de prueba. De lo contrario, use una instalación remota con SAPinst.
13. Descargue el software SAP de SAP Service Marketplace.
14. Cree un grupo "sapsys" con el identificador de grupo 1001 en la máquina virtual del servidor de HANA DB.
15. Instale SAP HANA en la máquina virtual del servidor de base de datos mediante el administrador de ciclos de vida de base de datos de HANA.
16. Instale la instancia de ASCS de SAP en la máquina virtual del servidor de aplicaciones.
17. Comparta el directorio /sapmnt entre las máquinas virtuales de prueba mediante el uso de NFS. La máquina virtual del servidor de aplicaciones es el servidor NFS.
18. Instale la instancia de base de datos, que incluye HANA, por medio de SWPM en la máquina virtual del servidor de bases de datos de HANA.
19. Instale al servidor principal de la aplicación en la máquina virtual del servidor de aplicaciones.
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
Compruebe si hay actualizaciones y correcciones del sistema operativo Linux antes de instalar software adicional. Con la instalación de una revisión, puede evitar una llamada al servicio de soporte técnico.

Asegúrese de que usa:
* SUSE Linux Enterprise Server for SAP Applications.
* Red Hat Enterprise Linux for SAP Applications o Red Hat Enterprise Linux for SAP HANA. 

Si no lo ha hecho ya, registre la implementación del sistema operativo con la suscripción de Linux del proveedor de Linux. SUSE tiene imágenes del SO para aplicaciones SAP que ya incluyen servicios y que se registran automáticamente.

Este es un ejemplo de cómo buscar las revisiones disponibles para SUSE Linux mediante el comando **zypper**:

 `sudo zypper list-patches`

Según el tipo de problema, las revisiones se clasifican por categoría y gravedad. Los valores usados normalmente por categoría son: 
- Seguridad
- Recomendado
- Opcional
- Característica
- Documento
- Yast

Los valores usados normalmente por gravedad son:

- Crítico
- Importante
- Moderado
- Bajo
- Sin especificar

El comando **zypper** busca solo las actualizaciones que necesitan los paquetes instalados. Por ejemplo, puede utilizar este comando:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Puede agregar el parámetro `--dry-run` para probar la actualización sin actualizar realmente el sistema.


### <a name="disk-setup"></a>Configuración de disco
El sistema de archivos raíz en una máquina virtual Linux en Azure tiene un límite de tamaño. Por lo tanto, necesita acoplar espacio en disco adicional a una máquina virtual de Azure para ejecutar SAP. Para las máquinas virtuales de Azure con el servidor de aplicaciones de SAP, el uso de discos de almacenamiento Estándar de Azure puede ser suficiente. Para las máquinas virtuales de Azure con SAP HANA DBMS, el uso de discos de Azure Premium Storage para implementaciones de producción y de no producción.

En función de los [requisitos de almacenamiento de SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), se sugiere la siguiente configuración de Azure Premium Storage: 

| SKU de la máquina virtual | RAM |  /hana/data y /hana/log <br /> seccionado con LVM o mdaadm | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

En la configuración de disco sugerida, el volumen de datos y el volumen de registro de HANA se colocan en el mismo conjunto de discos de Azure Premium Storage que están seccionados con LVM o mdaadm. No es necesario definir ningún nivel de redundancia RAID ya que Azure Premium Storage mantiene tres imágenes de los discos de cara a la redundancia. 

Para asegurarse de que ha configurado almacenamiento suficiente, consulte los [requisitos de almacenamiento de SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) y la [guía de instalación y actualización del servidor de SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Considere también los diferentes volúmenes de rendimiento de disco duro virtual (VHD) de los diferentes discos de Azure Premium Storage, como se documenta en [Discos administrados y almacenamiento prémium de alto rendimiento para máquinas virtuales](../../windows/disks-types.md). 

Puede agregar más discos de almacenamiento premium a las máquinas virtuales de DBMS para almacenar copias de seguridad del registro de transacciones o de base de datos.

Para más información sobre las dos herramientas principales usadas para configurar el seccionamiento, consulte:

* [Configuración del software RAID en Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Configuración del LVM en una máquina virtual Linux en Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para más información sobre cómo conectar discos a máquinas virtuales de Azure que ejecutan Linux como sistema operativo invitado, consulte [Adición de un disco a una máquina virtual de Linux](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Con discos SSD Premium de Azure, puede definir modos de almacenamiento en caché de disco. Para el conjunto seccionados que contiene /hana/data y /hana/log, deshabilite el almacenamiento en caché. Para los otros volúmenes, es decir, discos, establezca el modo de almacenamiento en caché en **ReadOnly**.

Para buscar plantillas de JSON de ejemplo para usarlas para crear máquinas virtuales, vaya a [Plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates).
La plantilla vm-simple-sles es una plantilla básica. Incluye una sección de almacenamiento, con un disco de datos adicional de 100 GB. Use esta plantilla como base. La plantilla se puede adaptar a su configuración específica.

> [!NOTE]
> Es importante conectar el disco de Azure Storage mediante un UUID, como se documenta en [Ejecución de SAP NetWeaver en máquinas virtuales de SUSE Linux de Microsoft Azure](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

En el entorno de prueba, se conectan dos discos de almacenamiento estándar de Azure a la máquina virtual del servidor de aplicaciones SAP, como se indica en la siguiente captura de pantalla. Un disco almacena todo el software de SAP, como NetWeaver 7.5, GUI de SAP y SAP HANA, para la instalación. El segundo disco garantiza que haya suficiente espacio libre disponible para requisitos adicionales. Por ejemplo, los datos de copia de seguridad y prueba y el directorio /sapmnt, es decir, los perfiles de SAP, necesitan compartirse entre todas las máquinas virtuales que pertenecen a la misma infraestructura de SAP.

![Ventana de discos de servidor de aplicaciones de SAP HANA que muestra dos discos de datos y sus tamaños](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Parámetros de kernel
SAP HANA requiere configuraciones específicas de kernel de Linux. Estas configuraciones de kernel no forman parte de las imágenes estándar de la galería de Azure y que se deben establecer manualmente. Dependiendo de si usa SUSE o Red Hat, los parámetros pueden ser diferentes. En las notas de SAP enumeradas anteriormente se proporciona información sobre esos parámetros. En las capturas de pantalla mostradas, se ha utilizado SUSE Linux 12 SP1. 

La disponibilidad general de SLES for SAP Applications 12 y SLES for SAP Applications 12 SP1 tienen una nueva herramienta, **tuned-adm**, que sustituye a la antigua herramienta **sapconf**. Hay un perfil de SAP HANA especial disponible para **tuned-adm**. Para ajustar el sistema para SAP HANA, escriba el siguiente perfil como un usuario raíz:

   `tuned-adm profile sap-hana`

Para más información sobre **tuned-adm**, consulte la [documentación de SUSE](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

En la siguiente captura de pantalla, puede ver cómo **tuned-adm** cambió los valores `transparent_hugepage` y `numa_balancing`, de acuerdo con la configuración necesaria de SAP HANA:

![La herramienta tuned-adm cambia los valores en función de la configuración necesaria de SAP HANA](./media/hana-get-started/image005.jpg)

Para convertir en permanentes las configuraciones de kernel de SAP HANA, se debe usar **grub2** en SLES 12. Para más información sobre **grub2**, consulte la sección [Configuration file structure](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) (Estructura de archivos de configuración) de la documentación de SUSE.

La siguiente captura de pantalla muestra cómo se han cambiado las configuraciones del kernel en el archivo de configuración y después se han compilado mediante **grub2-mkconfig**:

![Configuraciones de kernel cambiadas en el archivo de configuración y compiladas mediante grub2 mkconfig](./media/hana-get-started/image006.jpg)

Otra opción es cambiar las configuraciones por medio de YaST y la configuración de  > **parámetros de kernel** del **cargador de arranque**:

![Pestaña de configuración de parámetros de kernel en el cargador de arranque de YaST](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Sistemas de archivos
La siguiente captura de pantalla muestra los dos sistemas de archivos que se crearon en la máquina virtual del servidor de aplicaciones SAP usando los dos discos de almacenamiento estándar de Azure conectados. Ambos sistemas de archivos son del tipo XFS y están montados en /sapdata y /sapsoftware.

No es obligatorio para estructurar los sistemas de archivos de esta manera. Tiene otras opciones para estructurar el espacio en disco. La consideración más importante que debe tener es la de evitar que el sistema de archivos raíz se quede sin espacio libre.

![Se crearon dos sistemas de archivos en la máquina virtual del servidor de aplicaciones SAP](./media/hana-get-started/image008.jpg)

Para la máquina virtual de base de datos de SAP HANA, cuando se usa SAPinst con SWPM y la opción de instalación **típica** durante una instalación de la base de datos, todo se instala en /hana y /usr/sap. La ubicación predeterminada de la copia de seguridad del registro de SAP HANA se encuentra en /usr/sap. De nuevo, es importante evitar que el sistema de archivos raíz se quede sin espacio de almacenamiento. Asegúrese de que haya suficiente espacio disponible en /hana y /usr/sap antes de instalar SAP HANA mediante SWPM.

Para ver una descripción del diseño del sistema de archivos estándar de SAP HANA, consulte la [guía de instalación y actualización del servidor de SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Dos sistemas de archivos adicionales creados en la máquina virtual del servidor de aplicaciones de SAP](./media/hana-get-started/image009.jpg)

Cuando instala SAP NetWeaver en una imagen estándar de la galería de Azure de SLES/SLES-for-SAP Applications 12, se muestra un mensaje que dice que no hay espacio de intercambio, como se ilustra en la siguiente captura de pantalla. Para descartar este mensaje, puede agregar manualmente un archivo de intercambio mediante **dd**, **mkswap** y **swapon**. Para saber cómo, busque "Adding a Swap File Manually" (Adición manual de un archivo de intercambio) en la sección ["Using the YaST Partitioner"](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) (Uso del particionador YaST) de la documentación de SUSE.

Otra opción consiste en configurar el espacio de intercambio mediante el agente de la máquina virtual Linux. Consulte la [Guía de usuario del Agente de Linux de Azure](../../extensions/agent-linux.md) para más información.

![Mensaje emergente que avisa de que no hay espacio de intercambio suficiente](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>El archivo /etc/hosts
Antes de iniciar la instalación de SAP, asegúrese de que incluir los nombres de host y las direcciones IP de las máquinas virtuales de SAP en el archivo/etc/hosts. Implemente todas las máquinas virtuales de SAP dentro de una red virtual de Azure. Después, use las direcciones IP internas, como se muestra aquí:

![Nombres de host y direcciones IP de las máquinas virtuales de SAP enumeradas en el archivo /etc/hosts](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>El archivo /etc/fstab

Resulta útil agregar el parámetro **nofail** al archivo fstab. De esta manera, si algo fuera mal con los discos, la máquina virtual no dejaría de responder en el proceso de arranque. Pero recuerde que es posible que no haya más espacio en disco disponible y que los procesos llenen el sistema de archivos raíz. Si falta /hana, SAP HANA no se iniciará.

![Adición del parámetro nofail al archivo fstab](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Escritorio GNOME gráfico en SLES 12/SLES for SAP Applications 12
En esta sección se explica cómo:

* Instalar el escritorio GNOME y xrdp en SLES 12/SLES for SAP Applications 12.
* Ejecutar la instancia de SAP MC basada en Java mediante Firefox en SLES 12/SLES for SAP Applications 12.

También puede usar otras alternativas, como Xterminal o VNC, que no se describen en esta guía.

### <a name="install-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Instalación del escritorio GNOME y xrdp en SLES 12/SLES for SAP Applications 12
Si conoce Windows, puede usar fácilmente un escritorio gráfico directamente dentro de las máquinas virtuales Linux de SAP para ejecutar Firefox, Sapinst, GUI de SAP, SAP MC o HANA Studio. Después, puede conectarse a la máquina virtual a través de Protocolo de escritorio remoto (RDP) desde un equipo Windows. En función de las directivas de la empresa sobre cómo agregar GUI a sistemas Linux de producción y que no son de producción, puede que quiera instalar GNOME en su servidor. Siga estos pasos para instalar el escritorio GNOME en una máquina virtual SLES 12/SLES for SAP Applications 12 de Azure.

1. Instale el escritorio GNOME; para ello, escriba el siguiente comando, por ejemplo, en una ventana de PuTTY:

   `zypper in -t pattern gnome-basic`

2. Instale xrdp para permitir una conexión a la máquina virtual a través de RDP:

   `zypper in xrdp`

3. Edite /etc/sysconfig/windowmanager y establezca el administrador de ventanas predeterminado en GNOME:

   `DEFAULT_WM="gnome"`

4. Ejecute **chkconfig** para asegurarse de que xrdp se inicie automáticamente después de un reinicio:

   `chkconfig -level 3 xrdp on`

5. Si hay un problema con la conexión RDP, pruebe a reiniciar, por ejemplo, desde una ventana de PuTTY:

   `/etc/xrdp/xrdp.sh restart`

6. Si el reinicio de xrdp mencionado en el paso anterior no funciona, busque un archivo .pid:

   `check /var/run` 

   Busque `xrdp.pid`. Si lo encuentra, quítelo y pruebe a reiniciar de nuevo.

### <a name="start-sap-mc"></a>Inicio de SAP MC
Después de instalar el escritorio de GNOME, inicie la instancia de SAP MC gráfica basada en Java desde Firefox. Si se ejecuta en una máquina virtual de Azure SLES 12/SLES for SAP Applications 12 VM, podría mostrar un error. El error se produce por el complemento del explorador Java que falta.

La dirección URL para iniciar SAP MC es `<server>:5<instance_number>13`.

Para más información, consulte [Starting the Web-Based SAP Management Console](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm) (Inicio de la instancia de SAP Management Console basada en web).

La captura de pantalla siguiente muestra el mensaje de error que aparece cuando el complemento del explorador de Java falta:

![Mensaje de error que indica que falta el complemento del explorador de Java](./media/hana-get-started/image013.jpg)

Una manera de resolver el problema es instalar el complemento que falta mediante YaST, tal y como se muestra en la captura de pantalla siguiente:

![Uso de YaST para instalar el complemento que falta](./media/hana-get-started/image014.jpg)

Cuando vuelva a escribir la dirección URL de SAP Management Console, se le pedirá que active el complemento:

![Cuadro de diálogo que solicita la activación del complemento](./media/hana-get-started/image015.jpg)

Puede que también reciba un mensaje de error sobre un archivo que falta, javafx.properties. Está relacionado con el requisito de Oracle Java 1.8 for SAP GUI 7.4. Para más información, consulte [SAP Note 2059429](https://launchpad.support.sap.com/#/notes/2059424) (Nota de SAP 2059429).
La versión de IBM Java y el paquete openjdk que se proporciona con SLES/SLES for SAP Applications 12 no incluyen el archivo javafx necesario. La solución es descargar e instalar Java SE8 desde Oracle.

Para información sobre un problema similar con openjdk openSUSE, consulte el hilo de discusión [SAPGui 7.4 Java for openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Instalación manual de SAP HANA: SWPM
La serie de capturas de pantalla de esta sección muestra los pasos principales para instalar SAP NetWeaver 7.5 y SAP HANA SP12 cuando usa SWPM con SAPinst. Como parte de la instalación de NetWeaver 7.5, SWPM también puede instalar la base de datos de HANA como una sola instancia.

En un entorno de prueba de ejemplo, instalamos un servidor de aplicaciones de Advanced Business Application Programming (ABAP). Como se muestra en la siguiente captura de pantalla, se ha usado la opción **Sistema distribuido** para instalar las instancias de ASCS y del servidor de aplicaciones principal en una máquina virtual de Azure. Usamos SAP HANA como el sistema de base de datos en otra máquina virtual de Azure.

![Instancias de ASCS y del servidor de aplicaciones principal instaladas con la opción de "Distributed System" (Sistema distribuido)](./media/hana-get-started/image012.jpg)

Después de instalar la instancia de ASCS en la máquina virtual del servidor de aplicaciones, se identifica mediante un icono verde en SAP Management Console. El directorio /sapmnt, que incluye el directorio del perfil SAP, se debe compartir con la máquina virtual del servidor de base de datos de SAP HANA. El paso de instalación de la base de datos necesita acceso a esta información. La mejor manera de proporcionar acceso es utilizar NFS, que se puede configurar mediante YaST.

![SAP Management Console que muestra la instancia de ASCS instalada en la máquina virtual del servidor de aplicaciones mediante un icono verde](./media/hana-get-started/image016.jpg)

En la máquina virtual del servidor de aplicaciones, el directorio /sapmnt se comparte mediante NFS con las opciones **rw** y **no_root_squash**. Los valores predeterminados son **ro** y **root_squash**, lo que podría producir problemas al instalar la instancia de base de datos.

![Uso compartido del directorio /sapmnt mediante NFS mediante el uso de las opciones rw y no_root_squash](./media/hana-get-started/image017b.jpg)

Como muestra la siguiente captura de pantalla, el recurso compartido /sapmnt de la máquina virtual del servidor de aplicaciones tiene que configurarse en la máquina virtual del servidor de base de datos de SAP HANA mediante el **cliente NFS** y YaST:

![El recurso compartido /sapmnt configurado mediante el cliente NFS](./media/hana-get-started/image018b.jpg)

Para realizar una instalación distribuida de NetWeaver 7.5, es decir, una **instancia de base de datos**, inicie sesión en la máquina virtual del servidor de base de datos de SAP HANA e inicie SWPM:

![Instalación de una instancia de base de datos mediante el inicio de sesión en la máquina virtual del servidor de SAP HANA DB y el inicio de SWPM](./media/hana-get-started/image019.jpg)

Después de seleccionar una instalación **típica** y la ruta de acceso a los medios de instalación, escriba un SID de la base de datos, el nombre de host, el número de instancia y la contraseña de administrador del sistema de base de datos:

![La página de inicio de sesión del administrador del sistema de base de datos de SAP HANA](./media/hana-get-started/image035b.jpg)

Escriba la contraseña para el esquema DBACOCKPIT:

![El cuadro para escribir la contraseña para el esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Escriba una pregunta para la contraseña del esquema SAPABAP1:

![Escriba una pregunta para la contraseña del esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Después de que se ha completado cada tarea, aparece una marca de verificación verde junto a cada fase del proceso de instalación de la base de datos. Se muestra el mensaje "Execution of... Database Instance has completed" (Ejecución de la instancia de base de datos… completada).

![Ventana de tarea completada con el mensaje de confirmación](./media/hana-get-started/image023.jpg)

Tras una instalación correcta, SAP Management Console también muestra la instancia de BD con un icono verde. Muestra la lista completa de los procesos de SAP HANA, como hdbindexserver y hdbcompileserver.

![Ventana de la consola de administración de SAP con la lista de procesos de SAP HANA](./media/hana-get-started/image024.jpg)

La siguiente captura de pantalla muestra las partes de la estructura de archivos en el directorio /hana/shared que SWPM creó durante la instalación de HANA. Como no hay ninguna opción para especificar una ruta de acceso diferente, es importante montar el espacio en disco adicional en el directorio /hana antes de la instalación de SAP HANA mediante SWPM. Este paso evita que el sistema de archivos raíz se quede sin espacio libre.

![La estructura de archivos del directorio /hana/shared creada durante la instalación de HANA](./media/hana-get-started/image025.jpg)

Esta captura de pantalla muestra la estructura de archivos del directorio /usr/sap:

![La estructura de archivos del directorio /usr/sap](./media/hana-get-started/image026.jpg)

El último paso de la instalación distribuida de ABAP es instalar la instancia del servidor de aplicaciones principal:

![Instalación de ABAP que muestra la instancia del servidor de aplicaciones principal como último paso](./media/hana-get-started/image027b.jpg)

Después de que se han instalado la instancia del servidor de aplicaciones principal y GUI de SAP, use la transacción **DBA Cockpit** para confirmar que la instalación de SAP HANA finalizó correctamente:

![Ventana DBA Cockpit que confirma la instalación correcta](./media/hana-get-started/image028b.jpg)

Como paso final, es posible que desee instalar primero HANA Studio en la máquina virtual del servidor de aplicaciones SAP. Después, conéctese a la instancia de SAP HANA que se ejecuta en la máquina virtual del servidor de base de datos.

![Instalación de SAP HANA Studio en la máquina virtual del servidor de aplicaciones SAP](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Instalación manual de SAP HANA: HDBLCM
Además de instalar SAP HANA como parte de una instalación distribuida mediante SWPM, puede instalar primero HANA de forma independiente por medio de HDBLCM. A continuación, puede instalar, por ejemplo, SAP NetWeaver 7.5. Las capturas de pantalla de esta sección muestran cómo funciona este proceso.

Para más información sobre la herramienta HDBLCM de HANA, consulte:

* [Choose the correct SAP HANA HDBLCM for your task](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm) (Elección de la herramienta HDBLCM de SAP HANA correcta para su tarea).
* [SAP HANA lifecycle management tools](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm) (Herramientas de administración del ciclo de vida SAP HANA).
* [SAP HANA server installation and update guide](https://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf) (Guía de instalación y actualización del servidor SAP HANA).

Lo que quiere es evitar problemas con un valor de identificador de grupo predeterminado para `\<HANA SID\>adm user`, que se crea mediante la herramienta HDBLCM. Antes de instalar SAP HANA mediante HDBLCM, defina un nuevo grupo denominado `sapsys` mediante el identificador de grupo `1001`:

![Nuevo grupo "sapsys" definido mediante el uso del identificador de grupo 1001](./media/hana-get-started/image030.jpg)

La primera vez que inicia HDBLCM, se muestra un sencillo menú de inicio. Seleccione el elemento 1, **Install new system** (Instalar nuevo sistema):

![Opción "Install new system" (Instalar nuevo sistema) en la ventana de inicio de HDBLCM](./media/hana-get-started/image031.jpg)

La captura de pantalla siguiente muestra todas las opciones principales que seleccionó anteriormente.

> [!IMPORTANT]
> Los directorios con nombre para los registros y volúmenes de datos de HANA, la ruta de acceso de instalación, que es /hana/shared en este ejemplo, y /usr/sap no deben formar parte del sistema de archivos raíz. Estos directorios pertenecen a los discos de datos de Azure que se conectaron a la máquina virtual. Para más información, consulte la sección "Configuración de disco". 

Este enfoque ayuda a evitar que el sistema de archivos raíz se quede sin espacio. Observe que el administrador del sistema HANA tiene un identificador de usuario `1005` y forma parte del grupo `sapsys`, con identificador `1001`, que se definió antes de la instalación.

![Lista de todos los componentes de SAP HANA principales seleccionados anteriormente](./media/hana-get-started/image032.jpg)

Compruebe los detalles de `\<HANA SID\>adm user` en el directorio /etc/passwd. Busque `azdadm`, como se muestra en la siguiente captura de pantalla:

![Detalles del usuario \<HANA SID\>adm de HANA mostrados en el directorio /etc/passwd](./media/hana-get-started/image033.jpg)

Después de instalar SAP HANA mediante HDBLCM, puede ver la estructura de archivos en SAP HANA Studio, como se muestra en la siguiente captura de pantalla. El esquema SAPABAP1 que incluye todas las tablas de SAP NetWeaver aún no está disponible.

![Estructura de archivos de SAP HANA en SAP HANA Studio](./media/hana-get-started/image034.jpg)

Después de instalar SAP HANA, puede instalar SAP NetWeaver sobre él. Como se muestra en la siguiente captura de pantalla, la instalación se realizó como una instalación distribuida usando SWPM. Este proceso se describe en las secciones anteriores. Cuando instale la instancia de base de datos mediante SWPM, escriba los mismos datos con HDBLCM. Por ejemplo, escriba el nombre de host, el SID de HANA y el número de instancia. Posteriormente, SWPM usa la instalación existente de HANA y agrega más esquemas.

![Una instalación distribuida que se ha realizado mediante SWPM](./media/hana-get-started/image035b.jpg)

La siguiente captura de pantalla muestra el paso de instalación de SWPM donde escribe los datos acerca del esquema DBACOCKPIT:

![El paso de instalación de SWPM donde se escriben los datos del esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Escriba los datos acerca del esquema SAPABAP1:

![Escritura de los datos acerca del esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Una vez finalizada la instalación de la instancia de base de datos de SWPM, puede ver el esquema SAPABAP1 en SAP HANA Studio:

![El esquema de SAPABAP1 en SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Finalmente, después de que las instalaciones del servidor de aplicaciones de SAP y la GUI de SAP finalicen, puede comprobar la instancia de base de datos de HANA mediante la transacción **DBA Cockpit**:

![La instancia de base de datos de HANA comprobada con la transacción DBA Cockpit](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>Descargas de software de SAP
Puede descargar el software desde SAP Service Marketplace, como se muestra en las siguientes capturas de pantalla.

Descargue NetWeaver 7.5 para Linux/HANA:

 ![Ventana de instalación y actualización de servicios de SAP para la descarga de NetWeaver 7.5](./media/hana-get-started/image001.jpg)

Descargue HANA SP12 Platform Edition:

 ![Ventana de instalación y actualización de servicios de SAP para la descarga de HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)

