---
title: Evaluación de servidores con datos de servidor importados con Azure Migrate Server Assessment
description: Describe cómo evaluar los servidores en el entorno local para la migración a Azure con Azure Migrate Server Assessment y datos importados.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 856f7f7735435579ac14918ee8026f27b222773e
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715516"
---
# <a name="assess-servers-using-imported-data"></a>Evaluación de servidores con datos importados

> [!NOTE]
> Si aún no ve esta característica en el portal de Azure Migrate, espere. Aparecerá aproximadamente la semana siguiente.

En este artículo se explica cómo evaluar servidores locales con [Azure Migrate: Server Assesment](migrate-services-overview.md#azure-migrate-server-assessment-tool), mediante la importación de metadatos de servidor con CSV. Con este método de evaluación, no es necesario configurar el dispositivo Azure Migrate para crear una evaluación. Resulta útil cuando: 

- Desea crear una evaluación inicial rápida antes de implementar el dispositivo.
- No se puede implementar el dispositivo de Azure Migrate en la organización.
- No se pueden compartir credenciales que permitan el acceso a servidores locales.
- Las restricciones de seguridad impiden la recopilación y el envío de datos recopilados por el dispositivo a Azure. Con un archivo importado, puede controlar los datos que comparte y gran cantidad de ellos son opcionales (por ejemplo, las direcciones IP que se proporcionan).


## <a name="before-you-start"></a>Antes de comenzar

Observe lo siguiente:

- Puede agregar hasta un máximo de 20 000 servidores en un único archivo CSV.
- Puede agregar hasta 20 000 servidores en un proyecto Azure Migrate mediante CSV.
- Puede cargar la información del servidor mediante CSV varias veces para Azure Migrate Server Assessment.
- Aunque recopilar información de la aplicación es útil al evaluar el entorno local para la migración, Azure Migrate Server Assessment no realiza actualmente una evaluación en el nivel de la aplicación y no tiene en cuenta las aplicaciones al crear una evaluación.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un proyecto de Azure Migrate.
> * Rellene un archivo CSV con información del servidor.
> * Importe el archivo para agregar información del servidor en Azure Migrate Server Assessment.
> * Cree y revise una evaluación.

> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para obtener instrucciones detalladas, consulte los artículos acerca de los distintos procedimientos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="set-azure-permissions-for-azure-migrate"></a>Configuración de los permisos de Azure para Azure Migrate 

la cuenta de Azure necesita permisos para crear un proyecto de Azure Migrate.

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.


## <a name="set-up-an-azure-migrate-project"></a>Configuración de un proyecto de Azure Migrate

Para configurar un proyecto nuevo de Azure Migrate, siga los pasos que se indican a continuación.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, en **Detectar, evaluar y migrar servidores**, haga clic en **Evaluar y migrar servidores**.

    ![Detectar y evaluar servidores](./media/tutorial-assess-import/assess-migrate.png)

4. En **Introducción**, haga clic en **Agregar herramientas**.
5. En **Migrar proyecto**, seleccione la suscripción a Azure y cree un grupo de recursos, en caso de que no lo tenga.     
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la geografía en que desea crearlo.

    - [Revise](migrate-support-matrix.md#supported-geographies) las ubicaciones geográficas admitidas. La geografía del proyecto solo se utiliza para almacenar los metadatos que se recopilan de las máquinas virtuales locales.
    - Al realizar una migración se puede seleccionar cualquier región de destino.

    ![Crear un proyecto de Azure Migrate](./media/tutorial-assess-import/migrate-project.png)


7. Haga clic en **Next**.
8. En **Seleccione una herramienta de evaluación**, seleccione **Azure Migrate: Server Assessment** > **Siguiente**.

    ![Crear un proyecto de Azure Migrate](./media/tutorial-assess-import/assessment-tool.png)

9. En **Seleccione una herramienta de migración**, seleccione **Omitir por ahora la adición de una herramienta de migración** > **Siguiente**.
10. En **Revisar y agregar herramientas**, revise la configuración y haga clic en **Agregar herramientas**.
11. Espere unos minutos para que se implemente el proyecto de Azure Migrate. Se le dirigirá a la página del proyecto. Si no ve el proyecto, puede acceder a él desde **Servidores** en el panel de Azure Migrate.


## <a name="prepare-the-csv"></a>Preparación del archivo CSV

Descargue la plantilla de CSV y agréguele información de servidor.


### <a name="download-the-template"></a>Descarga de la plantilla

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, haga clic en **Detectar**.
2. En **Detectar máquinas**, seleccione **Import using .CSV** (Importar con un archivo .CSV).
3. Haga clic en **Descargar** para descargar la plantilla .CSV. O bien, puede [descargarla directamente](https://go.microsoft.com/fwlink/?linkid=2108404).

    ![Descarga de la plantilla .CSV](./media/tutorial-assess-import/download-template.png)


### <a name="add-server-information"></a>Incorporación de información del servidor

Recopile los datos del servidor y agréguelos al archivo CSV.

- Para recopilar los datos, puede exportarlos desde herramientas que se usan para la administración de servidores locales, como VMware vSphere o la base de datos de administración de configuración (CMDB).
- Para revisar los datos de ejemplo, descargue el [archivo de ejemplo](https://go.microsoft.com/fwlink/?linkid=2108405).


En la tabla siguiente se resumen los campos de archivo que se van a rellenar.

**Nombre del campo** | **Obligatorio** | **Detalles**
--- | --- | ---
**Nombre del servidor** | Sí | Se recomienda especificar el FQDN. 
**Dirección IP** | Sin | Dirección del servidor.
**Number of cores** (Número de núcleos) | Sí | El número de núcleos de procesador asignados al servidor.
**Memoria** | Sí | RAM total (MB) asignada al servidor.
**OS name** (Nombre de SO) | Sí | Sistema operativo del servidor.
**Versión del sistema operativo** | Sin | Versión del sistema operativo del servidor.
**Número de discos** | Sin | No es necesario si se proporcionan detalles del disco individual.
**Disk 1 size** (Tamaño de disco 1)  | Sin | Tamaño máximo del disco (GB)<br/> Puede agregar detalles de más discos mediante la [adición de columnas](#add-multiple-disks) en la plantilla. Puede agregar hasta ocho discos.
**Disk 1 read ops** (Operaciones de lectura de disco 1) | Sin | Operaciones de lectura de disco por segundo.
**Disk 1 write ops** (Operaciones de escritura de disco 1) | Sin | Operaciones de escritura de disco por segundo.
**Disk 1 read throughput** (Rendimiento de lectura de disco 1) | Sin | Datos leídos del disco por segundo en MB por segundo.
**Disk 1 write throughput** (Rendimiento de escritura de disco 1) | Sin | Datos escritos en el disco por segundo en MB por segundo.
**CPU utilization percentage** (Porcentaje de uso de la CPU) | Sin | Porcentaje de uso de la CPU.
**Memory utilization percentage** (Porcentaje de uso de la memoria) | Sin | Porcentaje de uso de la RAM.
**Total disks read ops** (Total de operaciones de lectura de discos) | Sin | Operaciones de lectura de disco por segundo.
**Total disks write ops** (Total de operaciones de escritura de discos) | Sin | Operaciones de escritura de discos por segundo.
**Total disks read throughput** (Total de rendimiento de lectura de discos) | Sin | Datos leídos del disco en MB por segundo.
**Total disks write throughput** (Total de rendimiento de escritura de discos) | Sin | Datos escritos en el disco en MB por segundo.
**Network in throughput** (Rendimiento de entrada de red) | Sin | Datos recibidos por el servidor en MB por segundo.
**Network out throughput** (Rendimiento de salida de red) | Sin | Datos transmitidos por el servidor en MB por segundo.
**Firmware type** (Tipo de firmware) | Sin | Firmware del servidor. Los valores pueden ser "BIOS" o "UEFI"
**Tipo de servidor** | Sin | Los valores pueden ser "Físico" o "Virtual".
**Hipervisor** | Sin | Hipervisor en el que se está ejecutando una máquina. <br/> Los valores pueden ser "VMware", "Hyper-V", "Xen", "AWS", "GCP" u "Otro".
**Hypervisor version number** (Número de versión de hipervisor) | Sin | Versión del hipervisor.
**Virtual machine ID** (Identificador de máquina virtual) | Sin | Identificador de la máquina virtual. Este es el **InstanceUUid** para máquinas virtuales de VMware vCenter o **identificador de máquina virtual de Hyper-V** para Hyper-V.
**Virtual machine manager ID** (Identificador de Virtual Machine Manager) | Sin | Este es el **InstanceUUid** para VMWare vCenter. No es necesario para Hyper-V.
**MAC address** (Dirección MAC)| Sin | Dirección MAC de servidor.
**BIOS ID** (IDENTIFICADOR DE BIOS) | Sin | Identificador del BIOS de servidor.
**Custom server ID** (Identificador de servidor personalizado)| Sin | Identificadores de servidores únicos locales en el entorno local. <br/> Resulta útil para realizar el seguimiento del servidor importado mediante el identificador local. 
**Application 1 name** (Nombre de aplicación 1) | Sin | Nombre de las cargas de trabajo que se ejecutan en el servidor.<br/> Puede agregar detalles de más aplicaciones si [agrega columnas](#add-multiple-applications) en la plantilla. Puede agregar hasta cinco aplicaciones.
**Application 1 type** (Tipo de aplicación 1) | Sin | Tipo de la carga de trabajo que se ejecuta en el servidor
**Application 1 version** (Versión de aplicación 1) | Sin | Versión de la carga de trabajo que se ejecuta en el servidor.
**Application 1 license expiry** (Expiración de licencia de aplicación 1) | Sin | Expiración de la licencia para la carga de trabajo (si es aplicable).
**Business unit** (Unidad de negocio) | Sin | Unidad de negocio a la que pertenece el servidor.
**Business owner** (Propietario de empresa) | Sin | Propietario de la unidad de negocio.
**Business application name** (Nombre de la aplicación empresarial) | Sin | Nombre de la aplicación a la que pertenece la aplicación.
**Ubicación** | Sin | Centro de datos en el que se encuentra el servidor.
**Server decommission date** (Fecha de retirada del servidor) | Sin | La fecha de retirada del servidor físico o del servidor físico subyacente del servidor virtual

### <a name="add-operating-systems"></a>Incorporación de sistemas operativos

La evaluación reconoce nombres específicos del sistema operativo. Cualquier nombre de sistema operativo que especifique debe coincidir con una de las opciones de la lista de [nombres admitidos](#supported-operating-system-names).


### <a name="add-multiple-disks"></a>Agregar varios discos

La plantilla proporciona los campos predeterminados para el primer disco.  Puede agregar columnas similares para un máximo de ocho discos. 

Por ejemplo, para especificar todos los campos de un segundo disco, agregue las columnas:

Disk 2 size Disk 2 read ops Disk 2 write ops Disk 2 read throughput Disk 2 write throughput

Opcionalmente, puede agregar campos específicos solo para un disco.


### <a name="add-multiple-applications"></a>Incorporación de varias aplicaciones

La plantilla proporciona campos para una sola aplicación. Puede agregar columnas similares para un máximo de cinco aplicaciones.  

Por ejemplo, para especificar todos los campos de una segunda aplicación, agregue las columnas:

Application 2 name Application 2 type Application 2 version Application 2 license expiry


Si lo desea, puede agregar campos específicos solo para una aplicación.

> [!NOTE]
> La información de la aplicación es útil al evaluar el entorno local para la migración. Sin embargo, Azure Migrate Server Assessment no realiza actualmente ninguna evaluación en el nivel de aplicación y no tiene en cuenta las aplicaciones al crear una evaluación.


## <a name="upload-the-server-information"></a>Carga de la información del servidor

Después de agregar información a la plantilla de CSV, importe los servidores en Azure Migrate: Server Assessment.

1. En Azure Migrate > **Detectar máquinas**, vaya a la plantilla rellenada.
2. Haga clic en **Import**.
3. Se muestra el estado de la importación. 
    - Si aparecen advertencias en el estado, puede corregirlas o continuar.
    - Si se mejora la información del servidor según se recomienda en las advertencias, aumenta la precisión de la evaluación.
    - Para ver y corregir advertencias, si aparecen, haga clic en **Download warning details .CSV** (Descargar archivos .CSV de detalles de advertencias). De este modo, se descarga el archivo CSV, con las advertencias agregadas. Puede revisar las advertencias y corregir los problemas según sea necesario. 
    Si aparecen errores en el estado [el estado de la importación es **Failed** (Error)], debe corregirlos para poder continuar con la importación. Para ello, descargue el archivo CSV, que ahora tiene los detalles de error agregados. Revise y solucione los errores según sea necesario. Después, vuelva a cargar el archivo modificado.
4. Si el estado de la importación es **Completed** (Completado), la información del servidor se ha importado.


> [!NOTE]
> Para actualizar la información del servidor cargada en Azure Migrate, vuelva a cargar los datos para el servidor con el mismo **nombre del servidor**. Tenga en cuenta que el campo **Nombre del servidor** no se puede modificar después de importar la plantilla. La eliminación de servidores no se admite actualmente.

## <a name="updating-server-information"></a>Actualización de la información del servidor

Para actualizar la información de un servidor, puede cargar los datos para el servidor de nuevo con el mismo **nombre del servidor**. No puede modificar el campo **Nombre del servidor**. 

La eliminación de servidores no se admite actualmente.

### <a name="verify-servers-in-the-portal"></a>Comprobación de los servidores en el portal

Después de la detección, puede comprobar que los servidores aparecen en Azure Portal.

1. Abra el panel de Azure Migrate.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el icono que muestra el número de **servidores detectados**.
3. Haga clic en la pestaña **Import based** (Basado en la importación).

## <a name="set-up-an-assessment"></a>Configuración de una evaluación

Con Azure Migrate: Server Assessment se pueden crear dos tipos de evaluaciones.

**Valoración** | **Detalles** | **Datos**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los valores de los datos de rendimiento especificados | **Tamaño de máquina virtual recomendado**: se basa en los datos de uso de la CPU y de la memoria.<br/><br/> **Tipo de disco recomendado (disco administrado estándar o Premium**): se basa en IOPS y en el rendimiento de los discos locales.
**Como local** | Evaluaciones que se basan en el tamaño local. | **Tamaño de máquina virtual recomendado**: Según el tamaño del servidor especificado<br/><br> **Tipo de disco recomendado**: se basa en el valor del tipo de almacenamiento que se selecciona para la evaluación.


### <a name="run-an-assessment"></a>Ejecución de una evaluación

Las evaluaciones se realizan como se indica a continuación:

1. Consulte los [procedimientos recomendados](best-practices-assessment.md) para crear evaluaciones.
2. En la pestaña **Servidores**, en el icono **Azure Migrate: Server Assessment**, haga clic en **Evaluar**.

    ![Evaluación](./media/tutorial-assess-physical/assess.png)

2. En **Evaluar los servidores**, especifique el nombre de la evaluación.
3. En **Discovery source** (Origen de detección), seleccione **Machines added via import to Azure Migrate** (Máquinas agregadas a través de la importación a Azure Migrate).
3. Haga clic en **View all** (Ver todo) para revisar la configuración de la valoración.

    ![Propiedades de la evaluación](./media/tutorial-assess-physical/view-all.png)

3. En **Seleccionar o crear un grupo**, seleccione **Crear nuevo** y especifique un nombre de grupo. Un grupo recopila una o varias máquinas virtuales para su evaluación.
4. En **Agregar máquinas al grupo**, seleccione los servidores que se van a agregar al grupo.
5. Haga clic en **Crear evaluación** para crear el grupo y realizar la evaluación.

    ![Crear una evaluación](./media/tutorial-assess-physical/assessment-create.png)

6. Una vez creada la evaluación, se puede ver en **Servidores** > **Azure Migrate: Server Assessment** > **Evaluaciones**.
7. Haga clic en **Exportar la evaluación** para descargarla como un archivo de Excel.



## <a name="review-an-assessment"></a>Revisión de una evaluación

Una evaluación describe:

- **Preparación para Azure**: si los servidores son adecuados para la migración a Azure.
- **Estimación del costo mensual**: los costos mensuales estimados de proceso y almacenamiento por ejecutar los servidores en Azure.
- **Estimación del costo mensual de almacenamiento**: costos estimados del almacenamiento en disco después de la migración.

### <a name="view-an-assessment"></a>Visualización de una evaluación

1. En **Objetivos de migración** >  **Servidores**, haga clic en **Evaluaciones** en **Azure Migrate: Server Assessment**.
2. En **Evaluaciones**, haga clic en una evaluación para abrirla.

    ![Resumen de evaluaciones](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Revisión de la preparación para Azure

1. En **Preparación para Azure**, compruebe si los servidores están listos para la migración a Azure.
2. Revise el estado:
    - **Preparada para Azure**: Azure Migrate recomienda unas estimaciones de tamaño y costo de las máquinas virtuales en la evaluación.
    - **Preparado con condiciones**: muestra los problemas y las soluciones que se sugieren.
    - **No está preparado para Azure**: muestra los problemas y las soluciones que se sugieren.
    - **Preparación desconocida**: se usa cuando Azure Migrate no puede evaluar la preparación debido a problemas de disponibilidad de datos.

2. Haga clic en cualquiera de los estados de **Preparación para Azure**. Puede ver los detalles de la preparación del servidor y explorar en profundidad sus detalles, entre los que se incluye la configuración del proceso, el almacenamiento y la red.

### <a name="review-cost-details"></a>Revisión de los datos de costo

Esta vista muestra el costo estimado de almacenamiento y proceso que conlleva ejecutar máquinas virtuales en Azure.

1. Revise los costos mensuales de proceso y almacenamiento. Los costos se agregan para todos los servidores del grupo evaluado.

    - Las estimaciones de costo se basan en las recomendaciones de tamaño de una máquina, así como en sus discos y propiedades.
    - Se muestran los costos mensuales estimados de proceso y almacenamiento.
    - La estimación de costos es por la ejecución de los servidores en el entorno local como máquinas virtuales IaaS. Azure Migrate Server Assessment no tiene en cuenta los costos de PaaS o SaaS.

2. Puede revisar las estimaciones del costo de almacenamiento mensual. Esta vista muestra los costos de almacenamiento agregados del grupo evaluado, divididos por los diferentes tipos de discos de almacenamiento.
3. Puede explorar en profundidad para ver los detalles de cada una de las máquinas virtuales.

> [!NOTE]
> No se asignan clasificaciones de confianza a las evaluaciones de servidores importadas en Azure Migrate Server Assessment mediante un archivo CSV.


## <a name="supported-operating-system-names"></a>Nombres de los sistemas operativos compatibles

NOMBRE | NOMBRE
--- | ---
**A - H** | 
Apple Mac OS X 10 | Asianux 3<br/>Asianux 4<br/>Asianux 5
CentOS<br/>CentOS 4/5 | CoreOS Linux 
Debian GNU/Linux 4<br/>Debian GNU/Linux 5<br/>Debian GNU/Linux 6<br/>Debian GNU/Linux 7<br/>Debian GNU/Linux 8 | FreeBSD 
**I - R** | 
IBM OS/2 | MS-DOS |
Novell NetWare 5<br/>Novell NetWare 6 | Oracle Linux<br/> Oracle Linux 4/5<br/>Oracle Solaris 10<br/> Oracle Solaris 11 
Red Hat Enterprise Linux 2<br/>Red Hat Enterprise Linux 3<br/>Red Hat Enterprise Linux 4<br/>Red Hat Enterprise Linux 5<br/>Red Hat Enterprise Linux 6<br/>Red Hat Enterprise Linux 7<br/>Red Hat Fedora | 
**S-T** | 
SCO OpenServer 5<br/>SCO OpenServer 6<br/>SCO UnixWare 7 | Serenity Systems eComStation 1<br/>Serenity Systems eComStation 2
Sun Microsystems Solaris 8<br/>Sun Microsystems Solaris 9 | SUSE Linux Enterprise 10<br/> SUSE Linux Enterprise 11<br/>SUSE Linux Enterprise 12<br/>SUSE Linux Enterprise 8/9<br/>SUSE Linux Enterprise 11<br/>SUSE openSUSE
**U-Z** | 
Ubuntu Linux | VMware ESXi 4<br/>VMware ESXi 5<br/>VMware ESXi 6
Windows 10<br/>Windows 2000<br/>Windows 3<br/>Windows 7<br/>Windows 8<br/>Windows 95<br/>Windows 98<br/>Windows NT<br/>Windows Server (R) 2008<br/>Windows Server 2003 | Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016<br/>Windows Server 2019<br/>Windows Server Threshold<br/>Windows Vista<br/>Windows Web Server 2008 R2<br/>Windows XP Professional
    

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Importó servidores en Azure Migrate: Server Assessment con CSV.
> * Crear y revisar una evaluación

Ahora, [implemente un dispositivo](./migrate-appliance.md) para realizar evaluaciones más precisas y recopilar los servidores en grupos para una valoración más profunda con [análisis de dependencias](./concepts-dependency-visualization.md).
