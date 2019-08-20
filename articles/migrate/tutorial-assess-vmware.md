---
title: Evaluación de máquinas virtuales de VMware para migrarlas a Azure con Azure Migrate
description: Describe cómo evaluar máquinas virtuales de VMware locales para su migración a Azure mediante Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: 04162f074dba05ac6492c16acb446912296cd673
ms.sourcegitcommit: acffa72239413c62662febd4e39ebcb6c6c0dd00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952096"
---
# <a name="assess-vmware-vms-with-azure-migrate-server-assessment"></a>Evaluación de máquinas virtuales de VMware con Azure Migrate: Server Assessment

En este artículo se muestra cómo evaluar máquinas virtuales de VMware locales mediante Azure Migrate: Herramienta Server Assessment.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas que le ayuda a detectar las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros.



Este tutorial es el segundo de una serie que muestra cómo evaluar máquinas virtuales de VMware y migrarlas a Azure. En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un proyecto de Azure Migrate.
> * Configurar un dispositivo con Azure Migrate que se ejecute de forma local para evaluar las máquinas virtuales.
> * Iniciar la detección continua de máquinas virtuales locales. El dispositivo envía a Azure datos de configuración y rendimiento de las máquinas virtuales detectadas.
> * Agrupar las máquinas virtuales detectadas y evaluar el grupo de máquinas virtuales.
> * Revisar la evaluación.



> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para obtener instrucciones detalladas, consulte los artículos acerca de los distintos procedimientos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

- [Complete](tutorial-prepare-vmware.md) el primer tutorial de esta serie. Si no lo hace, las instrucciones de este tutorial no funcionarán.
- Esto es lo que debería haber hecho en el primer tutorial:
    - [Configurar los permisos de Azure](tutorial-prepare-vmware.md#prepare-azure) para Azure Migrate.
    - [Preparar VMware](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) para la evaluación. Se debe comprobar la configuración de VMware y es preciso tener los permisos necesarios para crear una máquina virtual de VMware con una plantilla OVA. También es preciso tener una cuenta configurada para la detección de máquinas virtuales. Los puertos necesarios deben estar disponibles y es preciso saber cuáles son las direcciones URL necesarias para acceder a Azure.


## <a name="set-up-an-azure-migrate-project"></a>Configuración de un proyecto de Azure Migrate

Para configurar un proyecto nuevo de Azure Migrate, siga los pasos que se indican a continuación.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, en **Detectar, evaluar y migrar servidores**, haga clic en **Evaluar y migrar servidores**.

    ![Detectar y evaluar servidores](./media/tutorial-assess-vmware/assess-migrate.png)

4. En **Introducción**, haga clic en **Agregar herramientas**.
5. En **Migrar proyecto**, seleccione la suscripción a Azure y cree un grupo de recursos, en caso de que no lo tenga.     
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la geografía en que desea crearlo. Se admiten Asia, Europa, Reino Unido y Estados Unidos.

    - La geografía del proyecto solo se utiliza para almacenar los metadatos que se recopilan de las máquinas virtuales locales.
    - Al realizar una migración se puede seleccionar cualquier región de destino.

    ![Crear un proyecto de Azure Migrate](./media/tutorial-assess-vmware/migrate-project.png)


7. Haga clic en **Next**.
8. En **Seleccione una herramienta de evaluación**, seleccione **Azure Migrate: Server Assessment** > **Siguiente**.

    ![Crear un proyecto de Azure Migrate](./media/tutorial-assess-vmware/assessment-tool.png)

9. En **Seleccione una herramienta de migración**, seleccione **Omitir por ahora la adición de una herramienta de migración** > **Siguiente**.
10. En **Revisar y agregar herramientas**, revise la configuración y haga clic en **Agregar herramientas**.
11. Espere unos minutos para que se implemente el proyecto de Azure Migrate. Se le dirigirá a la página del proyecto. Si no ve el proyecto, puede acceder a él desde **Servidores** en el panel de Azure Migrate.


## <a name="set-up-the-appliance-vm"></a>Configuración de la máquina virtual del dispositivo

Azure Migrate: Server Assessment ejecuta un dispositivo ligero de máquina virtual de VMware.

- Dicho dispositivo realiza la detección de la máquina virtual y envía metadatos y datos de rendimiento de estas a Azure Migrate Server Assessment.
- Para configurar el dispositivo:
    - Descargue una plantilla OVA e impórtela en vCenter Server.
    - Crear el dispositivo y comprobar que se puede conectar a Azure Migrate Server Assessment.
    - Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate.
- Se pueden configurar varias aplicaciones para un solo proyecto de Azure Migrate. En todos los dispositivos se admite la detección de hasta 35 000 máquinas virtuales. Se puede detectar un máximo de 10 000 servidores por dispositivo.

### <a name="download-the-ova-template"></a>Descarga de la plantilla OVA

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, haga clic en **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Sí, con VMware vSphere Hypervisor**.
3. Haga clic en **Descargar** para descargar el archivo de plantilla .OVA.

    ![Descarga del archivo .ova](./media/tutorial-assess-vmware/download-ova.png)


### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo OVA es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. En el caso de la versión 2.19.07.30, el código hash generado debe coincidir con estos valores. 

  **Algoritmo** | **Valor del código hash**
  --- | ---
  MD5 | 27230f3b012187860281b912ee661709
  SHA256 | c0a5b5998b7f38ac6e57ea9a808ecc4295795e18f9ca99c367585068883f06e7


### <a name="create-the-appliance-vm"></a>Creación de la máquina virtual del dispositivo

Importe el archivo descargado y cree una máquina virtual.

1. En la consola de cliente de vSphere, haga clic en **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).

    ![Implementación de OVF](./media/tutorial-assess-vmware/deploy-ovf.png)

2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo OVA.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para la máquina virtual. Seleccione el objeto de inventario en el que se hospedará la máquina virtual.
5. En **Host/Cluster** (Host o clúster), especifique el host o clúster en que se ejecutará la máquina virtual.
6. En **Storage**, especifique el destino de almacenamiento de la máquina virtual.
7. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
8. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará la máquina virtual. La red necesita conectividad a Internet para enviar metadatos a Azure Migrate Server Assessment.
9. Revise y confirme la configuración y haga clic en **Finish** (Finalizar).


### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la máquina virtual del dispositivo se puede conectar a las [direcciones URL de Azure](migrate-support-matrix-vmware.md#assessment-url-access-requirements).


### <a name="configure-the-appliance"></a>Configuración del dispositivo

Para configurar el dispositivo, siga estos pasos:

1. En la consola del cliente de vSphere, haga clic con el botón derecho en VM > **Open Console** (Abrir consola).
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Abra un explorador en cualquier equipo que pueda conectarse a la máquina virtual y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio del dispositivo, para lo que debe hacer clic en el acceso directo de la aplicación.
4. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
    - **License** (Licencia): Acepte los términos de licencia y lea la información de terceros.
    - **Connectivity** (Conectividad): la aplicación comprueba que la máquina virtual tiene acceso a Internet. Si la máquina virtual usa un proxy:
        - Haga clic en **Configuración de proxy** y especifique el puerto de escucha y la dirección del proxy con los formatos http://ProxyIPAddress o http://ProxyFQDN.
        - Especifique las credenciales si el proxy requiere autenticación.
        - Solo se admite un proxy HTTP.
    - **Time sync** (Sincronización de hora): para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
    - **Install updates** (Instalar actualizaciones): el dispositivo garantiza que se instalan las actualizaciones más recientes.
    - **Install VDDK** (Instalar VDDK): el dispositivo comprueba si VMWare vSphere Virtual Disk Development Kit (VDDK) está instalado.
        - Azure Migrate: Server Migration usa VDDK para replicar los equipos durante la migración a Azure.
        - Descargue VDDK 6.7 de VMware y extraiga el contenido del archivo zip descargado en la ubicación especificada del dispositivo.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Haga clic en **Iniciar sesión**. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
2. En la pestaña nueva, inicie sesión con sus credenciales de Azure.
    - Inicie sesión con su nombre de usuario y contraseña.
    - No se admite el inicio de sesión con un PIN.
3. Después de iniciar sesión correctamente, vuelva a la aplicación web.
2. Seleccione la suscripción en la que se creó el proyecto de Azure Migrate y selecciónelo.
3. Escriba un nombre para el dispositivo. Este nombre debe ser alfanumérico y no puede tener más de 14 caracteres.
4. Haga clic en **Registrar**.


## <a name="start-continuous-discovery"></a>Inicio de detección continua

Ahora, conéctese desde el dispositivo a vCenter Server e inicie la detección de máquinas virtuales.

1. En **Specify vCenter Server details** (Especificar detalles de vCenter Server), especifique el nombre (nombre de dominio completo) o la dirección IP de vCenter Server. Puede dejar el puerto predeterminado o especificar un puerto personalizado en el que vCenter Server escuche.
2. En **User name** (Nombre de usuario) y **Password** (Contraseña), especifique las credenciales de la cuenta de solo lectura que el dispositivo utilizará para detectar las máquinas virtuales en vCenter Server. Asegúrese de que la cuenta tenga los [permisos necesarios para la detección](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions). Puede limitar el ámbito de la detección. Para ello, debe limitar el acceso a la cuenta de vCenter en consecuencia; [aquí](tutorial-assess-vmware.md#scoping-discovery)encontrará más información acerca del ámbito de la detección.
3. Haga clic en**Validate connection** (Validar conexión) para asegurarse de que el dispositivo puede conectarse a vCenter Server.
4. Una vez establecida la conexión, haga clic en **Save and start discovery** (Guardar e iniciar detección).

De esta forma comienza la detección. Los metadatos de las máquinas virtuales detectadas tardan unos 15 minutos en aparecer en el portal.

### <a name="scoping-discovery"></a>Ámbito de detección

El ámbito de la detección se puede limitar. Para ello, es preciso limitar el acceso de la cuenta de vCenter usada para la detección. Puede establecer el ámbito en centros de datos, clústeres, carpeta de clústeres, hosts, carpeta de hosts o máquinas virtuales individuales de vCenter Server.

Para establecer el ámbito, tiene que realizar los siguientes pasos:
1.  Crear una cuenta de usuario de vCenter.
2.  Definir un nuevo rol con los privilegios necesarios. (<em>necesario para la migración de un servidor sin agente</em>)
3.  Asignar permisos a la cuenta de usuario en los objetos de vCenter.

**Creación de una cuenta de vCenter**
1.  Inicie sesión en el cliente web de vSphere como administrador de vCenter Server.
2.  Haga clic en la pestaña **Administration** > **SSO users and Groups** > **Users** (Administración > Usuarios y grupos de SSO > Usuarios).
3.  Haga clic en el icono **New User** (Nuevo usuario).
4.  Rellene la información necesaria para crear un usuario y haga clic en **OK** (Aceptar).

**Defina un nuevo rol con los privilegios requeridos** (<em>necesario para la migración del servidor sin agente</em>)
1.  Inicie sesión en el cliente web de vSphere como administrador de vCenter Server.
2.  Vaya a **Administration** > **Role Manager** (Administración > Administrador de roles).
3.  Seleccione el servidor vCenter Server en el menú desplegable.
4.  Haga clic en la acción **Create role** (Crear rol).
5.  Escriba un nombre para el nuevo rol. (Por ejemplo, <em>Azure_Migrate</em>).
6.  Asigne estos [permisos](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-vcenter-server-permissions) al rol recién definido.
7.  Haga clic en **OK**.

**Asignación de permisos de objetos de vCenter**

Hay dos enfoques para asignar permisos de objetos de inventario en vCenter a la cuenta de usuario de vCenter con un rol asignado.
- En Server Assesment, el rol de **solo lectura** debe aplicarse a la cuenta de usuario de vCenter para todos los objetos primarios en los que se hospedan las máquinas virtuales que se van a detectar. Se incluyen todos los objetos primarios (host, carpeta de hosts, clúster, carpeta de clústeres) de la jerarquía hasta el centro de datos. Estos permisos se propagan a los objetos secundarios en la jerarquía. 

    De forma similar en Server Migration, debe aplicarse un rol definido por el usuario (se puede llamar <em>Azure _Migrate</em>) con estos [privilegios](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-vcenter-server-permissions) asignados a la cuenta de usuario de vCenter para todos los objetos primarios donde se hospeden las máquinas virtuales que se van a migrar.

![Asignación de permisos](./media/tutorial-assess-vmware/assign-perms.png)

- La alternativa consiste en asignar la cuenta y el rol de usuario en el nivel de centro de datos y propagarlos a los objetos secundarios. A continuación, conceda a la cuenta un rol **Sin acceso** para cada objeto (como las máquinas virtuales) que no quiera detectar o migrar. Esta configuración es complicada. Expone controles de acceso accidental, ya que cada nuevo objeto secundario también concede automáticamente el acceso que se hereda del objeto primario. Por lo tanto, se recomienda usar el primer enfoque.
 
> [!NOTE]
> En la actualidad, Server Assessment no puede detectar máquinas virtuales si a la cuenta de vCenter se le ha concedido acceso en el nivel de carpeta de máquina virtual de vCenter. Si tiene intención de limitar el ámbito de detección por carpetas de máquina virtual, asegúrese de que la cuenta de vCenter tiene asignado acceso de solo lectura asignado a nivel de máquina virtual.  A continuación se proporcionan instrucciones para hacerlo:
>
> 1. Asigne permisos de solo lectura en todas las máquinas virtuales de las carpetas de las máquinas virtuales en las que desea limitar el ámbito de la detección. 
> 2. Conceda acceso de solo lectura a todos los objetos primarios en los que se hospeden las máquinas virtuales. Se incluyen todos los objetos primarios (host, carpeta de hosts, clúster, carpeta de clústeres) de la jerarquía hasta el centro de datos. No es necesario propagar los permisos a todos los objetos secundarios.
> 3. Use las credenciales para la detección al seleccionar el centro de datos como *Ámbito del grupo*. La configuración de RBAC garantiza que el usuario correspondiente de vCenter solo tenga acceso a las máquinas virtuales específicas del inquilino.
>
> Tenga en cuenta que se admite la carpeta de hosts y clústeres.

### <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

Después de la detección, puede comprobar que las máquinas virtuales aparecen en Azure Portal.

1. Abra el panel de Azure Migrate.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el icono que muestra el número de **servidores detectados**.

## <a name="set-up-an-assessment"></a>Configuración de una evaluación

Con Azure Migrate: Server Assessment se pueden crear dos tipos de evaluaciones.

**Valoración** | **Detalles** | **Datos**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los datos de rendimiento recopilados | **Tamaño de máquina virtual recomendado**: se basa en los datos de uso de la CPU y de la memoria.<br/><br/> **Tipo de disco recomendado (disco administrado estándar o Premium**): se basa en IOPS y en el rendimiento de los discos locales.
**Como local** | Evaluaciones que se basan en el tamaño local. | **Tamaño de máquina virtual recomendado**: se basa en el tamaño de la máquina virtual local<br/><br> **Tipo de disco recomendado**: se basa en el valor del tipo de almacenamiento que se selecciona para la evaluación.


### <a name="run-an-assessment"></a>Ejecución de una evaluación

Las evaluaciones se realizan como se indica a continuación:

1. Consulte los [procedimientos recomendados](best-practices-assessment.md) para crear evaluaciones.
2. En la pestaña **Servidores**, en el icono **Azure Migrate: Server Assessment**, haga clic en **Evaluar**.

    ![Evaluación](./media/tutorial-assess-vmware/assess.png)

2. En **Evaluar los servidores**, especifique el nombre de la evaluación.
3. Haga clic en **View all** (Ver todo) para revisar la configuración de la valoración.

    ![Propiedades de la evaluación](./media/tutorial-assess-vmware/view-all.png)

3. En **Seleccionar o crear un grupo**, seleccione **Crear nuevo** y especifique un nombre de grupo. Un grupo recopila una o varias máquinas virtuales para su evaluación.
4. En **Agregar máquinas al grupo**, seleccione las máquinas virtuales que se van a agregar al grupo.
5. Haga clic en **Crear evaluación** para crear el grupo y realizar la evaluación.

    ![Crear una evaluación](./media/tutorial-assess-vmware/assessment-create.png)

6. Una vez creada la evaluación, se puede ver en **Servidores** > **Azure Migrate: Server Assessment** > **Evaluaciones**.
7. Haga clic en **Exportar la evaluación** para descargarla como un archivo de Excel.



## <a name="review-an-assessment"></a>Revisión de una evaluación

Una evaluación describe:

- **Preparación para Azure**: si las máquinas virtuales son adecuadas para la migración a Azure.
- **Estimación del costo mensual**: los costos mensuales estimados de proceso y almacenamiento por ejecutar las máquinas virtuales en Azure.
- **Estimación del costo mensual de almacenamiento**: costos estimados del almacenamiento en disco después de la migración.

### <a name="view-an-assessment"></a>Visualización de una evaluación

1. En **Objetivos de migración** >  **Servidores**, haga clic en **Evaluaciones** en **Azure Migrate: Server Assessment**.
2. En **Evaluaciones**, haga clic en una evaluación para abrirla.

    ![Resumen de evaluaciones](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Revisión de la preparación para Azure

1. En **Preparación para Azure**, compruebe si las máquinas virtuales están listas para su migración a Azure.
2. Revise el estado de la máquina virtual:
    - **Preparada para Azure**: Azure Migrate recomienda unas estimaciones de tamaño y costo de las máquinas virtuales en la evaluación.
    - **Preparado con condiciones**: muestra los problemas y las soluciones que se sugieren.
    - **No está preparado para Azure**: muestra los problemas y las soluciones que se sugieren.
    - **Preparación desconocida**: se usa cuando Azure Migrate no puede evaluar la preparación debido a problemas de disponibilidad de datos.

2. Haga clic en cualquiera de los estados de **Preparación para Azure**. Puede ver los detalles de la preparación de la máquina virtual y explorar en profundidad los detalles de esta, entre los que se incluye la configuración de proceso, almacenamiento y red.



### <a name="review-cost-details"></a>Revisión de los datos de costo

Esta vista muestra el costo estimado de almacenamiento y proceso que conlleva ejecutar máquinas virtuales en Azure.

1. Revise los costos mensuales de proceso y almacenamiento. Los costos se agregan para todas las máquinas virtuales del grupo evaluado.

    - Las estimaciones de costo se basan en las recomendaciones de tamaño de una máquina, así como en sus discos y propiedades.
    - Se muestran los costos mensuales estimados de proceso y almacenamiento.
    - La estimación de costos es para ejecutar las máquinas virtuales locales como máquinas virtuales IaaS. Azure Migrate Server Assessment no tiene en cuenta los costos de PaaS o SaaS.

2. Puede revisar las estimaciones del costo de almacenamiento mensual. Esta vista muestra los costos de almacenamiento agregados del grupo evaluado, divididos por los diferentes tipos de discos de almacenamiento.
3. Puede explorar en profundidad para ver los detalles de cada una de las máquinas virtuales.


### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

Cuando se realizan valoraciones basadas en el rendimiento, se asigna una clasificación de confianza a la evaluación.

![Clasificación de confianza](./media/tutorial-assess-vmware/confidence-rating.png)

- Se concede una clasificación que oscila entre 1 estrella (la más baja) y 5 estrellas (la más alta).
- La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño que proporciona la evaluación.
- La clasificación de confianza se basa en la disponibilidad de los puntos de datos necesarios para calcular tal evaluación.

Estas son las posibles clasificaciones de confianza de una evaluación.

**Disponibilidad del punto de datos** | **Clasificación de confianza**
--- | ---
0 % - 20 % | 1 estrella
21 % - 40 % | 2 estrellas
41 % - 60 % | 3 estrellas
61 % - 80 % | 4 estrellas
81 % - 100 % | 5 estrellas

[Más información](best-practices-assessment.md#best-practices-for-confidence-ratings) acerca de los procedimientos recomendados para las clasificaciones de confianza.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configurar un dispositivo de Azure Migrate
> * Crear y revisar una evaluación

Vaya al tercer tutorial de la serie, donde aprenderá a migrar máquinas virtuales de VMware a Azure con Azure Migrate Server Migration.

> [!div class="nextstepaction"]
> [Migración de máquinas virtuales de VMware](./tutorial-migrate-vmware.md)
