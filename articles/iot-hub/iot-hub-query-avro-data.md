---
title: Consulta de datos de Avro con Azure Data Lake Analytics | Microsoft Docs
description: Use propiedades del cuerpo de mensaje para enrutar la telemetría del dispositivo a Blob Storage y consultar los datos con formato Avro que se escriben en Blob Storage.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: c56b567498047ee996018675134c252ec1de7e0c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081375"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Consulta de datos de Avro con Azure Data Lake Analytics

En este artículo se explica cómo consultar los datos de Avro para enrutar de forma eficaz los mensajes de Azure IoT Hub a servicios de Azure. Como se anunció en la entrada de blog [Azure IoT Hub message routing: now with routing on message body] (Enrutamiento de mensajes de Azure IoT Hub: ahora con enrutamiento en el cuerpo del mensaje), IoT Hub ahora admite el enrutamiento en las propiedades o en el cuerpo del mensaje. Para obtener más información, consulte [Enrutamiento en los cuerpos del mensaje][Routing on message bodies]. 

La dificultad radica en que cuando Azure IoT Hub enruta los mensajes a Azure Blob Storage, IoT Hub escribe el contenido en formato Avro, que tiene propiedades de mensaje y de cuerpo de mensaje. IoT Hub solo admite la escritura de datos en Blob Storage en formato de datos de Avro, y este formato no se usa para ningún otro punto de conexión. Para obtener más información, consulte [Cuándo usar los contenedores de Azure Storage][When using Azure storage containers]. Aunque el formato Avro es muy útil para la conservación de datos y mensajes, es difícil de usar para consultar datos. En comparación, es mucho más fácil consultar datos en formato JSON o CSV.

Para atender las necesidades y los formatos de los macrodatos no relacionales y superar esta dificultad, puede usar muchos de los patrones de macrodatos para transformar y escalar datos. Uno de los modelos, "pago por consulta", es Azure Data Lake Analytics, que es el enfoque de este artículo. Aunque puede ejecutar fácilmente la consulta en Hadoop o en otras soluciones, Data Lake Analytics es con frecuencia más conveniente para el enfoque de "pago por consulta". 

Hay un "extractor" para Avro en U-SQL. Para obtener más información, consulte [Ejemplo de Avro en U-SQL] (Ejemplo de Avro en U-SQL).

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Consulta y exportación de datos de Avro a un archivo CSV
En esta sección, se consultan datos de Avro y se exportan a un archivo CSV en Azure Blob Storage, aunque podría colocar fácilmente los datos en otros almacenes de datos o repositorios.

1. Configure Azure IoT Hub para enrutar los datos a un punto de conexión de Azure Blob Storage usando una propiedad en el cuerpo del mensaje para seleccionar los mensajes.

    ![La sección "Puntos de conexión personalizados"][img-query-avro-data-1a]

    ![El comando Rutas][img-query-avro-data-1b]

2. Asegúrese de que el dispositivo tiene la codificación, el tipo de contenido y los datos necesarios en las propiedades o en el cuerpo del mensaje, tal y como se menciona en la documentación del producto. Cuando se consultan estos atributos en Device Explorer, como se muestra aquí, puede comprobar que están establecidos correctamente.

    ![El panel de datos Centro de eventos][img-query-avro-data-2]

3. Configure una instancia de Azure Data Lake Store y una instancia de Data Lake Analytics. Azure IoT Hub no enruta a una instancia de Data Lake Store, pero una instancia Data Lake Analytics requiere una.

    ![Instancias de Data Lake Store y Data Lake Analytics][img-query-avro-data-3]

4. En Data Lake Analytics, configure Azure Blob Storage como almacén adicional, el mismo Blob Storage a donde Azure IoT Hub enruta los datos.

    ![El panel "Orígenes de datos"][img-query-avro-data-4]
 
5. Como se describe en [Ejemplo de Avro en U-SQL] (Ejemplo de Avro en U-SQL), necesita cuatro archivos DLL. Cargue estos archivos en una ubicación de la instancia de Data Lake Store.

    ![Cuatro archivos DLL cargados][img-query-avro-data-5] 

6. En Visual Studio, cree un proyecto de U-SQL.
 
    ![Crear un proyecto de U-SQL][img-query-avro-data-6]

7. Pegue el contenido del siguiente script en el archivo recién creado. Modifique las tres secciones resaltadas: su cuenta de Data Lake Analytics, las rutas de acceso de los archivos DLL asociados y la ruta de acceso correcta para la cuenta de almacenamiento.
    
    ![Las tres secciones que se van a modificar][img-query-avro-data-7a]

    El script U-SQL real para la salida sencilla a un archivo CSV:
    
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

    Data Lake Analytics tardó cinco minutos en ejecutar el siguiente script, que se limitó a 10 unidades analíticas y procesó 177 archivos. El resultado se muestra en la salida del archivo CSV que aparece en la siguiente imagen:
    
    ![Resultados de la salida en el archivo CSV][img-query-avro-data-7b]

    ![Salida convertida en el archivo CSV][img-query-avro-data-7c]

    Para analizar el JSON, continúe en el paso 8.
    
8. La mayoría de los mensajes de IoT están en formato de archivo JSON. Al agregar las líneas siguientes, puede analizar el mensaje en un archivo JSON, que permite agregar cláusulas WHERE y generar solo los datos necesarios.

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

    La salida muestra una columna para cada elemento del comando `SELECT`. 
    
    ![Salida que muestra una columna para cada elemento][img-query-avro-data-8]

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a consultar los datos de Avro para enrutar de manera eficaz los mensajes de Azure IoT Hub a servicios de Azure.

Para ver ejemplos de soluciones completas que usen IoT Hub, vea [Azure IoT Remote Monitoring solution accelerator][lnk-iot-sa-land] (Acelerador de la solución de supervisión remota de Azure IoT).

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

[Ejemplo de Avro en U-SQL]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[Guía para desarrolladores de IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
