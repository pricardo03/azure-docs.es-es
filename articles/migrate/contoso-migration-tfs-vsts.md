---
title: Refactorizar una implementación de Team Foundation Server a Visual Studio Team Services (VSTS) en Azure | Microsoft Docs
description: Obtenga información acerca de cómo Contoso refactoriza su implementación de TFS local mediante la migración a Visual Studio Team Services (VSTS) en Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 05340c8504150ed568e0d5ce5c8250127e59bca0
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003244"
---
# <a name="contoso-migration--refactor-a-team-foundation-server-deployment-to-visual-studio-team-services-vsts"></a>Migración de Contoso: Refactorizar una implementación de Team Foundation Server a Visual Studio Team Services (VSTS)

En este artículo se muestra cómo Contoso refactoriza su implementación de Team Foundation Server (TFS) en el entorno local mediante la migración a Visual Studio Team Services (VSTS) en Azure. El equipo de desarrollo de Contoso ha utilizado TFS para la colaboración en equipo y el control de código fuente durante los últimos cinco años. Ahora, quiere pasar a una solución basada en la nube para el trabajo de desarrollo y pruebas, y para el control de código fuente. VSTS jugará un papel importante a medida que Contoso se mueva a un modelo DevOps, y desarrollará nuevas aplicaciones nativas de la nube.

Este documento es el onceavo de una serie de artículos que muestran cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. La serie incluye información general y escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios aumenta e iremos agregando otros artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Se describe cómo Contoso prepara su infraestructura local y de Azure para la migración. Se usa la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Se muestra cómo Contoso realiza una valoración de su aplicación de dos niveles local SmartHotel que se ejecuta en VMware. Se evalúan las VM de la aplicación mediante el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de aplicaciones con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehospedar en máquinas virtuales de Azure y una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Muestra cómo Contoso migra la aplicación SmartHotel a Azure. La VM de la aplicación web de la aplicación se migra con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de la aplicación con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview), para migrar a una instancia administrada de SQL. | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Muestra cómo Contoso migra SmartHotel a VM IaaS de Azure mediante el servicio Site Recovery.
[Article 6: Rehost to Azure VMs and SQL Server Availability Groups](contoso-migration-rehost-vm-sql-ag.md) (Artículo 6: Rehospedaje de VM de Azure y grupos de disponibilidad de SQL Server) | Muestra cómo migra Contoso la aplicación SmartHotel. Usa Site Recovery para migrar las VM de la aplicación y el servicio Database Migration para migrar la base de datos de aplicaciones a un grupo de disponibilidad de SQL Server. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso migra la aplicación osTicket de Linux a VM IaaS de Azure mediante Azure Site Recovery.
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Muestra cómo Contoso migra la aplicación osTicket de Linux. Usa Site Recovery para la migración de VM y MySQL Workbench para migrar a una instancia de Azure MySQL Server. | Disponible
[Artículo 9: Refactorizar una aplicación a Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a una aplicación web basada en contenedor de Azure y migra la base de datos de la aplicación a Azure SQL Server. | Disponible
[Artículo 10: Refactorizar una aplicación Linux en Azure App Service y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Muestra cómo Contoso migra la aplicación Linux osTicket a Azure App Service mediante el contenedor Docker de PHP 7.0. El código base para la implementación se migra a GitHub. La base de datos de la aplicación se migra a Azure MySQL. | Disponible
Artículo 11: Refactorizar una implementación de TFS en VSTS | Migrar la aplicación de desarrollo TFS a VSTS en Azure | Este artículo
[Artículo 12: Rediseño de la arquitectura de una aplicación en Azure Containers y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a Azure y rediseña su arquitectura. Se rediseña la arquitectura del nivel web de la aplicación como contenedor de Windows, y la base de datos de la aplicación en una instancia de Azure SQL Database. | Disponible
[Artículo 13: Volver a compilar una aplicación en Azure](contoso-migration-rebuild.md) | Muestra cómo Contoso vuelve a compilar su aplicación de SmartHotel con una gama de funcionalidades y servicios de Azure, como App Services, Azure Kubernetes, Azure Functions, Cognitive Services y Cosmos DB. | Disponible


## <a name="business-drivers"></a>Impulsores del negocio

El equipo de liderazgo de TI ha trabajado estrechamente con su empresa para identificar los objetivos futuros. Los asociados comerciales no están demasiado preocupados por las tecnologías ni las herramientas de desarrollo, pero han capturado estos puntos:

- **Software**: sin tener en cuenta el negocio principal, todas las compañías ahora son compañías de software, incluida Contoso. El liderazgo empresarial está interesado en cómo el departamento de TI puede ayudar a liderar la empresa con nuevas prácticas de trabajo para los usuarios y experiencias para sus clientes.
- **Eficacia**: Contoso debe eliminar los procedimientos innecesarios y optimizar los procesos para los desarrolladores y usuarios. Esto le permitirá cumplir con los requisitos de los clientes de manera más eficaz. La empresa necesita que el departamento de TI sea rápido, sin perder tiempo ni dinero.
- **Agilidad**: el departamento de TI de Contoso debe responder a las necesidades empresariales y reaccionar más rápidamente que el mercado para permitir el éxito en una economía global. Así mismo, no debe ser un impedimento para la empresa.

## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha establecido los objetivos de la migración a VSTS:

- Necesita una herramienta para migrar los datos a la nube. Es posible que sean necesarios algunos procesos manuales.
- El historial y los datos de elementos de trabajo del año pasado deberán migrarse.
- No quiere configurar nuevos nombres de usuario ni contraseñas. Se deben mantener todas las asignaciones del sistema actual.
- Quiere cambiar del Control de versiones de Team Foundation (TFVC) a GIT para el control de código fuente.
- La transición a GIT será una "migración sugerida" que importa solo la versión más reciente del código fuente. Ocurrirá durante un tiempo de inactividad y se detendrá todo el trabajo mientras se desplace el código base. Se entiende que únicamente el historial de la rama maestra actual estará disponible después del movimiento.
- Está preocupado por el cambio y quiere probarlo antes de completarlo totalmente. Quiere conservar el acceso a TFS incluso después de la migración a VSTS.
- Tiene varias colecciones y quiere empezar por una que tenga solo unos pocos proyectos para comprender mejor el proceso.
- Comprende que las colecciones de TFS son una relación uno a uno con cuentas de VSTS, por lo que tendrá varias direcciones URL. Sin embargo, esto coincide con el modelo actual de separación para proyectos y bases de código.


## <a name="proposed-architecture"></a>Arquitectura propuesta

- Contoso moverá sus proyectos de TFS a la nube y dejará de hospedar sus proyectos o control de código fuente localmente.
- TFS se migrará a VSTS.
- Actualmente, Contoso tiene una colección de TFS denominada **ContosoDev**, que se migrará a una cuenta de VSTS denominada **contosodevmigration.visualstudio.com**.
- Los proyectos, los elementos de trabajo, los errores y las iteraciones del último año se migrarán a VSTS.
- Contoso aprovechará su instancia de Azure Active Directory, que se configuró al [implementar su infraestructura de Azure](contoso-migration-infrastructure.md) al principio de la planeación de la migración. 


![Arquitectura del escenario](./media/contoso-migration-tfs-vsts/architecture.png) 


## <a name="migration-process"></a>Proceso de migración

Contoso completará el proceso de migración como se indica a continuación:

1. El proceso requiere mucha preparación. Como primer paso, Contoso debe actualizar su implementación de TFS a un nivel admitido. Actualmente ejecuta TFS 2017 Update 3, pero para usar la migración de base de datos es necesario ejecutar una versión de 2018 compatible con las actualizaciones más recientes.
2. Después de actualizar, deberá ejecutar la herramienta de migración de TFS y validar su colección.
3. Compilará un conjunto de archivos de preparación y realizará un simulacro de migración de prueba.
4. A continuación, ejecutará otra migración: una migración completa con los elementos de trabajo, errores, sprints y código.
5. Después de la migración, moverá el código de TFVC a GIT.

![Proceso de migración](./media/contoso-migration-tfs-vsts/migration-process.png) 


## <a name="scenario-steps"></a>Pasos del escenario

A continuación, le mostramos cómo Azure completará la migración:

> [!div class="checklist"]
> * **Paso 1: Crear una cuenta de almacenamiento de Azure**: esta cuenta de almacenamiento se usará durante el proceso de migración.
> * **Paso 2: Actualizar TFS**: actualizará su implementación a TFS 2018 Upgrade 2. 
> * **Paso 3: Validar colección**: validará su colección de TFS para prepararla para la migración.
> * **Paso 4: Compilar el archivo de preparación**: creará los archivos de migración mediante la herramienta de migración de TFS. 


## <a name="step-1-create-a-storage-account"></a>Paso 1: Creación de una cuenta de almacenamiento

1. En Azure Portal, Contoso crea una cuenta de almacenamiento (**contosodevmigration**).
2. La cuenta se colocará en la región secundaria que se usa para la conmutación por error: Centro de EE. UU. Usa una cuenta de uso general estándar con almacenamiento localmente redundante.

    ![Cuenta de almacenamiento](./media/contoso-migration-tfs-vsts/storage1.png) 


**¿Necesita más ayuda?**

- [Introducción a Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction).
- [Crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).


## <a name="step-2-upgrade-tfs"></a>Paso 2: Actualizar TFS

Contoso actualiza su servidor TFS a TFS 2018 Update 2. Antes de empezar:

- Contoso descarga [TFS 2018 Update 2](https://visualstudio.microsoft.com/downloads/).
- Verifica los [requisitos de hardware](https://docs.microsoft.com/tfs/server/requirements) y lee las [notas de la versión](https://docs.microsoft.com/visualstudio/releasenotes/tfs2018-relnotes) y los [problemas de la actualización](https://docs.microsoft.com/tfs/server/upgrade/get-started#before-you-upgrade-to-tfs-2018).

Realiza la actualización como se muestra a continuación:

1. Para empezar, realiza una copia de seguridad de su servidor TFS (que se ejecuta en una máquina virtual de VMware) y toma una instantánea de VMware.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade1.png) 

2. Se inicia el programa de instalación TFS y elige la ubicación de instalación. El instalador necesita acceso a Internet.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade2.png) 

3. Una vez finalizada la instalación, se inicia el Asistente para configuración de servidor.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade3.png) 

4. Después de la verificación, el asistente completa la actualización.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade4.png) 

5. Contoso comprueba la instalación de TFS mediante la revisión de código, proyectos y elementos de trabajo.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade5.png) 

> [!NOTE]
> Algunas actualizaciones de TFS deben ejecutar el asistente de configuración de características una vez finalizada la actualización. [Más información](https://docs.microsoft.com/vsts/work/customize/configure-features-after-upgrade?utm_source=ms&utm_medium=guide&utm_campaign=vstsdataimportguide&view=vsts).

**¿Necesita más ayuda?**

Obtenga información sobre la [actualización de TFS](https://docs.microsoft.com/tfs/server/upgrade/get-started).

## <a name="step-3-validate-the-tfs-collection"></a>Paso 3: Validar la colección de TFS

Contoso ejecuta la herramienta de migración de TFS en la base de datos de la colección de ContosoDev para validarla antes de la migración.

1. Contoso descarga y descomprime la [herramienta de migración de TFS](https://www.microsoft.com/download/details.aspx?id=54274). Es importante descargar la versión para la actualización de TFS que se está ejecutando. La versión puede comprobarse en la consola de administración.

    ![TFS](./media/contoso-migration-tfs-vsts/collection1.png)

2. Ejecuta la herramienta para llevar a cabo la validación, especificando la dirección URL de la colección de proyectos de equipo:

        **TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev**


3. La herramienta muestra un error.

    ![TFS](./media/contoso-migration-tfs-vsts/collection2.png)

5. Encuentra los archivos de registro en la carpeta **Registros**, justo antes de la ubicación de la herramienta. Se genera un archivo de registro para cada validación principal. **TfsMigration.log** contiene la información principal.

    ![TFS](./media/contoso-migration-tfs-vsts/collection3.png)

4. Contoso encuentra esta entrada relacionada con la identidad.

    ![TFS](./media/contoso-migration-tfs-vsts/collection4.png)

5. Contoso ejecuta **TfsMigration validate /help** en la línea de comandos y ve que el comando **/tenantDomainName** parece ser necesario para validar las identidades.

     ![TFS](./media/contoso-migration-tfs-vsts/collection5.png)

6. Vuelve a ejecutar el comando de validación e incluye este valor, junto con su nombre de Azure AD: **TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com**.

    ![TFS](./media/contoso-migration-tfs-vsts/collection7.png)

7. Aparece la pantalla de inicio de sesión de Azure AD e introduce las credenciales de un usuario administrador global.

     ![TFS](./media/contoso-migration-tfs-vsts/collection8.png)

8. La validación se pasa y la herramienta la confirma.

    ![TFS](./media/contoso-migration-tfs-vsts/collection9.png)



## <a name="step-4-create-the-migration-files"></a>Paso 4: Crear los archivos de migración

Con la validación completa, Contoso puede usar la herramienta de migración de TFS para compilar los archivos de migración.

1. Ejecuta el paso de preparación en la herramienta.

    **TfsMigrator prepare /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus**

     ![Preparación](./media/contoso-migration-tfs-vsts/prep1.png)

    La preparación hace lo siguiente:
    - Examina la colección para obtener una lista de todos los usuarios y rellena el registro de mapa de identificación (**IdentityMapLog.csv**).
    - Prepara la conexión a Azure Active Directory para buscar una coincidencia para cada identidad.
    - Contoso ya ha implementado y sincronizado Azure AD con AD Connect, por lo que con la preparación se deberían poder buscar las identidades coincidentes y marcarlas como activas.

2. Aparece la pantalla de inicio de sesión de Azure AD y Contoso introduce las credenciales de un administrador global.

    ![Preparación](./media/contoso-migration-tfs-vsts/prep2.png)

3. La preparación se completa, y la herramienta notifica que se han generado correctamente los archivos de importación.

    ![Preparación](./media/contoso-migration-tfs-vsts/prep3.png)

4. Contoso puede ver ahora que se crearon los archivos IdentityMapLog.csv y import.json en una carpeta nueva.

    ![Preparación](./media/contoso-migration-tfs-vsts/prep4.png)

5. El archivo import.json proporciona opciones de importación. Incluye información como el nombre de cuenta deseado e información de la cuenta de almacenamiento. La mayoría de los campos se rellenan automáticamente. Algunos campos requieren la intervención del usuario. Contoso abre el archivo y agrega el nombre de la cuenta de VSTS que se creará: **contosodevmigration**. Con este nombre, su dirección URL de VSTS será **contosodevmigration.visualstudio.com**.

    ![Preparación](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > La cuenta debe crearse antes de la migración y puede cambiarse una vez concluida.

6. Revisa el archivo de asignación de registros de identidad que muestra las cuentas que se incluirán en VSTS durante la importación. 

    - Las identidades activas hacen referencia a las identidades que se convertirán en usuarios de VSTS después de la importación.
    - En VSTS, se concederá una licencia a estas identidades y se mostrarán como un usuario en la cuenta después de la migración.
    - Estas identidades se marcan como **Active**  (Activas) en la columna **Expected Import Status**  (Estado de importación esperado) del archivo.

    ![Preparación](./media/contoso-migration-tfs-vsts/prep6.png)



## <a name="step-5-migrate-to-vsts"></a>Paso 5: Migrar a VSTS

Una vez realizada la preparación, Contoso puede centrarse en la migración. Después de ejecutar la migración, pasará de usar TFVC a usar GIT para el control de versiones.

Antes de empezar, Contoso programa el tiempo de inactividad con el equipo de desarrollo, para desconectar la colección para la migración. Estos son los pasos para el proceso de migración:

1. **Desasociar la colección**: los datos de identidad de la colección residen en la base de datos de configuración del servidor de TFS mientras la colección está adjunta y en línea. Cuando se desasocia una colección del servidor TFS, esta toma una copia de los datos de identidad y la empaqueta con la colección para el transporte. Sin estos datos, no se puede ejecutar la parte de la identidad de la importación. Se recomienda que la colección permanezca desasociada hasta que se complete la importación, ya que no hay ninguna manera de importar los cambios que se produjeron durante la importación.
2. **Generar una copia de seguridad**: el siguiente paso del proceso de migración consiste en generar una copia de seguridad que puede importarse en VSTS. Los paquetes de componentes de aplicación de capa de datos (DACPAC) son una característica de SQL Server que permite que los cambios de la base de datos se empaqueten en un único archivo e implementen en otras instancias de SQL. También se pueden restaurar directamente en VSTS y, por lo tanto, usarse como método de empaquetado para introducir los datos de la colección en la nube. Contoso usará la herramienta SqlPackage.exe para generar el archivo DACPAC. Esta herramienta se incluye en SQL Server Data Tools.
3. **Cargar en el almacenamiento**: una vez creado el archivo DACPAC, Contoso lo carga en Azure Storage. Una vez cargado, obtiene una firma de acceso compartido (SAS), para permitir el acceso de la herramienta de migración de TFS al almacenamiento.
4. **Rellenar la importación**: a continuación, Contoso puede rellenar los campos que faltan en el archivo de importación, incluida la configuración de DACPAC. Para empezar, especificará que quiere hacer una importación **DryRun** para comprobar que todo funciona correctamente antes de la migración completa.
5. **Realizar un simulacro**: el simulacro de importaciones ayuda a probar la migración de la colección. Los simulacros tienen una vida limitada y se eliminan antes de que se ejecute la migración de producción. Se eliminan automáticamente tras un período determinado de tiempo. Se incluye una nota acerca de cuándo se eliminará el simulacro en el correo electrónico de operación correcta que se recibirá cuando finalice la importación. Le recomendamos anotar la fecha y realizar una planeación en consecuencia.
6. **Completar la migración de producción**: una vez completada la migración de simulacro, Contoso realiza la migración final actualizando el archivo import.json y vuelve a ejecutar la importación.



### <a name="detach-the-collection"></a>Desasociar la colección

Antes de comenzar y desasociar, Contoso realiza una copia de seguridad local de SQL Server y toma una instantánea de VMware del servidor TFS.

1.  En la consola de administración de TFS, selecciona la recopilación que quiere desasociar (**ContosoDev**).

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate1.png)

2. En **General**, selecciona **Desasociar colección**

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate2.png)

3. En Detach Team Project Collection Wizard (Asistente de la colección de proyectos de equipo para desasociar) > **Mensaje de mantenimiento**, proporciona un mensaje para los usuarios que puede que intenten conectarse a proyectos de la colección.

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate3.png)

4. En **Progreso de desasociación**, supervisa el progreso y, cuando el proceso finaliza, hace clic en **Siguiente**.

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate4.png)

5. En **Comprobaciones de disponibilidad**, al finalizar las comprobaciones, hace clic en **Desasociar**.

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate5.png)

6. Hace clic en **Cerrar** para finalizar.

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate6.png)

6. Ya no se hace referencia a la colección en la consola de administración de TFS.

    ![Migrar](./media/contoso-migration-tfs-vsts/migrate7.png)


### <a name="generate-a-dacpac"></a>Generar un archivo DACPAC

Contoso crea una copia de seguridad (DACPAC) para la importación en VSTS.

- SqlPackage.exe en SQL Server Data Tools se usa para crear el archivo DACPAC. Hay varias versiones de SqlPackage.exe que se instalan con SQL Server Data Tools, ubicadas en carpetas con nombres como 120, 130 y 140. Es importante usar la versión correcta para preparar el archivo DACPAC.
- Las importaciones de TFS 2018 deben usar SqlPackage.exe desde la carpeta 140 o superior.  Para CONTOSOTFS, este archivo se encuentra en la carpeta: **C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140**.


Contoso genera el archivo DACPAC como se indica a continuación:

1. Abre un símbolo del sistema y se desplaza hasta la ubicación de SQLPackage.exe. Escribe el comando siguiente para generar el archivo DACPAC:

    **SqlPackage.exe /sourceconnectionstring:"Data Source=SQLSERVERNAME\INSTANCENAME;Initial Catalog=Tfs_ContosoDev;Integrated Security=True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData=true /p:IgnoreUserLoginMappings=true /p:IgnorePermissions=true /p:Storage=Memory** 

    ![Copia de seguridad](./media/contoso-migration-tfs-vsts/backup1.png)

2. Aparece el siguiente mensaje después de que se ejecute el comando.

    ![Copia de seguridad](./media/contoso-migration-tfs-vsts/backup2.png)

3. Verifica las propiedades del archivo DACPA.

    ![Copia de seguridad](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="update-the-file-to-storage"></a>Actualizar el archivo al almacenamiento

Una vez creado el archivo de DACPAC, Contoso lo carga en Azure Storage.

1. [Descarga e instala](https://azure.microsoft.com/features/storage-explorer/) el Explorador de Azure Storage.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup5.png)

4. Se conecta a su suscripción y localiza la cuenta de almacenamiento que se creó para la migración (**contosodevmigration**). Crea un nuevo contenedor de blobs, **vstsmigration**.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup6.png)

5. Especifica el archivo DACPAC para la carga como un blob en bloques.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup7.png)
    
7. Después de cargar el archivo, hace clic en el nombre de archivo > **Generar SAS**. Expande los contenedores de blobs en la cuenta de almacenamiento, selecciona el contenedor con los archivos de importación y hace clic en **Obtener firma de acceso compartido**.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup8.png)

8. Acepta los valores predeterminados y hace clic en **Crear**. Esto permite el acceso durante 24 horas.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup9.png)

9. Copia la URL de Firma de acceso compartido, para que la pueda usar la herramienta de migración de TFS.

    ![Cargar](./media/contoso-migration-tfs-vsts/backup10.png)

> [!NOTE]
> La migración debe realizarse antes de que transcurra el tiempo permitido o los permisos expirarán.
> No genere ninguna clave SAS desde Azure Portal. Las claves generadas así son para el ámbito de cuenta y no funcionarán con la importación.

### <a name="fill-in-the-import-settings"></a>Rellenar la configuración de importación

Anteriormente, Contoso rellenó de forma parcial el archivo de especificación de importación (import.json). Ahora, debe agregar el resto de configuraciones.

Abre el archivo import.json y rellena los campos siguientes: • Ubicación: la ubicación de la clave SAS que se generó anteriormente.
• DACPAC: establece el nombre en el archivo DACPAC que cargó en la cuenta de almacenamiento. Incluye la extensión ".dacpac".
• ImportType: lo establece en DryRun por ahora.


![Importar configuración](./media/contoso-migration-tfs-vsts/import1.png)


### <a name="do-a-dry-run-migration"></a>Realizar una migración de simulacro

Contoso comienza con una migración de simulacro para asegurarse de que todo funciona de la forma esperada.

1. Abre un símbolo del sistema y busca la ubicación de TfsMigration (C:\TFSMigrator).
2. Como primer paso, valida el archivo de importación. Quiere asegurarse de que el archivo tiene el formato correcto y de que la clave SAS está funcionando.

    **TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly**

3. La validación devuelve un error que indica que la clave SAS necesita un tiempo de expiración más largo.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test1.png)

3. Usa el Explorador de Azure Storage para crear una nueva clave SAS cuya expiración se establece en siete días.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test2.png)

3. Actualiza el archivo import.json y ejecuta la validación de nuevo. Ahora, se completa correctamente.

    **TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly**

    ![Simulacro](./media/contoso-migration-tfs-vsts/test3.png)
    
7. Se inicia el simulacro:

    **TfsMigrator import /importFile:C:\TFSMigrator\import.json**

8. Se emite un mensaje para confirmar la migración. Tenga en cuenta el período de tiempo durante el cual se mantendrán los datos almacenados provisionalmente después del simulacro.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test4.png)

9. Se muestra el inicio de sesión de Azure AD que debería completarse con el inicio de sesión de administración de Contoso.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test5.png)

10. Se muestra un mensaje con información acerca de la importación.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test6.png)

11. Después de 15 minutos o menos, Contoso consulta la dirección URL y ve la siguiente información:

     ![Simulacro](./media/contoso-migration-tfs-vsts/test7.png)

12. Después de que finalice la migración, un director de desarrollo de Contoso inicia sesión en VSTS para comprobar que el simulacro funcionó correctamente. Después de la autenticación, VSTS necesita algunos datos para confirmar la cuenta.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test8.png)

13. En VSTS, el director de desarrollo puede ver que los proyectos se migraron a VSTS. Hay un aviso de que la cuenta se eliminará en 15 días.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test9.png)

14. El director de desarrollo abre uno de los proyectos y abre **Elementos de trabajo** > **Asignados a mí**. Esto muestra que se han migrado los datos de los elementos de trabajo, junto con su identidad.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test10.png)

15. También comprueba otros proyectos y el código para confirmar que se han migrado el código fuente y el historial.

    ![Simulacro](./media/contoso-migration-tfs-vsts/test11.png)


### <a name="run-the-production-migration"></a>Ejecutar la migración de producción

Con el simulacro completado, Contoso pasa a la migración de producción. Elimina el simulacro, actualiza la configuración de importación y vuelve a ejecutar la importación.

1. En el portal de VSTS, elimina la cuenta de simulacro.
2. Actualiza el archivo import.json para establecer **ImportType** en **ProductionRun**.

    ![Producción](./media/contoso-migration-tfs-vsts/full1.png)

3. Inicia la migración como lo hizo para el simulacro: **TfsMigrator import /importFile:C:\TFSMigrator\import.json**.
4. Se muestra un mensaje para confirmar la migración y se advierte de que es posible que los datos se queden almacenados en una ubicación segura, como un área de almacenamiento provisional, durante un máximo de siete días.

    ![Producción](./media/contoso-migration-tfs-vsts/full2.png)

5. En el inicio de sesión de Azure AD, Contoso especifica un inicio de sesión de administrador de Contoso.

    ![Producción](./media/contoso-migration-tfs-vsts/full3.png)

6. Se muestra un mensaje con información acerca de la importación.

    ![Producción](./media/contoso-migration-tfs-vsts/full4.png)

7. Después de unos 15 minutos, Contoso consulta la dirección URL y ve la siguiente información:

    ![Producción](./media/contoso-migration-tfs-vsts/full5.png)

8. Después de que finalice la migración, un director de desarrollo de Contoso inicia sesión en VSTS para comprobar que la migración funcionó correctamente. Después de iniciar sesión, ve que los proyectos se han migrado.

    ![Producción](./media/contoso-migration-tfs-vsts/full6.png)

8. El director de desarrollo abre uno de los proyectos y abre **Elementos de trabajo** > **Asignados a mí**. Esto muestra que se han migrado los datos de los elementos de trabajo, junto con su identidad.

    ![Producción](./media/contoso-migration-tfs-vsts/full7.png)

9. También comprueba otros datos de elementos de trabajo para confirmar.

    ![Producción](./media/contoso-migration-tfs-vsts/full8.png)

15. También comprueba otros proyectos y el código para confirmar que se han migrado el código fuente y el historial.

    ![Producción](./media/contoso-migration-tfs-vsts/full9.png)


### <a name="move-source-control-from-tfvc-to-git"></a>Mover el control de código fuente de TFVC a GIT

Con la migración completa, Contoso quiere cambiar de TFVC a GIT la administración del código fuente. Debe importar el código fuente que se encuentra actualmente en su cuenta de VSTS como repositorios de GIT en la misma cuenta.

1. En el portal de VSTS, abre uno de los repositorios de TFVC (**$/ PolicyConnect**) y lo revisa.

    ![Git](./media/contoso-migration-tfs-vsts/git1.png)

2. Hace clic en el menú desplegable **Código fuente** > **Importación**.

    ![Git](./media/contoso-migration-tfs-vsts/git2.png)

3. En **Tipo de origen**, selecciona **TFVC** y especifica la ruta de acceso al repositorio. Contoso decide no migrar el historial.

    ![Git](./media/contoso-migration-tfs-vsts/git3.png)

    > [!NOTE]
    > Debido a las diferencias en cómo TFVC y GIT almacenan la información del control de versiones, se recomienda no migrar el historial. Este es el enfoque que Microsoft adoptó cuando migró Windows y otros productos desde el control de versiones centralizado a GIT.

4. Después de la importación, Contoso revisa el código.

    ![Git](./media/contoso-migration-tfs-vsts/git4.png)

5. Repite el proceso para el segundo repositorio (**$/ SmartHotelContainer**).

    ![Git](./media/contoso-migration-tfs-vsts/git5.png)

6. Después de revisar el código fuente, los directores de desarrollo aceptan realizar la migración a VSTS. VSTS se convierte ahora en el código fuente para todo el desarrollo dentro de los equipos implicados en la migración.

    ![Git](./media/contoso-migration-tfs-vsts/git6.png)



**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/vsts/git/import-from-tfvc?view=vsts) sobre la importación desde TFVC.

##  <a name="clean-up-after-migration"></a>Limpiar después de la migración

Con la migración completa, Contoso debe hacer lo siguiente:

- Deberá revisar el artículo [posterior a la importación](https://docs.microsoft.com/vsts/articles/migration-post-import?view=vsts) para obtener información acerca de las actividades de importación adicionales.
- También deberá eliminar los repositorios TFVC, o definirlos en modo de solo lectura. Las bases de código no deben usarse, pero se puede hacer referencia a su historial.

## <a name="next-steps"></a>Pasos siguientes

Contoso deberá proporcionar formación sobre GIT y VSTS a los miembros del equipo pertinentes.



