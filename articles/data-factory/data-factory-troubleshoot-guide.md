---
title: Solución de problemas de Azure Data Factory | Microsoft Docs
description: Obtenga información acerca de cómo solucionar problemas de actividades de control externo en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: 1995ce2a91bfbc115f80c99687cc84b52ef614ec
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950110"
---
# <a name="troubleshoot-azure-data-factory"></a>Solución de problemas de Azure Data Factory

En este artículo se exploran métodos comunes de solución de problemas para actividades de control externo en Azure Data Factory.

## <a name="azure-databricks"></a>Azure Databricks

| Código de error | Mensaje de error                                          | DESCRIPCIÓN                             | Recomendación                             |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | Error 403.                                                    | El token de acceso de Databricks ha expirado.                         | De forma predeterminada, el token de acceso de Databricks es válido durante 90 días.  Cree un nuevo token y actualice el servicio vinculado. |
| 3201           | Falta un campo necesario: settings.task.notebook_task.notebook_path | Creación incorrecta: No ha especificado correctamente la ruta de acceso de Notebook. | Especifique la ruta de acceso de Notebook en la actividad de Databricks. |
| 3201           | El clúster no existe.                                 | Error de creación: El clúster de Databricks no existe o se ha eliminado. | Compruebe que existe el clúster de Databricks. |
| 3201           | URI del archivo de Python no válido... visite la guía de usuario de Databricks para ver los esquemas de URI compatibles. | Creación incorrecta.                                                | Especifique las rutas de acceso absolutas para los esquemas de direccionamiento del área de trabajo, o bien `dbfs:/folder/subfolder/foo.py` para los archivos almacenados en el sistema de archivos de Databricks. |
| 3201           | {0}   LinkedService debe tener accessToken y dominio como propiedades necesarias. | Creación incorrecta.                                                | Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | {0}    LinkedService debe especificar el identificador de clúster existente o la información de nuevo clúster para su creación. | Creación incorrecta.                                                | Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | No se admite el tipo de nodo Standard_D16S_v3. Tipos de nodo admitidos:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2. | Creación incorrecta.                                                | Consulte el mensaje de error.                                          |
| 3201           | notebook_path no válido:... en la actualidad solo se admiten rutas de acceso absolutas. Las rutas de acceso tiene que comenzar por "/". | Creación incorrecta.                                                | Consulte el mensaje de error.                                          |
| 3202           | Ya se han creado 1000 trabajos en los últimos 3600 segundos, se va a exceder el límite de velocidad:   1000 creaciones de trabajo en 3600 segundos. | Demasiadas ejecuciones de Databricks en una hora.                         | Compruebe todas las canalizaciones que usen esta área de trabajo de Databricks para la velocidad de creación de trabajos.  Si las canalizaciones inician demasiadas ejecuciones de Databricks en agregado, migre algunas canalizaciones a un área de trabajo nueva. |
| 3202           | No se pudo analizar el objeto de solicitud: Se esperaba "key" y "value" establecidos para base_parameters del campo de asignación JSON, se obtuvo 'key: "..."' en su lugar. | Error de creación: No se proporcionó ningún valor para el parámetro.         | Inspeccione el json de canalización y asegúrese de que todos los parámetros del cuaderno baseParameters tienen especificado un valor no vacío. |
| 3202           | Usuario: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` no está autorizado para obtener acceso al clúster. | El usuario que ha generado el token de acceso no tiene permiso para obtener acceso al clúster de Databricks especificado en el servicio vinculado. | Asegúrese de que el usuario tiene los permisos necesarios en el área de trabajo.   |
| 3203           | El clúster está en estado Terminado y no está disponible para recibir trabajos. Repare el clúster o inténtelo más tarde. | El clúster se ha finalizado.    Para un clúster interactivo, esto podría ser una condición de carrera. | La mejor manera de evitar este error es usar clústeres de trabajo.             |
| 3204           | Error al ejecutar el trabajo.  | Los mensajes de error indican varias incidencias, como un estado de clúster inesperado o una actividad específica. Normalmente, no aparece ningún mensaje de error.                                                          | N/D                                                          |



## <a name="azure-data-lake-analytics"></a>Análisis con Azure Data Lake

La tabla siguiente se aplica a U-SQL.

| Código de error         | Mensaje de error                                                | DESCRIPCIÓN                                          | Recomendación                            |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | El token de acceso procede del inquilino incorrecto.                    | Inquilino de Azure Active Directory (Azure AD) incorrecto.                                         | La entidad de servicio que se usa para acceder a Azure Data Lake Analytics pertenece a otro inquilino de Azure AD. Cree una nueva entidad de servicio en el mismo inquilino que la cuenta de Data Lake Analytics. |
| 2711,   2705,   2704 | Prohibido. Error de verificación de ACL. El recurso no existe o el usuario no está autorizado para realizar la operación solicitada.<br/><br/>El usuario no puede obtener acceso a Data Lake Store.  <br/><br/>El usuario no está autorizado a usar Data Lake Analytics. | La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento. | Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz. |
| 2711                 | No se encuentra el archivo o la carpeta "Azure Data Lake Store".       | La ruta de acceso al archivo U-SQL es incorrecta o las credenciales del servicio vinculado no tienen acceso. | Compruebe la ruta de acceso y las credenciales proporcionadas en el servicio vinculado. |
| 2707                 | No se puede resolver la cuenta de AzureDataLakeAnalytics. Compruebe "AccountName" y "DataLakeAnalyticsUri". | La cuenta de Data Lake Analytics en el servicio vinculado es incorrecta.                  | Compruebe que se proporciona la cuenta adecuada.             |
| 2703                 | Identificador del error: E_CQO_SYSTEM_INTERNAL_ERROR (o cualquier error que empiece por "Id. de Error:"). | El error es de Data Lake Analytics.                                    | Cualquier error similar al ejemplo significa que el trabajo se envió a Data Lake Analytics y el script produjo un error allí. Investigue el error en Data Lake Analytics. En el portal, vaya a la cuenta de Data Lake Analytics y busque el trabajo mediante el identificador de ejecución de actividad de Data Factory (no el identificador de ejecución de canalización). El trabajo proporcionará más información sobre el error y le ayudará a solucionar el problema. Si la resolución no está clara, póngase en contacto con el equipo de soporte técnico de Data Lake Analytics y proporcióneles la dirección URL del trabajo, que incluye el nombre de cuenta y el identificador del trabajo. |
| 2709                 | No podemos aceptamos su trabajo en este momento. El número máximo de trabajos en cola para su cuenta es de 200. | Este error se debe a una limitación en Data Lake Analytics.                                           | Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics. |
| 2709                 | Este trabajo se rechazó porque requiere 24 UA. Esta cuenta tiene una directiva definida por el administrador que impide que un trabajo utilice más de 5 UA. | Este error se debe a una limitación en Data Lake Analytics.                                           | Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics. |



## <a name="azure-functions"></a>Azure Functions

| Código de error | Mensaje de error                           | DESCRIPCIÓN                                                  | Recomendación                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | El contenido de la respuesta no es un elemento JObject válido. | La función de Azure a la que se llamó no devolvió una carga JSON en la respuesta. La actividad de la función de Azure de Data Factory solo admite contenido de respuesta JSON. | Actualice la función de Azure para que devuelva una carga JSON válida. Por ejemplo, una C# función puede devolver `(ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}");`. |
| 3600         | HttpMethod no válido: "...".               | La actividad de función de Azure no admite el método HTTP especificado en la carga de la actividad. | Use un método HTTP compatible, como PUT, POST, GET, DELETE, OPTIONS, HEAD o TRACE. |



## <a name="custom"></a>Personalizado

La tabla siguiente se aplica a Azure Batch.

| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Recomendación                          |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2\.500         | Se obtuvo una excepción inesperada y ha fallado la ejecución.             | No se puede iniciar el comando, o el programa devolvió un código de error. | Compruebe que existe el archivo ejecutable. Si se inicia el programa, compruebe si los archivos *stdout.txt* y *stderr.txt* se cargaron en la cuenta de almacenamiento. Es un procedimiento recomendado emitir abundantes registros en el código para la depuración. |
| 2501         | No se puede obtener acceso a la cuenta de lote de usuario, compruebe la configuración de la cuenta de lote. | Clave de acceso o nombre de grupo de Batch incorrectos.            | Compruebe el nombre del grupo y la clave de acceso de Batch en el servicio vinculado. |
| 2502         | No se puede obtener acceso a la cuenta de almacenamiento del usuario, compruebe la configuración de la cuenta de almacenamiento. | Nombre de cuenta de almacenamiento o clave de acceso incorrectos.       | Compruebe la clave de acceso y el nombre de cuenta de almacenamiento en el servicio vinculado. |
| 2504         | La operación devolvió un código de estado no válido "BadRequest".     | Hay demasiados archivos en folderPath de la actividad personalizada. El tamaño total de resourceFiles no puede superar los 32 768 caracteres. | Elimine los archivos innecesarios. También puede comprimirlos y agregar un comando unzip para extraerlos. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`. |
| 2505         | No se puede crear la firma de acceso compartido a menos que se usen las credenciales de clave de cuenta. | Las actividades personalizadas solo admiten cuentas de almacenamiento que usen una clave de acceso. | Consulte la descripción del error.                                            |
| 2507         | La ruta de acceso a la carpeta no existe o está vacía:...            | No hay archivos en la cuenta de almacenamiento en la ruta de acceso especificada.       | La ruta de acceso a la carpeta debe contener los archivos ejecutables que desea ejecutar. |
| 2508         | La carpeta de recursos tiene archivos duplicados.               | Hay varios archivos con el mismo nombre en subcarpetas diferentes de folderPath. | Estructura de carpetas de actividades personalizadas aplanada en folderPath.  Si necesita conservar la estructura de carpetas, comprima los archivos y extráigalos en Azure Batch mediante un comando unzip. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`. |
| 2509         | La dirección URL del lote no es válida, tiene que estar en formato de URI.         | Las direcciones URL del lote tienen que ser similares a `https://mybatchaccount.eastus.batch.azure.com` | Consulte la descripción del error.                                            |
| 2510         | Se produjo un error al enviar la solicitud.               | La dirección URL del lote no es válida.                                         | Compruebe la dirección URL del lote.                                            |

## <a name="hdinsight"></a>HDInsight

La tabla siguiente se aplica a Spark, Hive, MapReduce, Pig y Hadoop Streaming.

| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Recomendación                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300,   2310 | Error en el envío del trabajo de Hadoop. Error: No se pudo resolver el nombre remoto. <br/><br/>No se encuentra el clúster. | El URI del clúster proporcionado no es válido.                              | Asegúrese de que el clúster no se ha eliminado y que el URI proporcionado es correcto. Al abrir el URI en cualquier explorador, debe ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado. Para abrirlo, use una máquina virtual que forme parte de la misma red virtual. Para más información, consulte [Conexión directa a los servicios de Apache Hadoop](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2300         | Error en el envío del trabajo de Hadoop. Trabajo:..., Clúster:... /. Error: Se ha cancelado una tarea. | Se agotó el tiempo de espera de envío del trabajo.                         | Podría tratarse de un problema de conectividad general de HDInsight o de un problema de conectividad de red. En primer lugar, confirme que la interfaz de usuario de HDInsight Ambari está disponible desde cualquier explorador. Confirme que las credenciales siguen siendo válidas. Si usa Integrated Runtime (IR) autohospedado, asegúrese de hacerlo desde la máquina virtual o el equipo donde está instalada la instancia de IR autohospedado. A continuación, intente volver a enviar el trabajo desde Data Factory. Si sigue sin funcionar, póngase en contacto con el equipo de Data Factory para obtener soporte técnico. |
| 2300         | No autorizado:   El nombre de usuario o la contraseña de Ambari es incorrecto  <br/><br/>No autorizado:   El administrador de usuario está bloqueado en Ambari.   <br/><br/>403 - Prohibido: Acceso denegado. | Las credenciales para HDInsight son incorrectas o han expirado. | Corrija las credenciales y vuelva a implementar el servicio vinculado. En primer lugar, asegúrese de que las credenciales funcionan en HDInsight, para lo que debe abrir el URI del clúster en cualquier explorador e intentar iniciar sesión. Si las credenciales no funcionan, puede restablecerlas desde Azure Portal. |
| 2300,   2310 | 502, el servidor web ha recibido una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy.       <br/>Puerta de enlace incorrecta. | Este error procede de HDInsight.                               | Este error procede del clúster de HDInsight. Para más información, consulte [Error 502 de la interfaz de usuario de Ambari](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) y [Solución de errores de puerta de enlace incorrecta en el servicio Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502). |
| 2300         | Error en el envío del trabajo de Hadoop. Job: …, Cluster: ... Error: {\"error\":\"no se puede atender la solicitud de trabajo de envío ya que el servicio templeton está ocupado con demasiadas solicitudes de trabajo enviadas. Espere un tiempo antes de reintentar la operación. Consulte la configuración templeton.parallellism.job.submit para configurar solicitudes simultáneas.  <br/><br/>Error en el envío del trabajo de Hadoop. Trabajo: 161da5d4-6fa8-4ef4-a240-6b6428c5ae2f, clúster: `https://abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/`.   Error: {\"error\":\"java.io.IOException: org.apache.hadoop.yarn.exceptions.YarnException: No se pudo enviar application_1561147195099_3730 a YARN: org.apache.hadoop.security.AccessControlException: La cola root.joblauncher ya tiene 500 aplicaciones, no puede aceptar el envío de una aplicación: application_1561147195099_3730\ | Se han enviado demasiados trabajos a la vez a HDInsight. | Considere la posibilidad de limitar el número de trabajos simultáneos que se envían a HDInsight. Consulte la simultaneidad de la actividad de Data Factory si los trabajos los envía la misma actividad. Cambie los desencadenadores para que las ejecuciones de canalización simultánea se repartan en el tiempo. Consulte la documentación de HDInsight para ajustar `templeton.parallellism.job.submit` tal y como sugiere el error. |
| 2303,   2347 | Error del trabajo de Hadoop con código de salida "5". Para más información, consulte "wasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr".  <br/><br/>Error de ejecución de Hive con código de error "UserErrorHiveOdbcCommandExecutionFailure".   Para más información, consulte "wasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out". | Se envió el trabajo a HDInsight y se produjo un error en HDInsight. | El trabajo se envió correctamente a HDInsight. El error se produjo en el clúster. Abra el trabajo y los registros en la interfaz de usuario de HDInsight Ambari o abra el archivo desde el almacenamiento como sugiere el mensaje de error. El archivo muestra los detalles del error. |
| 2328         | Se produjo un error interno al procesar la solicitud. Vuelva a intentar la solicitud o póngase en contacto con soporte técnico. | Este error se produce en HDInsight a petición.                              | Este error procede del servicio HDInsight cuando se produce un error en el aprovisionamiento de HDInsight. Póngase en contacto con el equipo de HDInsight y proporcione el nombre del clúster a petición. |
| 2310         | java.lang.NullPointerException                               | Este error se produce cuando el trabajo se envía a un clúster de Spark.      | Esta excepción procede de HDInsight. Oculta la incidencia real. Póngase en contacto con el equipo de HDInsight para obtener soporte técnico. Proporcione el nombre del clúster y el intervalo de tiempo de ejecución de la actividad. |
|              | Todos los demás errores                                             |                                                              | Consulte [Solución de problemas con HDInsight](../hdinsight/hdinsight-troubleshoot-guide.md) y las [preguntas más frecuentes de HDInsight](https://hdinsight.github.io/). |



## <a name="web-activity"></a>Actividad web

| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Recomendación                          |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | HttpMethod no válido: "...".                                    | La actividad web no admite el método HTTP especificado en la carga de la actividad. | Los métodos HTTP admitidos son PUT, POST, GET y DELETE. |
| 2108         | Error 500 de servidor no válido.                                     | Error interno en el punto de conexión.                               | Compruebe la funcionalidad en la dirección URL con Fiddler o Postman. |
| 2108         | 401 - No autorizado.                                             | Falta de autenticación válida en la solicitud.                      | El token podría haber expirado. Proporcione un método de autenticación válido. Compruebe la funcionalidad en la dirección URL con Fiddler o Postman. |
| 2108         | 403 - Prohibido.                                                | Faltan permisos necesarios.                                 | Compruebe los permisos de usuario en el recurso al que se obtiene acceso. Compruebe la funcionalidad en la dirección URL con Fiddler o Postman.  |
| 2108         | Solicitud incorrecta 400.                                              | Solicitud HTTP no válida.                                         | Compruebe la dirección URL, el verbo y el cuerpo de la solicitud. Use Fiddler o Postman para validar la solicitud.  |
| 2108         | No encontrado 404.                                                | No se encontró el recurso.                                       | Use Fiddler o Postman para validar la solicitud.  |
| 2108         | Servicio no disponible.                                          | El servicio no está disponible.                                       | Use Fiddler o Postman para validar la solicitud.  |
| 2108         | Tipo de medio no compatible.                                       | El tipo de contenido no coincide con el cuerpo de la actividad web.           | Especifique el tipo de contenido que coincida con el formato de carga. Use Fiddler o Postman para validar la solicitud. |
| 2108         | El recurso que está buscando se ha quitado, ha cambiado de nombre o no está disponible temporalmente. | El recurso no está disponible.                                | Use Fiddler o Postman para comprobar el punto de conexión. |
| 2108         | No se puede mostrar la página que está buscando porque se está usando un método no válido (verbo HTTP). | Se ha especificado un método de actividad web incorrecto en la solicitud.   | Use Fiddler o Postman para comprobar el punto de conexión. |
| 2108         | invalid_payload                                              | El cuerpo de la actividad web es incorrecto.                       | Use Fiddler o Postman para comprobar el punto de conexión. |

Para usar Fiddler para crear una sesión HTTP de la aplicación web supervisada:

1. Descargue, instale y abra [Fiddler](https://www.telerik.com/download/fiddler).

1. Si su aplicación web usa HTTPS, vaya a **Tools** > **Fiddler Options** > **HTTPS** (Herramientas>Opciones de Fiddler>HTTPS). Seleccione **Capture HTTPS CONNECTs** (Capturar CONEXIONES HTTPS) y **Decrypt HTTPS Traffic** (Descifrar tráfico HTTPS). 
   
   ![Opciones de Fiddler](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Si la aplicación usa certificados SSL, agregue el certificado de Fiddler al dispositivo. Vaya a **Tools** > **Fiddler Options** > **HTTPS** > **Actions** > **Export Root Certificate to Desktop** (Herramientas>Opciones de Fiddler>HTTPS>Acciones>Exportar el certificado raíz al escritorio).

1. Desactive la captura en **File** > **Capture Traffic** (Archivo > Capturar tráfico). O bien, presione **F12**.

1. Borre la caché del explorador para quitar todos los elementos almacenados en caché, que tendrá que volver a descargar.

1. Creación de una solicitud: 

   a. Seleccione la pestaña **Composer** (Compositor).

   b. Establezca el método HTTP y la dirección URL.

   c. Agregue encabezados y un cuerpo de la solicitud si es necesario.

   d. Seleccione **Execute**(Ejecutar).

9. Active la captura de tráfico de nuevo y complete la transacción problemática en su página.

10. Vaya a **File** > **Save** > **All Sessions** (Archivo>Guardar>Todas las sesiones).

Para más información, consulte [Introducción a Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)



