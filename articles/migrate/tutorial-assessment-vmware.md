---
title: Detección y evaluación de VM de VMware locales para la migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo detectar y evaluar VM de VMware locales para la migración a Azure, utilizando el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 12/05/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 04bc43093a6edc66cdbb661a94989f5980445027
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257818"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Detección y evaluación de VM de VMware locales para migración a Azure

El servicio [Azure Migrate](migrate-overview.md) evalúa las cargas de trabajo locales para su migración a Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta que Azure Migrate usará para detectar máquinas virtuales locales.
> * Cree un proyecto de Azure Migrate.
> * Configure una máquina virtual (VM) de recopilador local para detectar VM de VMware locales para la evaluación.
> * Agrupe las VM y cree una evaluación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- **VMware**: las máquinas virtuales que planea migrar deben administrarse mediante vCenter Server en la versión 5.5, 6.0 o 6.5. Además, necesita un host de ESXi que ejecute la versión 5.0 o posterior para implementar la máquina virtual del recopilador.
- **Cuenta de vCenter Server**: necesita una cuenta de solo lectura para acceder a vCenter Server. Azure Migrate usa esta cuenta para detectar las máquinas virtuales locales.
- **Permisos**: en el servidor vCenter Server, necesitará permisos para crear una máquina virtual mediante la importación de un archivo en formato .OVA.

## <a name="create-an-account-for-vm-discovery"></a>Creación de una cuenta para la detección de máquinas virtuales

Azure Migrate necesita acceso a los servidores de VMware para detectar automáticamente las máquinas virtuales para su evaluación. Cree una cuenta de VMware con las siguientes propiedades. Esta cuenta se especifica durante la instalación de Azure Migrate.

- Tipo de usuario: Al menos un usuario de solo lectura
- Permisos: Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura
- Detalles: El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.
- Para restringir el acceso, asigne el rol Sin acceso con Propagar a objetos secundarios a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).


## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Crear un proyecto

1. En Azure Portal, haga clic en **Crear un recurso**.
2. Busque **Azure Migrate** y seleccione el servicio **Azure Migrate** en los resultados de búsqueda. A continuación, haga clic en **Crear**.
3. Especifique un nombre de proyecto y la suscripción de Azure para el proyecto.
4. Cree un nuevo grupo de recursos.
5. Especifique la ubicación geográfica en la que desea crear el proyecto y haga clic en **Crear**. Los proyectos de Azure Migrate solo se pueden crear en las siguientes geografías. Sin embargo, todavía puede planear la migración de cualquier ubicación de Azure de destino. La ubicación geográfica especificada para el proyecto solo se utiliza para almacenar los metadatos que se recopilan a partir de máquinas virtuales locales.

**Geografía** | **Ubicación de almacenamiento**
--- | ---
Estados Unidos | Centro-oeste de EE. UU. o Este de EE. UU.
Azure Government | Gobierno de EE. UU. - Virginia

![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Descarga del dispositivo de recopilador

Azure Migrate crea una VM local conocida como el dispositivo de recopilador. Esta VM detecta VM de VMware locales y envía los metadatos sobre ellas para al servicio Azure Migrate. Para configurar el dispositivo de recopilador, descargue un archivo .OVA e impórtelo en el servidor vCenter local para crear la VM.

1. En el proyecto de Azure Migrate, haga clic en **Introducción** > **Detectar y evaluar** > **Detectar máquinas**.
2. En **Detectar máquinas**, haga clic en **Descargar**, para descargar la aplicación.

    El dispositivo de Azure Migrate se comunica con vCenter Server Detección y genera perfiles continuamente del entorno local con el fin de recopilar datos de uso en tiempo real para cada máquina virtual. Recopila contadores de número máximo de cada métrica (uso de la CPU, uso de memoria etc.). Este modelo no depende de la configuración de las estadísticas de vCenter Server para la recopilación de datos de rendimiento. Puede detener la generación de perfiles continua en cualquier momento desde el dispositivo.

    > [!NOTE]
    > El dispositivo de detección de una sola vez está en desuso, ya que este método dependía de la configuración de las estadísticas de vCenter Server para la disponibilidad de punto de datos de rendimiento y la media recopilada de los contadores de rendimiento, lo que daba lugar a un cálculo de tamaño insuficiente de las máquinas virtuales para la migración a Azure.

    **Gratificación instantánea:** con el dispositivo de detección continua, una vez que la detección se ha completado (tarda un par de horas según el número de máquinas virtuales), podrá crear valoraciones de forma instantánea. Puesto que la recopilación de datos de rendimiento se inicia al iniciar la detección, si desea obtener gratificación instantánea, para el criterio de tamaño en la valoración debe seleccionar *como local*. Para obtener valoraciones basadas en el rendimiento, se recomienda esperar al menos un día después de iniciar la detección para obtener recomendaciones de tamaño que sean confiables.

    El dispositivo solo recopila datos de rendimiento de forma continua, no detecta ningún cambio de configuración en el entorno local (es decir incorporación o eliminación de una máquina virtual, incorporación de un disco, etc.). Si se produce un cambio de configuración en el entorno local, puede hacer lo siguiente para reflejar los cambios en el portal:

    - Adición de elementos (máquinas virtuales, discos, núcleos, etc.): para reflejar estos cambios en Azure Portal, puede detener la detección desde el dispositivo y después iniciarla de nuevo. Así se asegurará de que los cambios se actualizan en el proyecto de Azure Migrate.

    - Eliminación de máquinas virtuales: debido a la forma en que está diseñado el dispositivo, la eliminación de las máquinas virtuales no se refleja aunque detenga e inicie la detección. Esto se debe a que los datos de las detecciones posteriores se agregan a las detecciones más antiguas y no se reemplazan. En este caso, puede simplemente omitir la máquina virtual en el portal quitándola del grupo y recalculando la valoración.


3. En **Copiar las credenciales del proyecto**, copie la clave y el identificador del proyecto. Los necesitará cuando configure el recopilador.

    ![Descarga del archivo .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Comprobación del dispositivo de recopilador

Compruebe que el archivo .OVA es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. El código hash generado debe coincidir con esta configuración.

#### <a name="continuous-discovery"></a>Detección continua

  Para la versión 1.0.10.9 de OVA

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | 169f6449cc1955f1514059a4c30d138b
  SHA1 | f8d0a1d40c46bbbf78cd0caa594d979f1b587c8f
  SHA256 | d68fe7d94be3127eb35dd80fc5ebc60434c8571dcd0e114b87587f24d6b4ee4d

  Para la versión 1.0.10.4 de OVA

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be


#### <a name="one-time-discovery-deprecated-now"></a>Detección de una sola vez (en desuso)

Este modelo está en desuso, se proporcionará soporte técnico para los dispositivos existentes.

  Para la versión 1.0.9.15 de OVA

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
  SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
  SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

  Para la versión 1.0.9.14 de OVA

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  Para la versión 1.0.9.12 de OVA

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

## <a name="create-the-collector-vm"></a>Creación de la VM de recopilador

Importe el archivo descargado en el servidor vCenter Server.

1. En la consola de cliente de vSphere, haga clic en **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).

    ![Implementación de OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo .ova.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para la VM de recopilador y el objeto de inventario en el que se hospedará la VM.
5. En **Host/Cluster** (Host o clúster), especifique el host o clúster donde se ejecutará el recopilador de máquina virtual.
7. En el almacenamiento, especifique el destino de almacenamiento para la VM de recopilador.
8. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
9. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará la VM de recopilador. La red necesita conectividad a Internet, para enviar metadatos a Azure.
10. Revise y confirme la configuración y haga clic en **Finish** (Finalizar).

## <a name="run-the-collector-to-discover-vms"></a>Ejecución del recopilador para detectar VM

1. En la consola del cliente de vSphere, haga clic con el botón derecho en VM > **Open Console** (Abrir consola).
2. Proporcione el idioma, la zona horaria y las preferencias de contraseña para el dispositivo.
3. En el escritorio, haga clic en el acceso directo **Run collector** (Ejecutar recopilador).
4. Haga clic en **Buscar actualizaciones** en la barra superior de la interfaz de usuario del recopilador y compruebe que este está ejecutando la versión más reciente. Si no es así, puede optar por descargar el último paquete de actualización desde el vínculo y actualizar el recopilador.
5. En Azure Migrate Collector, abra **Set Up Prerequisites** (Configurar requisitos previos).
    - Seleccione la nube de Azure a la que tiene previsto migrar (Azure Global o Azure Government).
    - Acepte los términos de licencia y lea la información de terceros.
    - El recopilador comprueba que la VM tenga acceso a Internet.
    - Si la VM tiene acceso a Internet a través de un proxy, haga clic en **Proxy settings** (Configuración de proxy) y especifique el puerto de escucha y la dirección del proxy. Especifique las credenciales si el proxy requiere autenticación. [Obtener más información](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-prerequisites) sobre los requisitos de conectividad de Internet y la [lista de direcciones URL](https://docs.microsoft.com/azure/migrate/concepts-collector#connect-to-urls) a las que tiene acceso el recopilador.

      > [!NOTE]
      > La dirección del proxy se tiene que especificarse con el formato http://ProxyIPAddress o http://ProxyFQDN. Solo se admite un proxy HTTP. Si tiene un proxy interceptor y no ha importado el certificado del mismo, se podría producir un error inicialmente en la conexión a Internet; [obtenga más información](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity-with-intercepting-proxy) acerca de cómo se puede solucionar este problema importando el certificado de proxy como un certificado de confianza en la máquina virtual del recopilador.

    - El recopilador comprueba que el servicio del recopilador se está ejecutando. El servicio se instala de forma predeterminada en la VM de recopilador.
    - Descargue e instale VMware PowerCLI.

6. En **Specify vCenter Server details** (Especificar detalles de vCenter Server), haga lo siguiente:
    - Especifique el nombre (FQDN) o la dirección IP del servidor vCenter.
    - En **Nombre de usuario** y **Contraseña**, especifique las credenciales de cuenta de solo lectura que utilizará el recopilador para detectar las VM en el servidor vCenter.
    - En **Ámbito del grupo**, seleccione un ámbito para la detección de VM. El recopilador solo puede detectar VM dentro del ámbito especificado. El ámbito se puede establecer en una carpeta, centro de datos o clúster específico. No debe contener más de 1500 máquinas virtuales. [Obtener más información](how-to-scale-assessment.md) acerca de cómo puede detectar un entorno mayor.

7. En **Specify migration project** (Especificar proyecto de migración), especifique la clave y el identificador de proyecto de Azure Migrate que copió del portal. Si no los copió, abra Azure Portal desde la VM de recopilador. En la página **Introducción** del proyecto, haga clic en **Detectar máquinas** y copie los valores.  
8. En **Ver el progreso de la recopilación**, supervise el estado de la detección. [Obtener más información](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected) sobre qué datos reúne Azure Migrate Collector.

> [!NOTE]
> El recopilador solo admite "Inglés (Estados Unidos)" como el idioma del sistema operativo y el idioma de la interfaz del recopilador.
> Si modifica la configuración de una máquina que desea evaluar, desencadene la detección de nuevo antes de ejecutar la evaluación. En el recopilador, utilice la opción **Volver a iniciar la recopilación** para hacer esto. Una vez realizada la recopilación, seleccione la opción **Recalcular** para la evaluación en el portal, para obtener resultados actualizados de la evaluación.


### <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

El dispositivo recopilador generará perfiles del entorno local de forma continuada y seguirá enviando los datos de rendimiento en intervalos de una hora. Puede visualizar las máquinas en el portal una hora después de que se inicie la detección.

1. En el proyecto de migración, haga clic en **Administrar** > **Máquinas**.
2. Compruebe que las VM que desea detectar aparecen en el portal.


## <a name="create-and-view-an-assessment"></a>Creación y visualización de una evaluación

Después de detectar las máquinas virtuales en el portal, tiene que agruparlas y crear valoraciones. Podrá crear inmediatamente valoraciones como locales una vez que las máquinas virtuales se detecten en el portal. Se recomienda esperar al menos un día antes de crear evaluaciones basadas en el rendimiento para que las recomendaciones de tamaño sean confiables.

1. En la página **Introducción** del proyecto, haga clic en **+Crear evaluación**.
2. Haga clic en **View all** (Ver todo) para revisar la configuración de la valoración.
3. Cree el grupo y especifique un nombre para él.
4. Seleccione las máquinas que desee agregar al grupo.
5. Haga clic en **Crear evaluación** para crear el grupo y la evaluación.
6. Una vez creada la evaluación, puede verla en **Introducción** > **Panel**.
7. Haga clic en **Exportar la evaluación** para descargarla como un archivo de Excel.

> [!NOTE]
> Es muy recomendable que antes de crear una valoración, se espere al menos un día después del inicio de la detección. Si desea actualizar una evaluación existente con los datos de rendimiento más recientes, puede usar el comando **Recalcular** en la evaluación para actualizarla.

### <a name="assessment-details"></a>Detalles de la valoración

La valoración incluye información sobre si las máquinas virtuales locales son compatibles con Azure, cuál sería el tamaño correcto de la máquina virtual para ejecutarla en Azure y cuáles serían los costos mensuales estimados de Azure.

![Informe de evaluación](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Preparación para Azure

La vista de preparación para Azure en la valoración muestra el estado de preparación de cada máquina virtual. Dependiendo de sus propiedades, cada máquina virtual se puede marcar como:
- Preparada para Azure
- Condicionalmente preparada para Azure
- No está preparada para Azure
- Preparación desconocida

Para las VM que están preparadas, Azure Migrate recomienda un tamaño de VM en Azure. La recomendación de tamaño que realiza Azure Migrate depende del criterio de tamaño que se especifica en las propiedades de la valoración. Si el criterio de tamaño se corresponde con el ajuste de tamaño basado en el rendimiento, se realiza la recomendación de tamaño teniendo en cuenta el historial de rendimiento de las máquinas virtuales (CPU y memoria) y discos (IOPS y rendimiento). Si el criterio de tamaño es "como local", Azure Migrate no tiene en cuenta los datos de rendimiento para las máquinas virtuales y los discos. La recomendación para el tamaño de la máquina virtual en Azure se hace examinando el tamaño de la máquina virtual local y el tamaño del disco se realiza basándose en el tipo de almacenamiento especificado en las propiedades de evaluación (de manera predeterminada son discos Premium). [Más información](concepts-assessment-calculation.md) acerca de cómo se realiza el ajuste de tamaño en Azure Migrate.

Para las máquinas virtuales que no están preparadas o están condicionalmente preparadas para Azure, Azure Migrate explica los problemas de preparación y proporciona los pasos para su corrección.

Las máquinas virtuales para las que Azure Migrate no puede identificar la preparación para Azure (debido a la falta de disponibilidad de datos) se consideran de preparación desconocida.

Además del ajuste de tamaño y la preparación para Azure, Azure Migrate también sugiere herramientas que puede usar para migrar la máquina virtual. Esto requiere una detección más profunda en el entorno local. [Obtenga más información](how-to-get-migration-tool.md) acerca de cómo realizar una detección más profunda mediante la instalación de agentes en los equipos locales. Si los agentes no están instalados en los equipos locales, se recomienda realizar una migración mediante lift-and-shift con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Si los agentes están instalados en el equipo local, Azure Migrate examina los procesos que se ejecutan dentro del equipo e identifica si es un equipo de base de datos o no. Si es un equipo de base de datos, se sugiere [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) como herramienta de migración; de lo contrario, se sugiere Azure Site Recovery.

  ![Preparación para Azure](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Costo mensual estimado

Esta vista muestra el cálculo total y el costo de almacenamiento por ejecutar las máquinas virtuales en ejecución en Azure, además de los detalles de cada máquina. Las estimaciones de costos se calculan según las recomendaciones de tamaño realizadas por Azure Migrate para una máquina, sus discos y las propiedades de la valoración.

> [!NOTE]
> La estimación de costos que proporciona Azure Migrate es por ejecutar las máquinas virtuales locales como infraestructura de Azure como máquinas virtuales de infraestructura como servicio (IaaS) de Azure. Azure Migrate no tiene en cuenta los costos de cualquier plataforma como servicio (PaaS) o software como servicio (SaaS).

Los costos mensuales estimados para el proceso y almacenamiento se agregan para todas las VM del grupo.

![Costo de VM de evaluación](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Clasificación de confianza

Cada valoración de Azure Migrate basada en el rendimiento está asociada con una clasificación de confianza comprendida entre 1 a 5 estrellas (siendo 1 estrella la más baja y 5 estrellas la más alta). La clasificación de confianza se asigna a una valoración que se basa en la disponibilidad de puntos de datos necesarios para calcular tal valoración. La clasificación de confianza de una valoración le ayuda a calcular la confiabilidad de las recomendaciones de tamaño que proporciona Azure Migrate. La clasificación de confianza no es aplicable a valoraciones como locales.

Para ajustar el tamaño basado en el rendimiento, Azure Migrate necesita los datos de uso de la CPU y la memoria de la máquina virtual. Además, para cada disco asociado a la máquina virtual, necesita el valor de IOPS de disco y los datos de rendimiento. De forma similar, para cada adaptador de red asociado a una máquina virtual, Azure Migrate necesita la entrada o la salida de red para realizar el ajuste de tamaño basado en el rendimiento. Si alguno de los números de uso anteriores no está disponible en vCenter Server, la recomendación de tamaño que realiza Azure Migrate podría no ser de confianza. Según el porcentaje de puntos de datos disponibles, se proporciona la clasificación de confianza para la valoración, tal como se indica a continuación:

   **Disponibilidad de puntos de datos** | **Clasificación de confianza**
   --- | ---
   0 % - 20 % | 1 estrella
   21 % - 40 % | 2 estrellas
   41 % - 60 % | 3 estrellas
   61 % - 80 % | 4 estrellas
   81 % - 100 % | 5 estrellas

Debido a uno de los siguientes motivos, puede que las valoraciones no tengan todos los puntos de datos disponibles:

- No generó un perfil de su entorno durante el tiempo que está creando la evaluación. Por ejemplo, si está creando la evaluación con la duración de rendimiento establecida en 1 día, debe esperar al menos un día después de empezar la detección para que se recopilen todos los puntos de datos.

- Se apagaron algunas máquinas virtuales en el período durante el que se calcula la valoración. Si alguna máquina virtual se apagó por algún tiempo, no podremos recopilar los datos de rendimiento durante ese período.

- Algunas máquinas virtuales se crearon en algún momento del período durante el cual se calcula la valoración. Por ejemplo, si va a crear una valoración para el historial de rendimiento del último mes, pero algunas máquinas virtuales se crearon en el entorno hace solo una semana. En tales casos, el historial de rendimiento de las nuevas máquinas virtuales no permanecerá durante toda la duración.

> [!NOTE]
> Si la clasificación de confianza de una valoración se sitúa por debajo de las 5 estrellas, espere al menos un día para que el dispositivo genere el perfil del entorno y luego *recalcule* la valoración. Si no se puede realizar lo anterior, el ajuste de tamaño basado en el rendimiento podría no ser de confianza y se recomienda cambiar a *como local* cambiando las propiedades de la valoración.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información acerca de](how-to-modify-assessment.md) cómo personalizar una valoración según los requisitos.
- Aprenda a crear grupos de evaluación de confianza elevada mediante la [asignación de dependencias de máquina](how-to-create-group-machine-dependencies.md)
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
- [Aprenda](how-to-scale-assessment.md) a detectar y evaluar un entorno grande de VMware.
- [Más información](resources-faq.md) sobre las preguntas más frecuentes sobre Azure Migrate
