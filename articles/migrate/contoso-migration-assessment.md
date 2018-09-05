---
title: Valorar las cargas de trabajo locales para la migración de Contoso a Azure |Microsoft Docs
description: Obtenga información sobre cómo Contoso valora sus máquinas locales para la migración a Azure con Azure Migrate y Data Migration Assistant.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/26/2018
ms.author: raynew
ms.openlocfilehash: 96f9617e8cabbad1919625fb717940443978b007
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046283"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migración de Contoso: valorar las cargas de trabajo locales para la migración a Azure

En este artículo, Contoso valora su aplicación SmartHotel local para la migración a Azure.

Este artículo forma parte de una serie que documenta cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. En la serie se incluye información general y escenarios de implementación detallados en los que se muestra cómo configurar una infraestructura de migración, valorar la idoneidad de los recursos locales para la migración y ejecutar diferentes tipos de migraciones. La complejidad de los escenarios va en aumento. Con el tiempo, se agregarán más artículos a la serie.

Artículo | Detalles | Status
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Información general de la serie de artículos, estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie. | Disponible
Artículo 3: Evaluación de los recursos locales para la migración a Azure | Contoso ejecuta una valoración de su aplicación local SmartHotel que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant. | Este artículo
[Artículo 4: Rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso ejecuta una migración "lift-and-shift" a Azure de su aplicación SmartHotel local. Migra el front-end de la aplicación mediante el servicio Azure Site Recovery. Migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante Azure Database Migration Service. | Disponible
[Artículo 5: Rehospedaje de una aplicación en VM de Azure](contoso-migration-rehost-vm.md) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible
[Artículo 6: Rehospedaje de una aplicación en máquinas virtuales de Azure y en un grupo de disponibilidad Always On de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Contoso migra la aplicación SmartHotel usando Site Recovery para migrar las máquinas virtuales de la aplicación, y Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server protegido por un grupo de disponibilidad Always On. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Contoso realiza una migración mediante "lift-and-shift" de su aplicación osTicket de Linux a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible
[Artículo 8: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure y Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migra su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Site Recovery. Migra la base de datos de la aplicación a Azure Database for MySQL con MySQL Workbench. | Disponible
[Artículo 9: Refactorización de una aplicación en una aplicación web de Azure y en Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Contoso migra su aplicación SmartHotel a una aplicación web de Azure, y migra la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant. | Disponible
[Artículo 10: Refactorización de una aplicación de Linux en una aplicación web de Azure y en Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. | Disponible
[Artículo 11: Refactorización de Team Foundation Server en Visual Studio Team Services](contoso-migration-tfs-vsts.md) | Contoso migra su implementación local de Team Foundation Server a Visual Studio Team Services en Azure. | Disponible
[Artículo 12: Rediseño de la arquitectura de una aplicación en Azure Containers y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database. | Disponible
[Artículo 13: Volver a compilar una aplicación en Azure](contoso-migration-rebuild.md) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB. | Disponible


## <a name="overview"></a>Información general

Al plantearse la migración a Azure, la empresa Contoso desea realizar una valoración técnica y financiera para determinar si sus cargas de trabajo locales son adecuadas para la migración a la nube. En concreto, el equipo de Contoso quiere valorar la compatibilidad de las máquinas y bases de datos para la migración. Desea calcular la capacidad y los costos de ejecutar los recursos de Contoso en Azure.

Para empezar y comprender mejor las tecnologías implicadas, Contoso valorará dos de sus aplicaciones locales, que se resumen en la tabla siguiente. La compañía valora los escenarios de migración de rehospedaje y refactorización de las aplicaciones para la migración. Obtenga más información sobre el rehospedaje y la refactorización en la [introducción a la migración de Contoso](contoso-migration-overview.md).

Nombre de la aplicación | Plataforma | Capas de aplicación | Detalles
--- | --- | --- | ---
SmartHotel<br/><br/> (administra los requisitos de viajes de Contoso) | Se ejecuta en Windows con una base de datos de SQL Server | Aplicación de dos niveles. El sitio web ASP.NET front-end se ejecuta en una máquina virtual (**WEBVM**) y el servidor SQL Server se ejecuta en otra máquina virtual (**SQLVM**). | Las máquinas virtuales son de VMware y se ejecutan en un host ESXi administrado por vCenter Server.<br/><br/> Puede descargar la aplicación de ejemplo de [GitHub](https://github.com/Microsoft/SmartHotel360).
osTicket<br/><br/> (Aplicación del servicio de atención al cliente de Contoso) | Se ejecuta en Linux/Apache con un PHP de MySQL (LAMP) | Aplicación de dos niveles. El sitio web PHP front-end se ejecuta en una máquina virtual (**OSTICKETWEB**) y la base de datos MySQL se ejecuta en otra máquina virtual (**OSTICKETMYSQL**). | La aplicación la usan las aplicaciones de servicio de atención al cliente con el fin de hacer un seguimiento de problemas para los empleados internos y clientes externos.<br/><br/> Puede descargar el ejemplo de [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Arquitectura actual

En este diagrama se muestra la infraestructura local actual de Contoso:

![Arquitectura actual de Contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso tiene un centro de datos principal. El centro de datos está situado en la ciudad de Nueva York, al este de los Estados Unidos.
- Contoso tiene tres sucursales locales más en los Estados Unidos.
- El centro de datos principal está conectado a Internet con una conexión Metro Ethernet de fibra (500 Mbps).
- Cada una de las sucursales está conectada localmente a Internet mediante conexiones de categoría empresarial, y con túneles VPN con IPSec hacia el centro de datos principal. Esta configuración permite que la red entera de Contoso esté conectada de forma permanente, y además optimiza la conectividad a Internet.
- El centro de datos principal está completamente virtualizado con VMware. Contoso tiene dos hosts de virtualización ESXi 6.5 que están administrados por vCenter Server 6.5.
- Contoso usa Active Directory para la administración de identidades. Contoso usa servidores DNS en la red interna.
- Los controladores de dominio del centro de datos se ejecutan en VM de VMware. Los controladores de dominio de las sucursales locales se ejecutan en servidores físicos.

## <a name="business-drivers"></a>Impulsores del negocio

El equipo directivo de TI de Contoso ha trabajado estrechamente con sus asociados comerciales para comprender lo que quiere lograr la empresa con esta migración:

- **Abordar el crecimiento del negocio**: Contoso está creciendo. Como resultado, ha aumentado la presión sobre los sistemas locales y la infraestructura de la empresa.
- **Aumento de la eficacia**: Contoso debe eliminar los procedimientos innecesarios y optimizar los procesos para sus desarrolladores y usuarios. La empresa necesita que el departamento de TI sea rápido y no malgaste tiempo ni dinero a fin de satisfacer más rápidamente los requisitos del cliente.
- **Aumentar la agilidad**: el equipo de TI de Contoso necesita más capacidad de respuesta a las necesidades de la empresa. Debe ser capaz de anticiparse a los cambios que se producen en el mercado para que la empresa tenga éxito en una economía global. No debe ser un obstáculo ni convertirse en un inhibidor del negocio.
- **Escalar**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.

## <a name="assessment-goals"></a>Objetivos de la valoración

El equipo de la nube de Contoso ha establecido los objetivos de estas valoraciones de la migración:

- Tras la migración, la aplicación de Azure debe tener las mismas funcionalidades de rendimiento que las que tiene actualmente en el entorno de VMWare local de Contoso. Pasar a la nube no significa que el rendimiento de la aplicación sea menos crítico.
- Contoso debe comprender la compatibilidad de sus aplicaciones y bases de datos con los requisitos de Azure. Contoso debe comprender también las opciones de hospedaje en Azure.
- La administración de las bases de datos de Contoso debe minimizarse después de trasladar las aplicaciones a la nube.  
- Contoso quiere comprender no solo las opciones de migración, sino también los costos asociados con la infraestructura una vez trasladada a la nube.

## <a name="assessment-tools"></a>Herramientas de valoración

Contoso usa las herramientas de Microsoft para su valoración de la migración. Estas herramientas se alinean con los objetivos de la compañía y ofrecen a Contoso toda la información que necesita.

Technology | DESCRIPCIÓN | Coste
--- | --- | ---
[Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso usa Data Migration Assistant para valorar y detectar problemas de compatibilidad que podrían afectar a la funcionalidad de su base de datos en Azure. Data Migration Assistant valora la paridad de características entre los orígenes y los destinos SQL. Recomienda mejoras de rendimiento y confiabilidad. | Data Migration Assistant es una herramienta gratuita y descargable.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso usa el servicio Azure Migrate para valorar sus máquinas virtuales de VMware. Azure Migrate valora la idoneidad de las máquinas para la migración. Proporciona estimaciones de tamaño y costos para su ejecución en Azure.  | A partir de mayo de 2018, Azure Migrate es un servicio gratuito.
[Mapa de servicio](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate usa Service Map para mostrar las dependencias entre las máquinas que la compañía desea migrar. | Service Map forma parte de Azure Log Analytics. Actualmente, Contoso puede usar Service Map durante 180 días sin ningún costo.

En este escenario, Contoso descarga y ejecuta Data Migration Assistant para valorar la base de datos de SQL Server local de su aplicación de viajes. Contoso usa Azure Migrate con asignación de dependencias para valorar las máquinas virtuales de la aplicación antes de migrarlas a Azure.

## <a name="assessment-architecture"></a>Arquitectura de valoración

![Arquitectura de evaluación de migraciones](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso es un nombre ficticio que representa una organización empresarial típica.
- Contoso tiene un centro de datos local (**contoso-datacenter**), con controladores de dominio locales (**CONTOSODC1**, **CONTOSODC2**).
- Las máquinas virtuales de VMware se encuentran en hosts VMware ESXi que ejecutan la versión 6.5 (**contosohost1**, **contosohost2**).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**, en ejecución en una máquina virtual).
- La aplicación de viajes SmartHotel tiene estas características:
    - La aplicación se divide en varios niveles entre las dos máquinas virtuales de VMware (**WEBVM** y **SQLVM**).
    - Las máquinas virtuales se encuentran en un host VMware ESXi **contosohost1.contoso.com** (versión 6.5).
    - Las máquinas virtuales ejecutan Windows Server 2008 R2 Datacenter con SP1.
- El entorno de VMware lo administra vCenter Server (**vcenter.contoso.com**), que se ejecuta en una máquina virtual.
- Aplicación del departamento de atención al cliente oSTicket:
    - La aplicación se divide en niveles entre dos máquinas virtuales (**OSTICKETWEB** y **OSTICKETMYSQL**).
    - Las máquinas virtuales se ejecutan en Ubuntu Linux Server 16.04-LTS.
    - **OSTICKETWEB** ejecuta Apache 2 y PHP 7.0.
    - **OSTICKETMYSQL** ejecuta MySQL 5.7.22.

## <a name="prerequisites"></a>Requisitos previos

Contoso y otros usuarios deben cumplir los siguientes requisitos previos para la valoración:

- Permisos de propietario o colaborador para la suscripción de Azure, o para un grupo de recursos en la suscripción de Azure.
- Una instancia local de vCenter Server que ejecute las versiones 6.5, 6.0 o 5.5.
- Una cuenta de solo lectura de vCenter Server, o permisos para crearla.
- Permisos para crear una máquina virtual en la instancia de vCenter Server mediante una plantilla .ova.
- Al menos un host ESXi en el que se ejecute la versión 5.0 o versiones posteriores.
- Al menos dos máquinas virtuales VMware locales, y en una de ellas debe ejecutarse una base de datos de SQL Server.
- Permisos para instalar agentes de Azure Migrate en todas las máquinas virtuales.
- Las máquinas virtuales deben tener conectividad directa a Internet.  
        - Puede restringir el acceso a Internet a las [direcciones URL requeridas](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).  
        - Si las máquinas virtuales no tienen conexión a Internet, la [puerta de enlace de Azure Log Analytics](../log-analytics/log-analytics-oms-gateway.md) debe estar instalada en ellas y se debe dirigir el tráfico a través de esta.
- El FQDN de la máquina virtual que ejecuta la instancia de SQL Server, para la evaluación de la base de datos.
- La instancia de Firewall de Windows que se ejecuta en la máquina virtual de SQL Server debe permitir conexiones externas en el puerto TCP 1433 (valor predeterminado). Esta configuración permite que Data Migration Assistant se conecte.

## <a name="assessment-overview"></a>Introducción a la valoración

Le mostramos cómo realiza Contoso la valoración:

> [!div class="checklist"]
> * **Paso 1: Descarga e instalación de Data Migration Assistant**: Contoso prepara Data Migration Assistant para la valoración de la base de datos de SQL Server local.
> * **Paso 2: valoración de la base de datos mediante Data Migration Assistant**: Contoso se ejecuta y analiza la valoración de la base de datos.
> * **Paso 3: Preparación de la valoración de las máquinas virtuales con Azure Migrate**: Contoso configura las cuentas locales y ajusta la configuración de VMware.
> * **Paso 4: Detección de las máquinas virtuales locales con Azure Migrate**: Contoso crea una máquina virtual de recopilador de Azure Migrate. Después, ejecuta el recopilador para detectar las máquinas virtuales que se valorarán.
> * **Paso 5: Preparación del análisis de dependencias con Azure Migrate**: Contoso instala los agentes de Azure Migrate en las máquinas virtuales para que la compañía pueda ver la asignación de dependencias entre las máquinas virtuales.
> * **Paso 6: valoración de las máquinas virtuales con Azure Migrate**: Contoso comprueba las dependencias, agrupa las máquinas virtuales y ejecuta la valoración. Una vez lista la valoración, Contoso la analiza para preparar la migración.

## <a name="step-1-download-and-install-data-migration-assistant"></a>Paso 1: Descarga e instalación de Data Migration Assistant

1. Contoso descarga Data Migration Assistant del [Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=53595).
    - Data Migration Assistant se puede instalar en cualquier máquina que pueda conectarse a la instancia de SQL Server. Contoso no necesita ejecutarlo en la máquina de SQL Server.
    - Data Migration Assistant no debe ejecutarse en el equipo host de SQL Server.
2. Para iniciar la instalación, Contoso ejecuta el archivo de instalación descargado (DownloadMigrationAssistant.msi).
3. En la página **Finalizar**, Contoso selecciona **Launch Microsoft Data Migration Assistant** (Iniciar Microsoft Data Migration Assistant) antes de finalizar el asistente.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Paso 2: Ejecutar y analizar la valoración de la base de datos para SmartHotel

Ahora, Contoso puede ejecutar una valoración para analizar su base de datos de SQL Server local para la aplicación SmartHotel.

1. En Data Migration Assistant, Contoso selecciona **Nuevo** > **Evaluación** y, a continuación, asigna a la valoración un nombre de proyecto (**SmartHotel**).
2. En **Tipo de servidor de origen**, Contoso selecciona **SQL Server en Azure Virtual Machines**.

    ![Data Migration Assistant: seleccionar origen](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Actualmente, Data Migration Assistant no permite realizar valoraciones para migrar a una instancia administrada de Azure SQL Database. Como alternativa, Contoso usa SQL Server en una máquina virtual de Azure como supuesto objetivo de la valoración.

3. En **Seleccionar versión de destino**, Contoso selecciona SQL Server 2017 como versión de destino. Contoso debe seleccionar esta versión, porque es la versión que usa la instancia administrada de SQL Database.
4. Contoso selecciona los informes para detectar información sobre la compatibilidad y nuevas características:
    - **Problemas de compatibilidad** indica los cambios que pueden interrumpir la migración o que requieren un ajuste menor antes de la migración. Este informe mantiene informado a Contoso sobre las características actualmente en uso que han quedado en desuso. Los problemas se organizan por nivel de compatibilidad.
    - **Nuevas recomendaciones de características** indica las nuevas características de la plataforma de SQL Server de destino que se pueden usar para la base de datos después de la migración. Las recomendaciones de características nuevas se organiza en los encabezados **Rendimiento**, **Seguridad** y **Almacenamiento**.

    ![Data Migration Assistant: problemas de compatibilidad y nuevas características](./media/contoso-migration-assessment/dma-assessment-2.png)

2. En **Conectar a un servidor**, Contoso escribe el nombre de la máquina virtual que ejecuta la base de datos y las credenciales para acceder a ella. Contoso selecciona **Confiar en el certificado del servidor** para asegurarse de que la máquina virtual puede acceder a SQL Server. A continuación, Contoso selecciona **Conectar**.

    ![Data Migration Assistant: conectar a un servidor](./media/contoso-migration-assessment/dma-assessment-3.png)

3. En **Agregar origen**, Contoso agrega la base de datos que desea valorar y, a continuación, selecciona **Siguiente** para iniciar la valoración.
4. Se crea la valoración.

    ![Data Migration Assistant: crear valoración](./media/contoso-migration-assessment/dma-assessment-4.png)

5. En **Revisar los resultados**, Contoso puede ver los resultados de la valoración.

### <a name="analyze-the-database-assessment"></a>Análisis de la evaluación de la base de datos

Los resultados se muestran en cuanto están disponibles. Si Contoso corrige los problemas, debe seleccionar **Reiniciar valoración** para volver a ejecutar la valoración.

1. En el informe **Problemas de compatibilidad**, Contoso comprueba si existen problemas en cada nivel de compatibilidad. Los niveles de compatibilidad se asignan a las versiones de SQL Server como sigue:

    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Data Migration Assistant: informe de problemas de compatibilidad](./media/contoso-migration-assessment/dma-assessment-5.png)

2. En el informe **Recomendaciones de características**, Contoso puede ver las características de rendimiento, seguridad y almacenamiento que la valoración recomienda después de la migración. Se recomiendan varias características, entre las que se incluyen OLTP en memoria, índices de almacén de columnas, Stretch Database, Always Encrypted, enmascaramiento dinámico de datos y cifrado de datos transparente.

    ![Data Migration Assistant: informe de recomendaciones de características](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Contoso debe [habilitar el cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) para todas las bases de datos de SQL Server. Esto es incluso más importante cuando una base de datos está en la nube que cuando está hospedada en el entorno local. El cifrado de datos transparente debe habilitarse solo después de la migración. Si el cifrado de datos transparente ya está habilitado, Contoso debe trasladar el certificado o la clave asimétrica a la base de datos maestra del servidor de destino. Vea cómo [mover una base de datos protegida por cifrado de datos transparente a otra instancia de SQL Server](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Contoso puede exportar la valoración en formato JSON o CSV.

> [!NOTE]
> Para valoraciones a gran escala:
> - Ejecute varias valoraciones simultáneamente y vea su estado en la página **Todas las valoraciones**.
> - Consolide las valoraciones en una [base de datos de SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
> - Consolide las valoraciones en un [informe de Power BI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).

## <a name="step-3-prepare-for-vm-assessment-by-using-azure-migrate"></a>Paso 3: Preparación de la valoración de las máquinas virtuales con Azure Migrate

Contoso debe crear una cuenta de VMware que Azure Migrate usará para detectar automáticamente las máquinas virtuales que se van a valorar, comprobar que dispone de permisos para crear una máquina virtual, anotar los puertos que deben estar abiertos y establecer el nivel de configuración de las estadísticas.

### <a name="set-up-a-vmware-account"></a>Configuración de una cuenta de VMware

La detección de máquinas virtuales requiere una cuenta de solo lectura en vCenter Server, con las siguientes propiedades:

- **User type** (Tipo de usuario): al menos un usuario de solo lectura.
- **Permissions** (Permisos): para el objeto de centro de datos, seleccione la casilla **Propagate to Child Objects** (Propagar a objetos secundarios). En **Role** (Rol), seleccione **Read-only** (Solo lectura).
- **Details** (Detalles): el usuario se asigna en el nivel de centro de datos con acceso a todos los objetos del mismo.
- Para restringir el acceso, asigne el rol **No access** (Sin acceso) con **Propagate to child objects** (Propagar a objetos secundarios) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).

### <a name="verify-permissions-to-create-a-vm"></a>Comprobación de los permisos para crear una máquina virtual

Para comprobar que tiene permisos para crear una máquina virtual, Contoso importa un archivo en formato .OVA. Vea cómo [crear y asignar un rol con privilegios](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Comprobación de puertos

La valoración de Contoso usa la asignación de dependencia. La asignación de dependencias requiere que haya un agente instalado en las máquinas virtuales que se valorarán. El agente debe poder conectarse a Azure desde el puerto TCP 443 de todas las máquinas virtuales. Más información acerca de los [requisitos de la conexión](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).

### <a name="set-statistics-settings"></a>Establecimiento de la configuración de las estadísticas

Antes de comenzar la implementación, Contoso debe establecer la configuración de las estadísticas para vCenter Server en el nivel 3. 

> [!NOTE]
> - Después de establecer el nivel, debe esperar al menos un día antes de ejecutar la valoración. Si no lo hace así, es posible que la valoración no funcione según lo previsto.
> - Si el nivel es mayor que 3, la evaluación funcionará, pero:
>    - No se recopilarán los datos de rendimiento de los discos ni de la red.
>    - Para el almacenamiento, Azure Migrate recomienda un disco estándar en Azure con el mismo tamaño que el disco local.
>    - En el caso de las conexiones de red, en Azure se recomienda un adaptador de red para cada adaptador de red local.
>    - En el caso de los recursos de proceso, Azure Migrate examinará los núcleos y el tamaño de la memoria de la máquina virtual y recomendará una máquina virtual de Azure con la misma configuración. Si hay varios tamaños de máquina virtual de Azure adecuados, se recomienda el que tiene el costo más bajo.
> - Para más información sobre el tamaño con el nivel 3, consulte [Tamaños](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Para establecer el nivel:

1. En vSphere Web Client, Contoso abre la instancia de vCenter Server.
2. Contoso selecciona **Manage** > **Settings** > **General** > **Edit** (Administrar > Configurar > General > Editar).
3. En **Statistics** (Estadísticas), establece la configuración del nivel de estadísticas en **Level 3** (Nivel 3).

    ![Nivel de estadísticas de vCenter Server](./media/contoso-migration-assessment/vcenter-statistics-level.png)

## <a name="step-4-discover-vms"></a>Paso 4: Detectar VM

Para detectar las VM, Contoso crea un proyecto de Azure Migrate. Contoso descarga y configura la máquina virtual del recopilador. Después, Contoso ejecuta el recopilador para detectar las máquinas virtuales locales.

### <a name="create-a-project"></a>Crear un proyecto

1. En [Azure Portal](https://portal.azure.com), Contoso busca **Azure Migrate**. Después, Contoso crea un proyecto.
2. Contoso especifica un nombre de proyecto (**ContosoMigration**) y la suscripción de Azure. Crea un nuevo grupo de recursos de Azure (**ContosoFailoverRG**). 
    > [!NOTE]
    > - Los proyectos de Azure Migrate solo se pueden crear en la región Centro-oeste de EE. UU. o Este de EE. UU.
    > - Puede planear una migración a cualquier ubicación de destino.
    > - La ubicación del proyecto solo se utiliza para almacenar los metadatos que se recopilan de las máquinas virtuales locales.

    ![Azure Migrate: crear proyecto de migración](./media/contoso-migration-assessment/project-1.png)

### <a name="download-the-collector-appliance"></a>Descarga del dispositivo de recopilador

Azure Migrate crea una máquina virtual local conocida como *dispositivo recopilador*. La máquina virtual detecta las máquinas virtuales de VMware locales y envía los metadatos sobre ellas al servicio Azure Migrate. Para configurar el dispositivo recopilador, Contoso descarga una plantilla OVA y la importa en la instancia local de vCenter Server para crear la máquina virtual.

1. En el proyecto de Azure Migrate, Contoso selecciona **Introducción** > **Detectar y evaluar** > **Detectar máquinas**. Contoso descarga el archivo de plantilla OVA.
2. Contoso copia el identificador y la clave del proyecto. El proyecto y el identificador son necesarios para configurar el recopilador.

    ![Azure Migrate - Descarga del archivo OVA](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Comprobación del dispositivo de recopilador

Antes de implementar la máquina virtual, Contoso comprueba que el archivo OVA sea seguro:

1. En el equipo en el que se descargó el archivo, Contoso abre una ventana del símbolo del sistema con permisos de administrador.
2. Contoso ejecuta el siguiente comando para generar el código hash del archivo OVA:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    
    **Ejemplo** 
    
    ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. El código hash generado debe coincidir con esta configuración (versión 1.0.9.12):

    Algoritmo | Valor del código hash
    --- | ---
    MD5 | d0363e5d1b377a8eb08843cf034ac28a
    SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
    SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Creación del dispositivo de recopilador

Ahora, Contoso puede importar el archivo descargado en la instancia de vCenter Server y aprovisionar la máquina virtual del dispositivo recopilador:

1. En la consola de vSphere Client, seleccione **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).

    ![vSphere Web Client: Implementar plantilla de OVF](./media/contoso-migration-assessment/vcenter-wizard.png)

2. En Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF), Contoso selecciona **Source** (Origen) y especifica la ubicación del archivo OVA.
3. En **Name and Location** (Nombre y ubicación), Contoso especifica un nombre para mostrar para la máquina virtual del recopilador. A continuación, selecciona la ubicación del inventario en el que se va a hospedar la máquina virtual. Contoso también especifica el host o clúster donde se ejecutará el dispositivo recopilador.
4. En **Storage** (Almacenamiento), Contoso especifica la ubicación de almacenamiento. En **Disk Format** (Formato de disco), Contoso selecciona cómo quiere aprovisionar el almacenamiento.
5. En **Network Mapping** (Asignación de red), Contoso especifica la red a la que se va a conectar la máquina virtual del recopilador. La red necesita conectividad a Internet, para enviar metadatos a Azure.
6. Contoso revisa la configuración y selecciona **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar). Una vez creado el dispositivo, aparece un mensaje que confirma la finalización correcta.

### <a name="run-the-collector-to-discover-vms"></a>Ejecución del recopilador para detectar VM

Ahora, Contoso ejecuta el recopilador para detectar las máquinas virtuales. Tenga en cuenta que, actualmente, el recopilador solo admite **Inglés (Estados Unidos)** como idioma del sistema operativo e idioma de la interfaz del recopilador.

1. En la consola de vSphere Client, Contoso selecciona **Open Console** (Abrir consola). Contoso especifica las preferencias de idioma, zona horaria y contraseña para la máquina virtual del recopilador.
2. En el escritorio, Contoso selecciona el acceso directo **Run collector** (Ejecutar recopilador).

    ![Consola de vSphere Client: Acceso directo al recopilador](./media/contoso-migration-assessment/collector-shortcut.png)

3. En Azure Migrate Collector, Contoso selecciona **Set Up Prerequisites** (Configurar requisitos previos). Contoso acepta los términos de licencia y lee la información de terceros.
4. El recopilador comprueba que la máquina virtual tiene acceso a Internet, que la hora está sincronizada y que el servicio de recopilador se está ejecuando. (El recopilador se instala de forma predeterminada en la máquina virtual). Contoso también instala VMware PowerCLI.

    > [!NOTE]
    > Se da por hecho que la máquina virtual tiene acceso directo a Internet, sin un proxy.

    ![Azure Migrate Collector: comprobar requisitos previos](./media/contoso-migration-assessment/collector-verify-prereqs.png)

5. En **Especificar los detalles de vCenter Server**, Contoso especifica el nombre completo (FQDN) o la dirección IP de la instancia de vCenter Server y las credenciales de solo lectura que se usan para la detección.
6. Contoso selecciona un ámbito para la detección de máquinas virtuales. El recopilador solo puede detectar máquinas virtuales dentro del ámbito especificado. El ámbito se puede establecer en una carpeta, un centro de datos o un clúster específicos. El ámbito no debe contener más de 1,500 máquinas virtuales.

    ![Especificar los detalles de vCenter Server](./media/contoso-migration-assessment/collector-connect-vcenter.png)

7. En **Especificar el proyecto de migración**, Contoso especifica la clave y el identificador de proyecto de Azure Migrate que se copiaron del portal. Para obtener el identificador y la clave de proyecto, Contoso puede ir a la página **Información general** del proyecto > **Detectar máquinas**.  

    ![Especificación del proyecto de migración](./media/contoso-migration-assessment/collector-connect-azure.png)

8. En **Ver el progreso de la recopilación**, Contoso puede supervisar la detección y comprobar que los metadatos recopilados de las máquinas virtuales están dentro del ámbito. El recopilador proporciona un tiempo de detección aproximado.

    ![Ver el progreso de la recopilación](./media/contoso-migration-assessment/collector-collection-process.png)

### <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

Una vez completada la recopilación, Contoso comprueba que las máquinas virtuales aparecen en el portal.

1. En el proyecto de Azure Migrate, haga clic en **Administrar** > **Máquinas**. Contoso comprueba que se muestran las máquinas virtuales que desea detectar.

    ![Azure Migrate: máquinas detectadas](./media/contoso-migration-assessment/discovery-complete.png)

2. Las máquinas actualmente no tienen instalados los agentes de Azure Migrate. Contoso debe instalar los agentes para ver las dependencias.

    ![Azure Migrate: es necesario instalar el agente](./media/contoso-migration-assessment/machines-no-agent.png)

## <a name="step-5-prepare-for-dependency-analysis"></a>Paso 5: Preparar el análisis de dependencias

Para ver las dependencias existentes entre las máquinas virtuales que Contoso quiere evaluar, este descarga e instala los agentes en las máquinas virtuales de la aplicación. Contoso instala los agentes en todas las máquinas virtuales para sus aplicaciones, tanto para Windows como para Linux.

### <a name="take-a-snapshot"></a>Realización de una instantánea

Para conservar una copia de las máquinas virtuales antes de modificarlas, Contoso crea una instantánea antes de instalar los agentes.

![Instantánea de la máquina](./media/contoso-migration-assessment/snapshot-vm.png)

### <a name="download-and-install-the-vm-agents"></a>Descarga e instalación de los agentes en máquinas virtuales

1. En **Máquinas**, Contoso selecciona la máquina. En la columna **Dependencias**, Contoso selecciona **Requiere instalación**.
2. En el panel **Detectar máquinas**, Contoso:
    - Descarga Microsoft Monitoring Agent (MMA) y Dependency Agent para cada máquina virtual Windows.
    - Descarga MMA y Dependency Agent para cada máquina virtual Linux.
3. Contoso copia la clave y el identificador de área de trabajo. Necesita el identificador de área de trabajo y la clave cuando instala el agente MMA.

    ![Descarga de agente](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Instalar los agentes en máquinas virtuales de Windows

Contoso ejecuta la instalación en cada máquina virtual.

#### <a name="install-the-mma-on-windows-vms"></a>Instalar el MMA en máquinas virtuales de Windows

1. Contoso hace doble clic en el agente descargado.
2. En la página **Destination Folder** (Carpeta de destino), cambie o mantenga la carpeta de instalación predeterminada y seleccione **Next** (Siguiente).
3. En **Agent Setup Options** (Opciones de instalación del agente), Contoso selecciona **Connect the agent to Azure Log Analytics** (Conectar el agente a Azure Log Analytics) > **Next** (Siguiente).

    ![Programa de instalación de Microsoft Monitoring Agent: Opciones de instalación del agente](./media/contoso-migration-assessment/mma-install.png)

4. En **Azure Log Analytics**, Contoso pega la clave y el identificador del área de trabajo que copió del portal.

    ![Programa de instalación de Microsoft Monitoring Agent: Azure Log Analytics](./media/contoso-migration-assessment/mma-install2.png)

5. En **Ready to Install** (Listo para instalar), Contoso instala el agente MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Instalar el agente de dependencia en VM Windows

1. Contoso hace doble clic en la instancia de Dependency Agent descargada.
2. Contoso acepta los términos de licencia y espera a que finalice la instalación.

    ![Programa de instalación de Dependency Agent: Instalación](./media/contoso-migration-assessment/dependency-agent.png)

### <a name="install-the-agents-on-linux-vms"></a>Instalar los agentes en VM Linux

Contoso ejecuta la instalación en cada máquina virtual.

#### <a name="install-the-mma-on-linux-vms"></a>Instalar el MMA en VM Linux

1. Contoso instala la biblioteca ctypes de Python en cada máquina virtual con el comando siguiente:

    `sudo apt-get install python-ctypeslib`
2. Contoso debe ejecutar el comando para instalar al agente MMA como usuario raíz. Para ser usuario raíz, se debe ejecutar el siguiente comando y escribir la contraseña raíz:

    `sudo -i`
3. Contoso instala el agente MMA:
    - Contoso especifica el identificador y la clave del área de trabajo en el comando.
    - Los comandos son para 64 bits.
    - El identificador y la clave principal del área de trabajo se encuentran en el portal de Microsoft Operations Management Suite (OMS). Seleccione **Configuración** y, a continuación, **Orígenes conectados**.
    - Ejecute los siguientes comandos para descargar el agente OMS, validar la suma de comprobación e instalar e incorporar el agente:

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```

#### <a name="install-the-dependency-agent-on-linux-vms"></a>Instalación de Dependency Agent en máquinas virtuales Linux

Después instalar el agente MMA, Contoso puede instalar Dependency Agent en las máquinas virtuales Linux.

1. Dependency Agent se instala en equipos Linux con InstallDependencyAgent-Linux64.bin, un script de shell que tiene un archivo binario autoextraíble. Contoso ejecuta el archivo con sh o agregar permisos de ejecución al propio archivo.

2. Contoso instala Dependency Agent para Linux como raíz:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```

## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Paso 6: Ejecutar y analizar la valoración de la VM

Ahora, Contoso puede comprobar las dependencias de la máquina y crear un grupo. A continuación, ejecuta la valoración para el grupo.

### <a name="verify-dependencies-and-create-a-group"></a>Comprobar las dependencias y crear un grupo

1. Para determinar qué máquinas se analizarán, Contoso selecciona **Ver dependencias**.

    ![Azure Migrate: ver las dependencias de máquinas](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Para SQLVM, el mapa de dependencias muestra los siguientes detalles:

    - Los procesos o grupos de procesos que tienen conexiones de red activas en ejecución en SQLVM, durante el período especificado (una hora de forma predeterminada).
    - Conexiones TCP de entrada (cliente) y de salida (servidor) a y desde todas las máquinas dependientes.
    - Las máquinas dependientes que tienen instalados los agentes de Azure Migrate se muestran como cuadros independientes.
    - Las máquinas que no tienen instalados los agentes muestran la información del puerto y la dirección IP.

3. En el caso de las máquinas que tienen instalado el agente (WEBVM), Contoso activa la casilla de la máquina para ver más información. La información incluye el nombre de dominio completo, el sistema operativo y la dirección MAC.

    ![Azure Migrate: ver las dependencias de grupos](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Contoso selecciona las máquinas virtuales para agregar al grupo (SQLVM y WEBVM). Utiliza CTRL+clic para seleccionar varias máquinas virtuales.
5. Contoso selecciona **Crear grupo** y, a continuación, escribe un nombre (**smarthotelapp**).

    > [!NOTE]
    > Para ver dependencias más pormenorizadas, puede expandir el intervalo de tiempo. Puede seleccionar una duración concreta o las fechas de inicio y finalización.

### <a name="run-an-assessment"></a>Ejecución de una evaluación

1. En **Grupos**, Contoso abre el grupo (**smarthotelapp**) y, a continuación, selecciona **Crear evaluación**.

    ![Azure Migrate: creación de una evaluación](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Para ver la evaluación, Contoso selecciona **Administrar** > **Evaluaciones**.

Contoso usa la configuración de valoración predeterminada, pero es posible [personalizar la configuración](how-to-modify-assessment.md).

### <a name="analyze-the-vm-assessment"></a>Análisis de la evaluación de la máquina virtual

En la valoración de Azure Migrate se incluye información sobre la compatibilidad del entorno local con Azure, una sugerencia para el tamaño correcto de la máquina virtual de Azure y los costos mensuales estimados de Azure.

![Azure Migrate: informe de valoración](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

![Azure Migrate: visualización de la valoración](./media/contoso-migration-assessment/assessment-display.png)

La valoración obtiene una clasificación de confianza de 1 a 5 estrellas (siendo 1 estrella la menor confianza y 5, la mayor).
- La clasificación de la confianza se asigna a una valoración en función de la disponibilidad de los puntos de datos necesarios para calcular dicha valoración.
- Esta clasificación le ayuda a calcular la confiabilidad de las recomendaciones de tamaño que proporciona Azure Migrate.
- La clasificación de confianza es útil cuando se realiza un *ajuste de tamaño basado en el rendimiento*. Azure Migrate podría no tener suficientes puntos de datos para ajustar el tamaño en función del uso. Para el ajuste de tamaño *como local*, la clasificación de confianza siempre es de 5 estrellas, puesto que Azure Migrate tiene todos los datos que necesita para ajustar el tamaño de la máquina virtual.
- Según el porcentaje de puntos de datos disponibles, se proporciona la clasificación de confianza para la valoración:

   Disponibilidad de puntos de datos | Clasificación de confianza
   --- | ---
   0 % - 20 % | 1 estrella
   21 % - 40 % | 2 estrellas
   41 % - 60 % | 3 estrellas
   61 % - 80 % | 4 estrellas
   81 % - 100 % | 5 estrellas

#### <a name="verify-azure-readiness"></a>Comprobación de la preparación para Azure

![Azure Migrate: valoración de la preparación](./media/contoso-migration-assessment/azure-readiness.png)  

El informe de la valoración muestra la información que se resume en la tabla. Para mostrar el ajuste de tamaño basado en el rendimiento, Azure Migrate necesita la siguiente información. Si dicha información no se pueden recopilar, es posible que la valoración del tamaño no sea precisa.

- Datos de uso de la CPU y la memoria.
- IOPS de lectura o escritura, y capacidad de proceso de cada disco conectado a la máquina virtual.
- Información de entrada y salida de la red de todos y cada uno de los adaptadores de red conectados a la máquina virtual.

Configuración | Indicación | Detalles
--- | --- | ---
**Preparación para VM de Azure** | Indica si la máquina virtual está preparada para la migración. | Los posibles estados son:</br><br/>- Preparado para Azure<br/><br/>- Preparado con condiciones <br/><br/>- No está preparada para Azure<br/><br/>- Preparación desconocida<br/><br/> Si alguna máquina virtual no está lista, Azure Migrate muestra varios pasos para solucionarlo.
**Tamaño de la máquina virtual de Azure** | Para las máquinas virtuales que están listas, Azure Migrate ofrece una recomendación de tamaño de máquina virtual de Azure. | El tamaño recomendado depende de las propiedades de la evaluación:<br/><br/>- Si utilizó un ajuste de tamaño basado en el rendimiento, dicho ajuste tiene en cuenta el historial de rendimiento de las máquinas virtuales.<br/><br/>- Si utilizó *como local*, el ajuste de tamaño se basa en el tamaño de la máquina virtual local y no se utilizan datos de uso.
**Herramienta sugerida** | Como las máquinas de Azure ejecutan los agentes, Azure Migrate examina los procesos que se ejecutan dentro de la máquina. Identifica si la máquina es una máquina de base de datos.
**Información de máquina virtual** | El informe muestra la configuración de la máquina virtual local, lo que incluye información acerca del sistema operativo, el tipo de arranque, los discos y el almacenamiento.

#### <a name="review-monthly-cost-estimates"></a>Examen de cálculos del costo mensual

Esta vista muestra el costo total de los recursos de proceso y almacenamiento para ejecutar las máquinas virtuales en Azure. También muestra información detallada para cada máquina.

![Azure Migrate: costos de Azure](./media/contoso-migration-assessment/azure-costs.png)

- Las estimaciones de los costos se calculan con las recomendaciones de tamaño de una máquina.
- Los costos mensuales estimados para el proceso y almacenamiento se agregan para todas las VM del grupo.

## <a name="clean-up-after-assessment"></a>Limpiar después de la valoración

- Una vez finalizada la valoración, Contoso conserva el dispositivo de Azure Migrate para valoraciones futuras.
- Contoso apaga la máquina virtual de VMware. La usará otra vez cuando valore otras máquinas virtuales adicionales.
- Contoso conserva el proyecto **Migración de Contoso** en Azure. Actualmente, el proyecto está implementado en el grupo de recursos **ContosoFailoverRG**, en la región Este de EE. UU de Azure.
-  La máquina virtual del recopilador tiene una licencia de evaluación de 180 días. Si este límite expira, Contoso deberá descargar y volver a configurar el recopilador.

## <a name="conclusion"></a>Conclusión

En este escenario, Contoso valora la base de datos de la aplicación SmartHotel con la herramienta Data Migration Assistant. Valora las máquinas virtuales locales con el servicio Azure Migrate. Contoso revisa las valoraciones para asegurarse de que los recursos locales están listos para la migración a Azure.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente artículo de esta serie, Contoso rehospeda su aplicación SmartHotel en Azure con una migración "lift-and-shift". Contoso migra la máquina virtual WEBVM front-end de la aplicación mediante Azure Site Recovery. Migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante Database Migration Service. [Introducción](contoso-migration-rehost-vm-sql-managed-instance.md) a esta implementación.
