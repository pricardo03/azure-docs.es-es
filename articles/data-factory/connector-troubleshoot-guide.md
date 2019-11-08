---
title: Solución de problemas de conectores en Azure Data Factory
description: Obtenga información acerca de la solución de problemas relacionados con conectores en Azure Data Factory.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 08/26/2019
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 8cabc1031f9d0be772ba087109ae1dfc881ce163
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73680085"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Solución de problemas de conectores en Azure Data Factory

En este artículo se exploran métodos comunes de solución de problemas de conectores en Azure Data Factory.

## <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Mensaje de error: El servidor remoto devolvió un error: (403) Prohibido

- **Síntomas**: Se produce el siguiente error en la actividad de copia: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Causa**: Una posible causa es que la entidad de servicio o la identidad administrada que usa no tiene permiso para acceder a una carpeta o un archivo determinados.

- **Resolución**: Conceda los permisos correspondientes en todas las carpetas y subcarpetas que necesita copiar. Consulte [este documento](connector-azure-data-lake-store.md#linked-service-properties).

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Mensaje de error: No se pudo obtener el token de acceso mediante la entidad de servicio. Error de ADAL: service_unavailable

- **Síntomas**: Se produce el siguiente error en la actividad de copia:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Causa**: Cuando el servicio de token de seguridad (STS) propiedad de Azure Active Directory no está disponible, es decir, cuando está demasiado ocupado para administrar las solicitudes, devuelve un error HTTP 503. 

- **Resolución**: Vuelva a ejecutar la actividad de copia después de unos minutos.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Mensaje de error: Error de conversión al convertir de una cadena de caracteres a uniqueidentifier

- **Síntomas**: Al copiar datos desde un origen de datos tabulares (como SQL Server) en Azure SQL Data Warehouse mediante copias almacenadas provisionalmente y PolyBase, se produce el siguiente error:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Causa**: PolyBase de Azure SQL Data Warehouse no puede convertir una cadena vacía en GUID.

- **Resolución**: En el receptor de la actividad de copia, en la configuración de PolyBase, establezca la opción **Use Type default** (Usar tipo predeterminado) en false.

### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Mensaje de error: Expected data type: DECIMAL(x,x), Offending value (Tipo de datos esperado: DECIMAL(x,x), valor incorrecto)

- **Síntomas**: Al copiar datos desde un origen de datos tabulares (como SQL Server) en SQL Data Warehouse mediante copias almacenadas provisionalmente y PolyBase, se produce el siguiente error:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Causa**: PolyBase de Azure SQL Data Warehouse no puede insertar una cadena vacía (valor null) en la columna decimal.

- **Resolución**: En el receptor de la actividad de copia, en la configuración de PolyBase, establezca la opción **Use Type default** (Usar tipo predeterminado) en false.

### <a name="error-message-java-exception-messagehdfsbridgecreaterecordreader"></a>Mensaje de error: Java exception message:HdfsBridge::CreateRecordReader (Mensaje de excepción de Java: HdfsBridge::CreateRecordReader)

- **Síntomas**: Al copiar datos en Azure SQL Data Warehouse mediante PolyBase se produce el siguiente error:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Causa**: La posible causa es que el esquema (ancho total de columna) es demasiado grande (más de 1 MB). Compruebe el esquema de la tabla de SQL Data Warehouse de destino sumando el tamaño de todas las columnas:

    - Int -> 4 bytes
    - Bigint -> 8 bytes
    - Varchar(n),char(n),binary(n), varbinary(n) -> n bytes
    - Nvarchar(n), nchar(n) -> n*2 bytes
    - Date -> 6 bytes
    - Datetime/(2), smalldatetime -> 16 bytes
    - Datetimeoffset -> 20 bytes
    - Decimal -> 19 bytes
    - Float -> 8 bytes
    - Money -> 8 bytes
    - Smallmoney -> 4 bytes
    - Real -> 4 bytes
    - Smallint -> 2 bytes
    - Time -> 12 bytes
    - Tinyint -> 1 byte

- **Resolución**: Reduzca el ancho de columna a menos de 1 MB.

- También puede deshabilitar PolyBase para usar un enfoque de inserción masiva.

### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Mensaje de error: The condition specified using HTTP conditional header(s) is not met (No se cumple la condición especificada mediante encabezados condicionales HTTP)

- **Síntomas**: Al utilizar una consulta SQL para extraer datos de Azure SQL Data Warehouse, se produce el siguiente error:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Causa**: Se produce un problema en Azure SQL Data Warehouse al consultar la tabla externa en Azure Storage.

- **Resolución**: Ejecute la misma consulta en SSMS y compruebe si ve el mismo resultado. En caso afirmativo, abra una incidencia de soporte técnico para Azure SQL Data Warehouse y proporcione el nombre de la base de datos y el servidor de SQL Data Warehouse para solucionar el problema.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Mensaje de error: Request size is too large (El tamaño de la solicitud es demasiado grande)

- **Síntomas**: Al copiar datos en Azure Cosmos DB con el tamaño de lote de escritura predeterminado, se produce el error *"**El tamaño de la solicitud es demasiado grande**"* .

- **Causa**: Cosmos DB limita el tamaño de una única solicitud a 2 MB. La fórmula es Tamaño de la solicitud = tamaño de documento único x tamaño de lote de escritura. Si el tamaño del documento es grande, el comportamiento predeterminado producirá un tamaño de solicitud demasiado grande. Puede ajustar el tamaño del lote de escritura.

- **Resolución**: En el receptor de la actividad de copia, reduzca el valor del tamaño del lote de escritura (el valor predeterminado es 10000).

### <a name="error-message-unique-index-constraint-violation"></a>Mensaje de error: Unique index constraint violation (Infracción de restricción de índice único)

- **Síntomas**: Al copiar datos en Cosmos DB, se produce el siguiente error:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception &#61; Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Causa**: Hay dos causas posibles:

    - Si utiliza **Insert** como comportamiento de escritura, este error indica que los datos de origen tienen filas/objetos con el mismo identificador.

    - Si usa **Upsert** como comportamiento de escritura y establece otra clave única en el contenedor, este error significa que los datos de origen tienen filas/objetos con identificadores diferentes pero con el mismo valor para la clave única definida.

- **Resolución**: 

    - Para la primera causa, establezca **Upsert** como comportamiento de escritura.
    - Para la segunda causa, asegúrese de que cada documento tiene un valor diferente para la clave única definida.

### <a name="error-message-request-rate-is-large"></a>Mensaje de error: La tasa de solicitudes es grande

- **Síntomas**: Al copiar datos en Cosmos DB, se produce el siguiente error:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Causa**: Las unidades de solicitud utilizadas son mayores que la RU disponible configurada en Cosmos DB. Obtenga información sobre cómo Cosmos DB calcula la RU [aquí](../cosmos-db/request-units.md#request-unit-considerations).

- **Resolución**: Hay dos soluciones:

    1. **Aumente la RU del contenedor** a un valor mayor en Cosmos DB, lo que mejorará el rendimiento de la actividad de copia, aunque acarreará un mayor costo en Cosmos DB. 

    2. Reduzca **writeBatchSize** a un valor más pequeño (por ejemplo, 1000) y establezca **parallelCopies** en un valor inferior, como 1, lo que hará que el rendimiento de la ejecución de copia sea peor que el actual, pero no acarreará más costos en Cosmos DB.

### <a name="column-missing-in-column-mapping"></a>Falta una columna en la asignación de columnas

- **Síntomas**: Cuando se importa un esquema para Cosmos DB para la asignación de columnas, faltan algunas columnas. 

- **Causa**: ADF infiere el esquema de los 10 primeros documentos de Cosmos DB. Si algunas columnas o propiedades no tienen valor en esos documentos, ADF no las detectará, por lo que no se mostrarán.

- **Resolución**: Puede ajustar la consulta como se muestra a continuación para hacer que la columna aparezca en el conjunto de resultados con un valor vacío (suponga que la columna "impossible" falta en los 10 primeros documentos). Como alternativa, puede agregar manualmente la columna para la asignación.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Mensaje de error: The GuidRepresentation for the reader is CSharpLegacy (El valor de GuidRepresentation del lector es CSharpLegacy)

- **Síntomas**: Al copiar datos desde Cosmos DB MongoAPI/MongoDB con el campo de UUID, se produce el siguiente error:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Causa**: Hay dos maneras de representar el UUID en BSON: UuidStardard y UuidLegacy. De forma predeterminada, UuidLegacy se usa para leer datos. Se producirá un error si los datos de UUID en MongoDB son UuidStandard.

- **Resolución**: En la cadena de conexión de MongoDB, agregue la opción "**uuidRepresentation=standard**". Para más información, consulte [Cadena de conexión de MongoDB](connector-mongodb.md#linked-service-properties).

## <a name="sftp"></a>SFTP

### <a name="error-message-invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>Mensaje de error: Invalid SFTP credential provided for 'SshPublicKey' authentication type (Se proporcionó una credencial SFTP no válida para el tipo de autenticación "SshPublicKey")

- **Síntomas**: Al usar la autenticación `SshPublicKey`, se produce el siguiente error:

    ```
    Invalid Sftp credential provided for 'SshPublicKey' authentication type
    ```

- **Causa**: Existen tres posibles causas:

    1. Si usa la interfaz de usuario de creación de ADF para crear el servicio vinculado de SFTP, este error indica que la clave privada que eligió usar tiene un formato incorrecto. Es posible que use un formato PKCS#8 de clave privada SSH, mientras que ADF solo admite el formato de clave SSH tradicional. Más concretamente, la diferencia entre el formato PKCS#8 y el formato de clave tradicional es que el contenido de la clave PKCS#8 comienza por " *-----BEGIN ENCRYPTED PRIVATE KEY-----* ", mientras que el formato de clave tradicional empieza por " *-----BEGIN RSA PRIVATE KEY-----* ".
    2. Si usa Azure Key Vault para almacenar el contenido de la clave privada o utiliza programación para crear el servicio vinculado de SFTP, este error indica que el contenido de la clave privada es incorrecto (probablemente no esté codificado en Base64).
    3. Contenido de clave privada o credencial no válido.

- **Resolución**: 

    - Para la primera causa, ejecute los siguientes comandos para convertir la clave al formato de clave tradicional y, a continuación, úsela en la interfaz de usuario de creación de ADF.

        ```
        # Decrypt the pkcs8 key and convert the format to traditional key format
        openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file

        chmod 600 traditional_format_key_file

        # Re-encrypte the key file using passphrase
        ssh-keygen -f traditional_format_key_file -p
        ```

    - Para la segunda causa, para generar esa cadena, el cliente puede usar los dos métodos siguientes:
    - Usar una herramienta de conversión a Base64 de terceros: pegue todo el contenido de la clave privada en herramientas como [Base64 Encode and Decode](https://www.base64encode.org/), codifíquelo en una cadena con formato de Base64 y, a continuación, pegue esta cadena en el almacén de claves o use este valor para la creación del servicio vinculado de SFTP mediante programación.
    - Usar código C#:

        ```c#
        byte[] keyContentBytes = File.ReadAllBytes(privateKeyPath);
        string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
        ```

    - Para la tercera causa, compruebe si el archivo de clave o la contraseña son correctos usando otras herramientas para validar si puede usarlos para acceder al servidor SFTP correctamente.

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)



