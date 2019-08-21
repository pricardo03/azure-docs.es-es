---
title: Solución de problemas de ejecución de paquetes en SSIS Integration Runtime | Microsoft Docs
description: En este artículo se proporciona orientación para solucionar problemas de ejecución de paquetes de SSIS en SSIS Integration Runtime
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: a7ad0f3be754029c654b04d19750aab7bbcd210d
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933647"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>Solución de problemas de ejecución de paquetes en SSIS Integration Runtime

En este artículo se incluyen los errores más comunes que puede encontrar al ejecutar paquetes de SQL Server Integration Services (SSIS) en SSIS Integration Runtime. Se describen las posibles causas y acciones para resolver los errores.

## <a name="where-to-find-logs-for-troubleshooting"></a>Dónde encontrar los registros para solucionar problemas

Use el portal de Azure Data Factory para comprobar la salida de la actividad de ejecución del paquete de SSIS. La salida incluye el resultado de la ejecución, los mensajes de error y el identificador de la operación. Para obtener instrucciones detalladas, consulte [Supervisar la canalización](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Use el catálogo de SSIS (SSISDB) para comprobar los registros de detalle de la ejecución. Para obtener más información, consulte [Monitor de ejecución de paquetes y otras operaciones](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017).

## <a name="common-errors-causes-and-solutions"></a>Errores, causas y soluciones comunes

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Mensaje de error: "Se ha agotado el tiempo de espera de la conexión" o "Error en el servicio al procesar la solicitud. Inténtelo de nuevo."

Estas son las posibles causas y las acciones recomendadas:
* El origen o el destino de los datos está sobrecargado. Compruebe la carga en el origen o el destino de los datos y observe si tiene suficiente capacidad. Por ejemplo, si usa Azure SQL Database, considere la posibilidad de escalar verticalmente si es probable que el tiempo de espera de la base de datos se agote.
* La red entre SSIS Integration Runtime y el origen o el destino de los datos es inestable, especialmente si la conexión se realiza entre regiones o entre un entorno local y Azure. Para aplicar el patrón de reintento en el paquete de SSIS, siga estos pasos:
  * Compruebe que los paquetes de SSIS se pueden volver a ejecutar en caso de error sin efectos secundarios (por ejemplo, pérdida o duplicación de datos).
  * Configure las opciones **Reintentar** e **Intervalo de reintentos** de la actividad **Ejecutar paquete de SSIS**  en la pestaña **General**. ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Para un componente de origen o destino ADO.NET y OLE DB, establezca **ConnectRetryCount** y **ConnectRetryInterval** en el administrador de conexiones en el paquete de SSIS o la actividad de SSIS.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Mensaje de error: "El origen ADO NET no pudo adquirir la conexión «...»" con "Se produjo un error relacionado con la red o específico de una instancia al establecer una conexión con SQL Server. No se detectó el servidor o no estaba accesible."

Normalmente, este problema significa que no se puede acceder al origen o el destino de los datos desde SSIS Integration Runtime. Las razones pueden variar. Pruebe estas acciones:
* Asegúrese de que está pasando el nombre o IP del origen o el destino de los datos correctamente.
* Asegúrese de que el firewall está configurado correctamente.
* Compruebe que la red virtual esté configurada correctamente si el origen o el destino de los datos es un entorno local:
  * Puede comprobar si el problema proviene de una configuración de red virtual mediante el aprovisionamiento de una VM de Azure en la misma red virtual. A continuación, compruebe si se puede acceder al origen o el destino de los datos desde la VM de Azure.
  * Puede encontrar más detalles sobre el uso de una red virtual con una instancia de SSIS Integration Runtime en [Unión de una instancia de Azure-SSIS Integration Runtime a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Mensaje de error: "El origen ADO NET no pudo adquirir la conexión «...»" con "Error al crear un administrador de conexiones administrado".

La causa posible es que el proveedor de ADO.NET que se usa en el paquete no está instalado en el entorno de ejecución de integración de SSIS. Para instalar el proveedor, puede usar una configuración personalizada. Puede encontrar más detalles sobre la configuración personalizada en [Customize setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Personalizar la configuración para Azure-SSIS Integration Runtime).

### <a name="error-message-the-connection--is-not-found"></a>Mensaje de error: "No se encontró la conexión «...»"

Un problema conocido en versiones anteriores de SQL Server Management Studio (SSMS) puede provocar este error. Si el paquete contiene un componente personalizado (por ejemplo, los componentes de asociados o el paquete de características de Azure de SSIS) que no está instalado en la máquina donde se usa SSMS para realizar la implementación, SSMS quitará el componente y provocará el error. Actualice [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) a la versión más reciente con el problema corregido.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Mensaje de error: "SSIS Executor exit code: -1073741819." (Código de salida del ejecutor SSIS: 1073741819).

* Causa posible y acción recomendada:
  * Este error puede deberse a la limitación del origen y el destino de Excel cuando se ejecutan varios orígenes o destinos de Excel en paralelo en varios subprocesos. Para solucionar esta limitación, cambie los componentes de Excel para que se ejecuten en secuencia, o sepárelos en diferentes paquetes y desencadénelos mediante "Tarea Ejecutar paquete" con la propiedad ExecuteOutOfProcess establecida como true.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Mensaje de error: "No hay suficiente espacio en el disco"

Este error significa que el disco local está agotado en el nodo de SSIS Integration Runtime. Compruebe si el paquete o la configuración personalizada consume una gran cantidad de espacio en disco:
* Si el paquete consume el disco, se liberará cuando finalice la ejecución del paquete.
* Si la configuración personalizada consume el disco, deberá detener el entorno de ejecución de integración de SSIS, modificar el script y volver a iniciar el entorno de ejecución de integración. El contenedor de blobs de Azure que especificó para la configuración personalizada se copiará en el nodo de SSIS Integration Runtime, así que compruebe si existe cualquier contenido innecesario en ese contenedor.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Mensaje de error: "Error al recuperar el recurso del elemento principal. Microsoft.SqlServer.IntegrationServices.Scale.ScaleoutContract.Common.MasterResponseFailedException: Código: 300004. Descripción: No se pudo cargar el archivo "***"".

* Causa posible y acción recomendada:
  * Si la actividad de SSIS ejecuta el paquete desde el sistema de archivos (archivo de paquete o archivo de proyecto), este error se producirá si el proyecto, el paquete o el archivo de configuración no son accesibles con la credencial de acceso del paquete que proporcionó en la actividad de SSIS.
    * Si usa Azure File:
      * La ruta de acceso del archivo comienza con \\\\\<nombre de la cuenta de almacenamiento\>.file.core.windows.net\\\<ruta de acceso del recurso compartido de archivos\>
      * El dominio debe ser "Azure".
      * El nombre de usuario debe ser \<nombre de la cuenta de almacenamiento.\>.
      * La contraseña debe ser \<clave de acceso de almacenamiento\>.
    * Si usa un archivo local, compruebe que la red virtual y la credencial y los permisos de acceso al paquete estén configurados correctamente para que Azure SSIS Integration Runtime pueda acceder al recurso compartido de archivos local.

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Mensaje de error: "El nombre de archivo "..." especificado en la conexión no era válido"

* Causa posible y acción recomendada:
  * Se ha especificado un nombre de archivo no válido.
  * Asegúrese de que usa el FQDN (nombre de dominio completo) en lugar del nombre corto en el administrador de conexiones.

### <a name="error-message-cannot-open-file-"></a>Mensaje de error: "No se puede abrir el archivo «...»"

Este error se produce cuando la ejecución del paquete no encuentra un archivo en el disco local en SSIS Integration Runtime. Pruebe estas acciones:
* No use la ruta de acceso absoluta en el paquete que se está ejecutando en SSIS Integration Runtime. Use el directorio de trabajo de ejecución actual (.) o la carpeta temporal (%TEMP%).
* Si necesita conservar algunos archivos en nodos de SSIS Integration Runtime, prepare los archivos como se describe en [Personalizar la configuración](how-to-configure-azure-ssis-ir-custom-setup.md). Cuando finalice la ejecución, se limpiarán todos los archivos del directorio de trabajo.
* En lugar de almacenar el archivo en el nodo de SSIS Integration Runtime, use Azure Files. Para obtener más información, consulte [Usar recursos compartidos de archivos de Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Mensaje de error: "La base de datos “SSISDB” ha alcanzado su cuota de tamaño"

Una causa posible es que la base de datos SSISDB que se creó en la base de datos de Azure SQL, o una instancia administrada si está creando un entorno de ejecución de integración, ha alcanzado su cuota. Pruebe estas acciones:
* Considere la posibilidad de aumentar el número de DTU de la base de datos. Para obtener más información, consulte [SQL Database resource limits for an Azure SQL Database server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) (Límites de recursos de SQL Database para un servidor de Azure SQL Database).
* Compruebe si el paquete generaría muchos registros. Si es así, puede configurar un trabajo elástico para limpiar estos registros. Para obtener más detalles, consulte [Limpieza de los registros de SSISDB con trabajos de Azure Elastic Database](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Mensaje de error: "The request limit for the database is ... and has been reached" (El límite de solicitudes para la base de datos es ... y se alcanzó).

Si se ejecutan muchos paquetes en paralelo en SSIS Integration Runtime, este error podría producirse porque SSISDB alcanzó su límite de solicitudes. Considere la posibilidad de aumentar el DTC de SSISDB para resolver este problema. Para obtener más información, consulte [SQL Database resource limits for an Azure SQL Database server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) (Límites de recursos de SQL Database para un servidor de Azure SQL Database).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Mensaje de error: "SSIS Operation failed with unexpected operation status: ..." (Error en la operación de SSIS con un estado de operación inesperado: ...).

El error se debe, principalmente, a un problema temporal. Intente volver a realizar la ejecución del paquete. Para aplicar el patrón de reintento en el paquete de SSIS, siga estos pasos:

* Compruebe que los paquetes de SSIS se pueden volver a ejecutar en caso de error sin efectos secundarios (por ejemplo, pérdida o duplicación de datos).
* Configure las opciones **Reintentar** e **Intervalo de reintentos** de la actividad **Ejecutar paquete de SSIS**  en la pestaña **General**. ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Para un componente de origen o destino ADO.NET y OLE DB, establezca **ConnectRetryCount** y **ConnectRetryInterval** en el administrador de conexiones en el paquete de SSIS o la actividad de SSIS.

### <a name="error-message-there-is-no-active-worker"></a>Mensaje de error: "No hay ningún trabajo activo".

Normalmente, este error significa que SSIS Integration Runtime tiene un estado incorrecto. Compruebe el estado y los errores detallados en Azure Portal. Para obtener más información, vea [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Mensaje de error: "Your integration runtime cannot be upgraded and will eventually stop working, since we cannot access the Azure Blob container you provided for custom setup" (El entorno de ejecución de integración no se puede actualizar y finalmente dejará de funcionar, ya que no se puede acceder al contenedor de blobs de Azure que proporcionó para la configuración personalizada)".

Este error se produce cuando SSIS Integration Runtime no puede acceder al almacenamiento configurado para la configuración personalizada. Compruebe si el URI de la firma de acceso compartido (SAS) que ha proporcionado es válido y no ha expirado.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Mensaje de error: "Proveedor OLE DB de Microsoft para Analysis Services. "Hresult: 0x80004005 Descripción:" Error de COM: Error de COM: mscorlib; Se produjo una excepción en el destino de la invocación"

Una posible causa es que el nombre de usuario o la contraseña con Azure Multi-Factor Authentication habilitado esté configurado para la autenticación de Azure Analysis Services. Esta autenticación no se admite en SSIS Integration Runtime. Intente usar una entidad de servicio para la autenticación de Azure Analysis Services:
1. Prepare una entidad de servicio como se describe en [Automatización con entidades de servicio](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal).
2. En el Administrador de conexiones, configure **Usar un nombre de usuario y contraseña específicos**: defina **AppID** como nombre de usuario y **clientSecret** como contraseña.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Mensaje de error: "ADONET Source has failed to acquire the connection {GUID} with the following error message: Login failed for user 'NT AUTHORITY\ANONYMOUS LOGON'" (El origen de ADONET no ha podido adquirir la conexión {GUID} con el siguiente mensaje de error: error de inicio de sesión para el usuario "NT AUTHORITY\ANONYMOUS LOGON")

Asegúrese de no configurar el método de autenticación del administrador de conexiones como **Autenticación de contraseña de Active Directory** cuando el parámetro *ConnectUsingManagedIdentity* sea **True** . Puede configurarlo como **Autenticación de SQL**, que se ignora si *ConnectUsingManagedIdentity* está definido.

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>Se desencadenan varias ejecuciones de paquetes de forma inesperada

* Causa posible y acción recomendada:
  * La actividad de procedimiento almacenado de ADF se usa para desencadenar la ejecución de paquetes de SSIS. El comando t-sql puede encontrarse con problemas transitorios y desencadenar una nueva ejecución, lo que provocaría varias ejecuciones de paquetes.
  * Use la actividad ExecuteSSISPackage en su lugar, que garantiza que la ejecución de paquetes no volverá a ejecutarse a menos que el usuario establezca un recuento de reintentos en la actividad. Se puede encontrar más información en [https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

### <a name="package-execution-takes-too-long"></a>La ejecución del paquete tarda demasiado

Estas son las posibles causas y las acciones recomendadas:

* Se han programado demasiadas ejecuciones de paquetes en SSIS Integration Runtime. Todas estas ejecuciones estarán esperando su turno en una cola.
  * Para determinar el valor máximo, use esta fórmula:

    Recuento de ejecución en paralelo máximo por IR = recuento de nodos * Máxima ejecución en paralelo por nodo
  * Para obtener información sobre cómo establecer el número de nodos y la ejecución en paralelo máxima por nodo, vea [Creación de una instancia de Azure-SSIS Integration Runtime en Azure Data Factory](create-azure-ssis-integration-runtime.md).
* SSIS Integration Runtime se ha detenido o su estado es incorrecto. Para obtener información sobre cómo comprobar el estado y los errores de SSIS Integration Runtime, vea [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).

También es recomendable definir un tiempo de expiración en la pestaña **General**: ![Definir propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png).

### <a name="poor-performance-in-package-execution"></a>Rendimiento deficiente en la ejecución del paquete

Pruebe estas acciones:

* Asegúrese de que SSIS Integration Runtime esté en la misma región que el origen y el destino de los datos.

* Defina el nivel de registro de la ejecución del paquete como **Rendimiento** para recopilar información sobre la duración para cada componente de la ejecución. Para obtener más información, consulte [Registro de Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Compruebe el rendimiento del nodo de IR en Azure Portal:
  * Para obtener información sobre cómo supervisar SSIS Integration Runtime, consulte [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * Para encontrar el historial de CPU/memoria para SSIS Integration Runtime, consulte las métricas de la instancia de Data Factory en Azure Portal.
    ![Supervisar las métricas de SSIS Integration Runtime](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
