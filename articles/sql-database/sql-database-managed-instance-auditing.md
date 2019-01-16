---
title: Auditoría de Instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtención de información acerca de cómo auditar Instancia administrada de Azure SQL Database mediante T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: vainolo
ms.reviewer: vanto
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 045314980d0051e8b5ef71bdf95023084eff1880
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063885"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Introducción a la auditoría de Instancia administrada de Azure SQL Database mediante T-SQL

La auditoría de [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de su cuenta de Azure Storage. La auditoría también:

- Puede ayudarle a mantener el cumplimiento de normativas, comprender la actividad de las bases de datos y conocer las discrepancias y anomalías que pueden indicar problemas en el negocio o infracciones de seguridad sospechosas.
- Posibilita y facilita la observancia de estándares reguladores aunque no garantiza el cumplimiento. Para obtener más información acerca de los programas de Azure compatibles con la observancia de estándares, consulte el [Centro de confianza de Azure](https://azure.microsoft.com/support/trust-center/compliance/).

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Configuración de la auditoría de un servidor en Azure Storage 

En la sección siguiente se describe la configuración de auditoría en su Instancia administrada.

1. Vaya a [Azure Portal](https://portal.azure.com).
2. Los pasos siguientes crean un **contenedor** de Azure Storage donde se almacenan los registros de auditoría.

   - Navegue hasta Azure Storage donde le gustaría almacenar los registros de auditoría.

     > [!IMPORTANT]
     > Use una cuenta de almacenamiento de la misma región que el servidor de Instancia administrada para evitar lecturas y escrituras entre regiones.

   - En la cuenta de almacenamiento, vaya a **Información general** y haga clic en **Blobs**.

     ![Panel de navegación][1]

   - En el menú superior, haga clic en **+ Contenedor** para crear un nuevo contenedor.

     ![Panel de navegación][2]

   - En **Nombre** proporcione un nombre de contenedor, establezca el nivel de acceso público en **Privado**y haga clic en **Aceptar**.

     ![Panel de navegación][3]

   - En la lista de contenedores, haga clic en el contenedor recién creado y luego en **Propiedades del contenedor**.

     ![Panel de navegación][4]

   - Copie la dirección URL del contenedor haciendo clic en el icono de copia y guarde dicha dirección (por ejemplo, en el Bloc de notas) para un uso futuro. El formato de dirección URL del contenedor debe ser `https://<StorageName>.blob.core.windows.net/<ContainerName>`.

     ![Panel de navegación][5]

3. Los siguientes pasos generan un **token de SAS** de Azure Storage usado para conceder derechos de acceso de auditoría de Instancia administrada a la cuenta de almacenamiento.

   - Vaya a la cuenta de Azure Storage donde se creó el contenedor en el paso anterior.

   - Haga clic en **Firma de acceso compartido** en el menú Configuración de Storage.

     ![Panel de navegación][6]

   - Configure SAS de la siguiente manera:
     - **Servicios permitidos**: Blob
     - **Fecha de inicio**: para evitar problemas relacionados con la zona horaria, se recomienda utilizar la fecha de ayer.
     - **Fecha de finalización**: elija la fecha en que expira este token de SAS. 

       > [!NOTE]
       > Renueve el token tras la expiración para evitar errores de auditoría.

     - Haga clic en **Generar SAS**.

       ![Panel de navegación][7]

   - Después de hacer clic en Generar SAS, el token de SAS aparecerá en la parte inferior. Copie el token haciendo clic en el icono de copia y guárdelo (por ejemplo, en el Bloc de notas) para un uso futuro.

     > [!IMPORTANT]
     > Quite el signo de interrogación ("?") caracteres del principio del token.

     ![Panel de navegación][8]

4. Conéctese a la Instancia administrada mediante SQL Server Management Studio (SSMS).

5. Ejecute la siguiente instrucción T-SQL para **crear una nueva credencial** utilizando la dirección URL del contenedor y el token de SAS que creó en los pasos anteriores:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Ejecute la siguiente instrucción T-SQL para crear una nueva auditoría de servidor (elija su propio nombre de auditoría y use la dirección URL del contenedor que creó en los pasos anteriores):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Si no se especifica, el valor predeterminado de `RETENTION_DAYS` es 0 (retención ilimitada).

    Para información adicional:
    - [Diferencias de auditoría entre Instancia administrada, Azure SQL DB y SQL Server](#auditing-differences-between-managed-instance-azure-sql-database-and-sql-server)
    - [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Cree una especificación de auditoría de servidor o la especificación de auditoría de base de datos como lo haría para SQL Server:
    - [Crear la guía de T-SQL de especificación de auditoría de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Crear la guía de T-SQL de especificación de auditoría de base de datos](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Habilite la auditoría de servidor que creó en el paso 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="set-up-auditing-for-your-server-to-event-hub-or-log-analytics"></a>Configuración de la auditoría de un servidor en Event Hubs o Log Analytics

Los registros de auditoría de una instancia administrada se pueden enviar a Event Hubs o Log Analytics mediante Azure Monitor. En esta sección se describe cómo configurar todo esto:

1. En [Azure Portal ](https://portal.azure.com/), vaya a la instancia administrada de SQL.

2. Haga clic en **Configuración de diagnóstico**.

3. Haga clic en **Activar diagnóstico**. Si ya está habilitado el diagnóstico, en su lugar, se mostrará *+Add diagnostic setting* (+Agregar configuración de diagnóstico).

4. Seleccione **SQLSecurityAuditEvents** en la lista de registros.

5. Elija un destino para los eventos de auditoría: Event Hubs, Log Analytics o ambos. Configure los parámetros necesarios (por ejemplo, el área de trabajo de Log Analytics) en cada destino.

6. Haga clic en **Save**(Guardar).

  ![Panel de navegación][9]

7. Conéctese a la instancia administrada mediante **SQL Server Management Studio (SSMS)** o cualquier otro cliente compatible.

8. Ejecute la siguiente instrucción T-SQL para crear una auditoría de servidor:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Cree una especificación de auditoría de servidor o la especificación de auditoría de base de datos como lo haría para SQL Server:

   - [Crear la guía de T-SQL de especificación de auditoría de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Crear la guía de T-SQL de especificación de auditoría de base de datos](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Habilite la auditoría de servidor que creó en el paso 7:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Uso de registros de auditoría

### <a name="consume-logs-stored-in-azure-storage"></a>Uso de registros almacenados en Azure Storage

Existen varios métodos que puede usar para ver los registros de auditoría de blobs.

- Use la función del sistema `sys.fn_get_audit_file` (T-SQL) para devolver los datos de registro de auditoría en formato tabular. Para más información sobre el uso de esta función, vea la [documentación de sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Puede explorar los registros de auditoría con una herramienta como el Explorador de Azure Storage. En Azure Storage, los registros de auditoría se guardan como una colección de archivos de blob dentro de un contenedor llamado "sqldbauditlogs". Para obtener más información sobre la jerarquía de la carpeta de almacenamiento, las convenciones de nomenclatura y el formato del registro, vea la referencia del formato de registro de auditoría de blobs.

- Para una lista completa de métodos de consumo del registro de auditoría, consulte el [Get started with SQL database auditing](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing) (Introducción a la auditoría de base de datos SQL).

> [!IMPORTANT]
> El método para ver los registros de auditoría desde Azure Portal (panel "Registros de auditoría") está disponible actualmente para Instancia administrada.

### <a name="consume-logs-stored-in-event-hub"></a>Uso de registros almacenados en Event Hubs

Para consumir datos de registros de auditoría desde el centro de eventos, deberá configurar una secuencia que consuma eventos y los escriba en un destino. Para obtener más información, consulte la documentación de Azure Event Hubs.

### <a name="consume-and-analyze-logs-stored-in-log-analytics"></a>Uso y análisis de los registros almacenados en Log Analytics

Si se escriben registros de auditoría en Log Analytics, estarán disponibles en el área de trabajo de Log Analytics, donde podrá ejecutar búsquedas avanzadas en los datos de auditoría. Empiece navegando a Log Analytics y, en la sección *General*, haga clic en *Registros* y escriba una consulta simple, como `search "SQLSecurityAuditEvents"`, para ver los registros de auditoría.  

Log Analytics proporciona conclusiones operativas en tiempo real gracias a uso de paneles personalizados y de búsqueda integrados para analizar fácilmente millones de registros en todas las cargas de trabajo y servidores. Para información útil adicional sobre los comandos y el lenguaje de búsqueda de Log Analytics, consulte la [referencia de búsqueda de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Diferencias de auditoría entre Instancia administrada, Azure SQL Database y SQL Server

Las diferencias principales entre la auditoría de SQL en Instancia administrada, Azure SQL Database y las instancias locales de SQL Server son:

- En Instancia administrada, la auditoría de SQL funciona en el nivel de servidor y almacena los archivos de registro `.xel` en la cuenta de Azure Blob Storage.
- En Azure SQL Database, la auditoría de SQL funciona en el nivel de base de datos.
- En las máquinas virtuales o instancias locales de SQL Server, la auditoría de SQL funciona en el nivel de servidor, pero almacena los eventos en registros de eventos de Windows o del sistema de archivos.

En Instancia administrada, la auditoría de XEvent admite Azure Blob Storage como destino. **No se admiten** archivos ni registros de Windows.

Las principales diferencias en la sintaxis de `CREATE AUDIT` para guardar la auditoría en Azure Blob Storage son:

- Se proporciona una nueva sintaxis de `TO URL` que permite especificar la dirección URL del contenedor de Azure Blob Storage donde se colocarán los archivos `.xel`.
- Se proporciona una nueva sintaxis `TO EXTERNAL MONITOR` para habilitar los destinos Event Hubs y Log Analytics.
- La sintaxis `TO FILE` **no se admite** porque Instancia administrada no puede acceder a los recursos compartidos de archivos de Windows.
- La opción de apagado **no se admite**.
- `queue_delay` de 0 **no se admite**.

## <a name="next-steps"></a>Pasos siguientes

- Para una lista completa de métodos de consumo del registro de auditoría, consulte el [Get started with SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) (Introducción a la auditoría de base de datos SQL).
- Para obtener más información acerca de los programas de Azure compatibles con la observancia de estándares, consulte el [Centro de confianza de Azure](https://azure.microsoft.com/support/trust-center/compliance/).

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
[9]: ./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png
