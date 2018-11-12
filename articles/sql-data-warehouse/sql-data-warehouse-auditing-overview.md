---
title: Auditoría en Azure SQL Data Warehouse | Microsoft Docs
description: Aprenda sobre la auditoría y cómo configurarla en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/11/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 199095c3cffc8df7f9755f1f2c4bb5a1acba3748
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260387"
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditoría en Azure SQL Data Warehouse

Aprenda sobre la auditoría y cómo configurarla en Azure SQL Data Warehouse.

## <a name="what-is-auditing"></a>¿Qué es la auditoría?
La auditoría de SQL Data Warehouse permite grabar los eventos de la base de datos en un registro de auditoría de una cuenta de Azure Storage. La auditoría puede ayudarle a mantener el cumplimiento de normativas, comprender la actividad de las bases de datos y conocer las discrepancias y anomalías que pueden indicar problemas en el negocio o infracciones de seguridad sospechosas.

Las herramientas de auditoría posibilitan y facilitan la observancia de estándares reguladores pero no garantizan el cumplimiento. Para obtener más información acerca de los programas de Azure compatibles con la observancia de estándares, consulte el [Centro de confianza de Azure](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Conceptos básicos de auditoría
La auditoría de SQL Data Warehouse la permite que:

* **Conservar** una traza de auditoría de eventos seleccionados. Puede definir categorías de acciones de base de datos para auditar.
* **Informar** sobre la actividad de la base de datos. Puede usar informes preconfigurados y un panel para empezar rápidamente con el informe de actividades y eventos.
* **Analizar** informes. Puede buscar eventos sospechosos, actividades inusuales y tendencias.

Los registros de auditoría se almacenan en su cuenta de almacenamiento de Azure. Puede definir un período de retención de registro de auditoría.


## <a id="subheading-4"></a>Definir la directiva de auditoría de nivel de servidor frente la de nivel de base de datos

Puede definirse una directiva de auditoría para una base de datos específica o como directiva de servidor predeterminada:

* Una directiva de servidor se **aplica a todas las bases de datos recién creadas** en el servidor.

* Si la *auditoría de blobs de servidor* está habilitada, *se aplica siempre a la base de datos*. La base de datos se auditará, independientemente de la configuración de auditoría de la base de datos.

* Habilitar la auditoría en la base de datos, además de en el servidor, *no* invalidará ni cambiará ninguna de las opciones de la auditoría del servidor de blobs. Ambas auditorías existirán en paralelo. En otras palabras, la base de datos se auditará dos veces en paralelo; una vez por la directiva de servidor y otra vez por la directiva de base de datos.

> [!NOTE]
> Se recomienda habilitar solo la **auditoría de blobs de nivel de servidor** y dejar que la auditoría de nivel de base de datos esté deshabilitada para todas las bases de datos.
> Debe habilitar tanto la auditoría de servidor como la auditoría de base de datos, a menos que:
> * Quiera usar una *cuenta de almacenamiento* o un *período de retención* diferentes para una base de datos específica.
> * Quiera auditar tipos de eventos o categorías para una base de datos específica que difieren del resto de las bases de datos del servidor. Por ejemplo, es posible que tenga inserciones de tabla que solo tengan que deban auditarse para una base de datos concreta.
> * Desea utilizar la detección de amenazas, que actualmente solo es compatible con la auditoría de nivel de base de datos.

> [!IMPORTANT]
>Si habilita la auditoría en una instancia de Azure SQL Data Warehouse, o en un servidor que tenga una instancia de este, **hará que esta instancia se reanude** incluso en el caso de que estuviera anteriormente en pausa. **Asegúrese de poner en pausa el almacenamiento de datos después de volver a habilitar la auditoría**.

## <a id="subheading-5"></a>Configuración de la auditoría de nivel de servidor para todas las bases de datos

Una directiva de auditoría de servidor se aplica a **todas las bases de datos recién creadas** en un servidor.

En la sección siguiente se describe la configuración de auditoría mediante Azure Portal.

1. Vaya a [Azure Portal](https://portal.azure.com).
2. Vaya al **servidor de SQL** que desea auditar (importante, asegúrese de que está viendo el servidor SQL, no una base de datos/DW específica). En el menú **Seguridad**, seleccione **Auditoría y detección de amenazas**.

    ![Panel de navegación][6]
4. En la hoja *Auditoría y detección de amenazas*, en **Auditoría** seleccione **Activar**. Esta directiva de auditoría de servidor se aplicará a todas las bases de datos existentes y recién creadas en este servidor.

    ![Panel de navegación][7]
5. Para abrir la hoja **Almacenamiento de registros de auditoría**, seleccione **Detalles de almacenamiento**. Seleccione o cree la cuenta de Azure Storage donde se guardarán los registros y, después, seleccione el período de retención (los registros antiguos se eliminarán). A continuación, haga clic en **Aceptar**.

    ![Panel de navegación][8]

    > [!IMPORTANT]
    > Los registros de auditoría de nivel de servidor se escriben en **Anexar blobs** en Azure Blob Storage en la suscripción de Azure.
    >
    > * **Premium Storage** actualmente **no es compatible** con Append Blobs.
    > * **El almacenamiento en VNet** actualmente **no se admite**.

8. Haga clic en **Save**(Guardar).



## <a id="subheading-2"></a>Configuración de la auditoría de nivel de base de datos para una base de datos única

Puede definir una directiva de auditoría para una base de datos específica o como directiva de servidor predeterminada.

Se recomienda encarecidamente utilizar la auditoría de nivel de servidor y no la auditoría de nivel de base de datos, tal como se describe en [Definición de la directiva de auditoría de nivel de servidor frente a la directiva de auditoría de nivel de base de datos](#subheading-4).

Antes de configurar la auditoría, compruebe si usa un ["Cliente de nivel inferior"](sql-data-warehouse-auditing-downlevel-clients.md).


1. Inicie [Azure Portal](https://portal.azure.com).
2. Vaya a **Configuración** de la instancia de SQL Data Warehouse que quiere auditar. Seleccione **Auditoría y detección de amenazas**.

    ![][1]
3. A continuación, para habilitar la auditoría, haga clic en el botón **ACTIVADO** .

    ![][3]
4. En el panel de configuración de auditoría, seleccione **DETALLES DE ALMACENAMIENTO** para abrir el panel Almacenamiento de registros de auditoría. Seleccione la cuenta de Azure Storage donde se guardarán los registros y el período de retención.
    >[!TIP]
    >Use la misma cuenta de almacenamiento para todas las bases de datos auditadas con el fin de obtener el máximo rendimiento de las plantillas de informes preconfiguradas.

    ![][4]

5. Haga clic en el botón **Aceptar** para guardar los detalles de configuración de almacenamiento.
6. En **REGISTRO POR EVENTO**, haga clic en **CORRECTO** y **ERROR** para registrar todos los eventos, o elija categorías individuales de eventos.
7. Si va a configurar la auditoría para una base de datos, puede que tenga que modificar la cadena de conexión del cliente para asegurarse de que los datos de auditoría se capturan correctamente. Consulte el tema [Modificación del FDQN de servidor en la cadena de conexión](sql-data-warehouse-auditing-downlevel-clients.md) sobre conexiones de cliente de nivel inferior.
8. Haga clic en **OK**.

## <a id="subheading-3"></a>Análisis de registros e informes de auditoría

### <a name="server-level-policy-audit-logs"></a>Registros de auditoría de la directiva de nivel de servidor
Los registros de auditoría de nivel de servidor se escriben en **Anexar blobs** en Azure Blob Storage en la suscripción de Azure. Se guardan como una colección de archivos de blob dentro de un contenedor llamado **sqldbauditlogs**.

Para obtener más información sobre la jerarquía de la carpeta de almacenamiento, las convenciones de nomenclatura y el formato del registro, vea la [referencia del formato de registro de auditoría de blobs](https://go.microsoft.com/fwlink/?linkid=829599).

Existen varios métodos que puede usar para ver los registros de auditoría de blobs:

* Use **Combinar archivos de auditoría** en SQL Server Management Studio (a partir de SSMS 17):
    1. En el menú SSMS, seleccione **Archivo** > **Abrir** > **Combinar archivos de auditoría**.

    2. Se abre el cuadro de diálogo **Agregar archivos de auditoría**. Seleccione una de las opciones **Agregar** para elegir si quiere combinar los archivos de auditoría desde un disco local o importarlos desde Azure Storage. Debe proporcionar los detalles de Azure Storage y la clave de cuenta.

    3. Una vez agregados todos los archivos que se van a combinar, haga clic en **Aceptar** para completar la operación de combinación.

    4. El archivo combinado se abre en SSMS, donde puede verlo y analizarlo, y también exportarlo a un archivo XEL o CSV, o a una tabla.

* Use la [aplicación de sincronización](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) que hemos creado. Se ejecuta en Azure y usa las API públicas de Log Analytics para insertar los registros de auditoría SQL en Log Analytics. La aplicación de sincronización inserta los registros de auditoría SQL en Log Analytics para su consumo mediante el panel de Log Analytics.

* Use Power BI. Puede ver y analizar los datos de registro de auditoría en Power BI. Obtenga más información sobre [Power BI y tenga acceso a una plantilla que se puede descargar](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Descargue los archivos de registro del contenedor de blobs de Azure Storage mediante el portal o con una herramienta como el [Explorador de Azure Storage](http://storageexplorer.com/).
    * Después de descargar localmente un archivo de registro, puede hacer doble clic en él para abrir, ver y analizar los registros en SSMS.
    * También puede descargar varios archivos al mismo tiempo a través del Explorador de Azure Storage. Haga clic con el botón derecho en una subcarpeta específica y seleccione **Guardar como** para guardar en una carpeta local.

* Otros métodos:
   * Después de descargar varios archivos o una subcarpeta que contenga archivos de registro, puede combinarlos localmente como se describe en las instrucciones anteriores para combinar archivos de auditoría de SSMS.

   * Ver los registros de auditoría de blobs mediante programación:

     * Use la biblioteca de C# de [lector de eventos extendidos](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/).
     * [Consulta de archivos de eventos extendidos](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) mediante PowerShell.



<br>
### <a name="database-level-policy-audit-logs"></a>Registros de auditoría de la directiva de nivel de base de datos
Los registros de auditoría de nivel de servidor se agregan en una recopilación de tablas de Almacenamiento con el prefijo **SQLDBAuditLogs** en la cuenta de almacenamiento de Azure que eligió durante la configuración. Puede ver archivos de registro usando una herramienta como el [Explorador de Azure Storage](http://azurestorageexplorer.codeplex.com).

Hay una plantilla de informe de panel preconfigurada disponible como [hoja de cálculo de Excel descargable](https://go.microsoft.com/fwlink/?LinkId=403540) para ayudarle a analizar datos de registro rápidamente. Para utilizar la plantilla en los registros de auditoría, necesita Excel 2013 o posterior y Power Query, que puede [descargar aquí](https://www.microsoft.com/download/details.aspx?id=39379).

La plantilla contiene datos de ejemplo ficticios y puede configurar Power Query para importar el registro de auditoría directamente desde la cuenta de almacenamiento de Azure.

## <a id="subheading-4"></a>Regeneración de clave de almacenamiento
En el entorno de producción, es probable que actualice periódicamente las claves de almacenamiento. Cuando actualice las claves, debe volver a guardar la directiva. El proceso es el siguiente:

1. En el panel de configuración de auditoría, descrito anteriormente en la sección de configuración de auditoría, cambie la **Clave de acceso de almacenamiento** de *Principal* a *Secundaria* y haga clic en **GUARDAR**.

   ![][4]
2. Vaya al panel de configuración de almacenamiento y **vuelva a generar** la *clave de acceso principal*.
3. Vuelva al panel de configuración de auditoría,
4. cambie el valor de **Clave de acceso de almacenamiento** de *Secundaria* a *Principal* y presione **GUARDAR**.
4. Vuelva a la interfaz de usuario de almacenamiento y **regenere** la *Clave de acceso secundaria* (como preparación para el siguiente ciclo de actualización de las claves).

## <a id="subheading-5"></a>Automation (PowerShell o API de REST)
También puede configurar la auditoría en Azure SQL Data Warehouse mediante las siguientes herramientas de automatización:

* **Cmdlets de PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Compatibilidad con clientes de nivel inferior para enmascaramiento de datos dinámicos y auditoría
La auditoría funciona con los clientes SQL que admiten el redireccionamiento de TDS.

Cualquier cliente que implementa TDS 7.4 también debe admitir el redireccionamiento. Entre las excepciones a esto se incluyen JDBC 4.0, en el que la función de redireccionamiento no es totalmente compatible y Tedious para Node.JS, en cuya redireccionamiento no se ha implementado.

Para "clientes de nivel inferior" que admiten TDS versión 7.3 e inferiores, se debe modificar el FQDN del servidor en la cadena de conexión como se indica a continuación:

- FQDN del servidor original en la cadena de conexión: <*nombre del servidor*>. database.windows.net
- FQDN del servidor modificado en la cadena de conexión: <*nombre del servidor*>. database.**secure**.windows.net

Una lista parcial de "Clientes de nivel inferior" incluye:

* .NET 4.0 y versiones posteriores,
* ODBC 10.0 y versiones posteriores.
* JDBC (aunque JDBC admite TDS 7.4, la característica de redireccionamiento de TDS no es totalmente compatible)
* Tedious (para Node.JS)

**Comentario:** la anterior modificación del FDQN del servidor puede resultar útil también para aplicar una directiva de auditoría de nivel de SQL Server sin necesidad de un paso de configuración en cada base de datos (mitigación temporal).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Define server-level vs. database-level auditing policy]: #subheading-4
[Set up server-level auditing for all databases]: #subheading-5

<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png
[6]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_1_overview.png
[7]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_2_enable.png
[8]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_3_storage.png
