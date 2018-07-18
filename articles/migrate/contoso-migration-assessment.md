---
title: Valorar las cargas de trabajo locales para la migración de Contoso a Azure |Microsoft Docs
description: Obtenga información sobre cómo Contoso evalúa sus máquinas locales para la migración a Azure con Azure Migration y la migración de la base de datos
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: fb987c95afc0f77386f4f78c44f3c6825f86ee43
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232222"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migración de Contoso: valorar las cargas de trabajo locales para la migración a Azure

En este artículo se muestra cómo Contoso evalúa su aplicación SmartHotel local con el fin de prepararse para su migración a Azure.

Este documento es el tercero de una serie de artículos que documentan cómo la empresa ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. En la serie se incluye información general y un conjunto de escenarios de implementación en los que se muestra cómo configurar una infraestructura de migración, valorar la idoneidad de los recursos locales para la migración y ejecutar diferentes tipos de migraciones. Los escenarios aumentan de complejidad e iremos agregando artículos adicionales a medida con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Se describe cómo Contoso prepara su infraestructura local y de Azure para la migración. Se usa la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
Artículo 3: Valorar los recursos locales (este artículo)  | Se muestra cómo Contoso realiza una valoración de su aplicación de dos niveles local SmartHotel que se ejecuta en VMware. Se evalúan las VM de la aplicación mediante el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de aplicaciones con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehospedar en máquinas virtuales de Azure y una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Muestra cómo Contoso migra la aplicación SmartHotel a Azure. La VM de front-end de la aplicación se migra mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de aplicaciones mediante el servicio [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) para migrar a una instancia administrada de SQL. | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Muestra cómo Contoso migra las VM de la aplicación SmartHotel solo con Site Recovery.
[Artículo 6: Rehospedar en VM de Azure y grupos de disponibilidad de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Muestra cómo migra Contoso la aplicación SmartHotel. Usa Site Recovery para migrar las VM de la aplicación y el servicio Database Migration para migrar la base de datos de aplicaciones a un grupo de disponibilidad de SQL Server. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso migra la aplicación osService de Linux mediante Azure Site Recovery.
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Muestra cómo Contoso migra la aplicación osService de Linux mediante el uso de Site Recovery para la migración de VM, y MySQL Workbench para migrar a una instancia de Azure MySQL Server. | Disponible


## <a name="overview"></a>Información general

Al plantearse la migración a Azure, la empresa Contoso desea realizar una valoración técnica y financiera para averiguar si sus cargas de trabajo locales son adecuadas para la migración a la nube. En concreto, el equipo de Contoso quiere valorar la compatibilidad de las máquinas y bases de datos para la migración y calcular la capacidad y los costos de ejecutar sus recursos en Azure.

Para adentrarse y comprender mejor las tecnologías implicadas, valorará dos de sus aplicaciones locales, que se resumen en la tabla siguiente. Tenga en cuenta que se evalúan escenarios de migración en los que se rehospedan y refactorizan las aplicaciones para la migración. Obtenga más información sobre el rehospedaje y la refactorización en la [introducción a la migración de Contoso](contoso-migration-overview.md).

**Nombre de la aplicación** | **Plataforma** | **Capas de aplicación** | **Detalles**
--- | --- | --- | ---
SmartHotel<br/><br/> Administra los requisitos de viajes de Contoso | Se ejecuta en Windows con una base de datos de SQL Server | Aplicación de dos niveles con el sitio web ASP.NET front-end que se ejecuta en una máquina virtual (WEBVM) y el servidor SQL Server que se ejecuta en otra máquina virtual (SQLVM) | Las máquinas virtuales son de VMware y se ejecutan en un host ESXi administrado por el servidor vCenter.<br/><br/> La aplicación de ejemplo se puede descargar desde [GitHub](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Aplicación del departamento de servicios de Contoso | Se ejecuta en Linux/Apache, con un PHP (LAMP) de MySQL. | Aplicación de dos niveles con un sitio web PHP front-end en una máquina virtual (OSTICKETWEB) y la base de datos MySQL que se ejecuta en otra máquina virtual (OSTICKETMYSQL) | La aplicación la usan las aplicaciones de servicio de atención al cliente con el fin de hacer un seguimiento de problemas para los empleados internos y clientes externos.<br/><br/> La aplicación de ejemplo se puede descargar desde [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Arquitectura actual


Aquí se muestra un diagrama de la infraestructura local de Contoso en la actualidad.

![Arquitectura de Contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso tiene un centro de datos principal situado en la ciudad de Nueva York, al este de los Estados Unidos.
- Así mismo, tiene tres sucursales locales más en los Estados Unidos.
- El centro de datos principal está conectado a Internet con una conexión Metro Ethernet de fibra (500 Mbps).
- Cada una de las sucursales está conectada localmente a Internet con conexiones de categoría empresarial, y con túneles VPN con IPSec hacia el centro de datos principal. Esto permite que toda la red esté conectada de forma permanente, así como optimizar la conectividad a Internet.
- El centro de datos principal está completamente virtualizado con VMware. Tiene dos hosts de virtualización de ESXi 6.5, que administra vCenter Server 6.5.
- Contoso usa Active Directory para la administración de identidades y servidores DNS en la red interna.
- Los controladores de dominio del centro de datos se ejecutan en VM de VMware. Los controladores de dominio de las sucursales locales se ejecutan en servidores físicos.





## <a name="business-drivers"></a>Impulsores de negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr las empresas con esta migración:

- **Responder al crecimiento del negocio**: Contoso está creciendo y, como resultado, sus sistemas locales e infraestructura está bajo presión.
- **Aumentar la eficacia**: Contoso debe quitar procedimientos innecesarios y optimizar los procesos para sus desarrolladores y usuarios.  La empresa necesita que el departamento de TI sea rápido y no malgaste tiempo ni dinero a fin de satisfacer más rápidamente los requisitos del cliente.
- **Aumentar la agilidad**: el equipo de TI de Contoso necesita más capacidad de respuesta a las necesidades de la empresa. Debe poder reaccionar con más rapidez que los cambios del mercado para facilitar el éxito en una economía global.  No se debe interponer en el camino ni bloquear el negocio.
- **Escalar**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.

## <a name="assessment-goals"></a>Objetivos de la valoración

El equipo de la nube de Contoso estableció objetivos para sus valoraciones de migración:

- Tras la migración, las aplicaciones de Azure deben tener las mismas funcionalidades de rendimiento que tienen actualmente en el entorno local de VMware.  Pasar a la nube no significa que el rendimiento de la aplicación sea menos crítico.
- Contoso debe comprender la compatibilidad de sus aplicaciones y bases de datos con los requisitos de Azure, así como sus opciones de hospedaje en Azure.
- La administración de base de datos de Contoso debe minimizarse después de que las aplicaciones se hayan trasladado a la nube.  
- Contoso quiere comprender no solo las opciones de migración, sino también los costos asociados con la infraestructura una vez trasladada a la nube.

## <a name="assessment-tools"></a>Herramientas de valoración
Contoso usa herramientas de Microsoft para la valoración. Estas herramientas se alinean con sus objetivos y ofrecen toda la información que necesita la empresa.

**Technology** | **Descripción** | **Costee**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Usará DMA para valorar y detectar problemas de compatibilidad que podrían afectar a la funcionalidad de su base de datos en Azure. DMA evalúa una paridad de características entre orígenes y destinos de SQL, y recomienda mejoras de rendimiento y confiabilidad. | Es una herramienta que se puede descargar de forma gratuita.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso usará este servicio para valorar sus máquinas virtuales de VMware. Evalúa la idoneidad de la migración de los equipos y proporciona cálculos de tamaño y costo de la ejecución en Azure.  | Actualmente (mayo de 2018), el uso de este servicio no tiene costo alguno.
[Mapa de servicio](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate usa Service Map para mostrar las dependencias entre los equipos que se desea migrar. |  Service Map forma parte de Azure Log Analytics. Actualmente se puede utilizar durante 180 días sin ningún costo.

En este escenario, Contoso descarga y ejecuta el DMA para valorar la base de datos de SQL Server local para su aplicación de viajes. Usa Azure Migrate con asignación de dependencias para valorar las máquinas virtuales de la aplicación antes de migrarlas a Azure.



## <a name="assessment-architecture"></a>Arquitectura de valoración


![Arquitectura de evaluación de migraciones](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso es un nombre ficticio que representa una organización empresarial típica. 
- Contoso tiene un centro de datos local (**contoso-datacenter**), con controladores de dominio locales (CONTOSODC1, CONTOSODC2).
- Las máquinas virtuales de VMware se encuentran en hosts VMware ESXI que ejecutan la versión 6.5. Hosts: **contosohost1**, **contosohost2**
- El entorno de VMware lo administra vCenter Server 6.5 (**venter** que se ejecuta en una máquina virtual.
- La aplicación de viajes SmartHotel:
    - La aplicación se divide en varios niveles entre dos VM de VMware, **WEBVM** y **SQLVM**.
    - Las máquinas virtuales se encuentran en un host VMware ESXi **contosohost1.contoso.com** (versión 6.5).
    - Las máquinas virtuales ejecutan Windows Server 2008 R2 Datacenter con SP1.
- El entorno de VMware lo administra vCenter Server (**vcenter.contoso.com**), que se ejecuta en una máquina virtual.
- Aplicación del departamento de servicios OSTicket:
    - La aplicación se divide en niveles entre dos VM: **OSTICKETWEB** y **OSTICKETMYSQL**.
    - Las VM se ejecutan en Ubuntu Linux Server 16.04-LTS.
    - La VM OSTICKETWEB ejecuta Apache 2 y PHP 7.0.
    - La VM OSTICKETMYSQL ejecuta MySQL 5.7.22.

![Architecture](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>requisitos previos

Esto es lo que Contoso (y usted) necesitan para la valoración:

- Acceso de propietario o colaborador para la suscripción de Azure, o para un grupo de recursos en la suscripción de Azure.
- Una instancia local de vCenter Server que ejecuta las versiones 5.5, 6.0 ó 6.5.
- Una cuenta de solo lectura de vCenter Server, o permisos para crearla.
- Permisos para crear una máquina virtual en vCenter Server mediante una plantilla .OVA.
- Al menos un host ESXi en el que se ejecute la versión 5.0, o cualquier versión superior.
- Al menos dos máquinas virtuales VMware locales, y en una de ellas debe ejecutarse una base de datos de SQL Server.
- Permisos para instalar agentes de Azure Migrate en todas las máquinas virtuales.
- Las máquinas virtuales deben tener conectividad directa a Internet.
        - Puede restringir el acceso a Internet a las [direcciones URL requeridas](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        -Para las máquinas sin conectividad a Internet, deben tener instalada la [puerta de enlace OMS](../log-analytics/log-analytics-oms-gateway.md).
- El FQDN de la máquina virtual que ejecuta la instancia de SQL Server, para la evaluación de la base de datos.
- La instancia de Firewall de Windows que se ejecuta en la máquina virtual de SQL Server debe permitir conexiones externas en el puerto TCP 1433 (valor predeterminado) para que DMA pueda conectarse.


## <a name="assessment-overview"></a>Introducción a la valoración

A continuación se muestra cómo Contoso realizará la valoración:


> [!div class="checklist"]
> * **Paso 1: Descargar e instalar DMA**. Preparar DMA para la valoración de la base de datos de SQL Server local.
> * **Paso 2: Valorar la base de datos con DMA**. Ejecutar y analizar la valoración de la base de datos.
> * **Paso 3: Preparar la valoración de la máquina virtual con Azure Migrate**. Configurar las cuentas locales y cambiar la configuración de VMware.
> * **Paso 4: Detectar máquinas virtuales locales con Azure Migrate**. Crear una máquina virtual de recopilador de Azure Migrate. Después, ejecutar el recopilador para detectar las VM que se valorarán.
> * **Paso 5: Preparar el análisis de dependencias con Azure Migrate**. Instalar los agentes de Azure Migrate en las máquinas virtuales para que se pueda ver la asignación de dependencias entre las máquinas virtuales.
> * **Paso 6: Valorar las VM con Azure Migrate**. Comprobar las dependencias, agrupar las VM y ejecutar la valoración. Una vez que esté lista la valoración, analizarla para preparar la migración.


## <a name="step-1-download-and-install-the-dma"></a>Paso 1: Descargar e instalar DMA

1. Contoso descarga DMA del [Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=53595).
    - DMA se puede instalar en cualquier equipo que pueda conectarse a la instancia de SQL. No es preciso ejecutarlo en el equipo de SQL Server.
    - No se debe ejecutar en el equipo host de SQL Server.
2. Para iniciar la instalación, ejecuta el archivo de configuración descargado (DownloadMigrationAssistant.msi).
3. En la página **Finalizar**, selecciona **Launch Microsoft Data Migration Assistant** (Iniciar Microsoft Data Migration Assistant) antes de finalizar el asistente.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Paso 2: Ejecutar y analizar la valoración de la base de datos para SmartHotel

Ahora, Contoso puede ejecutar una valoración para analizar su instancia local de SQL Server para la aplicación SmartHotel.

1. En Database Migration Assistant, hace clic en **Nuevo**, selecciona **Evaluación** y asigna a la evaluación un nombre de proyecto: **SmartHotel**.
2. En **Tipo de servidor de origen**, selecciona **SQL Server en máquinas virtuales de Azure**. 

    ![Seleccionar origen](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Actualmente, DMA no admite la migración para migrar a una instancia administrada de SQL Database. Como alternativa, Contoso usa SQL Server en una VM de Azure como objetivo supuesto para la valoración.

3. En **Seleccionar versión de destino**, selecciona SQL Server 2017 como la versión de destino. Debe seleccionar esto porque es la versión que la instancia administrada de SQL usa.
4. Selecciona la opción para detectar información sobre la compatibilidad y nuevas características:
    - **Problemas de compatibilidad** indica los cambios que pueden interrumpir la migración o que requieren un ajuste menor antes de la migración. Lo mantiene informado sobre las características actualmente en uso que han quedado en desuso. Los problemas se organizan por nivel de compatibilidad.
    - **Nuevas recomendaciones de características** indica las nuevas características de la plataforma de SQL Server de destino que se pueden usar para la base de datos después de la migración. Se organizan por rendimiento, seguridad y almacenamiento.

    ![Seleccionar destino](./media/contoso-migration-assessment/dma-assessment-2.png)

2. En **Conectar con un servidor**, escribe el nombre de la VM que ejecuta la base de datos y las credenciales para acceder a ella. Tiene que habilitar **Certificado de servidor de confianza** para asegurarse de que puede acceder a SQL Server. A continuación, hace clic en **Conectar**.

    ![Seleccionar destino](./media/contoso-migration-assessment/dma-assessment-3.png)

3. En **Agregar origen**, agrega la base de datos que quiere valorar y hace clic en **Siguiente** para iniciar la valoración.
4. Se crea la valoración.
    
    ![Crear evaluación](./media/contoso-migration-assessment/dma-assessment-4.png)

5. En **Revisar los resultados**, puede ver los resultados de la valoración.


### <a name="analyze-the-database-assessment"></a>Análisis de la evaluación de la base de datos

Los resultados se muestran en cuanto están disponibles. Si corrige los problemas, tiene que hacer clic en **Reiniciar valoración**.

1. En el informe **Problemas de compatibilidad**, comprueba si existen problemas en cada nivel de compatibilidad. Los niveles de compatibilidad se asignan a las versiones de SQL Server como sigue:

    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Problemas de compatibilidad](./media/contoso-migration-assessment/dma-assessment-5.png)

2. En el informe **Recomendaciones de características**, Contoso puede ver las características de rendimiento, seguridad y almacenamiento que la valoración recomienda después de la migración. Se recomiendan varias características, entre las que se incluyen OLTP en memoria y Almacén de columnas, Stretch Database, Always Encrypted, Enmascaramiento dinámico de datos y Cifrado de datos transparente (TDE).

    ![Recomendaciones de características](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Se recomienda que Contoso [habilite TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) para todas las bases de datos de SQL Server, que es incluso más importante cuando las bases de datos están en la nube. TDE solo debería habilitarse después de la migración. Si TDE ya está habilitado, será necesario mover el certificado o clave asimétrica a la base de datos maestra del servidor de destino. [Más información](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Puede exportar la valoración en formato JSON o CSV.

Tenga en cuenta que si ejecuta una valoración más grande puede hacer lo siguiente:

- Ejecutar varias valoraciones simultáneamente y ver su estado al abrir la página **Todas las valoraciones**.
- [Consolidar las valoraciones en una base de datos de SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Consolidar las valoraciones en un informe de PowerBI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Paso 3: Preparar la valoración de la VM con Azure Migrate

Contoso debe crear una cuenta de VMware que Azure Migrate usará para detectar automáticamente las VM que se van a valorar, comprobar que dispone de permisos para crear una VM, anotar los puertos que deben estar abiertos y establecer el nivel de configuración de las estadísticas.

### <a name="set-up-a-vmware-account"></a>Configuración de una cuenta de VMware

 La detección de VM requiere una cuenta de solo lectura en vCenter, con las siguientes propiedades: 

- Tipo de usuario: al menos un usuario de solo lectura.
- Permisos: Data Center -> Propagate to Child Object, role=Read-only (Objeto de Centro de datos -> Propagar a objeto secundario, rol = Solo lectura).
- Detalles: el usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.
- Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).

### <a name="verify-permissions-to-create-a-vm"></a>Comprobación de los permisos para crear una máquina virtual

Contoso comprueba que tiene permisos para crear una VM mediante la importación de un archivo en formato .OVA. [Más información](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Comprobación de puertos

La valoración de Contoso usa la asignación de dependencia. Esta característica requiere un agente instalado en las VM que desee valorar. El agente debe poder conectarse a Azure desde el puerto TCP 443 de todas las VM. [Más información](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) acerca de los requisitos de la conexión.


### <a name="set-statistics-settings"></a>Establecimiento de la configuración de las estadísticas

Antes de comenzar la implementación, Contoso debe establecer la configuración de las estadísticas para vCenter Server en el nivel 3. Observe lo siguiente:

- Después de establecer el nivel, debe esperar al menos un día antes de ejecutar la valoración. Si no lo hace así, es posible que no funcione según lo previsto.
- Si el nivel es mayor que 3, la evaluación funcionará, pero:
    - No se recopilarán los datos de rendimiento de los discos ni de la red.
    - Para el almacenamiento, Azure Migrate recomienda un disco estándar en Azure con el mismo tamaño que el disco local.
    - Para redes, para cada adaptador de red local, se recomendará un adaptador de red en Azure.
    - Para el proceso, Azure Migrate examinará los núcleos y el tamaño de la memoria de la máquina virtual y recomendará una máquina virtual de Azure con la misma configuración. Si hay varios tamaños de máquina virtual de Azure adecuados, se recomienda el que tiene el costo más bajo.
- [Más información](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) acerca del ajuste de tamaño con el nivel 3.

Establece el nivel de la manera siguiente:

1. En vSphere Web Client, abre la instancia de vCenter Server.
2. En **Administrar**  > **Configuración** > **General**, hace clic en **Editar**.
3. En **Estadísticas**, establece la configuración de nivel de estadísticas en **Nivel 3**.

    ![Nivel de estadísticas de vCenter](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Paso 4: Detectar VM

Para detectar las VM, Contoso crea un proyecto de Azure Migrate. Descarga y configura la VM del recopilador y ejecuta el recopilador para detectar sus VM locales.

### <a name="create-a-project"></a>Crear un proyecto

1. En [Azure Portal](https://portal.azure.com), busca **Azure Migrate** y crea un proyecto (ContosoMigration).
2. Especifica un nombre de proyecto y la suscripción a Azure, y crea un nuevo grupo de recursos de Azure: **ContosoFailoverRG**. Observe lo siguiente:

    - Los proyectos de Azure Migrate solo se pueden crear en la región Centro occidental de EE. UU. o Este de EE. UU.
    - Puede planear una migración a cualquier ubicación de destino.
    - La ubicación del proyecto solo se utiliza para almacenar los metadatos que se recopilan de las máquinas virtuales locales.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Descarga del dispositivo de recopilador

Azure Migrate crea una VM local conocida como el dispositivo de recopilador. Esta VM detecta VM de VMware locales y envía los metadatos sobre ellas para al servicio Azure Migrate. Para configurar el dispositivo de recopilador, Contoso descarga una plantilla .OVA y la importa en el servidor vCenter local para crear la VM.

1. En el proyecto de Azure Migrate > **Introducción** > **Detección y evaluación** > **Detectar máquinas**, descarga el archivo de plantilla .OVA.
2. Copia el identificador y la clave del proyecto. Estos se necesitan para configurar el recopilador.

    ![Descarga del archivo .ova](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Comprobación del dispositivo de recopilador

Antes de implementar la VM, Contoso comprueba que el archivo .OVA está protegido.

1. En el equipo en el que se descargó el archivo, abre una ventana de comandos del administrador.
2. Ejecuta el siguiente comando para generar el código hash del archivo OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. El código hash generado debe coincidir con esta configuración (versión 1.0.9.7)

    **Algoritmo** | **Valor del código hash**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c


### <a name="create-the-collector-appliance"></a>Creación del dispositivo de recopilador

Ahora, Contoso puede importar el archivo descargado en vCenter Server y aprovisionar la VM del servidor de configuración.

1. En la consola de vSphere Client, hace clic en **Archivo** > **Implementar plantilla de OVF**.

    ![Implementación de OVF](./media/contoso-migration-assessment/vcenter-wizard.png)

2. En el asistente para implementar la plantilla de OVF > **Origen**, especifica la ubicación del archivo OVA.
3. En **Nombre y ubicación**, especifica un nombre descriptivo para la VM del recopilador y la ubicación del inventario en el que se hospedará la VM. También especifica el host o clúster donde se ejecutará el dispositivo recopilador.
5. En **Almacenamiento**, especifica la ubicación de almacenamiento y en **Formato de disco**, especifica cómo se va a aprovisionar el almacenamiento.
7. En **Asignación de red**, especifica la red a la que se conectará la VM de recopilador. La red necesita conectividad a Internet, para enviar metadatos a Azure.
8. Revisa la configuración y selecciona **Encender después de la implementación**> **Finalizar**. Una vez creado el dispositivo, se genera un mensaje que confirma la finalización correcta.

### <a name="run-the-collector-to-discover-vms"></a>Ejecución del recopilador para detectar VM

Ahora ejecuta el recopilador para detectar las VM. Tenga en cuenta que, actualmente, el recopilador solo admite "Inglés (Estados Unidos)" como idioma del sistema operativo e idioma de la interfaz del recopilador.

1. En la consola de vSphere Client > **Abrir consola**, especifica las preferencias de idioma, zona horaria y contraseña para la VM de recopilador.
2. En el escritorio, hace clic en el acceso directo **Run collector** (Ejecutar recopilador).

    ![Acceso directo al recopilador](./media/contoso-migration-assessment/collector-shortcut.png)

4. En Azure Migrate Collector > **Configurar los requisitos previos**, acepta los términos de licencia y lee la información sobre otros fabricantes.
5. El recopilador comprueba que la VM tiene acceso a Internet, que la hora está sincronizada y que se ejecuta el servicio de recopilador (se instala de forma predeterminada en la VM). También instala VMware PowerCLI.

    > [!NOTE]
    > Suponemos que la máquina virtual tiene acceso directo a Internet, sin un proxy.

    ![Verificar los requisitos previos](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. En **Especificar los detalles de Center Server**, especifica el nombre (FQDN) o la dirección IP de vCenter Server y las credenciales de solo lectura que se usan para la detección.
7. Selecciona un ámbito para la detección de VM. El recopilador solo puede detectar VM dentro del ámbito especificado. El ámbito se puede establecer en una carpeta, centro de datos o clúster específico. No debe contener más de 1500 máquinas virtuales.

    ![Conectarse a vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. En **Especificar el proyecto de migración**, especifica la clave y el identificador de proyecto de Azure Migrate que se copiaron del portal. Puede obtener estos elementos de nuevo en la página **Introducción** > **Detectar máquinas** del proyecto.  

    ![Conexión a Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. En **Ver el progreso de la recopilación**, Contoso puede supervisar la detección y comprobar que los metadatos recopilados de las VM se encuentran dentro del ámbito. El recopilador proporciona un tiempo de detección aproximado.

    ![Colección en curso](./media/contoso-migration-assessment/collector-collection-process.png) 



### <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

Una vez que se completa la colección, Contoso comprueba que las VM aparecen en el portal.

1. En el proyecto de Azure Migrate > **Administrar** > **Máquinas**, comprueba que aparecen las VM que quiere detectar.

    ![Máquinas detectadas](./media/contoso-migration-assessment/discovery-complete.png)

3. Tenga en cuenta que las máquinas actualmente no tienen instalados los agentes de Azure Migrate. Contoso necesita instalar estos agentes para ver las dependencias.

    ![Máquinas detectadas](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Paso 5: Preparar el análisis de dependencias

Para ver las dependencias existentes entre las VM a las que Contoso quiere acceder, descarga e instala agentes en las VM de la aplicación. Contoso hace esto en todas las VM para sus aplicaciones, tanto las de Windows como las de Linux.

### <a name="take-a-snapshot"></a>Realización de una instantánea

Conserva una copia de la VM antes de modificarla al crear una instantánea antes de instalar los agentes.

![Instantánea de la máquina](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Descarga e instalación de los agentes en máquinas virtuales

1. En la página **Máquinas**, selecciona la máquina y, luego, **Requiere instalación** en la columna **Dependencias**.
2. En la página **Detectar máquinas**, hace lo siguiente:
    - Descargar el agente MMA y el agente de dependencia para cada VM Windows
    - Descargar el agente MMA y el agente de dependencia para cada VM Linux
3. Ahora copia la clave y el identificador de área de trabajo. Estos elementos se necesitan para instalar el MMA.

    ![Descarga de agente](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Instalar los agentes en máquinas virtuales de Windows

Ejecuta la instalación en cada máquina virtual.

#### <a name="install-the-mma-on-windows-vms"></a>Instalar el MMA en máquinas virtuales de Windows

1. Hace doble clic en el agente descargado.
2. En **Carpeta de destino**, conserva la carpeta de instalación predeterminada y hace clic en **Siguiente**.
2. En **Opciones de instalación del agente**, selecciona **Connect the agent to Azure Log Analytics** (Conectar el agente a Azure Log Analytics) > **Siguiente**.

    ![Instalación de MMA](./media/contoso-migration-assessment/mma-install.png)
    
5. En **Azure Log Analytics**, pega la clave y el identificador del área de trabajo que copió del portal. 

    ![Instalación de MMA](./media/contoso-migration-assessment/mma-install2.png)

6. En **Listo para instalar**, ahora puede instalar el MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Instalar el agente de dependencia en VM Windows

1. Hace doble clic en la instancia de descarga del agente de dependencia.
2. Acepta los términos de licencia y espera a que finalice la instalación.

    ![Dependency Agent](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Instalar los agentes en VM Linux

Ejecuta la instalación en cada VM.

#### <a name="install-the-mma-on-linux-vms"></a>Instalar el MMA en VM Linux

1. Instala la biblioteca de python ctypes en cada VM mediante: **sudo apt-get install python-ctypeslib**.
2. Debe ejecutar el comando para instalar al agente MMA como raíz.  Para ir a la raíz, se debe ejecutar el siguiente comando y escribir la contraseña raíz: **sudo -i**.
3. Ahora puede instalar al agente MMA.
    - Escriba el identificador y la clave de área de trabajo correctos en el comando.
    - Los comandos son para 64 bits.
    - El **Id. de área de trabajo** y la **clave principal** se encuentran en OMS Portal > **Configuración**, en la pestaña **Orígenes conectados**.
    - Ejecute los siguientes comandos para descargar el agente OMS, validar la suma de comprobación e instalar e incorporar el agente.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```
 


#### <a name="install-the-dependency-agent-on-linux-vms"></a>Instalar el agente de dependencia en Linux

Después instalar MMA, Contoso puede instalar el agente de dependencia en las VM Linux.

1. El agente de dependencia se instala en equipos Linux con InstallDependencyAgent-Linux64.bin, un script de shell con un archivo binario autoextraíble. Puede ejecutar el archivo con sh o agregar permisos de ejecución al propio archivo.

2. Instala al agente de dependencia de Linux como raíz:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Paso 6: Ejecutar y analizar la valoración de la VM

Ahora, Contoso puede comprobar las dependencias de la máquina y crear un grupo. A continuación, ejecuta la valoración para el grupo.

### <a name="verify-dependencies-and-create-a-group"></a>Comprobar las dependencias y crear un grupo


1. Para las máquinas que quiere analizar, hace clic en **Ver dependencias**.

    ![Ver las dependencias de máquinas](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Para SQLVM, el mapa de dependencias muestra los siguientes detalles:

    - Los grupos de procesos o los procesos con conexiones de red activas que se ejecutan en SQLVM, durante el período especificado (una hora de forma predeterminada)
    - Conexiones TCP de entrada (cliente) y de salida (servidor) a y desde todas las máquinas dependientes.
    - Las máquinas dependientes con los agentes de Azure Migrate se muestran como cuadros independientes
    - Las máquinas los agentes instalados muestran la información del puerto y dirección IP.

3. En las máquinas con el agente instalado (WEBVM), hace clic en el cuadro de la máquina para ver más información, como el FQDN, el sistema operativo y la dirección MAC.

    ![Visualización de las dependencias del grupo](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Ahora selecciona las VM para agregar al grupo (SQLVM y WEBVM).  Usa CTRL + clic para seleccionar varias VM.
5. Hace clic en **Crear grupo** y especifica un nombre (smarthotelapp).

> [!NOTE]
    > Para ver dependencias más pormenorizadas, puede expandir el intervalo de tiempo. Puede seleccionar una duración concreta, o las fechas de inicio y finalización.


### <a name="run-an-assessment"></a>Ejecución de una evaluación


1. En la página **Grupos** , abre el grupo (smarthotelapp) y hace clic en **Crear evaluación**.

    ![Crear una evaluación](./media/contoso-migration-assessment/run-vm-assessment.png)

2. La evaluación aparece en la página **Manage** > **Assessments** (Administrar > Evaluaciones).

Contoso usó la configuración de valoración predeterminada, pero es posible personalizar la configuración. [Más información](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>Análisis de la evaluación de la máquina virtual

En la valoración de Azure Migrate se incluye información sobre si las VM locales son compatibles con Azure, una sugerencia para el tamaño correcto de la VM de Azure y los costos mensuales estimados de Azure.

![Informe de evaluación](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

![Presentación de la evaluación](./media/contoso-migration-assessment/assessment-display.png)

La valoración obtiene una clasificación de confianza de 1 a 5 estrellas (siendo 1 estrella la menor confianza y 5, la mayor).
- La clasificación de confianza se asigna a una valoración que se basa en la disponibilidad de puntos de datos necesarios para calcular tal valoración.
- Esta clasificación le ayuda a calcular la confiabilidad de las recomendaciones de tamaño que proporciona Azure Migrate.
- La clasificación de confianza es útil cuando se está realizando un *ajuste del tamaño basado en el rendimiento* porque Azure Migrate puede no tener suficientes puntos de datos para realizar el ajuste de tamaño en función del uso. Para el *ajuste de tamaño como local*, la clasificación de confianza siempre es de 5 estrellas, puesto que Azure Migrate tiene todos los datos que necesita para ajustar el tamaño de la máquina virtual.
- Según el porcentaje de puntos de datos disponibles, se proporciona la clasificación de confianza para la valoración:

   **Disponibilidad de puntos de datos** | **Clasificación de confianza**
   --- | ---
   0 % - 20 % | 1 estrella
   21 % - 40 % | 2 estrellas
   41 % - 60 % | 3 estrellas
   61 % - 80 % | 4 estrellas
   81 % - 100 % | 5 estrellas

#### <a name="verify-readiness"></a>Comprobación de la preparación

![Preparación para Azure](./media/contoso-migration-assessment/azure-readiness.png)  

El informe de la evaluación muestra la información que se resume en la tabla. Tenga en cuenta que para mostrar el ajuste de tamaño basado en el rendimiento, Azure Migrate necesita la siguiente información. Si dicha información no se pueden recopilar, es posible que la evaluación del ajuste de tamaño no sea precisa.

- Datos de uso de la CPU y la memoria.
- IOPS de lectura o escritura, y capacidad de proceso de cada disco conectado a la máquina virtual.
- Información de entrada y salida de la red de todos y cada uno de los adaptadores de red conectados a la máquina virtual.


**Configuración** | **Indicación** | **Detalles**
--- | --- | ---
**Preparación para VM de Azure** | Indica si la máquina virtual está preparada para la migración | Los posibles estados son:</br><br/>- Preparado para Azure<br/><br/>- Preparado con condiciones <br/><br/>- No está preparada para Azure<br/><br/>- Preparación desconocida<br/><br/> Si alguna máquina virtual no está lista, mostraremos varios pasos para solucionarlo.
**Tamaño de la máquina virtual de Azure** | Para las máquinas virtuales listas, se recomienda un tamaño de máquina virtual de Azure. | El tamaño recomendado depende de las propiedades de la evaluación:<br/><br/>- Si utilizó un ajuste de tamaño basado en el rendimiento, dicho ajuste tiene en cuenta el historial de rendimiento de las máquinas virtuales.<br/><br/>- Si utilizó "como local", el ajuste de tamaño se basa en el tamaño de la máquina virtual local y no se usan datos de uso.
**Herramienta sugerida** | Dado que las máquinas ejecutan los agentes, Azure Migrate examina los procesos que se ejecutan en la máquina e identifica si es una máquina de base de datos o no.
**Información de máquina virtual** | El informe muestra la configuración de la máquina virtual local, lo que incluye información acerca del sistema operativo, del tipo de arranque, de los discos y del almacenamiento.


#### <a name="review-monthly-cost-estimates"></a>Examen de cálculos del costo mensual

Esta vista muestra el cálculo total y el costo de almacenamiento que supone ejecutar las máquinas virtuales en ejecución en Azure, además de los detalles de cada máquina.

![Preparación para Azure](./media/contoso-migration-assessment/azure-costs.png)

- Las estimaciones de los costos se calculan mediante las recomendaciones de tamaño de una máquina.
- Los costos mensuales estimados para el proceso y almacenamiento se agregan para todas las VM del grupo.


## <a name="clean-up-after-assessment"></a>Limpiar después de la valoración

- Una vez finalizada la valoración, Contoso conserva el dispositivo Azure Migration para valoraciones futuras.
- Apaga VMware en la VM. Lo vuelve a iniciar cuando valore VM adicionales.
- Conservará el proyecto de migración de Contoso en Azure.  Actualmente, se implementa en el grupo de recursos ContosoFailoverRG, en la región de Azure del Este de EE. UU.
-  La VM de recopilador tiene una licencia de evaluación de 180 días. Si este límite expira, se debe descargar y volver a configurar el recopilador.


## <a name="conclusion"></a>Conclusión

En este escenario, Contoso valoró su base de datos de la aplicación SmartHotel mediante la herramienta DMA y las VM locales mediante el servicio Azure Migrate. Luego revisaron las valoraciones para asegurarse de que los recursos locales estén listos para la migración a Azure.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente artículo de esta serie, Contoso rehospeda su aplicación SmartHotel en Azure a través de una migración mediante lift-and-shift. Contoso migra la máquina virtual front-end WEBVM para la aplicación mediante Azure Site Recovery y la base de datos de la aplicación a una instancia administrada de Azure SQL mediante Azure Database Migration Service. [Introducción](contoso-migration-rehost-vm-sql-managed-instance.md) a esta implementación.
