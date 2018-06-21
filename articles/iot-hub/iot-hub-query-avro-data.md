---
title: Consulta de datos de Avro con Azure Data Lake Analytics | Microsoft Docs
description: Use propiedades del cuerpo de mensaje para enrutar la telemetría del dispositivo al almacenamiento en blobs y consultar los datos con formato Avro escritos en almacenamiento de blobs.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 98a30155c73a937042b4bea6568543fb5152d748
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726676"
---
# <a name="query-avro-data-using-azure-data-lake-analytics"></a>Consulta de datos de Avro con Azure Data Lake Analytics

En este artículo se aborda sobre cómo consultar los datos de Avro para el enrutamiento eficaz de mensajes de Azure IoT Hub a servicios de Azure. Como se anunció en la entrada de blog [Azure IoT Hub message routing: now with routing on message body] (Enrutamiento de mensajes de Azure IoT Hub: ahora con enrutamiento en el cuerpo del mensaje), IoT Hub ahora admite el enrutamiento en las propiedades o en el cuerpo del mensaje. Vea también [Enrutamiento en los cuerpos del mensaje][Routing on message bodies]. 

La dificultad radica en que cuando Azure IoT Hub enruta los mensajes al almacenamiento de blobs, IoT Hub escribe el contenido en el formato Avro, que tiene propiedades de mensaje y de cuerpo de mensaje. Tenga en cuenta que IoT Hub solo admite la escritura de datos en el almacenamiento de blobs en el formato de datos de Avro, y este formato no se utiliza este formato para ningún otro punto de conexión. Consulte [Cuándo usar los contenedores de Azure Storage][When using Azure storage containers]. Aunque el formato Avro es muy útil para conservar datos/mensajes, resulta difícil para consultar los datos. En comparación, es mucho más fácil consultar datos en formato JSON o CSV.

Para resolver este problema, puede usar muchos de los patrones de macrodatos para transformar y escalar datos a fin de abordar las necesidades y los formatos de macrodatos no relacionales. Uno de los modelos, un patrón de "pago por consulta", es Azure Data Lake Analytics (ADLA). A él se dedica fundamentalmente este artículo. Aunque puede ejecutar fácilmente la consulta en Hadoop o en otras soluciones, ADLA es con frecuencia más conveniente para este "pago por consulta". Hay un "extractor" para Avro en U-SQL. Vea [U-SQL Avro Example] (Ejemplo de Avro en U-SQL).

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Consulta y exportación de datos de Avro a un archivo CSV
La sección le guía a través de la consulta de datos de Avro y su exportación a un archivo CSV en Azure Blob Storage, aunque podría colocar fácilmente los datos en otros almacenes de datos o repositorios.

1. Configure Azure IoT Hub para enrutar los datos a un punto de conexión de Azure Blob Storage utilizando una propiedad en el cuerpo del mensaje para seleccionar los mensajes.

    ![Captura de pantalla del paso 1a][img-query-avro-data-1a]

    ![Captura de pantalla del paso 1b][img-query-avro-data-1b]

2. Asegúrese de que el dispositivo tiene la codificación, el tipo de contenido y los datos necesarios en las propiedades o en el cuerpo del mensaje, tal y como se menciona en la documentación del producto. Cuando se ve en Device Explorer (ver abajo), puede comprobar que estos atributos están establecidos correctamente.

    ![Captura de pantalla del paso 2][img-query-avro-data-2]

3. Configure un almacén Azure Data Lake Store (ADLS) y una instancia de Azure Data Lake Analytics. Si bien Azure IoT Hub no enruta a un almacén Azure Data Lake Store, ADLA requiere uno.

    ![Captura de pantalla del paso 3][img-query-avro-data-3]

4. En ADLA, configure Azure Blob Storage como un almacén adicional, el mismo Blob Storage a donde Azure IoT Hub enruta datos.

    ![Captura de pantalla del paso 4][img-query-avro-data-4]
 
5. Como se describe en [U-SQL Avro Example] (Ejemplo de Avro en U-SQL), se necesitan 4 archivos DLL.  Cargue estos archivos en una ubicación en su ADLS.

    ![Captura de pantalla del paso 5][img-query-avro-data-5] 

6. En Visual Studio, cree un proyecto de U-SQL.
 
    ![Captura de pantalla del paso 6][img-query-avro-data-6]

7. Copie el contenido del siguiente script y péguelo en el archivo recién creado. Modifique las tres secciones resaltadas: su cuenta ADLA, las rutas de acceso de las DLL asociadas y la ruta de acceso correcta para la cuenta de almacenamiento.
    
    ![Captura de pantalla del paso 7a][img-query-avro-data-7a]

    El script U-SQL real para la salida sencilla a CSV:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Al ejecutar el script que se muestra a continuación, ADLA tardó 5 minutos cuando se limitó a 10 unidades analíticas y procesó 177 archivos, resumiendo la salida en un archivo CSV.
    
    ![Captura de pantalla del paso 7b][img-query-avro-data-7b]

    Al fijarse en la salida, puede ver que el contenido de Avro se ha convertido en un archivo CSV. Vaya al paso 8 si desea analizar el JSON.
    
    ![Captura de pantalla del paso 7c][img-query-avro-data-7c]

    
8. La mayoría de los mensajes de IoT están en formato JSON.  Al agregar las líneas siguientes, puede analizar el mensaje en JSON, para que pueda agregar las cláusulas WHERE y generar solo los datos necesarios.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

9. Al fijarse en la salida, verá ahora las columnas para cada elemento en el comando select. 
    
    ![Captura de pantalla del paso 8][img-query-avro-data-8]

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a consultar los datos de Avro para el enrutamiento eficaz de mensajes de Azure IoT Hub a servicios de Azure.

Para ver ejemplos de soluciones de un extremo a otro que usen IoT Hub, vea el [acelerador de la solución de supervisión remota de Azure IoT][lnk-iot-sa-land].

Para obtener más información sobre cómo desarrollar soluciones con IoT Hub, consulte la [Guía para desarrolladores de IoT Hub].

Para obtener más información sobre el enrutamiento de mensajes en IoT Hub, consulte [Envío y recepción de mensajes con IoT Hub][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[Azure IoT Hub message routing: now with routing on message body]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/ (Enrutamiento de mensajes de Azure IoT Hub: ahora con enrutamiento en el cuerpo del mensaje)

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[U-SQL Avro Example]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples (Ejemplo de Avro en U-SQL).

[lnk-iot-sa-land]: ../iot-accelerators/index.md
[Guía para desarrolladores de IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
