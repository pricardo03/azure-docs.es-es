---
title: Solución de problemas de Azure Data Factory | Microsoft Docs
description: Solución de problemas de Azure Data Factory. Documento común para todas las actividades de control externas.
services: data-factory
author: abnarain
manager: craigg
ms.service: data-factory
ms.topic: troubleshoot
ms.subservice: troubleshoot
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: 8d6ab565098e1ea40ede5c650f05e670a1edc7f6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454239"
---
# <a name="troubleshooting-azure-data-factory"></a>Solución de problemas de Azure Data Factory
En este artículo figuran las preguntas habituales sobre solución de problemas.

- [Azure Databricks (Notebook, Jars, Python)](#azure-databricks)
- [Azure Data Lake Analytics (U-SQL)](#azure-data-lake-analytics-u-sql)
- [Azure Functions](#azure-functions)
- [Personalizado (Azure Batch)](#custom-azure-batch)
- [HDInsight (Spark, Hive, MapReduce, Pig, Hadoop Streaming)](#hdinsight-spark-hive-mapreduce-pig-hadoop-streaming)
- [Actividad web](#web-activity)



## <a name="azure-databricks"></a>Azure Databricks
| Código de error | Mensaje de error                                          | Descripción del problema                             | Posible solución/acción recomendada                            |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | Error 403                                                    | El token de acceso de Databricks ha expirado.                         | De forma predeterminada, el token de acceso de Databricks es válido durante 90 días.  Cree un nuevo token y actualice el servicio vinculado. |
| 3201           | Falta un campo necesario: settings.task.notebook_task.notebook_path | Creación incorrecta: No ha especificado correctamente la ruta de acceso de Notebook. | Especifique la ruta de acceso de Notebook en la actividad de Databricks. |
| 3201           | El clúster no existe                                 | Error de creación: El clúster de Databricks no existe o se ha eliminado. | Compruebe que existe el clúster de Databricks. |
| 3201           | URI del archivo de Python no válido: ... visite la guía de usuario de Databricks para ver los esquemas de URI compatibles. | Creación incorrecta                                                | Especifique las rutas de acceso absolutas para los esquemas de direccionamiento del área de trabajo, o bien "dbfs:/folder/subfolder/foo.py" para los archivos almacenados en DBFS. |
| 3201           | {0}   LinkedService debe tener accessToken y dominio como propiedades necesarias | Creación incorrecta                                                | Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | {0}    LinkedService debe especificar el identificador de clúster existente o la información de nuevo clúster para su creación | Creación incorrecta                                                | Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | No se admite el tipo de nodo Standard_D16S_v3. Tipos de nodo admitidos:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s, Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2 | Creación incorrecta                                                | Consulte el mensaje de error                                          |
| 3201           | notebook_path no válido: ... en la actualidad solo se admiten rutas de acceso absolutas. Las rutas de acceso tiene que comenzar por "/". | Creación incorrecta                                                | Consulte el mensaje de error                                          |
| 3202           | Ya se han creado 1000 trabajos en los últimos 3600 segundos, se va a exceder el límite de velocidad:   1000 creaciones de trabajo en 3600 segundos. | Demasiadas ejecuciones de Databricks en una hora.                         | Compruebe todas las canalizaciones que usen esta área de trabajo de Databricks para la velocidad de creación de trabajos.  Si las canalizaciones inician demasiadas ejecuciones de Databricks en agregado, migre algunas canalizaciones a un área de trabajo nueva. |
| 3202           | No se pudo analizar el objeto de solicitud: Se esperaba "key" y "value" establecidos para base_parameters del campo de asignación JSON, se obtuvo 'key: "..."' en su lugar. | Error de creación: No se proporcionó ningún valor para el parámetro         | Inspeccione el json de canalización y asegúrese de que todos los parámetros en los baseParameters de Notebook tienen especificado un valor no vacío. |
| 3202           | User: SimpleUserContext{userId=..., name=user@company.com, orgId=…} no está autorizado para obtener acceso al clúster | El usuario que ha generado el token de acceso no tiene permiso para obtener acceso al clúster de Databricks especificado en el servicio vinculado. | Asegúrese de usuario tiene los permisos necesarios en el área de trabajo.   |
| 3203           | El clúster está en estado Terminado y no está disponible para recibir trabajos. Repare el clúster o inténtelo más tarde. | Se ha finalizado el clúster.    Para un clúster interactivo, esto podría ser una condición de carrera. | La mejor manera de evitar este problema es usar clústeres de trabajo.             |
| 3204           | Error al ejecutar el trabajo. Puede haber cualquier número de mensajes de error, desde un estado de clúster inesperado a un mensaje específico de actividad.  Lo más común es que no aparezca ningún mensaje de error. | N/D                                                          | N/D                                                          |



## <a name="azure-data-lake-analytics-u-sql"></a>Azure Data Lake Analytics (U-SQL)

| Código de error         | Mensaje de error                                                | Descripción del problema                                          | Posible solución/acción recomendada                             |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | El token de acceso procede del inquilino incorrecto                    | Inquilino de AAD incorrecto                                         | La entidad de servicio utilizada para obtener acceso a ADLA pertenece a otro inquilino de AAD. Cree una nueva entidad de servicio en el mismo inquilino que la cuenta de ADLA. |
| 2711,   2705,   2704 | Prohibido. Error de verificación de ACL. El recurso no existe o el usuario no está autorizado para realizar la operación solicitada<br/><br/>El usuario no puede obtener acceso al almacén datalake  <br/><br/>El usuario no está autorizado para Data Lake Analytics | La entidad de servicio o el certificado proporcionado no tiene acceso al archivo en el almacenamiento | Asegúrese de que la entidad de servicio o el certificado que se proporciona para los trabajos de ADLA tiene acceso a la cuenta de ADLA y al almacenamiento predeterminado de ADLS desde la carpeta raíz. |
| 2711                 | No se encuentra el archivo o la carpeta "Azure Data Lake Store"       | La ruta de acceso al archivo USQL es incorrecta o las credenciales del servicio vinculado no tienen acceso | Compruebe la ruta de acceso y las credenciales proporcionadas en el servicio vinculado |
| 2707                 | No se puede resolver la cuenta de AzureDataLakeAnalytics. Compruebe "AccountName" y "DataLakeAnalyticsUri". | La cuenta de ADLA en el servicio vinculado es incorrecta                  | Compruebe que se proporciona la cuenta adecuada             |
| 2703                 | Identificador del error: E_CQO_SYSTEM_INTERNAL_ERROR o cualquier error que empiece por "Id. de Error:" | El error procede de ADLA                                    | Cualquier error que se parece al ejemplo significa que el trabajo se envió a ADLA y el script produjo error allí. La investigación tiene realizarse en ADLA. Si abre el portal y va a la cuenta de ADLA, busque el trabajo mediante el uso del identificador de ejecución de actividad de ADF. (no el identificador de ejecución de canalización). El trabajo tendrá más información sobre el error y le ayudará a solucionar problemas. Si la resolución no está clara, póngase en contacto con el equipo de soporte técnico de ADLA y proporcióneles la dirección URL del trabajo, que incluye el nombre de cuenta y el identificador del trabajo. |
| 2709                 | No podemos aceptamos su trabajo en este momento. El número máximo de trabajos en cola para su cuenta es de 200. | Limitación en ADLA                                           | Reduzca el número de trabajos enviados a ADLA cambiando los desencadenadores de ADF y la configuración de simultaneidad en las actividades, o aumente los límites de ADLA |
| 2709                 | Este trabajo se rechazó porque requiere 24 AUS y esta cuenta tiene una directiva definida por el administrador que impide que un trabajo utilice más de 5 AUS. | Limitación en ADLA                                           | Reduzca el número de trabajos enviados a ADLA cambiando los desencadenadores de ADF y la configuración de simultaneidad en las actividades, o aumente los límites de ADLA |



## <a name="azure-functions"></a>Azure Functions

| Código de error | Mensaje de error                           | DESCRIPCIÓN                                                  | Posible solución/acción recomendada                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | El contenido de la respuesta no es un elemento JObject válido | Esto significa que la función de Azure a la que se llamó no devolvió una carga JSON en la respuesta. La actividad de la función de Azure de ADF solo admite contenido de respuesta JSON. | Actualice la función de Azure para devolver una carga JSON válida, por ejemplo, una función C# puede devolver (ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}"); |
| 3600         | HttpMethod no válido: ‘..’.               | Esto significa que la actividad de función de Azure no admite el método HTTP especificado en la carga de la actividad. | Los métodos HTTP que se admiten son:  <br/>PUT, POST,GET, DELETE, OPTIONS, HEAD, TRACE |



## <a name="custom-azure-batch"></a>Personalizado (Azure Batch)
| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Posible solución/acción recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2\.500         | Se obtuvo una excepción inesperada y ha fallado la ejecución.             | No se puede iniciar el comando, o el programa devolvió un código de error. | Compruebe que existe el archivo ejecutable. Si se inicia el programa, compruebe los archivos stdout.txt y stderr.txt cargados en la cuenta de almacenamiento. Es un procedimiento recomendado emitir abundantes registros en el código para la depuración. |
| 2501         | No se puede obtener acceso a la cuenta de lote de usuario, compruebe la configuración de la cuenta de lote. | Se ha proporcionado una clave de acceso o un nombre de grupo de Batch incorrecto.            | Es necesario que compruebe el nombre del grupo y la clave de acceso de Batch en el servicio vinculado. |
| 2502         | No se puede obtener acceso a la cuenta de almacenamiento del usuario, compruebe la configuración de la cuenta de almacenamiento. | Se ha proporcionado un nombre de cuenta de almacenamiento o una clave de acceso incorrecto.       | Tiene que comprobar la clave de acceso y el nombre de cuenta de almacenamiento en el servicio vinculado. |
| 2504         | La operación devolvió un código de estado no válido "BadRequest"     | Hay demasiados archivos en folderPath en la actividad personalizada.  (El tamaño total de resourceFiles no puede superar los 32 768 caracteres). | Quite los archivos innecesarios, o comprímalos y agregue un comando unzip para la extracción, por ejemplo use: powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe |
| 2505         | No se puede crear la firma de acceso compartido a menos que se usen las credenciales de clave de cuenta. | Las actividades personalizadas solo admiten cuentas de almacenamiento que usen una clave de acceso. | Consulte la descripción de error                                            |
| 2507         | La ruta de acceso a la carpeta no existe o está vacía: ...            | No hay archivos en la cuenta de almacenamiento en la ruta de acceso especificada.       | FolderPath tiene que contener los archivos ejecutables que desea ejecutar. |
| 2508         | La carpeta de recursos tiene archivos duplicados.               | Hay varios archivos con el mismo nombre en subcarpetas diferentes de folderPath. | Estructura de carpetas de actividades personalizadas aplanada en folderPath.  Si es necesario conservar la estructura de la carpeta, comprima los archivos y extráigalos en Azure Batch con un comando unzip, por ejemplo use: powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe |
| 2509         | La dirección URL del lote no es válida, tiene que estar en formato de URI.         | Las direcciones URL del lote tienen que ser similares a https://mybatchaccount.eastus.batch.azure.com | Consulte la descripción de error                                            |
| 2510         | Se produjo un error al enviar la solicitud.               | La dirección URL del lote no es válida                                         | Compruebe la dirección URL del lote.                                            |

## <a name="hdinsight-spark-hive-mapreduce-pig-hadoop-streaming"></a>HDInsight (Spark, Hive, MapReduce, Pig, Hadoop Streaming)

| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Posible solución/acción recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300,   2310 | Error en el envío del trabajo de Hadoop. Error: No se pudo resolver el nombre remoto. <br/><br/>No se encuentra el clúster. | El URI del clúster proporcionado no es válido                              | Asegúrese de que el clúster no se ha eliminado y que el URI proporcionado es correcto. Puede abrir el URI en cualquier explorador y debería ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado, y debe intentar abrirlo desde una máquina virtual que forme parte de la misma red virtual. Para más información consulte [Conexión directa a los servicios de Apache Hadoop](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2300         | Error en el envío del trabajo de Hadoop. Trabajo:..., Clúster:... /. Error: Se ha cancelado una tarea. | Se agotó el tiempo de espera del envío del trabajo.                         | Podría tratarse de un problema de conectividad general de HDInsight o de un problema de conectividad de red. En primer lugar, confirme que la interfaz de usuario de HDInsight Ambari está disponible a través de cualquier explorador y que sus credenciales siguen siendo válidas. Asegúrese de hacerlo desde la máquina virtual o máquina donde está instalado el IR autohospedado, si usa un IR autohospedado. A continuación, intente volver a enviar el trabajo desde ADF. Si sigue sin funcionar, póngase en contacto con el equipo de ADF para obtener soporte técnico. |
| 2300         | No autorizado:   El nombre de usuario o la contraseña de Ambari es incorrecto  <br/><br/>No autorizado:   El administrador de usuario está bloqueado en Ambari   <br/><br/>403 -   Prohibido: Acceso denegado | Las credenciales proporcionadas para HDInsight son incorrectas o han expirado | Corríjalas y vuelva a implementar el servicio vinculado. Asegúrese de que las credenciales funcionan en HDInsight en primer lugar, abra el URI del clúster en cualquier explorador e intente iniciar sesión. Si no funcionan, puede restablecerlas desde Azure Portal. |
| 2300,   2310 | 502 - el servidor web ha recibido una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy       <br/>Puerta de enlace incorrecta | El error procede de HDInsight                               | Este error procede del clúster de HDInsight. Consulte [Solucionador de problemas de HDInsight](https://hdinsight.github.io/ambari/ambari-ui-502-error.html) para los errores comunes.    <br/>Para los clústeres de Spark, también podría deberse a [esto](https://hdinsight.github.io/spark/spark-thriftserver-errors.html). <br/><br/>[Vínculo adicional](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502) |
| 2300         | Error en el envío del trabajo de Hadoop. Job: …, Cluster: ... Error: {\"error\":\"no se puede atender la solicitud de trabajo de envío ya que el servicio templeton está ocupado con demasiadas solicitudes de trabajo enviadas. Espere un tiempo antes de reintentar la operación. Consulte la configuración   templeton.parallellism.job.submit para configurar solicitudes simultáneas. \  <br/><br/>Error en el envío del trabajo de Hadoop. Trabajo: 161da5d4-6fa8-4ef4-a240-6b6428c5ae2f, clúster: https://abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/.   Error: {\"error\":\"java.io.IOException: org.apache.hadoop.yarn.exceptions.YarnException: No se pudo enviar application_1561147195099_3730 a YARN: org.apache.hadoop.security.AccessControlException: La cola root.joblauncher ya tiene 500 aplicaciones, no puede aceptar el envío de una aplicación: application_1561147195099_3730\ | Se han enviado demasiados trabajos a la vez a HDInsight | Considere la posibilidad de limitar el número de trabajos simultáneos que se envían a HDI. Si se envían por la misma actividad, consulte la simultaneidad de la actividad de ADF. Cambie los desencadenadores para que las ejecuciones de canalización simultánea se repartan en el tiempo. También acuda a la documentación de HDInsight con el fin de ajustar "templeton.parallellism.job.submit" como sugiere el error. |
| 2303,   2347 | Error del trabajo de Hadoop con código de salida "5". Para más información, consulte "wasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr".  <br/><br/>Error de ejecución de Hive con código de error "UserErrorHiveOdbcCommandExecutionFailure".   Para más información, consulte "wasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out" | Se envió el trabajo a HDInsight y se produjo un error en HDInsight | El trabajo se ha enviado correctamente a HDInsight. El error se produjo en el clúster. Abra el trabajo en la interfaz de usuario de HDInsight Ambari y abra los registros allí, o abra el archivo de almacenamiento como señala el mensaje de error. Los detalles del error estarán en ese archivo. |
| 2328         | Se produjo un error interno al procesar la solicitud. Vuelva a intentar la solicitud o póngase en contacto con soporte técnico | Se produce en HDInsight a petición.                              | Este error procede de servicio de HDInsight cuando se produce un error en el aprovisionamiento de HDInsight. Póngase en contacto con el equipo de HDInsight y proporcione el nombre del clúster a petición. |
| 2310         | java.lang.NullPointerException                               | Se produjo un error al enviar el trabajo al clúster de Spark      | Esta excepción procede de HDInsight y oculta el problema real.   Póngase en contacto con el equipo de HDInsight para obtener soporte técnico y proporcióneles el nombre del clúster y el intervalo de tiempo de ejecución de actividad. |
|              | Todos los demás errores                                             |                                                              | Consulte el [Solucionador de problemas de HDInsight](../hdinsight/hdinsight-troubleshoot-guide.md) y las [preguntas más frecuentes de HDInsight](https://hdinsight.github.io/) |



## <a name="web-activity"></a>Actividad web

| Código de error | Mensaje de error                                                | DESCRIPCIÓN                                                  | Posible solución/acción recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | HttpMethod no válido: ‘..’.                                    | Esto significa que la actividad web no admite el método HTTP especificado en la carga de la actividad. | Los métodos HTTP que se admiten son: <br/>PUT, POST, GET, DELETE |
| 2108         | 500 Error de servidor no válido                                     | Error interno en el punto de conexión                               | Compruebe la funcionalidad en la dirección URL (con Fiddler o Postman): [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | No autorizado 401                                             | Falta de autenticación válida en solicitud                      | Proporcione el método de autenticación válido (el token puede haber expirado).   <br/><br/>Compruebe la funcionalidad en la dirección URL (con Fiddler o Postman): [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Prohibido 403                                                | Faltan permisos necesarios                                 | Compruebe los permisos de usuario en el recurso al que se obtiene acceso.   <br/><br/>Compruebe la funcionalidad en la dirección URL (con Fiddler o Postman): [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Solicitud incorrecta 400                                              | Solicitud HTTP no válida                                         | Compruebe la dirección URL, el verbo y el cuerpo de la solicitud.   <br/><br/>Use Fiddler o Postman para validar la solicitud: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | No encontrado 404                                                | No se encontró el recurso                                       | Use Fiddler o Postman para validar la solicitud: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Servicio no disponible                                          | El servicio no está disponible                                       | Use Fiddler o Postman para validar la solicitud: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Tipo de medio no compatible                                       | Tipo de contenido que no coincide con el cuerpo de la actividad web           | Especifique el tipo de contenido correcto que coincida con el formato de la carga. Use Fiddler o Postman para validar la solicitud: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | El recurso que está buscando se ha quitado, se le ha cambiado el nombre o no está disponible temporalmente. | El recurso no está disponible                                | Use Fiddler o Postman para comprobar el punto de conexión: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | No se puede mostrar la página que está buscando porque se está usando un método no válido (verbo HTTP). | El método de actividad web que se especificó en la solicitud es incorrecto   | Use Fiddler o Postman para comprobar el punto de conexión: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | invalid_payload                                              | El cuerpo de la actividad web es incorrecto                       | Use Fiddler o Postman para comprobar el punto de conexión: [Cómo usar Fiddler para crear una sesión HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |

#### <a name="how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application"></a>Cómo usar Fiddler para crear una sesión HTTP de la aplicación web supervisada

1. Descargue e instale [Fiddler](https://www.telerik.com/download/fiddler)

2. Si su aplicación web usa HTTPS:

   1. Abra Fiddler

   2. Vaya a **Tools > Fiddler Options** (Herramientas > Opciones de Fiddler) y realice una selección como la de la siguiente captura de pantalla. 

      ![Opciones de Fiddler](media/data-factory-troubleshoot-guide/fiddler-options.png)

3. Si la aplicación usa certificados SSL, también tiene que agregar el certificado de Fiddler al dispositivo.

4. Para agregar el certificado de Fiddler al dispositivo, vaya a **Tools**(Herramientas) > **Fiddler Options**(Opciones de Fiddler) > **HTTPS** > **Actions**(Acciones) > **Export Root Certificate to Desktop** (Exportar el certificado raíz al escritorio) para obtener el certificado de Fiddler.

5. Desactive la opción de captura para que se pueda borrar la caché del explorador con el fin de iniciar un seguimiento nuevo.

6. 1. Vaya a **File**(Archivo) > **Capture Traffic** (Capturar tráfico) o presione **F12**.
   2. Borre la caché del explorador para quitar todos los elementos almacenados en caché, que tendrá que volver a descargar.

7. Cree una solicitud: 

8. 1. Haga clic en la pestaña Composer (Compositor)
   2. Establezca el método HTTP y la dirección URL
   3. Agregue encabezados y cuerpo de la solicitud si es necesario
   4. Haga clic en Execute (Ejecutar)

9. Empiece a capturar tráfico de nuevo y complete la transacción problemática en su página.

10. Una vez completado, vaya a **File** (Archivo) > **Save** (Guardar) > **All Sessions** (Todas las sesiones).

[Aquí](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler) encontrará información sobre Fiddler

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda en la búsqueda de soluciones para su problema, aquí tiene algunos otros recursos que puede probar.

*  [Blogs](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de función](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter](https://twitter.com/hashtag/DataFactory)



