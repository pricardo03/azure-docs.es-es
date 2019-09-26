---
title: Solución de problemas de Azure Data Factory | Microsoft Docs
description: Obtenga información acerca de cómo solucionar problemas de actividades de control externo en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: ed466b072a771c3aa288a96fa4a0037c31b875f9
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71091991"
---
# <a name="troubleshoot-azure-data-factory"></a>Solución de problemas de Azure Data Factory

En este artículo se exploran métodos comunes de solución de problemas para actividades de control externo en Azure Data Factory.

## <a name="connector-and-copy-activity"></a>Conector y actividad de copia

Para más información sobre los problemas con el conector (por ejemplo, errores relacionados con la actividad de copia), consulte [Solución de problemas de conectores en Azure Data Factory](connector-troubleshoot-guide.md).

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code--3200"></a>Código de error:  3200

- **Mensaje**: Error 403.

- **Causa**: `The Databricks access token has expired.`

- **Recomendación:** De forma predeterminada, el token de acceso de Azure Databricks es válido durante noventa días. Cree un nuevo token y actualice el servicio vinculado.


### <a name="error-code--3201"></a>Código de error:  3201

- **Mensaje**: `Missing required field: settings.task.notebook_task.notebook_path.`

- **Causa**: `Bad authoring: Notebook path not specified correctly.`

- **Recomendación:** Especifique la ruta de acceso de Notebook en la actividad de Databricks.

<br/>

- **Mensaje**: `Cluster   ... does not exist.`

- **Causa**: `Authoring error: Databricks cluster does not exist or has been deleted.`

- **Recomendación:** Compruebe que existe el clúster de Databricks.

<br/>

- **Mensaje**: `Invalid Python file URI.... Please visit Databricks user guide for supported URI schemes.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Especifique las rutas de acceso absolutas para los esquemas de direccionamiento del área de trabajo, o bien `dbfs:/folder/subfolder/foo.py` para los archivos almacenados en el sistema de archivos de Databricks.

<br/>

- **Mensaje**: `{0} LinkedService should have domain and accessToken as required properties.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/>

- **Mensaje**: `{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Causa**:  `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/>

- **Mensaje**: `Node type Standard_D16S_v3 is not supported. Supported node types:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Consulte el mensaje de error. 

<br/>

### <a name="error-code--3202"></a>Código de error:  3202

- **Mensaje**: `There were already 1000 jobs created in past 3600 seconds, exceeding rate limit:   1000 job creations per 3600 seconds.`

- **Causa**: `Too many Databricks runs in an hour.`

- **Recomendación:** Compruebe todas las canalizaciones que usen esta área de trabajo de Databricks para la velocidad de creación de trabajos.  Si las canalizaciones inician demasiadas ejecuciones de Databricks en agregado, migre algunas canalizaciones a un área de trabajo nueva.

<br/>

- **Mensaje**: `Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Causa**: `Authoring error: No value provided for the parameter.`

- **Recomendación:** Inspeccione el json de canalización y asegúrese de que todos los parámetros del cuaderno baseParameters tienen especificado un valor no vacío.

<br/>

- **Mensaje**: `User: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` is not   authorized to access cluster.`

- **Causa**: El usuario que ha generado el token de acceso no tiene permiso para obtener acceso al clúster de Databricks especificado en el servicio vinculado.

- **Recomendación:** Asegúrese de que el usuario tiene los permisos necesarios en el área de trabajo.


### <a name="error-code--3203"></a>Código de error:  3203

- **Mensaje**: `The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Causa**: El clúster se ha finalizado. Para un clúster interactivo, esto podría ser una condición de carrera.

- **Recomendación:** La mejor manera de evitar este error es usar clústeres de trabajo.


### <a name="error-code--3204"></a>Código de error:  3204

- **Mensaje**: `Job execution failed.`

- **Causa**:  Los mensajes de error indican varias incidencias, como un estado de clúster inesperado o una actividad específica. Normalmente, no aparece ningún mensaje de error. 

- **Recomendación:** N/D


## <a name="azure-data-lake-analytics"></a>Análisis con Azure Data Lake

La tabla siguiente se aplica a U-SQL.


### <a name="error-code--2709"></a>Código de error:  2709

- **Mensaje**: `The access token is from the wrong tenant.`

- **Causa**:  Inquilino de Azure Active Directory (Azure AD) incorrecto.

- **Recomendación:** Inquilino de Azure Active Directory (Azure AD) incorrecto.

<br/>

- **Mensaje**: `We cannot accept your job at this moment. The maximum number of queued jobs for   your account is 200. `

- **Causa**:  Este error se debe a una limitación en Data Lake Analytics.

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics.

<br/>

- **Mensaje**: `This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Causa**:  Este error se debe a una limitación en Data Lake Analytics. 

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics.


### <a name="error-code--2705"></a>Código de error:  2705

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.


### <a name="error-code--2711"></a>Código de error:  2711

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.

<br/>

- **Mensaje**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Causa**:  La ruta de acceso al archivo U-SQL es incorrecta o las credenciales del servicio vinculado no tienen acceso.

- **Recomendación:** Compruebe la ruta de acceso y las credenciales proporcionadas en el servicio vinculado.


### <a name="error-code--2704"></a>Código de error:  2704

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.


### <a name="error-code--2707"></a>Código de error:  2707

- **Mensaje**: `Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and   'DataLakeAnalyticsUri'.`

- **Causa**:  La cuenta de Data Lake Analytics en el servicio vinculado es incorrecta.

- **Recomendación:** Compruebe que se proporciona la cuenta adecuada.


### <a name="error-code--2703"></a>Código de error:  2703

- **Mensaje**: `Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error   Id:").`

- **Causa**:  El error es de Data Lake Analytics. 

- **Recomendación:** Cualquier error similar al ejemplo significa que el trabajo se envió a Data Lake Analytics y el script produjo un error allí. Investigue el error en Data Lake Analytics. En el portal, vaya a la cuenta de Data Lake Analytics y busque el trabajo mediante el identificador de ejecución de actividad de Data Factory (no el identificador de ejecución de canalización). El trabajo proporcionará más información sobre el error y le ayudará a solucionar el problema. Si la resolución no está clara, póngase en contacto con el equipo de soporte técnico de Data Lake Analytics y proporcióneles la dirección URL del trabajo, que incluye el nombre de cuenta y el identificador del trabajo.



## <a name="azure-functions"></a>Azure Functions

### <a name="error-code--3602"></a>Código de error:  3602

- **Mensaje**: `Invalid HttpMethod: {method}.`

- **Causa**: La actividad de las funciones de Azure no admite el método HTTP especificado en la carga de la actividad. 

- **Recomendación:** Los métodos HTTP que se admiten son PUT, POST, GET, DELETE, OPTIONS, HEAD y TRACE.


### <a name="error-code--3603"></a>Código de error:  3603

- **Mensaje**: `Response content is not a valid JObject.`

- **Causa**: La función de Azure a la que se llamó no devolvió una carga JSON en la respuesta. La actividad de la función de Azure de Data Factory solo admite contenido de respuesta JSON. 

- **Recomendación:** Actualice la función de Azure para que devuelva una carga JSON válida. Por ejemplo, una C# función puede devolver `(ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}");`.


### <a name="error-code--3606"></a>Código de error:  3606

- **Mensaje**: `Azure function activity missing function key.`

- **Causa**: La definición de actividad de funciones de Azure no está completa. 

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionKey".


### <a name="error-code--3607"></a>Código de error:  3607

- **Mensaje**: `Azure function activity missing function name.`

- **Causa**: La definición de actividad de funciones de Azure no está completa. 

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionName".


### <a name="error-code--3608"></a>Código de error:  3608

- **Mensaje**: `Call to provided Azure function '{FunctionName}' failed with status-'{statusCode}' and message - '{message}'.` 

- **Causa**: Los detalles de las funciones de Azure en la definición de la actividad pueden ser incorrectos. 

- **Recomendación:** Corrija los detalles de las funciones de Azure y vuelva a intentarlo.


### <a name="error-code--3609"></a>Código de error:  3609

- **Mensaje**: `Azure function activity missing functionAppUrl.` 

- **Causa**: La definición de actividad de funciones de Azure no está completa. 

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionAppUrl".


### <a name="error-code--3610"></a>Código de error:  3610

- **Mensaje**: `There was an error while calling endpoint.`

- **Causa**: La dirección URL de la función puede ser incorrecta.

- **Recomendación:** Asegúrese de que el valor de "functionAppUrl" en el JSON de actividad es correcto e inténtelo de nuevo.


### <a name="error-code--3611"></a>Código de error:  3611

- **Mensaje**: `Azure function activity missing Method in JSON.` 

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "method".


### <a name="error-code--3612"></a>Código de error:  3612

- **Mensaje**: `Azure function activity missing LinkedService definition in JSON.`

- **Causa**: La definición de actividad de funciones de Azure puede no estar completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene detalles del servicio vinculado.


## <a name="azure-machine-learning"></a>Azure Machine Learning


### <a name="error-code--4101"></a>Código de error:  4101

- **Mensaje**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Causa**: Formato incorrecto o falta la definición de una propiedad.

- **Recomendación:**  Compruebe si la actividad se ha definido con los datos correctos.


### <a name="error-code--4110"></a>Código de error:  4110

- **Mensaje**: Falta la definición de LinkedService en la actividad AzureMLExecutePipeline en JSON.

- **Causa**: La definición de la actividad AzureMLExecutePipeline no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado.


### <a name="error-code--4111"></a>Código de error:  4111

- **Mensaje**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Causa**: Definición de actividad incorrecta.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado correctos.


### <a name="error-code--4112"></a>Código de error:  4112

- **Mensaje**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Causa**: Formato incorrecto o falta la definición de una propiedad.

- **Recomendación:**  Compruebe que la definición del servicio vinculado tiene datos correctos.


### <a name="error-code--4121"></a>Código de error:  4121

- **Mensaje**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Causa**: La credencial usada para acceder al servicio de Azure ML ha expirado.

- **Recomendación:**  Compruebe que la credencial es válida y vuelva a intentarlo.


### <a name="error-code--4122"></a>Código de error:  4122

- **Mensaje**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Causa**: La credencial proporcionada en el servicio vinculado del servicio AzureML no es válida o no tiene permiso para la operación.

- **Recomendación:**  Compruebe que la credencial del servicio vinculado es válida y tienen permiso para acceder al servicio AzureML.


### <a name="error-code--4123"></a>Código de error:  4123

- **Mensaje**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Causa**: `Properties of the activity such as pipelineParamters are invalid for the Azure ML pipeline.`

- **Recomendación:**  Compruebe el valor de las propiedades de la actividad para que coincida con la carga esperada de la canalización de Azure ML publicada especificada en el servicio vinculado.


### <a name="error-code--4124"></a>Código de error:  4124

- **Mensaje**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Causa**: El punto de conexión de canalización de Azure ML publicado no existe.

- **Recomendación:**  Compruebe que el punto de conexión de la canalización de Azure ML publicado especificado en el servicio vinculado existe en el servicio de Azure ML.


### <a name="error-code--4125"></a>Código de error:  4125

- **Mensaje**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Causa**: Error de servidor en el servicio de Azure ML.

- **Recomendación:**  Inténtelo de nuevo más tarde. Póngase en contacto con el equipo del servicio de Azure ML para obtener ayuda si el problema persiste.


### <a name="error-code--4126"></a>Código de error:  4126

- **Mensaje**: `AzureML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in AzureMLService for more error loggings.`

- **Causa**: Error en la ejecución de la canalización AzureML.

- **Recomendación:**  Proteja AzureMLService para ver más registros de errores y corrija la canalización de ML.


## <a name="custom"></a>Personalizado

La tabla siguiente se aplica a Azure Batch.


### <a name="error-code--2500"></a>Código de error:  2.500

- **Mensaje**: `Hit unexpected exception and execution failed.`

- **Causa**: `Can't launch command, or the program returned an error code.`

- **Recomendación:**  Compruebe que existe el archivo ejecutable. Si se inicia el programa, compruebe si los archivos *stdout.txt* y *stderr.txt* se cargaron en la cuenta de almacenamiento. Es un procedimiento recomendado emitir abundantes registros en el código para la depuración.


### <a name="error-code--2501"></a>Código de error:  2501

- **Mensaje**: `Cannot access user batch account; please check batch account settings.`

- **Causa**: Clave de acceso o nombre de grupo de Batch incorrectos.

- **Recomendación:** Compruebe el nombre del grupo y la clave de acceso de Batch en el servicio vinculado.


### <a name="error-code--2502"></a>Código de error:  2502

- **Mensaje**: `Cannot access user storage account; please check storage account settings.`

- **Causa**: Nombre de cuenta de almacenamiento o clave de acceso incorrectos.

- **Recomendación:** Compruebe la clave de acceso y el nombre de cuenta de almacenamiento en el servicio vinculado.


### <a name="error-code--2504"></a>Código de error:  2504

- **Mensaje**:  `Operation returned an invalid status code 'BadRequest'.` 

- **Causa**: Hay demasiados archivos en folderPath de la actividad personalizada. El tamaño total de resourceFiles no puede superar los 32 768 caracteres.

- **Recomendación:** Elimine los archivos innecesarios. También puede comprimirlos y agregar un comando unzip para extraerlos. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`.


### <a name="error-code--2505"></a>Código de error:  2505

- **Mensaje**: `Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Causa**: Las actividades personalizadas solo admiten cuentas de almacenamiento que usen una clave de acceso.

- **Recomendación:** Consulte la descripción del error.


### <a name="error-code--2507"></a>Código de error:  2507

- **Mensaje**: `The folder path does not exist or is empty: ....`

- **Causa**: No hay archivos en la cuenta de almacenamiento en la ruta de acceso especificada.

- **Recomendación:** La ruta de acceso a la carpeta debe contener los archivos ejecutables que desea ejecutar.


### <a name="error-code--2508"></a>Código de error:  2508

- **Mensaje**:  `There are duplicate files in the resource folder.`

- **Causa**: Hay varios archivos con el mismo nombre en subcarpetas diferentes de folderPath.

- **Recomendación:** Estructura de carpetas de actividades personalizadas aplanada en folderPath.  Si necesita conservar la estructura de carpetas, comprima los archivos y extráigalos en Azure Batch mediante un comando unzip. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`.


### <a name="error-code--2509"></a>Código de error:  2509

- **Mensaje**: `Batch   url ... is invalid; it must be in Uri format.` 

- **Causa**: Las direcciones URL del lote tienen que ser similares a `https://mybatchaccount.eastus.batch.azure.com`

- **Recomendación:** Consulte la descripción del error.


### <a name="error-code--2510"></a>Código de error:  2510

- **Mensaje**: `An   error occurred while sending the request.`

- **Causa**: La dirección URL del lote no es válida. 

- **Recomendación:** Compruebe la dirección URL del lote.


## <a name="hdinsight"></a>HDInsight

La tabla siguiente se aplica a Spark, Hive, MapReduce, Pig y Hadoop Streaming.


### <a name="error-code--2300"></a>Código de error:  2300

- **Mensaje**: `Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **Causa**: El URI del clúster proporcionado no es válido. 

- **Recomendación:**  Asegúrese de que el clúster no se ha eliminado y que el URI proporcionado es correcto. Al abrir el URI en cualquier explorador, debe ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado. Para abrirlo, use una máquina virtual que forme parte de la misma red virtual. Para más información, consulte [Conexión directa a los servicios de Apache Hadoop](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services).

<br/>

- **Mensaje**: `Hadoop job submission failed. Job: …, Cluster: …/. Error: A task was canceled.`

- **Causa**: Se agotó el tiempo de espera de envío del trabajo. 

- **Recomendación:** Podría tratarse de un problema de conectividad general de HDInsight o de un problema de conectividad de red. En primer lugar, confirme que la interfaz de usuario de HDInsight Ambari está disponible desde cualquier explorador. Confirme que las credenciales siguen siendo válidas. Si usa Integrated Runtime (IR) autohospedado, asegúrese de hacerlo desde la máquina virtual o el equipo donde está instalada la instancia de IR autohospedado. A continuación, intente volver a enviar el trabajo desde Data Factory. Si sigue sin funcionar, póngase en contacto con el equipo de Data Factory para obtener soporte técnico.


- **Mensaje**: `Unauthorized: Ambari user name or password is incorrect  <br/><br/>Unauthorized: User admin is locked out in Ambari.   <br/><br/>403 - Forbidden: Access is denied.`

- **Causa**: Las credenciales para HDInsight son incorrectas o han expirado.

- **Recomendación:** Corrija las credenciales y vuelva a implementar el servicio vinculado. En primer lugar, asegúrese de que las credenciales funcionan en HDInsight, para lo que debe abrir el URI del clúster en cualquier explorador e intentar iniciar sesión. Si las credenciales no funcionan, puede restablecerlas desde Azure Portal.

<br/>

- **Mensaje**: `502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **Causa**: Este error procede de HDInsight.

- **Recomendación:** Este error procede del clúster de HDInsight. Para más información, consulte [Error 502 de la interfaz de usuario de Ambari](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) y [Solución de errores de puerta de enlace incorrecta en el servicio Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502).

<br/>

- **Mensaje**: `Hadoop job submission failed. Job: …, Cluster: ... Error:   {\"error\":\"Unable to service the submit job request as   templeton service is busy with too many submit job requests. Please wait for some time before retrying the operation. Please refer to the config   templeton.parallellism.job.submit to configure concurrent requests. <br/><br/>Hadoop job submission failed. Job: xx, Cluster: name.   Error: {\"error\":\"java.io.IOException:   org.apache.hadoop.yarn.exceptions.YarnException: Failed to submit   application_1561147195099_3730 to YARN :   org.apache.hadoop.security.AccessControlException: Queue root.joblauncher already has 500 applications, cannot accept submission of application:   application_1561147195099_3730\`

- **Causa**: Se han enviado demasiados trabajos a la vez a HDInsight.

- **Recomendación:** Considere la posibilidad de limitar el número de trabajos simultáneos que se envían a HDInsight. Consulte la simultaneidad de la actividad de Data Factory si los trabajos los envía la misma actividad. Cambie los desencadenadores para que las ejecuciones de canalización simultánea se repartan en el tiempo. Consulte la documentación de HDInsight para ajustar `templeton.parallellism.job.submit` tal y como sugiere el error.


### <a name="error-code--2303"></a>Código de error:  2303

- **Mensaje**: `Hadoop job failed with exit code '5'. See   'wasbs://adfjobs@xx.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **Causa**: Se envió el trabajo a HDInsight y se produjo un error en HDInsight.

- **Recomendación:** El trabajo se envió correctamente a HDInsight. El error se produjo en el clúster. Abra el trabajo y los registros en la interfaz de usuario de HDInsight Ambari o abra el archivo desde el almacenamiento como sugiere el mensaje de error. El archivo muestra los detalles del error.


### <a name="error-code--2310"></a>Código de error:  2310

- **Mensaje**: `Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **Causa**: El URI del clúster proporcionado no es válido. 

- **Recomendación:**  Asegúrese de que el clúster no se ha eliminado y que el URI proporcionado es correcto. Al abrir el URI en cualquier explorador, debe ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado. Para abrirlo, use una máquina virtual que forme parte de la misma red virtual. Para más información, consulte [Conexión directa a los servicios de Apache Hadoop](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services).

<br/>

- **Mensaje**: `502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **Causa**: Este error procede de HDInsight.

- **Recomendación:** Este error procede del clúster de HDInsight. Para más información, consulte [Error 502 de la interfaz de usuario de Ambari](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html), [Errores 502 al conectarse al servidor Thrift de Spark](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) y [Solución de errores de puerta de enlace incorrecta en el servicio Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502).

<br/>

- **Mensaje**: `java.lang.NullPointerException`

- **Causa**: Este error se produce cuando el trabajo se envía a un clúster de Spark. 

- **Recomendación:** Esta excepción procede de HDInsight. Oculta la incidencia real. Póngase en contacto con el equipo de HDInsight para obtener soporte técnico. Proporcione el nombre del clúster y el intervalo de tiempo de ejecución de la actividad.


### <a name="error-code--2347"></a>Código de error:  2347

- **Mensaje**: `Hadoop job failed with exit code '5'. See 'wasbs://adfjobs@xx.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **Causa**: Se envió el trabajo a HDInsight y se produjo un error en HDInsight.

- **Recomendación:** El trabajo se envió correctamente a HDInsight. El error se produjo en el clúster. Abra el trabajo y los registros en la interfaz de usuario de HDInsight Ambari o abra el archivo desde el almacenamiento como sugiere el mensaje de error. El archivo muestra los detalles del error.


### <a name="error-code--2328"></a>Código de error:  2328

- **Mensaje**: `Internal server error occurred while processing the request. Please retry the request or contact support. `

- **Causa**: Este error se produce en HDInsight a petición.

- **Recomendación:** Este error procede del servicio HDInsight cuando se produce un error en el aprovisionamiento de HDInsight. Póngase en contacto con el equipo de HDInsight y proporcione el nombre del clúster a petición.



## <a name="web-activity"></a>Actividad web

### <a name="error-code--2108"></a>Código de error:  2108

- **Mensaje**:  `Invalid HttpMethod: '...'.`

- **Causa**: La actividad web no admite el método HTTP especificado en la carga de la actividad.

- **Recomendación:**  Los métodos HTTP admitidos son PUT, POST, GET y DELETE.

<br/>

- **Mensaje**: `Invalid Server Error 500.`

- **Causa**: Error interno en el punto de conexión.

- **Recomendación:**  Compruebe la funcionalidad en la dirección URL con Fiddler o Postman.

<br/>

- **Mensaje**: `Unauthorized 401.`

- **Causa**: Falta de autenticación válida en la solicitud.

- **Recomendación:**  El token podría haber expirado. Proporcione un método de autenticación válido. Compruebe la funcionalidad en la dirección URL con Fiddler o Postman.

<br/>

- **Mensaje**: `Forbidden 403.`

- **Causa**: Faltan permisos necesarios.

- **Recomendación:**  Compruebe los permisos de usuario en el recurso al que se obtiene acceso. Compruebe la funcionalidad en la dirección URL con Fiddler o Postman.

<br/>

- **Mensaje**: `Bad Request 400.`

- **Causa**: Solicitud HTTP no válida.

- **Recomendación:**   Compruebe la dirección URL, el verbo y el cuerpo de la solicitud. Use Fiddler o Postman para validar la solicitud.

<br/>

- **Mensaje**: `Not found 404.` 

- **Causa**: No se encontró el recurso.   

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.

<br/>

- **Mensaje**: `Service unavailable.`

- **Causa**: El servicio no está disponible.

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.

<br/>

- **Mensaje**: `Unsupported Media Type.`

- **Causa**: El tipo de contenido no coincide con el cuerpo de la actividad web.

- **Recomendación:**  Especifique el tipo de contenido que coincida con el formato de carga. Use Fiddler o Postman para validar la solicitud.

<br/>

- **Mensaje**: `The resource you are looking for has been removed, has had its name changed, or is temporarily unavailable.`

- **Causa**: El recurso no está disponible. 

- **Recomendación:**  Use Fiddler o Postman para comprobar el punto de conexión.

<br/>

- **Mensaje**: `The page you are looking for cannot be displayed because an invalid method (HTTP verb) is being used.`

- **Causa**: Se ha especificado un método de actividad web incorrecto en la solicitud.

- **Recomendación:**  Use Fiddler o Postman para comprobar el punto de conexión.

<br/>

- **Mensaje**: `invalid_payload`

- **Causa**: El cuerpo de la actividad web es incorrecto.

- **Recomendación:**  Use Fiddler o Postman para comprobar el punto de conexión.


### <a name="error-code--2208"></a>Código de error:  2208

- **Mensaje**:  `Invoking Web Activity failed with HttpStatusCode - {0}.`

- **Causa**: El servicio de destino devolvió el estado de error.

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.


### <a name="error-code--2308"></a>Código de error:  2308

- **Mensaje**:  `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Causa**: Puede haber varios motivos para este error, como la conectividad de red, un error de DNS, la validación de certificados de servidor o el tiempo de espera.

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.


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



